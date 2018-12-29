# Scala

*Scala是编程语言， 级别同Java、 C*

1. 运行在JVM上，也就是运行在Java虚拟机上
2. 既是面向对象的，也是面向函数的
3. 语言设计者：Martin Odersky
4. [Scala Github](https://github.com/scala/scala)

![](http://allaboutscala.com/wp-content/uploads/2016/05/what_is_scala_1_1024-768x266.png)

*Scala可以和已有的Java库共同工作，感觉上，是无缝的。*

* Java->JavaC->字节码->JVM
* Scala->ScalaC->字节码->JVM

![](http://allaboutscala.com/wp-content/uploads/2016/05/jvm_and_scala-768x728.png)

## Scala 函数式编程特性

* 函数不要有副作用
* 推荐使用不变量
* 高阶函数，函数可以作为参数使用
* 模式匹配
* 异步编程
* 依赖注入

## Scala生态系统

![](http://allaboutscala.com/wp-content/uploads/2016/05/why_scala-768x528.png)

# 开发环境配置

*必备工具*
1. JDK
2. Intellij IDEA(不推荐Scala REPL)

## Hello World Scala Edition

1. [在IDEA中安装Scala插件(http://allaboutscala.com/tutorials/chapter-1-getting-familiar-intellij-ide/scala-environment-setup-install-scala-plugin-intellij/)
2. [建立HelloWorld项目](http://allaboutscala.com/tutorials/chapter-1-getting-familiar-intellij-ide/scala-tutorial-first-hello-world-application/)

注意，科学上网的必要性，以及为IDEA配置http代理，其次需要等待SBT进程完成，否则拿不下来项目模板，比如哪些文件夹该设置为folder，哪些是package，哪些文件夹该被标为source，哪些该被标为test，完成这一切，第一需要科学上网，第二需要等待。

## 解析HelloWorld入口函数
```scala
object HelloWorldMain {
  def main(args: Array[String]): Unit = {
      println("Hello World from Main Function!")
        
  }

}
```

1. object，Scala Class的三大类型之一，Class，Object，Trait
2. HelloWorldMain，类名
3. def，函数定义关键字
4. main，函数名
5. (args: Array[String])，函数参数名为args，类型为数组，数组里面装的是String
6. :Unit，Unit为关键字，意为没有返回值

一般来说，在创建的object类型的文件中声明`extends App`就可以不必自己写主入口函数了，因为在App的源码里面有这个主入口

