# HugeGraph && JanusGraph 图计算调研报告

作者：金世钰 2019-07-18

本报告主要从源代码出发，调研了HugeGraph 0.10.0和JanusGraph 0.4.1-SNAPSHOT两种图数据库在图计算方面的实现情况，两种图数据库的源码均从下方的github地址获取。

* [hugegraph](https://github.com/hugegraph/hugegraph.git)
* [JanusGraph](https://github.com/JanusGraph/janusgraph.git)

所有源码的调试环境主要信息以及详细信息如下
1. Ubuntu 16.04
2. Oracle java 1.8.0`_`211
3. IntelliJ IDEA 2019.1.3 (Ultimate Edition)

> 系统信息
```
jsy@jsy-ubuntu:~/docs$ uname -a
Linux jsy-ubuntu 4.15.0-54-generic #58~16.04.1-Ubuntu SMP 
Mon Jun 24 13:21:41 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

> Java版本信息
```
jsy@jsy-ubuntu:~/docs$ java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
```

> IDE信息
```
IntelliJ IDEA 2019.1.3 (Ultimate Edition)
Build #IU-191.7479.19, built on May 28, 2019
Licensed to Daemon Fox
Subscription is active until September 11, 2019
JRE: 1.8.0_202-release-1483-b58 amd64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
Linux 4.15.0-54-generic
```

## HugeGraph

![HugeGraph](https://anquan.baidu.com/upload/ue/image/20180803/1533281426937554.jpg)

#### HugeGraph简介
HugeGraph是一个可高度伸缩且速度极快的图数据库，可以通过其强大的OLTP性能轻松的存储数十亿的点和边并进行快速查询。兼容Apache Tinkerpop 3 框架，各种复杂的图查询都可以通过gremlin语言完成。

**HugeGraph特性**

* 兼容Apache Tinkerpop 3框架，支持Gremlin
* 元数据管理，包括点标签，边标签，属性键和索引标签
* 支持多种索引，包括准确查询，范围查询和复杂条件结合查询
* 插件式的后端存储索引框架驱动，支持RocksDB，Cassandra，ScyllaDB以及MySQL并且可以轻松添加其他的存储后端
* 集成Hadoop/Spark

#### 调研结论

HugeGraph**不支持图计算**

具体理由如下

1. Tinkerpop体系的图计算入口是Graph具体实现中的compute方法
2. HugeGraph的compute方法没有对图计算逻辑进行支持

详细内容，参见下方HugeGraph对Graph接口的实现以及compute方法源码。

备注：实线箭头代表`extends`，意为类的继承关系，虚线箭头代表`implements`，意为接口的实现关系。

@startuml
interface AutoCloseable
interface Host
interface Graph
interface GremlinGraph

AutoCloseable <|-- Graph
Host <|-- Graph
Graph <|-- GremlinGraph
GremlinGraph <|.. HugeGraph 

HugeGraph : GraphComputer compute() throws IllegalArgumentException
@enduml

下面是HugeGraph中对compute方法的具体实现，可以看到明显不支持对图计算后续逻辑的调用，所以HugeGraph不支持图计算。

```java
@Override
public <C extends GraphComputer> C compute(Class<C> clazz) throws IllegalArgumentException {
    throw Graph.Exceptions.graphComputerNotSupported();
}

@Override
public GraphComputer compute() throws IllegalArgumentException {
    throw Graph.Exceptions.graphComputerNotSupported();
}
```

### JanusGraph

![JanusGraph](https://janusgraph.org/img/janusgraph.png)
### JanusGraph简介
JanusGraph是一个为了存储和查询分散在多机集群里的数十亿点和边的做了优化的高度可伸缩的图数据库，此外，JanusGraph还是一个事务型数据库，可以同时支持数千用户同时在线执行图的复杂遍历和分析查询。

**PS：JanusGraph简介中的分布式指的是存储的分布式**

#### 调研结论

JanusGraph**支持图计算**，并且目前在JanusGraph中运行PageRank算法已成功获取结果，但是JanusGraph执行最短路径算法时无法正确计算。

另外，JanusGraph虽然号称是分布式的图数据库，但是仅仅是存储方面是分布式的，在图计算方面仍然是单机多线程并发计算，这和GDM的图计算方案一致。

就目前综合所有的图计算的调研情况来看，包括Titan、HugeGraph、JanusGraph，并没有Tinkerpop系列的分布式图计算的已有实现方案，也就是说，如果要做分布式图计算，我们只能从0开始，没有任何参考，而且不能保证一定能研发成功。


接下来讲主要介绍JanusGraph的图计算流程，分三部分进行介绍，如下

* JanusGraph对图计算相关组件的实现
* 图计算执行流程详细介绍
* JanusGraph图计算算法JUnit单元测试

#### JanusGraph的组件介绍

下面是JanusGraph对Tinkerpop中Graph组件实现的关系一览。

@startuml
interface Host
interface AutoCloseable
interface Graph
interface SchemaInspector
interface SchemaManager
interface Transaction
interface JanusGraph
interface TypeInspector
interface VertexFactory

Host <|-- Graph
AutoCloseable <|-- Graph
SchemaInspector <|-- SchemaManager
Graph <|-- Transaction
SchemaManager <|-- Transaction
Transaction <|-- JanusGraph
JanusGraph <|.. JanusGraphBlueprintsGraph
JanusGraphBlueprintsGraph <|-- StandardJanusGraph
Transaction <|--JanusGraphTransaction
JanusGraphTransaction <|.. JanusGraphBlueprintsTransaction
JanusGraphBlueprintsTransaction <|--StandardJanusGraphTx
SchemaInspector <|.. StandardJanusGraphTx
TypeInspector <|.. StandardJanusGraphTx
VertexFactory <|.. StandardJanusGraphTx
@enduml

下面介绍接下来的代码所使用到的图数据，该图称为诸神之图，主要描述了罗马万神殿中存在诸神之间的关系，如下图

![Graph of the Gods](https://docs.janusgraph.org/latest/images/graph-of-the-gods-2.png)

**JanusGraph图计算测试单元测试**

1. PageRank算法测试代码以及输出结果
```java
@Test
public void pr() throws ExecutionException, InterruptedException {
  JanusGraph graph = JanusGraphFactory.open("conf/janusgraph-berkeleyje-es.properties");

  final String PR_KEY = "gremlin.pageRankVertexProgram.pageRank";

  ComputerResult res = graph.compute().persist(GraphComputer.Persist.VERTEX_PROPERTIES)
      .result(GraphComputer.ResultGraph.NEW)
      .program(PageRankVertexProgram.build().create()).submit().get();

  res.graph().traversal().V().toStream()
      .sorted((o1, o2) -> {
          return (double) o1.property(PR_KEY).value() > (double) o2.property(PR_KEY).value() 
          ? -1 : 1;
      })
      .forEach((v) -> System.out.println(v.id() + ":\t" + v.property(PR_KEY).toString()));

  graph.close();
}


// PageRank排名算法输出结果
4312:	vp[gremlin.pageRankVertexProgram.pageRank->0.14384166781367147]
4272:	vp[gremlin.pageRankVertexProgram.pageRank->0.1100245264481361]
12480:	vp[gremlin.pageRankVertexProgram.pageRank->0.10275253402231772]
4192:	vp[gremlin.pageRankVertexProgram.pageRank->0.09708109811708185]
12464:	vp[gremlin.pageRankVertexProgram.pageRank->0.08251830870299674]
4216:	vp[gremlin.pageRankVertexProgram.pageRank->0.07937243474864954]
4136:	vp[gremlin.pageRankVertexProgram.pageRank->0.07524631627717837]
4288:	vp[gremlin.pageRankVertexProgram.pageRank->0.07524631627717837]
8368:	vp[gremlin.pageRankVertexProgram.pageRank->0.060683526863093265]
8384:	vp[gremlin.pageRankVertexProgram.pageRank->0.060683526863093265]
4336:	vp[gremlin.pageRankVertexProgram.pageRank->0.060683526863093265]
8288:	vp[gremlin.pageRankVertexProgram.pageRank->0.051866217003510184]
```

下面分析JanusGraph的图计算执行流程，以流程图形式表示，主要包含从提交任务到算法程序核心方法的执行流程。


