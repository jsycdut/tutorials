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
