### 在线DDL添加索引
在MySQL 5.5及更早版本，InnoDB不支持在线DDL操作，添加索引会锁定表，阻止任何读写操作，直到索引创建完成。<br />在MySQL 5.6及以上版本中，InnoDB存储引擎支持在线添加索引，这意味着在大多数情况下，表仍然可以被读取和写入。只有在索引创建的初始和最终阶段会短暂锁定表。
#### 使用方法
要在线添加索引，你可以使用标准的`ALTER TABLE`语句。MySQL会自动决定是否可以在线完成操作。
```
ALTERTABLE table_name ADD INDEX index_name (column_name);
```
#### 行为

- **初始和最终阶段锁定**：在索引创建的开始和结束阶段，表会被短暂锁定。
- **中间阶段无锁定**：在索引创建的中间阶段，表可以继续进行读写操作。
### 使用Percona Toolkit的pt-online-schema-change（主流）
对于一些复杂的DDL操作或者在不支持在线DDL的环境中，可以使用Percona Toolkit的`pt-online-schema-change`工具进行在线模式变更。这是一个强大的工具，可以在不中断服务的情况下进行DDL操作。
```
pt-online-schema-change --alter "ADD INDEX idx_last_name (last_name)" D=database_name,t=employees --execute
```
### 注意事项

1. **性能影响**：虽然在线DDL操作不会完全锁定表，但仍然会对性能产生一定影响，特别是在高负载的环境中。
2. **空间需求**：在线添加索引可能需要额外的磁盘空间，因为MySQL会创建一个临时表来构建新的索引。
3. **版本要求**：确保你的MySQL版本在5.6及以上，并且使用InnoDB存储引擎，以支持在线DDL操作。
