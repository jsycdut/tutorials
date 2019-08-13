# 在Intellij IDEA中编译运行Neo4j社区版

## 速度较快的做法
1. 下载源码

```bash
git clone https://github.com/neo4j/neo4j.git 
```
2. 使用Intellij-IDEA import该Maven项目，Neo4j的项目有Scala的代码，这部分代码的编译需要交给Maven来，pom.xml中定义了使用Scala的版本以及其他参数，就不要单独去配置Scala了

3. 在import项目完成后，需要做的就是在项目目录下执行
```
$ mvn clean
```
4. 配置运行社区版Neo4j的入口主类，CommunityEntryPoint.java，在运行的时候，需要配置以下几个程序参数（不是JVM参数）
```
-server --home-dir=xxxxx --config-dir=yyyyy
```
`--home-dir`配置的是Neo4j启动后的日志，数据等的写入目录，`--config-dir`是Neo4j的配置文件所在的目录，配置文件主要定义了Neo4j启动的HTTP端口等，可以在项目中搜索neo4j.conf，然后单独创建一个目录将文件复制过去。

5. IDEA中Build->Build Project，将整个项目编译一遍，如果编译不成功，就Build->Rebuild Project，一般两次操作之后就可以成功了。

6. 启动CommunityEntryPoint即可。

## 速度较慢的做法
除了上述办法，还有一种是采用`mvn clean install`的方式来编译发布版的Neo4j Community Server，使用这种方式的时候，不可以添加`-Dmaven.test.skip=true`选项，因为在pom文件中有很多scope为test的依赖，这些是必须执行测试的。一套流程走下来，估计是一个小时，成功之后，同样是配置好CommunityEntryPoint启动即可。
