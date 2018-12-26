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

