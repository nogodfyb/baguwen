### 1. 数据定义语言 (DDL)
DDL 语句用于定义和管理数据库结构，例如创建、修改和删除数据库和表。

- **CREATE**：创建数据库、表、索引等。
```
CREATE DATABASE mydatabase;
CREATE TABLE mytable (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);
CREATE INDEX idx_name ON mytable(name);
```

- **ALTER**：修改现有数据库对象的结构。
```
ALTER TABLE mytable ADD COLUMN age INT;
ALTER TABLE mytable MODIFY COLUMN name VARCHAR(100);
ALTER TABLE mytable DROP COLUMN age;
```

- **DROP**：删除数据库、表、索引等。
```
DROP DATABASE mydatabase;
DROP TABLE mytable;
DROP INDEX idx_name ON mytable;
```

- **TRUNCATE**：清空表中的所有数据，但保留表结构。
```
TRUNCATE TABLE mytable;
```
### 2. 数据操作语言 (DML)
DML 语句用于操作数据库中的数据，例如插入、更新、删除和查询数据。

- **INSERT**：插入数据。
```
INSERT INTO mytable (id, name) VALUES (1, 'Alice');
INSERT INTO mytable (id, name) VALUES (2, 'Bob');
```

- **UPDATE**：更新数据。
```
UPDATE mytable SET name = 'Charlie' WHERE id = 1;
```

- **DELETE**：删除数据。
```
DELETE FROM mytable WHERE id = 2;
```

- **SELECT**：查询数据。
```
SELECT * FROM mytable;
SELECT name FROM mytable WHERE id = 1;
```
### 3. 数据控制语言 (DCL)
DCL 语句用于控制数据库的访问权限。

- **GRANT**：授予用户权限。
```
GRANT SELECT, INSERT ON mydatabase.* TO 'user'@'localhost';
```

- **REVOKE**：撤销用户权限。
```
REVOKE INSERT ON mydatabase.* FROM 'user'@'localhost';
```
### 4. 事务控制语言 (TCL)
TCL 语句用于管理事务，确保数据的一致性和完整性。

- **START TRANSACTION**：开始一个事务。
```
START TRANSACTION;
```

- **COMMIT**：提交事务。
```
COMMIT;
```

- **ROLLBACK**：回滚事务。
```
ROLLBACK;
```

- **SAVEPOINT**：设置保存点。
```
SAVEPOINT savepoint1;
```

- **RELEASE SAVEPOINT**：释放保存点。
```
RELEASE SAVEPOINT savepoint1;
```

- **ROLLBACK TO SAVEPOINT**：回滚到保存点。
```
ROLLBACK TO SAVEPOINT savepoint1;
```
### 5. 数据查询语言 (DQL)
DQL 主要包括SELECT语句，用于查询数据库中的数据。

- **SELECT**：查询数据。
```
SELECT * FROM mytable;
SELECT name FROM mytable WHERE id = 1;
SELECT COUNT(*) FROM mytable;
```
### 6. 其他

- **EXPLAIN**：解释查询执行计划。
```
EXPLAIN SELECT*FROM mytable WHERE name ='Alice';
```

- **SHOW**：显示数据库对象的信息。
```
SHOW DATABASES;
SHOW TABLES;
SHOW COLUMNS FROM mytable;
SHOW INDEX FROM mytable;
```

- **DESCRIBE**：显示表的结构。
```
DESCRIBE mytable;
```
