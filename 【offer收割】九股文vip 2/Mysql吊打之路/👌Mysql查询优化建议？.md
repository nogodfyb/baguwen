### 1. 避免全表扫描
优化查询时，应避免全表扫描，优先在 WHERE 和 ORDER BY 涉及的列上建立索引。
### 2. 避免 NULL 值判断
避免在 WHERE 子句中对字段进行 NULL 值判断。创建表时，尽量使用 NOT NULL 或使用特殊值（如 0 或 -1）作为默认值。
### 3. 避免 != 或 <> 操作符
避免在 WHERE 子句中使用 != 或 <> 操作符。MySQL 仅对 <，<=，=，>，>=，BETWEEN，IN 以及某些情况下的 LIKE 使用索引。
### 4. 避免 OR 条件
避免在 WHERE 子句中使用 OR 连接条件，因这会导致全表扫描。可以使用 UNION 合并查询：
```
SELECT id FROM t WHERE num=10 UNION ALL SELECT id FROM t WHERE num=20;
```
### 5. 谨慎使用 IN 和 NOT IN
谨慎使用 IN 和 NOT IN，因其可能导致全表扫描。对于连续数值，用 BETWEEN 替代 IN：
```
SELECT id FROM t WHERE num BETWEEN 1 AND 3;
```
### 6. LIKE 查询优化
避免使用%abc%或%abc的 LIKE 查询，这会导致全表扫描。可以考虑使用全文检索。只有abc%的 LIKE 查询会使用索引。
### 7. 避免参数化查询导致的全表扫描
避免在 WHERE 子句中使用参数，这会导致全表扫描。可以强制查询使用索引：
```
SELECT id FROM t WITH (INDEX(索引名)) WHERE num=@num;
```
### 8. 避免表达式操作
避免在 WHERE 子句中对字段进行表达式操作，这会导致全表扫描。
### 9. 使用 EXISTS 替代 IN
使用 EXISTS 替代 IN，可以提高查询效率：
```
SELECT num FROM a WHERE EXISTS (SELECT 1 FROM b WHERE num=a.num);
```
### 10. 索引数量控制
索引可以提高 SELECT 效率，但会降低 INSERT 和 UPDATE 性能。一个表的索引数最好不要超过 6 个，需慎重考虑是否在不常用的列上建立索引。<br />通过这些优化建议，可以有效提高 MySQL 查询的执行效率。
