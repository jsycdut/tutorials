# git

git 作为一个版本管理系统（Version Control System，aka VCS），拥有以下特点

* 速度
* 设计简单
* 对非线性开发的强力支持（允许成千上万个并行开发的分支）
* 完全分布式
* 有能力管理类似Linux内核一样的超大项目（2500w行代码，总共2.4GB）

git诞生于2005年，那一年商业VCS BitKeeper终止了Linux内核社区的对BitKeeper的使用权，然后Linux之父Linus就开发了git，那是真的牛逼（最大声），然后将git的维护交给了日本籍开发者Junio C Hamano（滨野纯），在github上，git项目的提交次数中，Junio C Hamano独占鳌头，Linus曾说过，他做过的最正确的事情之一就是认识了滨野纯，然后把git的维护工作交给他。


## Git基础

**git的几个特点**
1. 直接保存每个代码提交时候整个项目的快照，而不是文件差异
2. 近乎所有的操作都是在本地执行（分布式
3. git使用SHA-1形成40位16进制字符来保证文件的完整性
4. git一般只往版本库里面添加数据

**git的三种状态**

1. 已提交，数据已经安全的保存在本地版本库中
2. 已修改，修改了文件，但还没有版存到本地版本库中
3. 已暂存，对一个已修改的文件的当前版本做了标记，使之包含在下次提交的快照中

**git的三个工作区域**

1. git版本库 Repository
2. 工作目录 Working Directory
3. 暂存区 Staging Area
![git 的三个工作区域](https://git-scm.com/book/en/v2/images/areas.png)

**git版本库**

git版本库是git用来保存项目的元数据和对象数据库的地方（.git目录），使用clone命令克隆项目的时候，拷贝的就是这里的数据。

**工作目录**

工作目录是对项目某个版本独立提取出来的内容，这些是从git仓库的压缩数据中取出来的文件，供你随便使用。

**暂存区**

暂存区是一个文件，保存了下次将要提交的文件列表信息，一般在git仓库目录中。

**基本的工作流**

1. 在工作目录修改文件（已修改状态）
2. 暂存文件，将修改的文件防区暂存区（已暂存状态）
3. 提交更新，找到暂存区域的文件，将快照永久的存储到git版本库中（已提交状态）

## git的配置

每个稍微高端一点点的git用户都应该懂得配置的重要性，比如全局用户配置和某个具体项目仓库的用户配置，访问github奇慢无比的时候的科学上网代理配置等。

**git有三个配置文件**

* `/etc/gitconfig` 适用于系统上所有用户的通用配置，使用`--system`选项的`git config`命令，就会读取这个文件
* `~/.gitconfig`或者`~/.config/git/config`，只针对当前用户，使用`--global`选项的`git config`命令，读取此文件。

* `当前项目/.git/config`文件，单独针对当前项目。

上面的配置文件从上到下，优先级逐渐增大，会出现优先级高的覆盖优先级低的配置项目。

以上都是针对Linux系统的配置，Windows类似。

下面介绍使用git的一些必须配置

**用户信息**

git每次提交都要写用户信息，表明是谁提交的这些代码，对应的信息都需要提前配置好。

```
git config --global user.name  "jsycdut"
git config --global user.email "jsycdut@gmail.com"
```

这个配置将会被应用到本用户的所有git仓库中，若需要对某个特殊的仓库进行特殊配置，可以在该仓库内运行没有`--global`选项的命令，如下

```
cd paht/to/git/repository
git config user.name  "jsycdut"
git config user.email "jsycdut@gmail.com"
```

比如在公司，我有公司的项目，还有自己的github的项目，公司的项目提交的时候，需要写公司分配给我的邮件信息以及我的真实名字，但是在github项目提交的话，我就可以使用我日常生活中用户邮件以及github昵称，此时我就可以先执行带`--global`配置，将所有的都配置为我自己的github所需的配置，然后到我公司项目下面执行不带`--global`的配置，来创建公司给我的信息的配置。


**文本编辑器**

在你使用`git pull`命令的时候，往往会形成一次分支合并提交，需要你键入提交的注释信息，这个时候就会调用你的系统默认文本编辑器，比如Vim，对于想配置使用不同编辑器的情况，就需要配置如下

```
git config --global core.editor emacs
```

**检查配置信息**

使用`git config --list`来查看所有的配置

**获取git帮助**

```
# git help <verb> 获取某个命令的帮助
# 下面的命令是等价的
git <verb> --help
man git-<verb>

git help config # 获取config命令的帮助
```

****

