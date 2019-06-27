# git :sparkles:

git 作为一个版本管理系统（Version Control System，aka VCS），拥有以下特点

* 速度
* 设计简单
* 对非线性开发的强力支持（允许成千上万个并行开发的分支）
* 完全分布式
* 有能力管理类似Linux内核一样的超大项目（2500w行代码，总共2.4GB）

git诞生于2005年，那一年商业VCS BitKeeper终止了Linux内核社区的对BitKeeper的使用权，然后Linux之父Linus就开发了git，那是真的牛逼（最大声），然后将git的维护交给了日本籍开发者Junio C Hamano（滨野纯），在github上，git项目的提交次数中，Junio C Hamano独占鳌头，Linus曾说过，他做过的最正确的事情之一就是认识了滨野纯，然后把git的维护工作交给他。


## Git基础

**git的几个特点**
1. 直接保存每个代码提交时候整个项目的快照，而不是文件差异（所以代码库在迭代版本越多，改动越大，体积就会越来越大）
2. 近乎所有的操作都是在本地执行（分布式
3. git使用SHA-1形成40位16进制字符来保证文件的完整性
4. git一般只往版本库里面添加数据

---

**git的三种状态**

1. 已提交，数据已经安全的保存在本地版本库中
2. 已修改，修改了文件，但还没有版存到本地版本库中
3. 已暂存，对一个已修改的文件的当前版本做了标记，使之包含在下次提交的快照中

---

**git的三个工作区域**

