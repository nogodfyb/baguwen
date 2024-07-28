# 典型回答

会。

因为数据库的锁锁的是索引，并不是记录。

当我们在事务中，更新一条记录的时候，如果用到普通索引作为条件，那么会先获取普通索引的锁，然后再尝试获取主键索引的锁。

那么这个时候，如果刚好有一个线程，已经拿到了这条记录的主键索引的锁后，同时尝试在该事务中去拿该记录的普通索引的锁。

这时候就会发生死锁。

```java
update my_table set name = 'hollis',age = 22 where name = "hollischuang";

这个SQL会先对name加锁， 然后再回表对id加锁。

-----

select * from my_table where id = 15 for update;

update my_table set age = 33 where name like "hollis%";

以上SQL，会先获取主键的锁，然后再获取name的锁。
```

为了避免这种死锁情况的发生，可以在应用程序中设置一个规定的索引获取顺序，例如，只能按照主键索引->普通索引的顺序获取锁，这样就可以避免不同的线程出现获取不同顺序锁的情况，进而避免死锁的发生（靠SQL保证）。

