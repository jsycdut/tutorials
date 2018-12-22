# groovy非语法类知识

* 下载

groovy，截至目前（2018-12-12），最新版本3.0，支持jdk8+，长期稳定版本为2.5，jdk7+

安装方式有很多，包括

1. 下载二进制文件安装(或者从源码编译安装)，然后将安装目录加入环境变量
2. 使用包管理器安装，包括各大Linux版本的包管理器，OS X的homebrew
3. 使用构建工具，如果项目需要groovy的话，可以用maven，gradle等构建工具声明使用groovy
4. [官方文档安装部分](http://www.groovy-lang.org/download.html)

基础使用方式有三

1. 命令执行，groovy groovy代码源文件
2. 交互式命令行，执行命令groovysh呼出
3. 图形化代码工具，执行groovyConsole呼出

* 与Java之区别

groovy是一门非常照顾Java背景的开发者的语言。

以下包是自动引入的，不必显式声明
1. java.io.*
2. java.lang.*
3. java.math.BigDecimal
4. java.math.BigInteger
5. java.net.*
6. java.util.*
7. groovy.lang.*
8. groovy.util.*


groovy中的方法是运行时绑定的，根据参数的类型来选择方法，而java则是编译期绑定的
```java
int method(String arg){
  return 1;
}

int method(Object arg){
  return 2;
}
Object o = "Object";
int result = method(o);
```

在Java里面，返回结果是2，因为编译的时候明确告诉被调用的method方法，我用的是Object类型的参数，所以jvm会执行参数类型为Object的方法，而groovy的返回结果是1，因为最终你穿给这个方法的是字符串"Object"，这就是编译期和运行时绑定的不同。


在groovy中，`{}`是留给闭包的，所以数组的字面量初始化用的是`[1, 2, 3]`，和js相同。

在Java中，没有访问权限限定符，也就是没有写public protected private的量，有包访问权限，为包私有，但groovy不一样，若在groovy中不写限定符，意为私有字段，要声明包可见性，需在字段前面加上@PackageScope

groovy不支持java7开始的自动资源管理(ARM)，但是可以用闭包实现。

groovy比java多了几个关键字，as in def trait

groovy啥类型都是对象，连数字这种类型都是自动被转为对象的，看见一个数字.方法这种一点都不奇怪

java可以耍lambda，groovy不行，groovy耍的是闭包





