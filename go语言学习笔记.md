# go语言学习笔记

## 1、开发环境搭建

- 目录

  |- `api`  存放`api`接口的目录

  |- `bin`  `go`命令，`go.exe`，`gofmt.exe`

  |- `doc`

  | - `lib`

  ...

- DOS命令

  控制台：`win+r` ---> `cmd`；

  （1）切换盘符： `c:` 、`d:`、 `e:` 大小写没有区分；

  （2）显示详细信息：`dir`;

  （3）改变当前目录：`cd`，（`cd.`代表进入当前目录，`cd..`代表进入上一级目录）；

  （4）清屏：`cls`;

  （5）切换命令：上下箭头；

  （6）补全命令：`tab`按键；

  （7）创建目录：`md`；

  （8）删除目录：`rd`、删除文件`del`；

  （9）复制文件：`copy`+源文件位置+目标文件位置；

- 验证go安装成功

  ```cmd
  PS D:\softwares\Go\bin> go version
  go version go1.25.4 windows/amd64
  ```

- 配置go安装环境

  计算机右键 --> 高级系统设置 --> 系统属性（高级）--> 环境变量 --> 系统变量（`path`）--> 编辑

## 2、语言前置基础

<workSpaceDir>(`goproject`)  >>> <src>(`src`) >>> <code>(`gocode`) >>> <project>(`testproject01`) >>> <main>(`main`)

```go
package main // 声明文件所在的包，每个go文件必须有归属的包声明

import "fmt" // 导入fmt包，引入程序需要的包，为了使用包下的函数和变量

func main() { // 程序入口函数，程序执行从这里开始
    fmt.Println("Hello, world!") // 调用fmt包下的Println函数，输出Hello, world!到控制台
}
```

- 对源文件进行编译

  `go build test.go`编译后产生二进制可编译文件`test.exe`

  `go build -o hello.exe test.go`

- 通过`go run`直接可以帮我们编译执行源文件

  ```go
  go run test.go  //编译运行一条龙
  ```

  > [!NOTE]
  >
  > `go run`时间比`go build` 要长

- 语法注意
  - 源文件以“`go`”为后缀扩展名
  - 程序的执行入口是`main()`函数
  - 严格区分大小写
  - 方法由一条条语句构成，每个语句后不需要分号（go语言会在每行的后自动加分号），不要把多个语句写在同一行，否则报错，多个语句在一行的需要添加分号
  - 定义的变量或者`import`的包声明就要使用
  - 大括号成对出现，缺一不可
  - 向后缩进：`tab`
  - 向前取消缩进： `shift` + `tab`
  - 通过命令完成格式化操作：`gofmt test.go` 不写入文件|| `gofmt -w test.go`写入文件
  - 一行最多不超过80个字符
- 注释
  - 行注释 `/  /`  `crtl` + `/` 官方推荐
  - 多行注释 `/*  */` `shift` + `alt` +`a`

- `api`

  `Golang`网站 https://golang.org

## 3、变量

>  变量相当于内存中一个数据储存空间的表示

```go
package main

import "fmt"

func main() { 
    var age int
    age = 25
    // var age int = 25  或者使用这种写法
    fmt.Println("My age is", age)  //"My age is 25"
}
```

- 变量的使用形式

  ```go
  package main
  import 'fmt'
  
  func main(){
      //指定值并且赋值
      var num int = 18
      fmt.printIn(num)  //18
      //指定变量的类型，但是不赋值，使用默认值
      var num2 int
      fmt.printIn(num2)  //0
      //如果没有填写变量的类型，那么根据=后面的值进行判定变量的类型（自动类型推断）
      var num3 = ‘tom’
      fmt.printIn(num3)  //'tom'
      //省略var，注意 := 不能写为 =
      sex := "男"
      fmt.printIn(sex)  //'男'
  }
  ```

- 支持一次性声明多个变量（多变量声明）

  ```go
  package main
  import 'fmt'
  //全局变量
  var n7 = 100
  var n8 = 9.7
  
  var {
      n9 = 500
      n10 = 'jack'
  }
  
  func main(){
      //局部变量
      //声明多个数字类型变量
      var n1,n2,n3 int
      fmt.printIn(n1)  	//0
      fmt.printIn(n2)		//0
      fmt.printIn(n3)		//0
      //声明多个不同类型变量
      var n4,name,n5 = 10, 'jack', 7.8
      fmt.printIn(n4)		//10
      fmt.printIn(name)	//‘jack’
      fmt.printIn(n5)		//‘7.8’
      //使用:=进行赋值
      n6,height := 6.9,100.6
      fmt.printIn(n6)    //6.9
      fmt.printIn(height)		//100.6
      
      //全局变量打印
      fmt.printIn(n9)    //500
      fmt.printIn(n10)		//'jack'
  }
  ```


## 4、数据类型

> 数据类型
>
> > 基本数据类型
> >
> > > 数值型：整数类型（`int`,`int8`,`int16`,`int32`,`int64`,`uint`,`uint8`,`uint16`,`uint32`,`uint64`,`byte`）；浮点类型（`float32`，`float64`）
> > >
> > > 字符型：（没有单独的字符型，使用`byte`来保存单个字母字符）
> > >
> > > 布尔型：（`bool`）
> > >
> > > 字符串：（`string`）
> >
> > 派生类型数据/复杂数据类型
> >
> > > 指针：
> > >
> > > 数组：
> > >
> > > 结构体：
> > >
> > > 管道：
> > >
> > > 函数：
> > >
> > > 切片：
> > >
> > > 接口：
> > >
> > > map：

- 进制和进制转换

  十进制：逢十进一的问题