# 典型回答

在不考虑 MySQL 8.0中的索引跳跃扫描（先别管是啥，后文有介绍）的情况下，走索引情况如下：

| 查询条件 | 是否走索引 |
| --- | --- |
| A | 能走 |
| B | 不走 |
| C | 不走 |
| AB | 能走 |
| AC | 能走 |
| BC | 不走 |
| ABC | 能走 |
| BA | 同 AB |
| CA | 同 AC |
| CB | 同 BC |
| BAC | 同 ABC |
| CBA | 同 ABC |


通过以上表格，你会发现，只有条件中包含 A 的才能走索引，不包含A 的都不能走索引。原因就是因为最左前缀匹配。

[✅什么是最左前缀匹配？为什么要遵守？](https://www.yuque.com/hollis666/fo22bm/cc9mglopp4nigg59?view=doc_embed)

**A,B,C 三个字段创建的联合索引，A 字段在最左边，其次是 B，在其次才是 C，所以，按照最左前缀匹配的原则，想要走索引，至少要先 match 上最左边的 A。**

在命中了 A 的情况下，**AB 相比于 AC 的性能要更好一些**，因为他能用到 A 和 B两个字段的索引，而 AC 只能用上 A，而用不上 C，因为他跳过了 B。

然后，还有就是 AB 和BA 是一样的，where 条件中的先后顺序，不影响索引的使用。

[✅where条件的顺序影响使用索引吗？](https://www.yuque.com/hollis666/fo22bm/nwm3ry85o8l0gega?view=doc_embed)


# 扩展知识

[✅什么是索引跳跃扫描](https://www.yuque.com/hollis666/fo22bm/ixpnm8nvbfa9l7gm?view=doc_embed)
