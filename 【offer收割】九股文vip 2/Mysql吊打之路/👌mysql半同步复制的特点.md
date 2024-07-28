（1）从库会在连接到主库时告诉主库，它是不是配置了半同步。<br />（2）如果半同步复制在主库端开启，并且至少有一个半同步复制的从库节点，那么此时主库的事务线程在提交时会被阻塞并等待，结果有两种可能：<br />（a）至少一个从库节点通知它已经收到了所有这个事务的Binlog事件；<br />（b）一直等待直到超过配置的某一个时间点为止，此时，半同步复制将自动关闭，转换为异步复制。（3）从库节点只有在接收到某一个事务的所有 Binlog，将其写入到 Relay Log 文件之后，才会通知对应主库上面的等待线程。<br />（4）如果在等待过程中，等待时间已经超过了配置的超时时间，没有任何一个从节点通知当前事务，那么此时主库会自动转换为异步复制，当至少一个半同步从节点赶上来时，主库便会自动转换为半同步方式的复制。（5）半同步复制必须是在主库和从库两端都开启时才行，如果在主库上没打开，或者在主库上开启了而在从库上没有开启，主库都会使用异步方式复制。
### 配置半同步复制
#### 在主服务器上

1. **安装半同步复制插件**：
```
INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so';
```

1. **启用半同步复制**：
```
SET GLOBAL rpl_semi_sync_master_enabled = 1;
```

1. **设置超时时间（可选）**：
```
SET GLOBAL rpl_semi_sync_master_timeout = 10000;  -- 单位为毫秒
```
#### 在从服务器上

1. **安装半同步复制插件**：
```
INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so';
```

1. **启用半同步复制**：
```
SET GLOBAL rpl_semi_sync_slave_enabled = 1;
```
### 检查半同步复制状态

- **在主服务器上**：
```
SHOW STATUS LIKE 'Rpl_semi_sync%';
```

- **在从服务器上**：
```
SHOW STATUS LIKE 'Rpl_semi_sync%';
```
### 半同步复制的监控和故障排除

- **监控复制状态**：定期检查SHOW SLAVE STATUS\G和SHOW STATUS LIKE 'Rpl_semi_sync%';的输出，确保半同步复制的各个线程正常运行。
- **处理超时问题**：如果发现复制延迟较大或超时，可以调整rpl_semi_sync_master_timeout参数，或者检查网络和从服务器的性能。
