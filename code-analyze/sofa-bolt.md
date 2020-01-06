# sofa-bolt 源码结构分析
<p align="center">
  <img src=""/>
</p>
<p align="center"></p>

`作者：金世钰`

## sofa-bolt主要代码模块UML
sofa-bolt作为一个通信框架，整体来看，是对Netty的一个封装，其底层仍然是Netty的体系，只是在上面做了一些自定义的处理，比如连接的管理，请求模式的封装，结果的处理等等，对外提供了比较友好的接口，使得用户不必关心底层的实现，不像Tinkerpop那样直接拿Netty来进行编程。封装的好处就在于使用更加简单了。

### 框架示意
sofa-bolt官方文档对sofa-bolt的架构如下图，主要分为协议实现，协议骨架和远程核心调用接口三部分组成
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/intro.png"/>
</p>
<p align="center">sofa-bolt的整体框架结构</p>

**协议实现**这一部分RPC协议和消息协议两部分，其中RPC协议这一部分讲述的比较清楚，至于消息协议目前在源码中表现的比较模糊，应该是指序列化那一部分。至于RPC这一部分，在源码中体现的比较明显，主要是`RpcProtocal.java和RpcProtocalV2.java`两部分。在整个RPC协议中，都主要包含了协议，请求id，编码器，响应状态，协议中传出的数据的类长度，协议头长度，协议内容长度等，其中RpcProtocolV2的编码字段如下所示。

```
 * Request command protocol for v2
 * 0     1     2           4           6           8      10     11     12          14     16
 * +-----+-----+-----+-----+-----+-----+-----+-----+-----+-+-----+------+-----+-----+----+--+
 * |proto| ver1|type | cmdcode   |ver2 |   requestId       |codec|switch|   timeout         |
 * +-----------+-----------+-----------+-----------+-------+------------+-----------+-------+
 * |classLen   |headerLen  |contentLen             |       ...                              |
 * +-----------+-----------+-----------+-----------+                                        +
 * |               className + header  + content  bytes                                     |
 * +                                                                                        +
 * |                               ... ...                              | CRC32(optional)   |
 * +----------------------------------------------------------------------------------------+
 * 
```

**协议框架**主要包含了请求响应封装为Command对象，对每个Command对象又提供了对应的CommandHandler用于处理该Command，另外对应对Command的编解码器，把Command转化为字节流，另外提供了端到端的心跳机制。

**RemotingCore**主要是提供了对外的请求接口，连接管理和用户自定义消息处理器的管理机制，这里面对使用者比较重要的是自定义消息处理器。

框架整体支持四种请求模式，分别是oneway, sync, future, callback四种，四种请求模式的示意图如下所示

<p align="center">
  <img src="https://github.com/sofastack/sofa-bolt/raw/master/.middleware-common/invoke_types.png"/>
</p>

<p align="center">sofa-bolt的四种请求响应模式</p>

* oneway 只负责发消息，不管消息对方是否能收到
* sync 负责发消息，同时必须在指定时间内接收到对方的响应，否则抛出异常
* future 发消息，将消息响应作为异步的结果进行获取
* callback 发消息，同时在收到对方的响应之后执行对应的回调函数

### c/s端划分
sofa-bolt基于c/s模型，也就是客户端/服务器模式，一个发请求，一个接受请求并处理请求然后回应结果。c/s整体的UML体系图如下所示。

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/Configurable.png"/>
</p>
<p align="center">sofa-bolt c/s体系UML示意图</p>

除去杂乱的配置，其核心的体系UML图如下，可以看到c端和s端的继承体系是非常相似的。
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/bolt-client.png">
</p>

```
@startuml
interface Configurable
interface RemotingServer
interface BoltClient

abstract AbstractRemotingServer
abstract AbstractBoltClient

Configurable <|-- RemotingServer
Configurable <|-- BoltClient

RemotingServer <|.. AbstractRemotingServer
BoltClient <|.. AbstractBoltClient

AbstractRemotingServer <|-- RpcServer
AbstractBoltClient <|-- RpcClient
@enduml
```


### 调用接口继承体系
在sofa-bolt中，将Netty封装后提供的接口主要在BaseRemoting为基类的类中，其继承结构主要如下图所示
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/bolt-remote.png"/>
</p>
<p align="center">sofa-bolt提供的主要的通信接口
</p>

其中，BaseRemoting主要定义了通信的几种方式，包括`oneway, invokeSync, invokeWithFuture, invokeWithFuture`四种，然后RpcRemoting进行了部分细化，将c端和s端的具体实现交给了RpcClientRemoting和RpcServerRemoting进行实现。这里的结构有点类似门面模式。上面的RpcClient和RPCServer也提供了对外的通信的接口，主要是对这里BaseRemoting相关方法的调用，比如RpcClient提供的oneway调用方式，其实是调用其内部RpcClientRemoting的oneway方法，细节由RpcClientRemoting处理，RpcClient就是调用就行了，这一点有点像Spring MVC里面的Controller层调用Service层的方法，然后Service层又调用Dao层的方法一样，是个一层层的调用（层级调用，不是链式调用）。


