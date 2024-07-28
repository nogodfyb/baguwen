UNION和UNION ALL是 SQL 中用于合并两个或多个结果集的操作符。它们的主要区别在于是否去除重复的行。
### UNION

- **去除重复行**：UNION操作会自动去除合并结果中的重复行。
- **排序操作**：由于UNION需要去除重复行，因此它会在内部执行一个排序操作来识别和删除重复行，这可能会影响性能。
```
SELECT column1, column2 FROM table1
UNION
SELECT column1, column2 FROM table2;
```
### UNION ALL

- **保留重复行**：UNION ALL操作不会去除重复行，所有的结果行都会被保留。
- **性能较好**：由于UNION ALL不需要进行去重操作，因此通常比UNION性能更好，特别是在处理大数据量时。
```
SELECT column1, column2 FROM table1
UNION ALL
SELECT column1, column2 FROM table2;
```
### 示例
假设有两个表table1和table2，它们的结构和数据如下：
```
-- table1
id | name
---|------
1  | Alice
2  | Bob

-- table2
id | name
---|------
2  | Bob
3  | Charlie
```
使用UNION：
```
SELECT id, name FROM table1
UNION
SELECT id, name FROM table2;
```
结果：
```
id | name
---|------
1  | Alice
2  | Bob
3  | Charlie
```
使用UNION ALL：
```
SELECT id, name FROM table1
UNION ALL
SELECT id, name FROM table2;
```
结果：
```
id | name
---|------
1  | Alice
2  | Bob
2  | Bob
3  | Charlie
```
### 总结

- **UNION**：去除重复行，需要额外的排序操作，性能较UNION ALL略低。
- **UNION ALL**：保留所有行，不去重，性能较好。
