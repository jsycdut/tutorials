# groovy的下载与安装

本章主要讲解groovy的下载与安装。

## 下载

获取groovy的方式有很多，主要如下
* 下载groovy的二进制发行版
* 使用操作系统（主要是OS X和众多Linux发行版）的包管理工具
* 从构建工具（如Maven，Gradle）获取相应的groovy jar包
* 从支持groovy的IDE中（如Intellij IDEA）获取groovy
* 从[Github](https://github.com/apache/groovy)上获取源码并安装其指示操作
* Docker用户可以从[Docker Hub](https://hub.docker.com/_/groovy/)获取

### 发行版
发行版意为源文件或者class文件的打包形式，可用于构建或者使用groovy。

所有的Apache项目都提供了一个源码zip包，所有人都可以从这个源码包从零开始来构建相应的二进制发型版。如果对发行版有任何疑惑，都可以将源码包视作该发行版最权威的解答。官方也提供二进制的可下载的文档和SDK套件（二进制程序，文档，以及源码的结合体）。对于Windows用户，可以找到一个适用于Windows的非Apache软件基金会（开发的）groovy安装程序（如果有的话）。(2018-12-15-23:06)

### 验证
官方的每一个groovy发行版都提供了一个OpenPGP签名文件（扩展名为".asc"的文件)，以及该发行版的校验和，建议用户生成自己的校验和并且和官方提供的校验和作比对，同时，使用[这个groovy发行版专用OpenPGP Key文件](https://www.apache.org/dist/groovy/KEYS)来检查签名，以此确定您下载的是官方的未经篡改的groovy发行版。

* [如何验证从Apache下载的软件包](https://www.apache.org/info/verification.html#how-to-verify)

*接下来将介绍groovy的几个版本*，具体的下载页面请点击[这里](http://www.groovy-lang.org/download.html)

### groovy 3.0
groovy 3.0是最新的groovy版本，针对jdk8+，默认开启了Parrot解析器，目前可以下载到3.0的提前稳定版本。

### groovy 2.6
groovy 2.6为jdk7+设计，支持新的Parrot解析器（当此功能被开启的时候），但是此版本已经“退役”，在2.6即将发布的时候，为了去开发groovy 3.0，就放弃了2.6。2.6的alpha版本可以帮助那些想要尝鲜groovy3.0特性（Parrot解析器）但是仍然困在jdk7的人们，相关的下载连接在groovy的官网上也有所提及。

### groovy 2.5
groovy 2.5是groovy的最稳定的版本。我在OS X上安装的时候homebrew为我选择的就是2.5，但是我在Ubuntu上从apt搜索的时候，groovy的版本却没有2.5，只有选择使用二进制源码包了（也许这会成为我最终使用arch的原因）

### groovy 2.4
groovy 2.4是groovy在2.5之前的另一个稳定版本。在2.4.4之前的groovy不是在Apache软件协会监管下的，属于没有任何保证的发布版本。

### 其他版本
想要下载所有版本的groovy，可以去
* [Apache发布镜像](http://www.apache.org/dyn/closer.cgi/groovy/)
* [Apache打包仓库](https://archive.apache.org/dist/groovy/)
* [Bintray's Groovy仓库](http://bintray.com/groovy/)

*调用动态支持*
如果需要在groovy启用indy支持并且你使用jdk7+的话，请参考[调用动态支持相关信息](http://www.groovy-lang.org/indy.html)

## 操作系统/包管理器安装

下载Apache groovy的zip发布包来安装并不难（需要配个环境变量），但如果你不想折腾的话，考虑下面的包管理器安装。

* [SDKMAN](http://sdkman.io/)

适用Unix相关系统（OS X以及Linux诸多发行版），SDKMAN用于管理多个平行的软件开发包，比如jdk，groovy，Scala等等等等等，详细信息请参考官方网站。
```
sdk install groovy
```

* [Homebrew](http://brew.sh/)

所谓的macOS缺失的包管理器
```
brew install groovy
```

* [MacPorts](http://www.macports.org/)

MacPorts是macOS下的系统管理工具
```
sudo port install groovy
```

* [Scoop](http://scoop.sh/)

Windows的命令行安装程序，据说是受homebrew启发的
```
 scoop install groovy
```

* [Chocolatey](https://chocolatey.org/)

Chocolatey，也是Windows上的，提供一个健全的方法管理软件
```
choco install groovy
```
`Linux/*nix`用户：你可能更喜欢使用你自己的包管理器来安装groovy，比如apt，dkpg，pacman等等（等等，要是包管理器上的groovy版本跟不上你以为我会用？ Ubuntu:你是在黑我没有groovy 2.5？）

Windows用户：你们还是乖乖的用exe的groovy安装器吧。(来自一个Windows黑子的窃笑(๑>◡<๑) )


## 从构建工具获取groovy
如果你希望在你的项目中使用groovy作为项目依赖的话，你可以在项目构建的依赖条目里声明如下

*如果只需要groovy的核心模块以及打为jar包的Antlr，ASM以及内部的CLI实现的相关类的拷贝*
```
# groovy
org.codehaus.groovy:groovy:x.y.z

# mvaen
<groupId>org.codehaus.groovy</groupId>
  <artifactId>groovy</artifactId>
<version>x.y.z</version>
```

*如果需要可选的groovy相关模块*
```
如果需要groovy-sql模块，将下面的$moudule换为sql即可
# groovy
org.codehaus.groovy:groovy-$module:x.y.z

# mvaen
<groupId>org.codehaus.groovy</groupId>
  <artifactId>groovy-$module</artifactId>
<version>x.y.z</version>
```

*groovy全模块*
```
# 如果需要groovy以及相关的模块（可选模块除外），可以使用groovy-all
# gradle
org.codehaus.groovy:groovy-all:x.y.z

# mvaen
<groupId>org.codehaus.groovy</groupId>
  <artifactId>groovy-all</artifactId>
  <version>x.y.z</version>
<type>pom</type> <!-- required JUST since Groovy 2.5.0 -->
```
这里实在是说的有点不明白，我在官网也没看太明白，那里的注释令人觉得匪夷所思，所以暂时先放一放，后期再看再改。

groovy的发布版jar包可以从[maven中央仓库](http://repo1.maven.org/maven2/org/codehaus/groovy/)，或者[JCenter](http://jcenter.bintray.com/org/codehaus/groovy/)获取

groovy的snapshot的jar可以从[JFrog OpenSource Snapshots repository](https://oss.jfrog.org/oss-snapshot-local/org/codehaus/groovy)获取，这并非官方发布的版本，但是被提供用于新的官方版本的继承测试。

## 系统需求
indy意为InvokeDynamic，这是一条jdk7才引入的新的虚拟机指令，主要用于动态语言，暂时还看不懂（手动无奈！！！）

| Groovy | JVM需求(non-indy) | JVM需求(indy) |
| :----: | :----------------:| :-----------: |
|3.0-目前版本|1.8+|1.8+|
|2.5-2.6|1.7+|1.7+|
|2.3-2.4|1.6+|1.7+|
|2.0-2.2|1.5+|1.7+|
|1.6-1.8|1.5+|N/A|
|1.5|1.4+|N/A|
|1.0|1.4-1.7|N/A|

如果想使用动态调用，也就是indy，参考[这里](http://www.groovy-lang.org/indy.html)
