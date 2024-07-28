### 1. 错误日志（Error Log）
**作用**：记录 MySQL 服务器启动、运行和停止期间发生的错误和重要事件。<br />**配置**： 可以在 MySQL 配置文件中设置错误日志的路径和名称：
```
[mysqld]
log_error = /var/log/mysql/mysql-error.log
```
**查看**： 错误日志可以通过查看配置的日志文件来检查，例如：
```
cat /var/log/mysql/mysql-error.log
```
### 2. 查询日志（General Query Log）
**作用**：记录所有客户端连接和执行的 SQL 语句。这对于调试和审计非常有用，但由于记录了所有查询，可能会对性能产生影响。<br />**配置**： 可以在 MySQL 配置文件中启用查询日志并设置日志文件路径：
```
[mysqld]
general_log = 1
general_log_file = /var/log/mysql/mysql-query.log
```
**查看**： 查询日志可以通过查看配置的日志文件来检查，例如：
```
cat /var/log/mysql/mysql-query.log
```
### 3. 慢查询日志（Slow Query Log）
**作用**：记录执行时间超过指定阈值的查询。这对于优化性能和识别慢查询非常有用。<br />**配置**： 可以在 MySQL 配置文件中启用慢查询日志并设置日志文件路径和阈值时间：
```
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/log/mysql/mysql-slow.log
long_query_time = 2  # 记录执行时间超过 2 秒的查询
```
**查看**： 慢查询日志可以通过查看配置的日志文件来检查，例如：
```
cat /var/log/mysql/mysql-slow.log
```
### 4. 中继日志（Relay Log）
**作用**：在 MySQL 复制环境中，从服务器（slave）使用中继日志记录从主服务器（master）接收到的二进制日志事件。中继日志用于应用这些事件以保持数据同步。<br />**配置**： 中继日志通常由 MySQL 自动管理，但可以在配置文件中指定中继日志的路径：
```
[mysqld]
relay_log = /var/log/mysql/mysql-relay-bin
```
**查看**： 中继日志的状态可以通过以下命令查看：
```
SHOW SLAVE STATUS\G;
```
### 5. 事务日志（Undo Log）
**作用**：记录事务的撤销操作，用于支持事务的回滚和 MVCC（多版本并发控制）。撤销日志通常是 InnoDB 存储引擎的一部分。<br />**配置**： 撤销日志由 InnoDB 自动管理，通常不需要手动配置。<br />**查看**： 撤销日志的状态可以通过以下命令查看：
```
SHOW ENGINE INNODB STATUS;
```
### 6. 表空间日志（Tablespace Log）
**作用**：记录表空间的扩展、收缩等操作。主要用于管理 InnoDB 的物理存储。<br />**配置**： 表空间日志由 InnoDB 自动管理，通常不需要手动配置。
