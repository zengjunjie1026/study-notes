### 第1章 Presto简介 1.1 Presto 概念
Presto 是一个开源的分布式 SQL 查询引擎，适用于交互式分析查询，数据量支持 GB 到PB字节。
Presto 的设计和编写完全是为了解决像 Facebook 这样规模的商业数据仓库的交互式分 析和处理速度的问题。
注意:虽然 Presto 可以解析 SQL，但它不是一个标准的数据库。不是 MySQL、Oracle 的代替品，也不能用来处
理在线事务(OLTP)。
#### 1.2 Presto 应用场景
Presto 支持在线数据查询，包括 Hive，关系数据库(MySQL、Oracle)以及专有数据存储。一条Presto查询可以将多
个数据源的数据进行合并，可以跨越整个组织进行分析。
Presto 主要用来处理响应时间小于 1 秒到几分钟的场景。 

#### 1.3 Presto 架构
Presto 是一个运行在多台服务器上的分布式系统。完整安装包括一个 Coordinator 和多个 Worker.
由客户端提交查询，从 Presto 命令行 CLI 提交到 Coordinator。Coordinator 进行 解析，
分析并执行查询计划，然后分发处理队列到 Worker。