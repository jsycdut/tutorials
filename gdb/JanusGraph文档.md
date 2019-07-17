# JanusGraph

一个JanusGraph实例集群可以有1到n个JanusGraph实例,每个实例都需要做配置,最最基础的配置是存储,如果需要高级的查询支持,比如全本文检索,地理位置检索,范围查询等,就需要配置索引后端,如果性能很重要,那么需要配置Cache.

可选的存储后端有

1. Cassandra
2. HBase
3. BerkeleyDb

可选的索引后端有

1. ElasticSearch
2. Solr
3. Lucene

可选的缓存有, 等待补充

## 内嵌的JanusGraph

可以在用户应用里面直接打开一个JanusGraph实例,这称为内嵌的JanusGraph, 在此情况下,JanusGraph是用户应用的一部分,用户应用可以直接从内嵌的JanusGraph实例调用其API.

## JanusGraph Server

JanusGraph本身是一些jar包,而且是没有线程运行的,要使用一个JanusGraph, 有两种方法

1. 内嵌的JanusGraph,由应用提供执行线程
2. JanusGraph打包成一个长期执行的server进程,一旦启动后,就允许远程客户端或者逻辑上运行中的单独应用执行使用JanusGraph调用来连接,打包出来的长期运行的进程就是所谓的JanusGraph Server.

对于JanusGraph Server, 本质上是包装了一个Tinkerpop的gremlin-server, 只是提供了一些不同的API,形成了一个开箱即用的配置以确保快速使用.

@startuml
Bob -> Alice : Hello
@enduml
