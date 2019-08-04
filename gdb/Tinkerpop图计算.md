# Tinkerpop图计算概念

* VertexComputeKey

顶点的属性之一，用于存储GraphComputer的数据，提供一个of方法，用于构造一个VCK，为trasient的VCK在图计算结束后，返回ComputerResult之前会被清理掉。


