# MySQL Optimization

## 数据库高并发读和写，如何优化

关于读

- 热点数据尽量避免请求数据库，可以用缓存加速。
- 使用cache table和 summary table。
- 使用索引，优化SQL。

关于写

- 尽量避免产生锁，使用插入代替修改。
- 读写通用。
- 分而治之，使用集群来降低每个MySQL的总读写请求数。