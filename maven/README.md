# maven
maven，常见的项目构建工具，用于源代码的编译，打包，发布，以及其他相关的构建工作，与之地位等同的，还有ant这个构建工具。

maven最核心的地方有两处，第一是pom文件，第二是maven生命周期以及相关的插件。

pom文件应该位于项目的根目录下，若项目由很多个小项目组成，那么可以在大项目下面放一个pom文件，在各个子项目下面也各自放一个pom文件。

![maven-how](http://ifeve.com/wp-content/uploads/2014/06/maven-overview-1.png)

maven的构建过程被分解为构建生命周期、阶段和目标，一个构建周期由一系列的构建阶段组成，每一个构建阶段又由一系列的目标组成。运行maven的时候，你会传入一条命令，命令就是构建生命周期，阶段或者是目标的名字。如果执行一个生命周期，那么该生命周期内的所有构建阶段都会被执行，如果执行一个构建阶段，所有处于当前构建阶段之前的阶段也都会被执行。

位于本机的maven仓库为本地仓库，除此之外，还有maven中央仓库，以及各大厂商的镜像站点，以及公司内部的远程仓库。

安装了maven之后，一定要注意你的maven的配置文件，可以通过maven配置代理以突破网络界限。

ant细致的让程序员控制每一个细节，而maven，你只需告诉它要做什么，它就会替你去做，但是这一切都建立在你对maven由相当的了解的时候，如果不了解你下达的指令的含义，那么结果很可能和你背道而驰。

pom之间可以有继承关系，子pom可以通过对同一项目的不同设置来覆盖父pom对同一条目已有的设置。

maven的配置文件有两处，安装目录下的conf子目录中的settings.xml，以及用户家目录下`.m2`目录下的配置文件，用户家目录优先。



# 组成关系
每个构建生命周期由不同的构建阶段组成，阶段代表了周期的一个状态。

每个构建阶段由不同的插件目标组成。

# 构建生命周期（Build lifecycle）
大的生命周期有三个，如下

1. default 默认
2. clean   清理
3. site    网站生成

![maven in my idea spring boot project](https://raw.githubusercontent.com/jsycdut/tutorials/master/maven/media/idea-maven.png)

在Intellij IDEA中，以我的spring-boot项目maven projects窗口可以看见，这里有三个主要的条目，lifecycle，plugins，dependencies。其中列出了常用的生命周期，其实只有clean和site是真正的生命周期，其余的全是default生命周期的构建阶段，因为clean和site这俩声明周期的构建阶段都比较少而且简单，所以就只列了其中与生命周期同名的构建阶段出来，而default的生命周期又太多，所以也只列出了常用的几个。

## default生命周期
default声明周期是用的最多的生命周期，它总共由23个构建阶段组成(2018-10-9)，翻译自[官网](http://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)如下

|构建阶段|描述|
|:-:|:-:|
|validate|验证项目是否正确，所有的必须信息是否完备|
|initialize|初始化构建阶段，比如设置属性或者创建目录|
|generate-sources|准备所有用于编译的源代码|
|process-sources|处理源码，比如过滤一些值|
|generate-resources|选取打包需要的资源文件|
|process-resources|拷贝和处理目标目录中的资源，准备打包|
|compile|编译项目源码|
|process-classes|处理编译后产生的文件，比如Java字节码强化|
|generate-test-sources|准备测试编译需要的源码|
|process-test-sources|处理测试源码，比如过滤某些值|
|test-compile|编译测试源码到指定的测试路径|
|process-test-classes|处理测试编译后产生的而文件，还是比如Java字节码强化|
|test|用合适的测试框架进行测试，这些测试不能要求代码被打包或者发布|
|prepare-package|执行打包所有的必要步骤，这会产生一个未打包的，但是预处理过的结果(Maven 2.1+)|
|package|将打编译后的字节码和资源文件打包为发布格式，比如jar|
|pre-integration-test|执行集成测试前的必要步骤，这可能会包括比如设置需要的环境这种操作|
|integration-test|进行处理和发布包到集成测试可以运行的环境中|
|post-integration-test|执行集成测试后必须的环节，比如清理环境|
|verify|运行一些必要的检查来验证包是有效的且符合质量标准|
|install|将打包结果安装到本地仓库，使其可以用于其他项目依赖|
|deploy|在集成测试或者发布环境之后，拷贝最终的包到远程仓库与其他开发者和项目共享|

## clean生命周期
clean生命周期比较简单，只有三个阶段

|构建阶段|描述|
|:-:|:-:|
|pre-clean|执行真正清理前必要的工作|
|clean|删除之前build产生的所有文件|
|post-clean|执行清理够的必要工作|

## site生命周期
site生命周期也比较简单，总共四个阶段

|构建阶段|描述|
|:-:|:-:|
|pre-site|执行生成网站前的必要工作|
|site|产生项目网站文档|
|post-site|执行文档产生后的工作，准备发布|
|site-deploy|发布产生的文档到指定的web服务器上|

## 内置生命周期绑定
一些构建阶段可能和某些目标做了内置绑定，这些绑定和打包的值有关，以下是某些目标到构建阶段的绑定。

## maven仓库

maven仓库有三处

- 本地仓库

- 远程仓库

- 中央仓库

  ![](https://www.tutorialspoint.com/maven/images/repository_structure.jpg)

本地仓库日常用，远程仓库和中央仓库解析依赖用。可以通过两个地方配置这些仓库

- settings.xml

- pom.xml


每次项目解析依赖的时候，优先使用本地的，所以面对本地没有的依赖，会先去远程仓库和中央仓库拿，然后安装到本地，以后就用本地的。这也会导致本地仓库越发的膨胀，体积越来越大，当然，本地存储位置是可选的，settings.xml文件改`localRepository`节点即可。

中央仓库是maven社区提供并且维护的，maven默认去此仓库取回依赖，链接为https://repo1.maven.org/maven2/。

远程仓库则是和用户，公司等有关，比如某公司内部开发了A产品，其B产品用到了A，那么就需要在B产品的依赖中声明A，但是公司内部产品，又不可能外流，所以只能放在公司内部的服务器上，这时候就将这些内部被依赖的“产品”或者说“组件”放在公司内部的maven服务器上，从而形成了远程仓库，亦或者说是“内部仓库”，与这种内部的环境相比，某些组织的镜像也可以称作是远程仓库，比如阿里云的maven仓库。

##  maven的依赖解析顺序

maven解析依赖有一个顺序，总体可以说是由远及近，顺序如下

1. 本地仓库

2. 中央仓库

3. 远程仓库


要是最后都找不到的话，就报错停止。