# bash教程

很多公司的招聘要求上面写着需要会写shell脚本的，当然也有Python和js也常被人称作脚本，但是这俩和bash一比，都太重量级了，js现在可是全栈语言，Python在机器学习以及web开发都很牛逼，bash脚本这这俩相比，基本上可以说不是一个领域了。

shell脚本，在99%的情况下，指的都是bash脚本，就是在Linux环境下以bash为脚本解释器的可执行文本文件，提到脚本解释器，除了bash，还有dash，以及zsh，ksh等等等等，很多人可能会糊里糊涂的，如果没有系统的了解过的话，听起来都令人晕。一方面是为了教程的完整性，另一方面也是方便我以后查阅，我就先简单的写一写这些shell之间的关系。

* shell sh bash 
shell，简单来说，就是人机的命令行交互程序；sh，完整的说叫Bourne Shell，由大佬Stephen Bourne开发，，是Unix一开始支持的唯一交互式shell，然后另一位大佬Brain Fox开发了一个兼容Bourne Shell，同时吸收了csh ksh等其他shell的特性的新一代shell，那就是bash，所以，不要搞混了这三者，另外，不要把你的终端模拟器叫做shell，你就该把他叫做终端模拟器，terminal emulator，，他只是一个给你提供可以运行sh，bash等程序的另一个程序而已。

需要注意，不同的shell，有不同的特性，sh，这种老掉牙的东西，依然有人用，因为它太通用，你的Linux也许没有bash，但百分比有sh，有一个需要注意的地方，现在我见过的Linux，sh这个应用程序，基本上都是另一个名为dash的应用程序的一个软链接而已，以后人家提起sh，你要晓得它说的是dash，不是bash，bash兼容dash，但是dash就可能不支持bash的一些特性，比如dash他就没有数组，（bash4.0后有数组），dash也不支持`[[]]`这种条件判断符，我见过有人将脚本的解释器写成`#!/bin/sh`，然后在里面大摇大摆的用`[[]]`，跟个神经病一样。


bash兼容dash，zsh兼容bash，所以一般来说，对于喜欢用`oh-my-zsh`的朋友，你的默认shell是zsh，当你用着`oh-my-zsh`享受着爽快的路径提示，骚气的命令别名，然后又不写脚本文件，喜欢直接在命令行里面实验bash语法的时候，如果出了什么问题，也许你需要切回bash试试（虽然兼容，也许就是你的语法和zsh的有点不一样）

# bash中变量

bash中的变量，没有类型之分，也就是没有int，string这样明显的区分，声明的时候也不需要关键字，你直接写然后赋值就行了，注意千万不要自作聪明的在等号两边加空格，这对bash来说，是语法错误。


# bash中的数据结构

bash中的数据结构？你别开玩笑了，bash没有数据结构，你要list，没有，set，没有，tree，没有，bash有个啥数据结构？数组算么？要算的话，就只有数组，数组分两类，一是基础数组，声明为`(1 2 3)`，括号之内为数组元素，以空格分割，不要自作聪明去加`,`，除非你的元素就包含`,`，曾经有一个人就这么改了我的代码，不过还好那只是我的一个数组赋初值而已。除了基础数组，bash 4.0之后加入了关联数组，这是一个类似于map的数组，举个例子
```shell
# 此句声明关联数组名为associative_array
declare -A associative_array

# 给关联数组associative_array添加一组元素，名为name，值为jsy
associative_array[name]=jsy

# 访问关联数组associative_array中的name元素
echo ${associative_array[name]} # 输出jsy


```
上面的代码在OS X的bash里面跑不了，我以为自己看了盗版书，白纸黑字写的`declare -A 关联数组命名`，我在OS X的Bash shell里面查看了手册，搜了一把关键字`associative`，死活没结果，然后登上我的Linux，查看Bash手册，发现一句`Associative arrays are created using declare -A name.`，然后一看OS X的Bash版本，3.2.57，我可真是服了，所以用个毛的OS X？

这里就不插播我是一个Windows，OS X黑子的事实了。

# 本教程架构

从[Advanced Bash-Scripting](http://www.tldp.org/LDP/abs/html/abs-guide.html)抄来的，以后都会按照该书来组织本教程。


