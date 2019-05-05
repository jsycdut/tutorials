## 约束
Go 默认支持Unicode,Go 是编译型语言， 源代码->ELF文件，以go命令为核心，辅以其他子命令进行编译连接以及运行,以Package组织代码
Go代码格式类似于：声明当前代码属于哪个包，然后引入其他包，之后就是正式的代码,Go的标准库提供百余个包，绝对够用。

Go代码里面一般有个main函数，作为入口

```go
package xxx

import xxx

func main(){

}
```

Go 语句末尾不加分号（这有点像Python），但是代码格式非常强硬，必须遵守相关规定，否则编译出错。可以用gofmt进行代码格式化。

自动引入包：goimports，按需引入，多了的删，少了的补，必须恰如其分，否则编译不通过。

** Hello World **

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello World!")
}
```

要直接运行一个go文件，只需要`go run file.go`即可，编译的话，就用`go build`命令


# 命令行参数处理

Go里面的命令行参数以os.Args表示，表示程序接收到的输入，是一个字符串slice，其长度使用len(os.Args)表示，os.Args[0]表示命令本身的名字，使用os.Args[m:n]获取索引m到n-1的所有数据，相当于切片或者子数组。Go的注释采用`//`来表示。

**Printf()函数的转义字符（Verb）**
|Verb|含义|
|:-:|:-:|
|%d|十进制整数|
|%x,%o,%b|十六进制，八进制，二进制整数|
|%f,%g,%e|浮点数|
|%t|布尔型，true或者false|
|%c|字符（Unicode码点）|
|%s|字符串|
|%q|带引号的字符串或者字符|
|%v|内置格式的任何值|
|%T|任何值得类型|
|%%|%|


