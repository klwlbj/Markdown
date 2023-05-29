#### 特征

转型首选语言

静态编译

没有对象、继承、多态、泛型以及try catch

有接口，并发编程

语言层面支持<mark>并发</mark>，可利用多核处理器，速度快

可弱类型

#### 环境变量

##### GO111MODULE 

o1.11引入的新版模块管理方式，用于开启或关闭 Go 语言中的模块支持，它有 off、on、auto 三个可选值，默认为 auto。GO111MODULE=off无模块支持，go会从 $GOPATH 文件夹和 vendor 目录中寻找依赖项。GO111MODULE=on模块支持，go忽略 $GOPATH 文件夹，只根据go.mod 下载依赖。GO111MODULE=auto时，在 $GOPATH/src 外层且根目录有go.mod 文件时，开启模块支持；否则无模块支持。

##### GOPROXY

更改国内源 `GOPROXY=https://goproxy.cn`

在 ~/.bashrc 或者 ~/.zshrc中加入，启动终端时自动更改配置

```
export GO111MODULE=on
export GOPROXY=https://goproxy.io
# 替换成自己的目录
export GOPATH="/Users/sks/go"
export PATH="$PATH:$GOPATH/bin"
```

##### GOPATH目录

```
src：源代码文件
pkg：包文件，'.a'库文件
bin：相关bin文件
```

#### 命令

##### go install xxx

安装自定义包

普通包内，会在pkg目录下生成文件

main包内，会在bin目录下生成文件

##### go build

编译代码，在包内生成可执行文件

##### go clean

移除编译生成的文件，提交代码时用

##### go fmt

格式化代码，一般少用，IDE会自带格式化

##### go get 

获取远端代码

##### go test

测试文件



#### 基础

##### 变量声明

```go
var a int  = 1
var a,b,c int= 1,2,3
a,b,c := 1,2,'2'  //仅函数内使用,弱类型
var (
    a=3
    b=true
)
// 变量类型：没有char只有rune
// 定义完类型后有默认值

```

##### 类型转换

Go语言中只有强制类型转换（float也不会转成int），没有隐式类型转换

##### 常量

常量无需大写，可不指定类型

```go
const a,b = 3,4
```

##### 字符串

```go 
// 改
s := "hello"
c := []byte(s)  // 将字符串 s 转换为 []byte 类型
c[0] = 'c'
s2 := string(c)  // 再转换回 string 类型
fmt.Printf("%s\n", s2)
```



##### 流程控制

if

goto

for

switch

##### 数据类型

###### 数组Array

###### 切片Slice

###### 映射Map

##### 函数

##### 面向对象

仅支持封装，不支持继承和多态，面向接口，没有class，只有struct

##### 指针

```go
func main() {
    a := 10
    b := &a     // &a代表取变量的地址
    fmt.Printf("a:%d ptr:%p\n", a, &a) // a:10 ptr:0xc00001a078
    fmt.Printf("b:%p type:%T\n", b, b) // b:0xc00001a078 type:*int
    fmt.Println(&b)                    // 0xc00000e018
    c := *b // 指针取值（根据指针去内存取值）
    fmt.Printf("type of c:%T\n", c) // type of c:int
    fmt.Printf("value of c:%v\n", c) // value of c:10
}
```

##### 结构体

```go
 type person struct {
        name string
        city string
        age  int8
    }
```

##### 协程

使用goroutine和channel；

goroutine

goroutine可使一组可复用的函数运行在一组线程上，即使协程阻塞，该线程的其它协程也可被runtime<mark>灵活调度</mark>，转移到其它可运行的线程上；

goroutine占用内存小，可利用多核，协程上下文切换是协同式，非抢占式，<mark>开销</mark>比线程小；

###### 信道通信

CSP模型

Golang必未完全实现，管道通信相关

###### **GPM调度模型**

进程切换会消耗CPU时间

G — 表示 Goroutine，它是一个待执行的任务；

M — 表示操作系统的线程，它由操作系统的调度器调度和管理；

P — 表示处理器，它可以被看做运行在线程上的本地调度器；

![img](D:\Markdown\assets\AgAABTPMKkzoPAQULNFL6Z3xvB9dgx_c.png)

无缓冲信道

有缓冲信道























