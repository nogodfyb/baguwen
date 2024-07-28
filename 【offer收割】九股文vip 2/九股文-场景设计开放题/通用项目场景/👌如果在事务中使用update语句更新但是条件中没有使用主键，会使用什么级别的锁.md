在 MySQL 的 InnoDB 存储引擎中，当使用`UPDATE`语句更新数据时，即使条件中没有使用主键，InnoDB 仍然会使用行级锁，而不是表级锁。具体的锁定行为和锁的范围取决于查询条件以及查询条件的选择性。
### 更新操作的锁定行为
在事务中执行`UPDATE`语句时，InnoDB 会对符合条件的行记录加上排他锁（X 锁）。如果条件中没有使用主键或唯一索引，InnoDB 会扫描表中的所有行，并对符合条件的行加锁。
### 锁定范围

- **使用主键或唯一索引**：如果条件中使用了主键或唯一索引，InnoDB 会通过索引快速定位到需要更新的行，并对这些行加锁。
- **不使用主键或唯一索引**：如果条件中没有使用主键或唯一索引，InnoDB 需要进行全表扫描，找到所有符合条件的行，并对这些行加锁。
### 示例
假设有一个表`employees`，结构如下：
```
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(100),
    salary DECIMAL(10, 2),
    INDEX (department)
) ENGINE=InnoDB;
```
#### 情况一：条件中使用主键
```
-- 使用主键更新
UPDATE employees SET salary = salary * 1.1 WHERE id = 1;
```
在这种情况下，InnoDB 会通过主键索引快速定位到`id = 1`的行，并对该行加上排他锁。
#### 情况二：条件中使用非主键索引
```
-- 使用非主键索引更新
UPDATE employees SET salary = salary * 1.1 WHERE department = 'Sales';
```
在这种情况下，InnoDB 会通过`department`索引扫描所有`department = 'Sales'`的行，并对这些行加上排他锁。
#### 情况三：条件中不使用索引
```
-- 不使用索引更新
UPDATE employees SET salary = salary * 1.1 WHERE name = 'John';
```
在这种情况下，InnoDB 需要进行全表扫描，找到所有`name = 'John'`的行，并对这些行加上排他锁。
### 间隙锁（Gap Lock）
在某些情况下，InnoDB 可能会使用间隙锁（Gap Lock）来防止幻读现象。间隙锁会锁定索引记录之间的间隙，防止其他事务在这些间隙中插入新记录。<br />例如，在可重复读（REPEATABLE READ）隔离级别下，以下查询可能会触发间隙锁：
```
-- 可重复读隔离级别下的间隙锁
UPDATE employees SET salary = salary * 1.1 WHERE department = 'Sales';
```
在扫描`department = 'Sales'`的行时，InnoDB 可能会对索引记录之间的间隙加锁，防止其他事务在这些间隙中插入新记录。
### 总结

- **行级锁**：InnoDB 使用行级锁来锁定符合条件的行记录，确保高并发性。
- **锁定范围**：具体的锁定范围取决于查询条件的选择性。如果条件中使用了主键或唯一索引，锁定范围较小。如果条件中没有使用索引，可能需要进行全表扫描。
- **间隙锁**：在某些隔离级别下，InnoDB 可能会使用间隙锁来防止幻读现象。

即使在条件中没有使用主键，InnoDB 仍然会尝试使用行级锁，只是锁定的范围可能会更大，影响性能。为了优化性能，建议尽量在查询条件中使用索引。
