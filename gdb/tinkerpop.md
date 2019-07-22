# Tinkerpop-图计算框架研究

![Tinkerpop](https://raw.githubusercontent.com/apache/tinkerpop/master/docs/static/images/tinkerpop3-splash.png)
## 调试环境

1. Intellij IDEA
2. Git
3. Maven
4. HTTP代理配置

## 特殊配置

* gremlin-groovy
当出现gremlin-groovy项目下的Error:(21, 54) java: 程序包org.apache.tinkerpop.gremlin.groovy.jsr223.ast不存在这个报错信息时，需要在project视图下，将gremlin-groovy/src/main/groovy标记为源代码，在该代码目录上右键->mark directory as source root即可

* gremlin-shaded

gremlin-shaded是一个只有pom.xml的子模块，需要在该子模块下执行`mvn clean install`然后移除该模块，这个模块的作用是提供其他gremlin项目所需的依赖，只需要安装好依赖即可。

* gremlin-server

执行gremlin核心逻辑的服务端，启动主类是GremlinServer.java，启动该类即可，在启动前，需要Run->Edit Configurations -> Working Directory设置为gremlin-server模块的路径，另外需要设置VM options: `-Dlog4j.configuration=file:conf/log4j-server.properties`用于配置日志输出。

* gremlin-console

这是连接gremlin-server所需的客户端，里面有个Console.groovy，启动该主类的main方法即可启动，另外也需要Run->Edit Configurations -> Working Directory设置为gremlin-console模块的路径，另外，为了使用对应的插件和连接远程客户端，需要执行以下代码
```groovy
gremlin> :plugin use tinkerpop.server

:plugin use tinkerpop.server
==>tinkerpop.server activated but ext/plugins.txt is readonly - the plugin list will remain unchanged on console restart
// 下面的remote.yaml是要连接的gremlin-server的信息
// 一定要加上语句末尾的session，否则所有语句的执行都是无状态的
// 也就是你上一条语句定义的变量下一条语句就不认识了
gremlin> :remote connect tinkerpop.server conf/remote.yaml session

:remote connect tinkerpop.server conf/remote.yaml session
log4j:WARN No appenders could be found for logger (com.jcabi.manifests.Manifests).
log4j:WARN Please initialize the log4j system properly.
log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
==>Configured localhost/127.0.0.1:8182-[b9980ed7-7c7d-4f96-b003-4ca32d3f0c9a]

gremlin> :remote console

:remote console
==>All scripts will now be sent to Gremlin Server - [localhost/127.0.0.1:8182]-[b9980ed7-7c7d-4f96-b003-4ca32d3f0c9a] - type ':remote console' to return to local mode
gremlin> // 此时就已经连接上了启动在localhost:8182的gremlin-server了
gremlin> // 所有这里执行的语句都会送给gremlin-server执行
```