1. git版本库 Repository
2. 工作目录 Working Directory
3. 暂存区 Staging Area
![git 的三个工作区域](https://raw.githubusercontent.com/jsycdut/photos/master/git/areas.png)

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

**代理**

git也可以使用本地的http代理，涉及http.proxy https.proxy两个变量，两个变量使用一个就够了。一般我会本地把socks5转化为http，然后就使用http代理。

```
$ # 设置代理
$ git config [--global] http.proxy "local http proxy address"
$ git config [--global] https.proxy "local http proxy address"

$ # 取消代理
$ git config [--global] unset http.proxy
$ git config [--global] unset https.proxy
```

**中文**

当git版本库里面的文件名包含中文的时候，`git status`将会把文件名的中文表示为一串斜杠和数字，比如`\345\210\206\346\224\257.`，这是因为git默认会将unicode码大于0x80的看做非寻常字符来看待，会将其编码为8进制的字符，为了避免这种情况发生，应该设置git不要对齐进行编码，只需要设置`git config --global core.quotepath false`即可。

**检查配置信息**

使用`git config --list`来查看所有的配置

**获取git帮助**

```
# 下面的命令是等价的
git help <verb> 获取某个命令的帮助
git <verb> --help
man git-<verb>

git help config # 获取config命令的帮助
```
## git仓库操作

**获取git仓库**

一般来说，获取git仓库有两种办法

1. `git clone /path/to/any/git/repository` 克隆一个已有的版本库
2. `git init`新建一个版本库

`git clone`支持很多种数据传输协议，包括`http https ssh git`等协议。

**git仓库文件的状态变化**

任何一个git仓库里面的文件，要么是已跟踪的，要么是未跟踪的状态，未跟踪的文件不受git仓库的管理，受跟踪的则是受仓库管理的。下图阐述了git仓库里的文件状态变化周期。

![git文件状态变化周期](https://raw.githubusercontent.com/jsycdut/photos/master/git/life-cycle.png)

下面不会介绍最基础的如何将文件添加到版本库，不会举例子说明，只简单的讲一下常用的git工作流。

---

**常用的git工作流**

1. git add file1 file2 ...
2. git commit -m "a commit comment"
3. git pull
4. git push

基本上来说就是修改了代码，形成一次提交，然后拉取远程代码，最后发布自己的提交，中间可能会遇见冲突，遇见冲突解决冲突就完事了。

---

**查看自己当前版本库的情况**

当自己写了一段时间代码，也做了几次提交，此时可以查看一下自己版本库的情况，适时的形成提交或者清理文件，保持代码库的整洁。可以使用`git status`查看自己的版本库的目前情况。

```
$ git status
On branch xxxx # 当前处于哪个分支
Your branch is ahead of 'origin/master' by 2 commits # 当前分支和主分支的版本差异

Changes to be committed: # 已经暂存了还没有提交的文件
...

Changes not staged for commit: # 改了还没有暂存的文件
.....


Untracked files: # 还没有被纳入版本库进行跟踪的文件
...
```

上面的输入实在是太过详细，可以用`git status -s 或者 git status --short`来得到精简的版本库状态信息。

```
git status -s
 M README.md
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```

其中符号的含义如下

* `??` 未跟踪的文件
* `A ` 新添加的加入暂存区的文件
* `M_` 已经修改了的并且放入暂存区
* `_M` 已经修改了，但是没有放入暂存区
* `MM` 修改了，放入暂存区了，然后又被修改了还没有放入暂存区

---

**忽略不重要的文件**

这相当重要。

在项目中，有些文件是附加产物，我们并不想将其纳入版本库，比如java编译后的class文件，Python编译后的pyc文件或者日志文件等等，所以需要提前规定好，如果项目出现了这些类型的文件，我们将视若无睹（不显示在Untracked files那里）

项目顶层应该有一个`.gitignore`文件，用于告诉git符合里面条件的文件不要纳入版本管理。 但是，这个文件对已经纳入版本库的文件是不起作用的，比如你之前并没有设置忽略文件，然后你将某些log结尾的日志文件放入了版本库，那么，即使你在后面的忽略文件里面忽略所有的log文件，那也是无效的。所以忽略文件应该趁早建立，应该在那些文件出现之前，就建立好。

`.gitignore`文件的格式规范如下

* 空行或者`#`开头的内容当做注释处理
* 可以使用标准的glob匹配
* 匹配模式以`/`开头防止递归
* 匹配模式以`/`结尾以指定目录
* 要否定某个规则，可以在前面加个惊叹号`!`

glob指的是shell所使用的简化了的正则表达式

* `*`匹配0到任意多个任意字符
* `[abc]`匹配括号里面的单个字符
* `?`只匹配任意1个字符
* `[a-z]`表示a到z的任意一个字符 [0-9]表示0到9的任意一个数字，其余类似
* `**`表示任意中间目录，`a/**/z`匹配a/z, a/b/c/z等

下面看一个`.gitignore`文件样例。

```
# 忽略所有的.a文件
*.a

# 不要忽略lib.a这个文件
!lib.a

# 忽略当前目录下的TODO文件，而不是当前目录下面的任何子目录里面的TODO文件
/TODO

# 忽略 build目录下的所有文件
build/

# 忽略doc目录下的txt文件，不包含doc/sub/*.txt这种子目录中的文件
doc/*.txt

# 忽略doc目录下的所有pdf文件，包含doc/server/arch.txt这样的子目录的文件
doc/**/*.pdf
```

github有个`.gitignore`模板项目，可以参考，https://github.com/github/gitignore

---

**git中文件的移除**

当不想在版本库中管理某个文件的时候，可以`git rm file`来将该文件从版本库中移除，同时将文件实体移除，这种移除是硬移除，另外还有一种软移除，只将文件从版本管理中剔除，但是不删除文件，这种操作往往在添加了`.gitignore`文件之后，但是发现自己的文件没有被忽略的情况下使用，只需要`git rm --cached file`即可，这种操作会将该文件置为Untracked files状态，从版本管理中踢出，但是并不删除文件。

---

**git中文件的移动**

文件的移动就是重命名`git mv file_1 file_2`就把file_1 => file_2了，其实移动相当于下面的三条命令
```bash
$ mv file_1 file_2
$ git rm file_1
$ git add file_2
```
---

**提交历史**

`git log`可以按照提交时间由近到远的查看项目的提交历史。另外，附加的很多选项提供了丰富显示格式和内容文本。

```
git log -p -2  # 显示提交差异 仅限于最近的两次提交
git log --stat # 显示提交所涉及的文件
git log --pretty=oneline # 美化，仅将提交信息显示为1行
git log --pretty=format:"%h - %an, %ar : %s" # 使用自定义格式显示提交
git log --graph # 图形化的显示提交情况
```

关于自定义的格式，里面有很多占位符，具体含义如下

| 选项 | 说明                                    |
| :-:  | :-:                                     |
| %H   | 提交对象的完整hash串                    |
| %h   | 提交对象的简短hash串                    |
| %T   | 树对象的完整hash串                      |
| %t   | 树对象的简短hash串                      |
| %P   | 父对象的完整hash串                      |
| %p   | 父对象的简短hash串                      |
| %an  | 作者的名字                              |
| %ae  | 作者的电子邮件地址                      |
| %ad  | 作者修订日期，可以用--date=选项定制格式 |
| %ar  | 作者修订日期，按多久以前的方式显示      |
| %cn  | 提交者的名字                            |
| %ce  | 提交者的电子邮件地址                    |
| %cd  | 提交日期                                |
| %cr  | 提交日期，按多久以前的方式显示          |
| %s   | 提交说明                                |

作者指的是修改代码的人，提交者是指将工作成果提交到仓库的人。比如，你向一个项目提供了补丁，你就是作者，该项目的核心成员接受你的补丁，将其并入项目，他就是提交者。

**git log常用选项**

| 选项              | 说明                                                                                    |
| :-:               | :-:                                                                                     |
| -p                | 按照补丁格式显示每个更新之间的差异                                                      |
| --stat            | 显示每次更新后的文件修改统计信息                                                        |
| --shortstat       | 只显示--stat最后的行数修改添加移除统计                                                  |
| --name-only       | 仅在提交信息后显示已修改的文件清单                                                      |
| --name-status     | 显示新增、修改、删除的文件清单                                                          |
| --abbrev-commit   | 仅显示SHA-1的40个hash字符的前几个字符                                                   |
| --relative-date   | 使用相对时间显示提交日期，比如两天前                                                    |
| --graph           | 显示ascii图形表示的分支合并历史                                                         |
| --pretty          | 使用其他格式显示历史提交信息，可用选项包括oenline, short, full, fuller, format+指定格式 |
| -(n)              | 仅显示最近的n条提交                                                                     |
| --since, --after  | 仅显示指定时间之后的提交                                                                |
| --until, --before | 仅显示指定时间之前的提交                                                                |
| --author          | 仅显示指定作者相关的提交                                                                |
| --commiter        | 仅显示指定提交者相关的提交                                                              |
| --grep            | 仅显示含指定关键字的提交                                                                |
| -S                | 仅显示添加或者移除了某个关键字的提交                                                    |

---

**git中的某些撤销操作**

* 将某个文件从暂存区撤出

```
$ git add file1 file2
$ # 这个时候突然发现file2的代码还没有写完，不应该放入暂存区
$ # 可以用下面的命令将file2从暂存区拖出来
$ git reset HEAD file2
```
* 将某个文件还原成未修改状态
```
$ vim file2
$ # 此时你发现自己并不想要修改后的file2，更希望它没有被修改过
$ # 只需要下面的命令，就将该文件的修改都抹掉
$ git checkout -- file2
```

## git远程仓库

一群人开发项目的时候，肯定要共享大家的代码提交，此时就需要在版本库之间拉取和推送数据。一般来说，git还需要一个裸版本库来做大家的代码共享，裸版本库没有工作区，仅保存项目的提交历史，而且git不允许向一个不是裸版本库的git仓库做代码推送。这个裸版本库一般就是我们的远程仓库了。

**远程仓库的相关操作**
```
$ # 查看有哪些远程仓库
$ git remote -v

$ # 为本地仓库添加一个名为jsy-github-tutorials的实际地址为https://....的远程仓库
$ git remote add jsy-github-tutorials https://github.com/jsycdut/tutorials

$ # 从名为remote-name的远程仓库拉取我还没有的数据
$ git fetch [remote-name]

$ 将本地提交推送到名为remote-name的branch-name分支上
$ git push [remote-name] [branch-name]

$ # 查看远程仓库的详细信息
$ git remote show [remote-name]

$ # 远程仓库改名 original-remote-name => new-remote-name
$ git remote rename [original-remote-name] [new-remote-name]

$ # 移除一个名为remote-name的远程仓库
$ git remote rm [remote-name]

```
## git里面的标签

标签是一个很好用的git特性，常用的打标签的场景是发版，比如项目开发的差不多了，要发布一个版本，表示一下这是一个里程碑的成就，就可以给这个版本打个标签，以后直接通过这个标签来操作该版本的代码。

标签分两种，轻量标签和附注标签，前者就只是某个特定提交的引用，后者则是存储在git数据库中的完整对象，是可以被校验的，其中包含打标签的人的名字，电子邮件地址，日期时间等等复杂信息，如果要大吹特吹或者大批特批某个标签，就该使用这种附注标签。

```
$ # 查看有哪些标签
$ git tag

$ # 查看名字以v1.8.5*开头的标签
$ git tag -l 'v1.8.5*'

$ # 将当前版本打个附注标签标签，名为v1.4， 附加信息为'my version 1.4'
$ git tag -a v1.4 -m "my version 1.4"

$ # 查看v1.4那个标签
$ git show v1.4

$ # 打一个叫v1.5的轻量标签
$ git tag v1.5

$ # 以上的标签都是针对当前的最新提交
$ # 如果要针对版本历史的提交打标签需要加版本的hash

$ # 给8fclk323553提交打一个轻量标签v0.1
$ git tag v0.1 8fclk323553
```

一般来说，本地打的标签它只存在于本地，要想别人也能看见你打的标签，你得push到远程仓库里面。

```
$ # 把v1.4这个标签推送到远程仓库
$ git push origin master v1.4

$ # 把所有标签一股脑扔到远程仓库
$ git push origin --tags
```

标签太多了也很麻烦，当想要删除某个标签的时候，使用`git tag -d v1.4`就删掉了v1.4这个标签了，当然，这也是本地删除这个标签，要想删除远程的标签，还得`git push origin :refs/tags/v1.4`

当想要检出某个标签对应的提交的代码时，`git checkout v1.5`这样的操作就足够了。但是检出某个标签后，当前的版本库将出现于一个分离头指针状态，这个时候如果你在这个状态下做了新的提交，那么这些提交只能通过该提交的hash来访问，然而谁会去记一个hash值呢，所以你的提交基本上是不可追溯的，除非你新开个分支来标记它。检出一个标签后，使用`git checkout master`来回到主分支。

## git 别名

当你经常使用某个命令，然后又不想每次都输那么长的一串，别名就是个好选择。

```
$ git config --global alias.co checkout --
$ git config --global alias.cm commit -m

$ # 放弃f.txt的更改 co就等价于checkout --
$ git co f.txt
```

## 小技巧

git里面有很多小技巧，比如创建一个已经过去的日期的提交，当我出差无法更新github的时候，我会在后期补上改日的提交，毕竟我不想看到我github的某一天是空白的。

**创建一个其他日期的提交**

1. 写入GIT_COMMITTER_DATE环境变量为其他日期，设置committer date
2. 在提交的时候指定--date选项改写author date

比如

```
export GIT_COMMITTER_DATE="2018-08-08T14:22:21"
git commit --date="2018-08-08T14:22:21" -m "blablablah"
```

github是以提交作者的日期来判定该天的contribution的。
