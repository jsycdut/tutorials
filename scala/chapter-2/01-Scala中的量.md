# Scala中的变量

Scala中，不变量是一等公民，这意味着，Scala中的量分为两派，可变类型，以及不可变类型，也就是可变量与不可变量，可变量用var声明，不可变量用val声明。

Scala中，没有基础类型，只有类，代表数据类型的类如下

* Int
* Long
* Short
* Double
* Float
* String
* Byte
* Char
* Unit

大多数都比较好理解，除了Unit，Unit可以看做java中的void。


## 量的声明
Scala中量的声明与java和c++不同，略显复杂。规则如下：
```
var/val <量名>: <Scala类型> = <字面量>
```

比如声明一个整型的数值变量a，赋值为5
```
var a: Int = 5
```

再如声明一个字符串型的不变量b，赋值为"Suck"
```
val b: String = "Suck"
```

## 懒初始化
当你不想过早的初始化你的量的时候，可以使用lazy关键字，这会使你的量在它真正在你的程序中使用的时候才会去初始化
```
lazy val suck = "Suck"
```

可以看见，上面没写类型，Scala会自动给你推断类型，为String。

## 不初始化
当你声明一个变量，却不知道该赋予什么值得时候，就用`_`来替代，以后想好了该是什么值得时候再赋值
```
var no: String = _
no = "Suck"
```

*2019-01-01:14:14:54 插播一句，我的arch安装中的pacstrap终于执行完了，我只想说，国内网络环境真的Suck*


