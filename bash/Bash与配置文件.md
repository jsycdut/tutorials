# Bash与配置文件

在Linux中，Bash有很多配置文件，比如
```
$HOME/.bashrc
/etc/bash.bashrc
$HOME/.bash_profile
/etc/profile
$HOME/.profile
$HOME/.bash_login
$HOME/.bash_logout
```

总的来说就是，配置多的头皮发麻，但是，这些配置文件是针对不同情况的Bash的(也可能是其他Shell)。

具体的情况有以下几种

1. 登录Bash
2. 交互式非登录Bash
3. SSH执行命令
4. SSH登录
5. 用户登出

* 登录Bash

就是Ctrl + Alt + F<1-6>（不同Linux不一样）开启的非图形化的登录界面，这种就是登录Bash

* 交互式非登录Bash

这种情况出现在图形化界面登陆之后打开的Bash，这种Bash属于交互式Bash，且不是登录式的。

其实区分登录式Bash和非登录式Bash很简单，问你要账户密码的就是登录式的，一般发生在开机时，不问你要密码就是非登录式的，一般在登录完成后进入图形化界面后打开Bash时。

* SSH执行单条命令

之前我也不知道原来用SSH是可以执行单条命令的，一直以为SSH只能输密码，进去之后再执行命令，其实可以利用SSH执行单条命令的，如下

```bash
ssh user@host command
```

* SSH登录

SSH登录就很简单了，就是利用SSH协议登入支持SSH协议的远程主机，形成SSH会话

* 用户登出

用户登出，针对的也是登录进来的用户（有SSH登入和Bash登录两种登录进来的用户）的登出行为。

上面的这些操作，都针对着不同的配置文件。


> 针对不同情形，参考《Linux Shell脚本攻略》以及个人在我的Arch Linux 4.20.4上以及我的Digital Ocean Ubuntu 16.04 的实践，得出如下结果，但是，如果Linux发行版不一样的话，可能会有所不同，如果某位同学不幸看到了这篇文章，请一定，辩证的看待，因为我用Arch，你要是用其他的Linux发行版，那么可能会有不一样的结果。




##  登录Bash

登录Bash对应的配置文件是

1. /etc/profile
2. $HOME/.bash_profile
3. $HOME/.bash_login
4. $HOME/.profile

就我的实验结果，和参考书中的结果一致。综合书本和实践，针对登录Bash，得出的结论是，上述的4个文件，属于登录Bash的配置文件，其中有的文件一定会被执行，有的文件可能会被执行，也有的文件可能不会被执行，具体如下

`/etc/pfofile`一定会被执行，其余的文件存在着优先级关系，也就是上面 2 3 4的优先级了，数值越低，优先级越高，且，一旦高优先级的文件存在，肯定就会被执行，那么即使后面存在低优先级的文件，那么也不会被执行了。

详细的说，就是，`/etc/profile`在Bash登陆的时候雷打不动的将会被执行，然后如果存在`$HOME/.bash_profile`，那么执行`$HOME/.bash_profile`，登录完成，不管`$HOME/.bash_login   $HOME/.profile`。如果没有`$HOME/.bash_profile`，那么查看有没有`$HOME/.bash_login`，有的话，执行，完成登录，不管`$HOME/.profile`，要是没有`$HOME/.bash_profile   $HOME/.bash_login`那么就终于到了`$HOME/.profile`登场的时间了，执行`$HOME/.profile`，要是连`$HOME/.profile`都没有，那么，那么真的就没有了。

现在我似乎理解了我的Arch里面为啥没有上面的所有4个文件了，因为，根本没必要啊，知道了执行顺序，其实只需要`/etc/profle`，然后后面三选一就行了，你说气不气？ 好了，我要去把我的配置文件改回来了。

##  交互式非登录Bash

交互式非登录Bash的配置文件是

1. /etc/bash.bashrc
2. $HOME/.bashrc

这个不存在什么选择性的问题，都要执行，而且是按照顺序来执行的，`/etc/bash.bashrc`先，`$HOME/.bashrc`后。每开一个新的交互式非登录Bash，这俩文件就会被执行一遍。

注意交互式非登录Bash往往出现在图形化界面的Linux发行版中。


## 非交互式非登录Bash

简单点说，就是在交互式非登录Bash里面执行脚本文件，这种情况，默认情况是不会读取任何配置文件的，除非你定义了环境变量`BASH_ENV`，将其指定为某个配置文件，比如`export BASH_ENV="$HOME/.bashrc"`

## SSH执行命令

《Linux Shell脚本攻略》提及，使用ssh执行单条命令，会读取/etc/bash.bashrc $HOME/.bashrc，但是，经我的实验（实现对象，Digital Ocean Ubuntu 16.04，执行ssh user@host command），却发现这俩文件并没有执行，因为我在这俩文件的末尾分别导出了一个环境变量，然后ssh执行的命令恰好是回显环境变量，却发现为空，所以，这俩文件不会被执行单条命令的SSH执行，因此，我觉得书上表述有误，当然，也可能是我的实验对象有问题，毕竟我也只实验了一次。

##  SSH登录

SSH登录也涉及到shell的配置文件，主要是以下三个

1. /etc/profile
2. /etc/bash.bashrc
3. $HOME/.profile

/etc/profile雷打不动的加载，同时在加载的过程中又加载了/etc/bash.bashrc文件，除此之外，$HOME/.profile也会被读取。

所以，SSH登陆进去的话，里面的$HOME/.bashrc是不会默认加载的，里面的环境变量也是不会被读取的。

一般来讲，我的Arch，以及我的VPS（Ubuntu 16.04），都会在/etc/profile中读取/etc/bash.bashrc文件。

## 用户登出

用户登出会话，针对的就是登录的用户，包括SSH登录，以及Bash登录，在登出的时候，都会执行$HOME/.bash_logout文件，

比如，你想在SSH会话登出的时候，清掉屏幕，那么只需要在$HOME/.bash_logout文件中写入一条clear命令即可。

## 最后

弄清楚Bash配置文件的加载场合很重要，是SSH会话，还是Bash登录，还是交互式的非登录Bash，哪些文件在哪些场合会被用上，都要清楚。遇见问题的时候，先分析自己处于哪种情况，然后去查找对应的配置文件，看看问题究竟出在哪里。
