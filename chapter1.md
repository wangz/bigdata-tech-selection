
# 热门大数据引擎/组件概要

## TeraData
老牌数仓公司，已经上市十几年，数仓领导者地位（from Gartner），目前在向云端发力。主要提供一体机，MPP架构，运行稳定，之前工行用的是TD的系统，价格相对较贵。

## Greenplum
2006年第一款产品，基于PostgreSQL，采用无共享MPP架构，主要用于数据分析OLAP。2010年被EMC收购，于2015年开源，拥有完整的生态。Greenplum是全球十大经典和实时数据分析产品中唯一的开源数据库。

## Vertica
无共享列存MPP，开创列存DBMS先河，2006年1.0版本，2011被惠普收购，商业版功能强大，被很多以数据为主的公司采购使用。主要用于做数据仓库和OLAP，支持时序数据、机器学习等，也可以适配hadoop,spark等，即便是适配hadoop，速度也显著超越impala，就更不用说hive on tez了。

## Hadoop(HDFS+MapReduce+Yarn)
2006年HDFS和MapReduce被纳入Hadoop项目，2008年Cloudera基于Hadoop开始提供服务。Hadoop是一个能够对大量数据进行分布式处理的软件框架，高扩展、高容错、低成本等特点可以认为是为大数据领域开了另一扇门。

## Hive
hive是基于Hadoop构建的一套数据仓库分析系统，它提供了丰富的SQL查询方式来分析存储在Hadoop分布式文件系统中的数据。Hive比较适合离线处理，因为它把SQL转MapReduce执行响应速度较慢，也可以通过Hive on Tez通过DAG来减少落盘次数来提速。

## HBase
基于Hadoop的列存数据库，特点是对大宽表的支持，支持结构化和半结构化数据。2007年第一个可用的HBase随着Hadoop 0.15.0发布。HBase的LSM-Tree架构大幅提升了写入性能，但影响了实时读取性能，真是鱼和熊掌啊。

## Impala
Hadoop之上的开源MPP查询分析引擎，C++，存储支持hdfs、hbase、S3等。 主要解决Hive速度太慢的问题。Cloudera support。可以搭配hdfs+parquet存储格式使用，也可以集成kudu一起使用。

## Spark
用于大规模数据处理的统一分析引擎。既支持作业任务处理，又支持流处理（SparkStreaming）和SQL（SparkSQL），以及机器学习和图处理，社区生态活跃。通常认为，与MR相比spark通过内存计算来显著提速。Spark社区非常成熟，后面提到的很多平台或大数据组件都与spark实现无缝集成。

## MongoDb
  todo 
## Kylin
一个开源的分布式分析引擎，提供Hadoop/Spark之上的SQL查询接口及多维分析（OLAP）能力以支持超大规模数据，最初由eBay Inc. 开发并贡献至开源社区。它能在亚秒内查询巨大的Hive表。核心是预加载和构建cube，cube指定度量维度，我觉得本质上是一种物化视图吧。

## Apache Kudu
Kudu是cloudera开源的运行在hadoop平台上的列式存储系统（fast analytics on fast data），核心C++编写。有了hdfs和hbase为什么还需要kudu？一是kudu的表结构与关系型数据库类似，使用简单；二是提供高效插入/更新机制，大量随机读性能要显著超过hbase，因此可以适用于近实时的分析，快速分析那些快速变化的数据。kudu适合做基于SQL的OLAP，其存储不依赖hdfs,也可以集成impala一起使用。

## ClickHouse
最快开源OLAP引擎。列存+定长，更能发挥向量计算的优势。稀疏矩阵+近似计算提升响应速度，但不能用于点查。对标Vertica宣称比vertica快3到5倍,速度是HIVE的300倍。俄罗斯产，代码开源很可贵，但详细文档不够丰富。

