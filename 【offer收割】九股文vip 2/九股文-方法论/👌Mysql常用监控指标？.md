1、QPS：数据库每秒处理的请求数量
```
show global status where variable_name in ('Queries'， 'uptime');
```
QPS = (Queries2 -Queries1) / (uptime2 - uptime1)<br />2、TPS：数据库每秒处理的事务数量
```
show global status where variable_name in ('com_insert' ， 'com_delete' ， 'com_update'， 'uptime');
```
**事务数TC** ≈'com_insert' ， 'com_delete' ， 'com_update'<br />**TPS** ≈ (TC2 -TC1) / (uptime2 - uptime1)<br />3、并发数：数据库实例当前并行处理的会话数量
```
show global status like 'Threads_running';
```
4、连接数：连接到数据库会话的数量
```
show global status like 'Threads_connected';
```
5、缓存命中率：查询命中缓存的比例<br />**innodb缓冲池查询总数：**
```
show global status like 'innodb_buffer_pool_read_requests';
```
**innodb从磁盘查询数：**
```
show global status like 'innodb_buffer_pool_reads';
```
生产中配置报警阈值：innodb_buffer_pool_read_requests /(innodb_buffer_pool_read_requests + innodb_buffer_pool_reads) > 0.95 <br />6、可用性：数据库是否可以正常对外服务<br />周期性连接数据库并执行 **select @@version;**<br />7、阻塞：当前阻塞的会话数
```
select waiting_pid as '被阻塞线程'，
    waiting_query as '被阻塞SQL'，
     blocking_pid as '阻塞线程'，
     blocking_query as '阻塞SQL'，
     wait_age as '阻塞时间'，
     sql_kill_blocking_query as '建议操作'
from sys.innodb_lock_waits
where(unix_timestamp()-unix_timestamp(wait_started))>阻塞秒数
```
8、慢查询：慢查询情况<br />开启慢查询日志**。**my.inf 
```
slow_query_log=on
slow_query_log_file=存放目录
long_query_time=0.1秒
log_queries_not_using_indexes=on
```
