# Neo4j Preparation

## 整体架构

Neo4j属于C/S或者B/S类型，分为服务端和客户端两部分，其实就是服务器端提供了一个侦听端口，然后可以通过浏览器通过HTTP协议访问，另外由于前端技术可以将一个前端应用打包为不同操作平台下的可执行文件，所以可以通过客户端的形式访问。

Neo4j总体来说有两部分，第一是Neo4j-Server，第二是Neo4j-Browser，除此之外，还有一个Neo4j-algorithm，用于Neo4j-Server执行一些图算法。

Neo4j-Server有企业版，社区版之分，Neo4j-Browser打包之后的产品就是Neo4j-Desktop。建议的操作是，下载Neo4j-Server社区版，启动之后通过浏览器URL访问。

Neo4j-Server启动的http侦听端口是localhost:7474，http这一端口负责侦听前台请求，然后具体执行，还要将请求发送给进一步的后端，这一步使用的协议是bolt协议，该协议的典型端口是7687，我查看了bolt的官网，里面很多示例都和neo4j有关，估计是neo4j公司自己开发的协议。在浏览器连接上Neo4j-Server之后，可以通过`:server status`查看连接状态

```bash
# 进入Neo4j社区版的目录
$ cd /path/to/your/neorj-community-edition

# 进入Neo4j社区版的bin目录
$ cd bin

# 启动Neo4j-Server，启动之后仔细阅读屏幕输出，里面包含了访问信息
$ bash neo4j start

# 接下来就可以使用浏览器访问，第一次访问的时候，有一个验证，用户名密码都是neo4j，然后会要求你改密码。
```

* [Neo4j下载中心](https://neo4j.com/download-center/)
* [Neo4j-Github](https://github.com/neo4j/neo4j)
