# gremlin基础知识

## TinkerGraph

TinkerGraph是一个内存中的数据库，没太多配置选项，下面的语句可以创建一个gremlin中的一个图，不过这个图是空的，啥也没有。

 ```groovy
graph = TinkerGraph.open() //打开一个空的图
g = graph.traversal() //获取图的遍历器
 ```

 要创建带有数据的用于演示的图，可以使用TinkerFactory的一些静态方法
 
 - createClassic() 创建tinkerpop2.x的结构的图
 - createModern() 创建tinkerpop3.x结构的图
 - createTheCrew() 创建tinkerpop3.x结构的图，同时里面有些3.x才有的顶点元属性和多属性

关于创建出来的图的数据量

- createModern()创建出来的图只有6个顶点6条边
- createGratefulDead()对应的图拥有808个顶点，8049条边

这些方法都会去gremlin-console发行版下面的data目录找对应的json文件来初始化图。

## gremlin-consoel里面的命令

gremlin-console里面的命令以`:`开头，可以使用`:help`来查看所有的命令，`:help command`查看某个命令的具体用法，比如查看连接gremlin-server的:connect命令的用法就是`:help :connect`，比如连接到本地Intellij IDEA里面运行在localhost:8182端口的gremlin-server的时候，使用的命令是

```
# 使用conf/remote.yaml里面的配置，连接到tinkerpop.server服务器，同时启用会话支持
:remote connect tinkerpop.server conf/remote.yaml session

# 将所有命令都送给远程的Intellij-IDEA里面的gremlin-server执行
# 要是远程的gremlin-server是用debug模式跑起来的，就可以调试gremlin-server的执行流程了
:remote console
```