### 请求响应封装
sofa-bolt将请求响应封装成了以RemotingCommand为基类的一系列Command实现，请求封装为RequestCommand，响应封装为ResponseCommand，特定类型的Command又继续细分，所有的Command都是可序列化的，可以通过序列化器形成为字节数组然后通过Netty在网络间传输，其继承体系结构如下图

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/RemotingCommand.png"/>
</p>
<p align="center">请求响应命令的封装体系 </p>

每个Command有一个CommandCode标识其类型，表示其为请求还是响应或者心跳之类的Command类型，这个的体系uml图如下，其中CommonCommandCode主要是对心跳类型的，请求和响应类型在RpcCommandCode里面，这里的实现代码在枚举应用了方法，这是很常用的，枚举不仅只是单独写几个类型在里面那么简单，另外枚举中标识了类型的值用的是short，要省一点空间使用。

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/CommandCode.png"/>
</p>
<p align="center"></p>

有了Command作为请求响应的封装，那么如何构建Command就是接下来要解决的问题，sofa-bolt采用了类似简单工厂的设计模式（其实很难对sofa-bolt里面的某些类型直接归入某种特定的设计模式，一方面来源于我对设计模式的掌握程度不高，另一方面则来源于sofa-bolt确实没有特别规范的使用设计模式）

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/RpcCommandFactory.png"/>
</p>
<p align="center">sofa-bolt的命令制造工厂</p>

在CommandFactory里面，涉及到了对应Command的封装和对应的序列化器的设置，结果的设置等，都是给了当前的Command一个标记，在另一个地方将会取出这些标记，然后根据标记去找对应的组件，来解析这个Command。

### 连接创建和管理
每一个请求和响应，都是和一个客户端和服务器端的连接相关的，这里涉及到了Connection的一系列东西，主要是Connection的创建、管理、相关事件的处理等，Connection这个实体类倒是没什么好说的，主要是对Netty Channel的封装，Connection的创建，涉及到一个ConnectionFactory，大部分重体力活都交给了AbstractConnectionFactory，具体见下图。

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/ConnectionFactory.png"/>
</p>
<p align="center">sofa-bolt中的连接创建</p>

连接管理（ConnectionManager）是一个重头戏，涉及到连接的创建（调用ConnectionFactory进行创建），连接池中的连接的移除等，整个体系也比较复杂，ConnectionManager的继承体系如下图
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/ConnectionManager.png"/>
</p>
<p align="center">sofa-bolt中的连接管理</p>

其中连接创建涉及较多的异步线程代码，这一部分放在后面的源码分析里面详细陈述。

## 相关流程图

### sofa-bolt C端启动流程图
sofa-bolt C端，主要是封装了各个组件的RpcClient类，RpcClient的启动，就是其内部封装的各个组件的启动和配置。RpcClient主要包含连接管理器、地址解析器、连接事件监听器、连接事件处理器，用户自定义请求处理器以及最主要的RPC内部调用接口。
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/init-client.png"/>
</p>

### sofa-bolt S端启动流程图
sofa-bolt S端，主要是RPCServer类，这个类和RpcClient大体上类似，但是不同的是RPCServer明显多出了和Netty的EventLoopGroup，显式的调用Netty，在RpcClient中，调用RpcClient需要创建连接时才会调用Netty相关的组件，S端的也提供了和C端类似的请求方式，并且也是通过内部的RPC内部调用接口实现具体的操作的。S端在启动时需要提供至少一个端口参数，而C端在启动时不需要。

<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/server-init.png"/>
</p>
<p align="center"></p>

### 请求处理和响应时序图

在发送请求这一块，由RpcClient提供最外层的api接口，然后由其内部的RpcRemoting具体执行，在执行之前，需要先获取和服务端的连接对象Connection，连接这一块由连接池提供，在首次连接时，需要先创建一个连接池，然后再由其内部逻辑创建Connection（根据目标地址的URL），然后才能发送具体的请求，根据请求方式的不同，可能有返回结果，也可能没有返回结果。

发送请求的时序图如下图所示
<p align="center">
  <img src="https://raw.githubusercontent.com/jsycdut/photos/master/sofa-bolt/send-request.png"/>
</p>


## sofa-bolt源码分析

接下来主要分析sofa-bolt中的组件源码，未完待续。

### 请求和结果获取


### 连接池设计


### 连接创建和管理


### 配置管理


### 自定义消息处理器


### 序列化处理


### 请求响应模式解析


## sofa-bolt高级数据结构


## sofa-bolt算法亮点




