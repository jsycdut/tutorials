# Scala相关概念
Scala主要文件分4类，class，trait，object，package object。

* class

类同Java中的class，可以看做方法和函数的结合体

* trait

类同Java中的接口，但比接口在语义上更加复杂

* object

单例，在JVM中声明为object的只会有一个实例

* package object

全局的对象以及方法

## SBT（死变态？？？）
SBT, Simple(Scala) Build Tool，用于处理Scala的依赖，可以对标Java的ant以及ivy。用于编译，运行，测试，打包你的Scala项目，参考[SBT](http://www.scala-sbt.org/0.13/docs/index.html)

在Scala项目里面，依赖一般都写在一个叫作build.sbt的文件中，比如引入日志模块依赖的sbt代码如下
```
libraryDependencies ++= Seq(
  "ch.qos.logback" % "logback-classic" % "1.1.7",
   "com.typesafe.scala-logging" %% "scala-logging" % "3.5.0"
   )
```

sbt引入依赖的地方其实是maven中央仓库（万万没想到啊！）依赖里面的三段式其实就是maven的groupId，artifactID，Version
