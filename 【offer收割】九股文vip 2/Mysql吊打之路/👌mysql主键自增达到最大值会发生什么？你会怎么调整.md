当自增主键达到其数据类型的最大值时：

- **有符号整数**：
   - TINYINT: 最大值为127
   - SMALLINT: 最大值为32767
   - MEDIUMINT: 最大值为8388607
   - INT: 最大值为2147483647
   - BIGINT: 最大值为9223372036854775807
- **无符号整数**：
   - TINYINT UNSIGNED: 最大值为255
   - SMALLINT UNSIGNED: 最大值为65535
   - MEDIUMINT UNSIGNED: 最大值为16777215
   - INT UNSIGNED: 最大值为4294967295
   - BIGINT UNSIGNED: 最大值为18446744073709551615

当达到这些最大值时，再插入新的记录会导致错误，通常是类似ERROR 1062 (23000): Duplicate entry '...' for key 'PRIMARY'。
### 如何调整
#### 1、更改数据类型
如果预计主键值会超过当前数据类型的范围，可以更改主键的数据类型，使其支持更大的值。例如，将INT更改为BIGINT：
```
ALTERTABLE your_table MODIFY COLUMN id BIGINT AUTO_INCREMENT;
```
#### 2、使用无符号类型
如果当前使用的是有符号类型，可以考虑切换到无符号类型，以获得更大的正整数范围：
```
ALTERTABLE your_table MODIFY COLUMN id INT UNSIGNED AUTO_INCREMENT;
```
#### 3、重置自增值
如果主键值的增长是由于大量删除操作导致的中断，可以尝试重置自增值：
```
ALTERTABLE your_table AUTO_INCREMENT =1;
```
但这通常需要确保当前表中没有重复的主键值，并且可能需要删除或归档现有数据。
#### 4、归档旧数据
如果表中的数据量很大，可以考虑将旧数据归档到其他表或数据库中，以释放主键值的空间。
#### 5、复用删除的主键值
在某些情况下，可以复用删除的主键值，但这通常是不推荐的，因为它会导致主键值的不连续性和潜在的冲突。
### 预防措施
#### 1、监控自增值
定期监控表中的自增值，确保在接近最大值之前采取措施。可以通过以下查询查看当前自增值：
```
SELECT AUTO_INCREMENTFROM information_schema.TABLESWHERE TABLE_SCHEMA ='your_database'AND TABLE_NAME ='your_table';
```
#### 2、规划数据类型
在设计数据库时，合理规划主键的数据类型，根据预期的数据量选择合适的数据类型。
