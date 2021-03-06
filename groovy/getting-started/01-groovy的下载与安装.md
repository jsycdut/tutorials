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

可选的groovy模块包括 "ant", "bsf", "console", "docgenerator", "groovydoc", "groovysh", "jmx", "json", "jsr223", "servlet", "sql", "swing", "test", "testng" and "xml". 
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
```

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

## 安装groovy
其实前面介绍了很多的关于下载和安装的方式，比如sdk，homebrew，以及maven，gradle以及二进制包等方式，这里的内容其实显得有点累赘，整理一下放在下面。


### 稳定版
你可以下载二进制版本的groovy发行版，也可以下载相关的文档

 * [groovy 2.5 二进制发行版](https://bintray.com/artifact/download/groovy/maven/apache-groovy-binary-2.5.5.zip) | [源码发行版](https://bintray.com/artifact/download/groovy/maven/apache-groovy-src-2.5.5.zip)
 
 * [文档](https://bintray.com/artifact/download/groovy/maven/apache-groovy-docs-2.5.5.zip)
 
 * [2进制发行版 + 源码 + 文档](https://bintray.com/artifact/download/groovy/maven/apache-groovy-sdk-2.5.5.zip)

如果想获取更多的关于groovy 2.5的资料，参考[发布版说明](http://groovy-lang.org/releasenotes/groovy-2.5.html)或者[变更日志](http://groovy-lang.org/changelogs/changelog-2.5.5.html)

[invokedynamic参考](http://docs.groovy-lang.org/latest/html/documentation/invokedynamic-support.html)

### snapshot版

如果你是一个具有冒险精神的家伙，可以使用snapshot版本的groovy，请参阅[这里](https://oss.jfrog.org/oss-snapshot-local/org/codehaus/groovy)

### 前提条件

groovy 2.5需要jdk6+，目前来说在使用java9的snapshots版本的时候，在切面（aspect）方面有些问题，如果需要使用groovy-nio，需要jdk7+，使用groovy invokedynamic也是需要jdk7+，但用jdk8+更好。

groovy的持续集成服务器也是一个去查找groovy与java版本适配关系的号地方，那里拥有丰富的测试资源。


### SDKMAN!(The Software Development Kit Manager)
在bash平台，安装groovy可以使用SDK Manager(Mac OS X, Linux, Cygwin, Solaris, FreeBSD)，
```
# 按照以下代码操作
curl -s get.sdkman.io | bash

# 如果你用的不是bash，记得在你的shell的rc文件里面写入下面一行代码
# 是bash的话，直接执行就可以了，或者打开一个新的窗口
source "$HOME/.sdkman/bin/sdkman-init.sh"

sdk install groovy

groovy --version
```

### OS X 下groovy的安装

* MacPorts
```
sudo port install groovy
```

* Homebrew
```brew install groovy```

### Windows下的安装
Windows用户可以使用[NSIS Windows安装器](http://docs.groovy-lang.org/latest/html/documentation/TODO-Windows+NSIS-Installer)

### 其他发行版
从[这里](https://bintray.com/groovy/maven)下载groovy的其他版本

### 源码
参考[Github仓库](https://github.com/apache/groovy)

### IDE插件
从IDE插件获取Groovy参考[这里](http://docs.groovy-lang.org/latest/html/documentation/tools-ide.html)

### 安装二进制版本
这个稍显复杂，需要自己配点环境变量，操作流程如下

1. 下载[二进制安装包](http://www.groovy-lang.org/install.html#download-groovy)，解压
2. 设置GROOVY_HOME指向解压目录
3. 将GROOVY_HOME/bin添加到PATH环境变量
4. 设置JAVA_HOME指明jdk所在

这一波操作过后，在你的命令行输入如下命令，可以打开交互式的groovyshell

```groovysh```

你要是喜欢[图形化的交互界面](http://www.groovy-lang.org/install.html#..\..\../subprojects/groovy-console/src/spec/doc/groovy-console.adoc)，可以

```groovyConsole```

如果执行groovy脚本，命令行里面敲

```groovy path/to/your-groovy-script```

第一章完结撒花  (๑>◡<๑) (2018-12-29:09:38:18)
