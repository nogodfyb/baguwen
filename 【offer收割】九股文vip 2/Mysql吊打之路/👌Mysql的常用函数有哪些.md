### 字符串函数
1. **CONCAT(str1, str2, ...)**：连接多个字符串。
```
SELECT CONCAT('Hello', ' ', 'World');
```

1. **SUBSTRING(str, pos, len)**：从字符串str的pos位置开始，截取长度为len的子字符串。
```
SELECT SUBSTRING('Hello World', 7, 5);
```

1. **LENGTH(str)**：返回字符串的长度（字节数）。
```
SELECT LENGTH('Hello');
```

1. **UPPER(str)**：将字符串转换为大写。
```
SELECT UPPER('hello');
```

1. **LOWER(str)**：将字符串转换为小写。
```
SELECT LOWER('HELLO');
```

1. **TRIM(str)**：去除字符串两端的空格。
```
SELECT TRIM('  Hello World  ');
```

1. **REPLACE(str, from_str, to_str)**：将字符串str中的from_str替换为to_str。
```
SELECT REPLACE('Hello World', 'World', 'MySQL');
```
### 数值函数

1. **ABS(x)**：返回x的绝对值。
```
SELECT ABS(-10);
```

1. **CEIL(x)**或**CEILING(x)**：返回大于或等于x的最小整数。
```
SELECT CEIL(4.2);
```

1. **FLOOR(x)**：返回小于或等于x的最大整数。
```
SELECT FLOOR(4.8);
```

1. **ROUND(x, d)**：将x四舍五入到d位小数。
```
SELECT ROUND(123.456, 2);
```

1. **RAND()**：返回一个 0 到 1 之间的随机数。
```
SELECT RAND();
```

1. **POWER(x, y)**或**POW(x, y)**：返回x的y次幂。
```
SELECT POWER(2, 3);
```
### 日期和时间函数

1. **NOW()**：返回当前日期和时间。
```
SELECT NOW();
```

1. **CURDATE()**或**CURRENT_DATE()**：返回当前日期。
```
SELECT CURDATE();
```

1. **CURTIME()**或**CURRENT_TIME()**：返回当前时间。
```
SELECT CURTIME();
```

1. **DATE_FORMAT(date, format)**：格式化日期。
```
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
```

1. **DATEDIFF(date1, date2)**：返回两个日期之间的天数差。
```
SELECT DATEDIFF('2024-12-31', '2024-01-01');
```

1. **ADDDATE(date, interval)**或**DATE_ADD(date, interval)**：在日期上加上一个时间间隔。
```
SELECT ADDDATE('2024-01-01', INTERVAL10DAY);
```

1. **SUBDATE(date, interval)**或**DATE_SUB(date, interval)**：在日期上减去一个时间间隔。
```
SELECT SUBDATE('2024-01-11', INTERVAL10DAY);
```
### 聚合函数

1. **COUNT(expression)**：返回满足条件的行数。
```
SELECT COUNT(*) FROM table_name;
```

1. **SUM(expression)**：返回数值列的总和。
```
SELECT SUM(column_name) FROM table_name;
```

1. **AVG(expression)**：返回数值列的平均值。
```
SELECT AVG(column_name) FROM table_name;
```

1. **MAX(expression)**：返回列的最大值。
```
SELECT MAX(column_name) FROM table_name;
```

1. **MIN(expression)**：返回列的最小值。
```
SELECT MIN(column_name) FROM table_name;
```
### 其他常用函数

1. **IF(condition, true_value, false_value)**：条件判断函数。
```
SELECT IF(1>0, 'true', 'false');
```

1. **COALESCE(value1, value2, ...)**：返回第一个非 NULL 的值。
```
SELECT COALESCE(NULL, NULL, 'MySQL');
```

1. **IFNULL(expression, alt_value)**：如果expression为 NULL，返回alt_value。
```
SELECT IFNULL(NULL, 'default');
```

1. **NULLIF(expr1, expr2)**：如果expr1等于expr2，返回 NULL，否则返回expr1。
```
SELECT NULLIF(1, 1);
```
