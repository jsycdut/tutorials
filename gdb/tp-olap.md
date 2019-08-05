# Tinkerpop图计算概念

**tp中的olap概念**

包括GraphComputer Memory MapReduce Message Messenger MessageBoard VertexProgram ComputeKey等。

需要搞清楚以下的点

1. olap的主要流程是怎么样的
2. vp是如何被每个顶点使用的
3. 每个顶点又是怎么把消息传出去和接收到的
4. vp是怎么利用消息的
5. 消息如何传递
6. 怎么决定一个vp执行已经执行完毕
7. mr在干啥

* VertexComputeKey

顶点的属性之一，用于存储GraphComputer的数据，提供一个of方法，用于构造一个VCK，为trasient的VCK在图计算结束后，返回ComputerResult之前会被清理掉。

* MemoryComputeKey

MCK主要维护一个BinaryOperator，用于在图计算中将多个并行的值规约为一个值，需要注意一个BinaryOperator的可广播性和可trasient性，同样提供了of方法，用于构造一个MCK

### 参考
* [官方文档-olap实现](http://tinkerpop.apache.org/docs/3.4.2/dev/provider/#olap-implementations)
