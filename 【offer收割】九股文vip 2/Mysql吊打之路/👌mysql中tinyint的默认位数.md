在 MySQL 中，TINYINT类型的默认显示宽度（位数）是 4。这意味着如果你没有指定显示宽度，MySQL 会默认使用 4 位来显示TINYINT类型的值。
### 1. 存储范围
TINYINT类型用于存储非常小的整数，其存储范围如下：

- **有符号**（默认）：-128到127
- **无符号**：0到255
### 2. 显示宽度

- 当你定义一个TINYINT字段时，例如TINYINT或TINYINT(3)，括号中的数字表示显示宽度。
- 如果没有指定显示宽度，MySQL 默认使用 4 作为显示宽度。
### 3. 示例
```
CREATETABLE example (
    default_tinyint TINYINT,
    custom_tinyint TINYINT(3),
    zerofill_tinyint TINYINT(3) ZEROFILL
);
INSERTINTO example (default_tinyint, custom_tinyint, zerofill_tinyint) VALUES (42, 42, 42);
SELECT default_tinyint, custom_tinyint, zerofill_tinyint FROM example;
```
在上面的例子中：

- default_tinyint字段使用默认的显示宽度 4。
- custom_tinyint字段使用指定的显示宽度 3。
- zerofill_tinyint字段使用指定的显示宽度 3，并且使用ZEROFILL属性，会显示为042。
### 4. 影响

- **存储大小**：显示宽度不会影响实际的存储大小。TINYINT类型始终占用 1 个字节的存储空间。
- **显示效果**：显示宽度主要影响的是数据的显示效果，尤其是在使用ZEROFILL属性时。没有ZEROFILL属性时，显示宽度的影响很小。
