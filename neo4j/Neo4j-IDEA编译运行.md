# 在Intellij IDEA中编译运行Neo4j社区版

```bash
git clone https://github.com/neo4j/neo4j.git 
```
然后使用Intellij-IDEA import该Maven项目，Neo4j的项目有Scala的代码，这部分代码的编译需要交给Maven来，pom.xml中定义了使用Scala的版本以及其他参数，就不要单独去配置Scala了

在import项目完成后，需要做的就是在项目目录下执行
```
$ mvn clean
```
然后配置运行社区版Neo4j的入口主类，CommunityEntryPoint.java，在运行的时候，需要配置以下几个程序参数（不是JVM参数）
```
-server --home-dir=xxxxx --config-dir=yyyyy
```
`--home-dir`配置的是Neo4j启动后的日志，数据等的写入目录，`--config-dir`是Neo4j的配置文件所在的目录，配置文件主要定义了Neo4j启动的HTTP端口等，可以在项目中搜索neo4j.conf，然后单独创建一个目录将文件复制过去。