## snappyData
基于Spark+GemFire的组合实现的一个统一的OLTP+OLAP+流式写入的内存分布式数据库，Spark能适配的存储，它都可以适配支持，通过缓存到内存来提速。某些增强功能（如近似计算等）由商用版提供。

## Druid
OLAP数据库+时序支持，支持高频insert,摄入数据分Timestamp、Dimensions、Metrics三部分，按时间粒度给数据做预聚合,维度不爆炸（对比kylin）。索引使用bitmap，速度快。数据存储在Deep Storage（永久）,并加载到Historicals，可理解为冷数据和热数据，可以适配各种后端存储。	
				
## Presto
纯内存计算的OLAP引擎，原HIVE的团队搞的，HIVE依赖的MR太慢了，所以又搞了个Presto，通过并行的内存计算去提速，本身不存数据。用于做数据仓库和OLAP。TeraData support

## Google Mesa
Mesa是一个分布式、多副本的、高可用的数据处理、存储和查询系统，针对结构化数据。一般数据从上游服务产生（比如一个批次的spark streaming作业产生），在内部做数据的聚合和存储。支持近实时更新（与Cube方案比），数据分维度列和指标列，指标列指定聚合函数。

## Apache Doris
前身是百度2017年开源系统PALO，后贡献给apache更名为Doris。Doris 是一个 MPP 的 OLAP 系统，主 要整合了 Google Mesa(数据模型)，Apache Impala(MPP Query Engine)和 Apache ORCFile (存储格式，编码和压缩) 的 技术。高度兼容Mysql协议。 元数据管理对impala的p2p模式做了更新，Doris 采用 Paxos 协议以及 Memory + Checkpoint + Journal 的机制来确保元数据的 高性能及高可靠 。			
## ElasticSearch
2010年第一个版本，2012成立公司运营。目前Elasticsearch是最受欢迎的企业搜索引擎，其次是Apache Solr，也是基于Lucene。用于实时检索。看家本事是通过倒排索引支撑的全文检索

## Parquet
  基于Google Dremel 实现的一种精巧的列式存储格式。核心思想是使用“分片和聚合算法”来标示嵌套类型数据。支持多种查询引擎和计算框架，能够平滑转换多种数据模型（如Avro, Thrift..等）。聚合算法的核心是分r、d值，代表Repetition level 和 definition level。R标示发生了重复的字段的level，D:optional 或者 repeated可以不存在，但若实际存在则D值+1。通过这两个值可以使用向量机恢复数据结构。

## carbonData
华为开源的一种用于交互查询的indexed列存。通过建立多级索引，来支持多维OLAP和点查询。支持物化视图。使用上建议根据列值特点来定义良好的索引维度，避免出现强distinct值带来的负面结果。与SPARK集成更平滑，但仍然要注意使用多种后端存储格式可能带来的工作量。


# 参考：

[Gartner 2019排名：Greenplum跃居第三][1]

[浅谈从Google Mesa到百度PALO][2]

[使用Apache Kudu和Impala实现存储分层][3]

[Oracle Exadata体系笔记][4]

[Teradata 架构][5]

[有了HBase为什么还要Kudu？][6]


[1]: https://cloud.tencent.com/developer/news/405232 "Gartner 2019排名：Greenplum跃居第三"

[2]: http://neoremind.com/2017/09/%E6%B5%85%E8%B0%88%E4%BB%8Egoogle-mesa%E5%88%B0%E7%99%BE%E5%BA%A6palo/ "浅谈从Google Mesa到百度PALO"

[3]: https://cloud.tencent.com/developer/article/1491021 "使用Apache Kudu和Impala实现存储分层"

[4]: https://www.cnblogs.com/zhenxing/p/3905047.html "Oracle Exadata体系笔记"

[5]: https://www.w3cschool.cn/teradata/teradata_architecture.html "Teradata 架构"

[6]: https://leonlibraries.github.io/2017/05/18/%E4%BB%8ELSM%E5%88%B0HBase/ "有了HBase为什么还要Kudu？"
 







