INT(1)和INT(10)的区别并不在于它们能存储的数值范围，而是在于显示宽度（display width）。以下是详细的解释：
### 1. 数值范围

- 无论是INT(1)还是INT(10)，它们的数值范围都是由INT类型决定的，而不是括号中的数字。
- INT类型的数值范围（无符号）是-2147483648到2147483647。
### 2. 显示宽度

- INT(1)和INT(10)中的数字（1 和 10）表示的是显示宽度，而不是存储的数值范围。
- 显示宽度是指当你使用ZEROFILL属性时，MySQL 会将数字填充到指定的宽度。例如，如果你定义了INT(5) ZEROFILL，并插入值42，它会显示为00042。
### 3. 示例
```
CREATE TABLE example (
    int_one INT(1),
    int_ten INT(10) ZEROFILL
);

INSERT INTO example (int_one, int_ten) VALUES (5, 42);

SELECT int_one, int_ten FROM example;
```
在上面的例子中：

- int_one字段的显示宽度为 1，但实际存储的数值范围和普通的INT类型一样。
- int_ten字段的显示宽度为 10，并且由于使用了ZEROFILL属性，插入的值42会显示为0000000042。
### 4. 影响

- **存储大小**：显示宽度不会影响实际的存储大小。INT类型始终占用 4 个字节的存储空间。
- **显示效果**：显示宽度主要影响的是数据的显示效果，尤其是在使用ZEROFILL属性时。没有ZEROFILL属性时，显示宽度的影响很小。
