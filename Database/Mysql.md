# Mysql数据库知识点

## 基本操作

### 数据库操作
- 创建数据库 `CREATE DATABASE database_name;`
- 删除数据库 `DROP DATABASE database_name;`

## 数据库概念

### 数据库引擎

#### 1. InnoDB存储引擎
- 支持事务安全表（ACID），支持行锁定和外键。Mysql5.5之后是默认引擎
- 提供了提交、回滚和崩溃恢复能力的事务安全特性
- 为处理巨大数据量的最大性能设计
- 在主内存中缓存数据和索引而维持自己的缓冲池。表和索引放在同一个逻辑表空间中，表空间可以包含多个文件。
- 锁是行级的，粒度小，写操作不会锁定全表，在并发高的时候，效率高。适合update和insert多的表
- 支持外键完整性约束
- 不保存行数，执行 `SELECT COUNT(*) FROM table;`时需要全表扫描。

#### 2. MyISAM存储引擎
- 不支持事务
- 用一个变量保存了整个表的行数，查询快
- 锁是表级的，所以频繁更新数据效率低，适合大量查询的表
- 每个表的最大索引数是64（可以通过编译改变），最大的键长度是1000B（也可以通过编译改变）
- BLOB和TEXT列可以被索引，NULL允许在索引的列中
- 可以把数据文件和索引文件放在不同的目录下
- 每个字符列可以有不同的字符集
- 创建数据库时，产生3个文件，frm存储表定义，MYD是数据文件，MYI是索引文件

### 聚集索引和非聚集索引
- InnoDB是聚集索引，MyISAM是非聚集索引。
- 聚集索引的文件存放在主键索引的叶子节点上，所以InnoDB必须要有主键。主键索引效率高，辅助索引需要查两次，第一次查主键索引，再通过主键查询到数据
- 非聚集索引的数据文件是分离的，索引保存的数据文件的指针，主键索引和辅助索引是分离的。