在Redis中，你可以使用多种命令来设置键的过期时间或将键设置为永久有效。以下是相关命令和示例：
### 设置键的过期时间

1. **使用EXPIRE命令，**EXPIRE命令用于设置键的过期时间，以秒为单位。
```
EXPIRE key seconds
例如：
EXPIRE mykey 60  # 设置 mykey 的过期时间为60秒
```

1. **使用PEXPIRE命令，**PEXPIRE命令用于设置键的过期时间，以毫秒为单位。
```
PEXPIRE key milliseconds
例如：
PEXPIRE mykey 60000  # 设置 mykey 的过期时间为60000毫秒（即60秒）
```

1. **使用EXPIREAT命令，**EXPIREAT命令用于设置键的过期时间为指定的 Unix 时间戳，以秒为单位。
```
EXPIREAT key timestamp
例如：
EXPIREAT mykey 1672531199  # 设置 mykey 的过期时间为指定的 Unix 时间戳
```

1. **使用PEXPIREAT命令，**PEXPIREAT命令用于设置键的过期时间为指定的 Unix 时间戳，以毫秒为单位。
```
PEXPIREAT key milliseconds-timestamp
例如：
PEXPIREAT mykey 1672531199000  # 设置 mykey 的过期时间为指定的 Unix 时间戳（毫秒）
```

1. **使用SET命令带选项，**SET命令可以在设置键值的同时指定过期时间。
```
SET key value EX seconds
SET key value PX milliseconds
例如：
SET mykey "value" EX 60  # 设置 mykey 的值为 "value" 并使其在60秒后过期
SET mykey "value" PX 60000  # 设置 mykey 的值为 "value" 并使其在60000毫秒（即60秒）后过期
```
### 设置键为永久有效

1. **使用PERSIST命令，**PERSIST命令用于移除键的过期时间，使其变为永久有效。
```
PERSIST key
例如：
PERSIST mykey  # 移除 mykey 的过期时间，使其变为永久有效
```
