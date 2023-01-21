## Golang

https://github.com/shockerli/go-awesome

https://www.topgoer.com/

https://chai2010.cn/advanced-go-programming-book

### 快速开始

下载安装：https://golang.org/

~~~shell
# 1、查看版本
go version

# 2、查看环境
go env
# GOPATH 安装目录
# GOROOT 项目目录，多个项目windows用;隔开、linux用:隔开
# GOBIN 编译打包后的目录
# GOOS 当前系统，如windows、linux、darwin(mac os)
# GOARCH # CPU架构，例如386、amd64、arm等

# 下载代理
go env GOPROXY
# https://goproxy.cn # 国内
# https://goproxy.io # 国外
# 私有仓库
go env GOPRIVATE
# www.domain.com

# 3、交叉编译
# 通过GOOS和GOARCH进行交叉编译。
# 在64位Linux上编译
GOOS=linux GOARCH=amd64 go build demo.go

GOOS      	GOARCH
windows		386/amd64
linux		386/amd64/arm/arm64/ppc6/ppc641e/mips/mipsle/mips64/mips64le/s390x
darwin		386/amd64/arm/arm64
android 	arm
freebsd 	386/amd4/arm
dragonfly 	amd64
netbsd 		386/amd64/arm
plan9		386/amd64
solaris		amd64

# 4、项目目录
- src # 源码目录
- pkg # 编译后产生的中间文件
- bin # 编译后的可执行文件

# 初始化项目依赖
go mod init 项目名
# 在go.mod中指定依赖的包和版本
require github.com/lxn/win letest
# 使用该命令整理依赖：删除不需要的依赖包、下载新的依赖包、更新go.sum
go mod tidy
~~~

>Hello World

~~~go
package main

import "fmt"

func main() {
	fmt.Println("Hello, World!") // 打印
    name := "World"
    fmt.Printf("Hello, %s!", name)
}

%t 布尔
%s 字符串 %b二进制 %c字符unicode码
%q 带引号的
%d 十进制数值 %x 十六进制小写 %X 十六进制大写
%v 默认格式
%+v 添加字段名
%#v 值
%T 类型
%% 百分号
~~~

~~~bash
# 1、运行
go run demo.go
# 2、可编译为二进制
go builder demo.go
# 3、执行二进制文件
./demo.exe
~~~

### 基础语法

一个函数对应一个栈帧（先进后出）。

一个栈帧包含局部变量、形参、内存字段描述值（栈基指针、栈顶指针）。

传值：拷贝赋值。

引用：指向同一块地址。

左值：空间

右值：内容

#### 数据类型

~~~go
// 1、布尔：默认false
var b bool = true

// 2、数字：默认0，有int/float32/
var age int = 5

// 3、字符串：默认""
var str = "hello world"
var str string = "hello world"

// 4、类型转换
float32(age)

// 5、衍生类型
int int32 int64 // 整型类型
byte // 字符
float32 float64 // 浮点类型
string // 字符串
bool // 布尔类型

// Channel
var a chan int

// 接口
var a error

// map
var a map[string] int

// 5、反射获取字符串类型
package main
import (
	"fmt"
	"reflect"
)
func main() {
    var age = 18
	fmt.Println(reflect.TypeOf(age).Kind().String())
}
~~~

##### 结构体

首字母大写的变量相对于包来说是public的，小写则是私有的

~~~go
// 声明：类似java中的类
type User struct{
	name string
	age int
}

// 初始化
var user User = User{"world", 20} // 需依次全部初始化
user := User{name:"world"} // 指定成员变量初始化
var User user = new(User)

// 使用
var User user
user.name = "belean"
user.age = 20
fmt.Println(user.name)

// 结构体比较：所有成员的比较
user1 == user2
user1 != user2

// 结构体赋值
user := User{name: "world", 20};
var user2 User
user2 = user

// 函数传参：值拷贝

// 结构体的地址 == 结构体首个元素的地址
fmt.Println(&user, &user.name)

// 占用空间大小
unsafe.Sizeof(user)
~~~

##### 指针

- 声明未初始化的指针：默认nil（空指针）
- 野指针：指向无效地址

~~~go
package main
import "fmt"
func main() {
    var age int = 18
    
    // p指向age的地址（指针需声明并初始化，仅一次赋值）
    var p *int = &age
    fmt.Println(p) // 0xc0000ac058
	fmt.Println(*p) // 18
    
    // 重新赋值，地址不变
    *p = 20
    fmt.Println(p) // 0xc0000ac058
	fmt.Println(*p) // 20
	fmt.Println(age) // 20
}
~~~

~~~go
// 指向数组的指针
var p []*int

// 指向指针的指针
var p **int

// 返回值初始化：不能返回局部变量的地址值
func initUser() *User {
    user := new(User)
    user.name = "world"
    user.age = 20
    return user
}
user := initUser()
~~~

##### 切片（数组）

slice：本质是包装了数组的结构体

~~~go
package main
import "fmt"
func main() {
    // 1、声明
    // make只能创建slice、map和channel，并且返回一个有初始值（非零）的对象
    // arr := make([]int, 长度, 容量) // 容量默认跟随长度
	arr := [6]int{1,2,3,4,5} // 容量默认跟随长度
	
    // 2、切片
    // [low:high:max] [low起始,high结束) 容量=max-low
    // [:high:max] 0开始，high结束
    // [low:] low开始，数组长度结束
	s := arr[1:3:5]
    
	fmt.Printf("%T\n",s) // []int
	fmt.Println(s) // [2 3]
	fmt.Println(len(s)) // 2
	fmt.Println(cap(s)) // 4
    
    // 未指定max容量,则默认为原数组(切片)的容量
    s := arr[1:3]
    
	fmt.Printf("%T\n",s) // []int
	fmt.Println(s) // [2 3]
	fmt.Println(len(s)) // 2
	fmt.Println(cap(s)) // 5
    
    // 3、append，必须接收返回值
    s = append(s, 4);
	fmt.Println(s) // [2 3 4]
    
    // 4、copy位置拷贝
    // copy(target, dest) 将dest切片拷贝到target切片
    arr1 := []int{8,9,3,4}
    arr2 := []int{1,2}
    copy(arr1,arr2)
    fmt.Println(arr1) // [1 2 3 4]
}
~~~

- `arr := make([]int, 长度)` // 声明切片
- `s := arr[1:3]` // 切片（截取成新切片）

- `s = append(s, 4);` // 给切片添加元素，超出容量时（1024以下），会两倍增加容量
- `copy(target, dest)` // 拷贝数组

##### Map

~~~go
// 初始化方式 int key, string value
// key不能是引用类型
var dict map[int]string // 该方式只声明了map,初始值为nil
var dict map[int]string = map[int]string{1:"ha", 2:"world"}
dict := map[int]string{1:"ha", 2:"world"} // 可初始化赋值
dict := make(map[int]string, 10) // 第二个参数可以指定容量。注意：不能使用cap函数

// 赋值
dict[1] = "hello"
// 取值
dict[1]
v, has := dict[1] // 返回两个值，一个value, has是否存在
if v, has := dict[1]; has { // 写在一行进行判断
    
}

// 遍历, v可省略, 当需要key省略时替换为_
for k, v := range dict {
    
}

// 删除key
delete(dict, 1)

// map做函数参数，传递的是引用

// 拆分字符串，成字符串切片
import "strings"
str := "A B C D"
strArr := strings.Fields(str)
~~~

##### 接口

~~~go
type Service interface{
    add 
}
~~~

#### 变量&常量

~~~go
// 1、指定类型：有对应类型的默认值
var age int // 默认0
var b bool // 默认false
var a *int // 派生类型默认nil

// 2、未指定类型：根据初始值判断数据类型
var str = "hello world"

// 3、声明并赋值
var age int = 18
// 等价
age := 18

// 4、多变量声明并赋值
var i, j = 1, 2
// 等价
i, j := 1, 2

// 5、多变量声明
var i, j int

// 6、多变量赋值
i, j = j, i // 交换值

// 7、常量，基本于变量声明和赋值一样
const str = "Hello"
~~~

- 局部变量需被使用，全局变量可允许不使用。

#### 逻辑控制

##### if else

~~~
if 表达式 { // if
} else if 表达式 { // else if
	if 表达式 { // 嵌套if
	}
}
~~~

- 不支持三目运算符（?:）

##### switch

~~~go
switch val {
    case 1:
    	break
    case 2:
    	break
    default:
    	
}
~~~

##### for

break：跳出循环

continue：跳过本次循环，继续下轮循环

goto：跳到标记处

~~~go
arr := []string{"1","2","3"}

// range：类似forEach，可遍历数组和map
for i, str := range arr {
    fmt.Println(str)
}

// for
for i := 0; i<len(arr); i++ {
    fmt.Println(arr[i])
}

// 类似while
for true {
    
}

// goto
EXIT: for i := 0; i<len(arr); i++ {
    for j := 0; j<len(arr); j++ {
        if i == 5 && j == 5 {
            goto EXIT
        }
	}
}
~~~

#### 函数

~~~go
func 函数名(形参列表) 返回值类型 {
}

// 例如
func getMax(a, b int) int{
    if a > b {
        return a
    } else {
        return b
    }
}

// 可返回多个值
func swap(a, b int) (int, int) {
    return b, a
}

// 匿名函数
func() {
		
}()
~~~

- 函数传值，都是拷贝传递的，如需改变传入的值，需进行引用传递。
  - 也就是将形参定义为指针，传入参数的地址。

> init函数

每个源码中可以使用一个init函数

init函数在main执行前调用

~~~go
func init() {
    
}
~~~

> 匿名导入包

导入包，不使用包内的结构和类型，也不调用包内的任何函数。

一样会将导入的包编译到可执行文件中，同时触发init函数

~~~go
import(
	_ "path/to/package"
)
~~~





### 常用函数

#### 字符串

~~~go
import "strings"

// Split 拆分字符串
str := "A,B,C,D"
arr := strings.Split(str, ",") // [A B C D]

// Fields 按空格拆分
str := "A B C D"
arr := srings.Fields(str) // [A B C D]

// HasSuffix 判断后缀
str := "a.pdf"
has := strings.HasSuffix(str, ".pdf") // true

// HasPrefix 判断前缀
str := "application-dev.yml"
has := strings.HasPrefix(str, "application") // true

~~~

#### 文件

~~~go
import "os"
~~~

> 创建/打开文件

~~~go
// Create 创建文件：不存在就创建，存在则清空。
f, err := os.Create("./newFile.text")
if err != nil {
    fmt.Println("create err:", err)
    return
}
defer f.Close()
fmt.Println("success")

// Open 打开文件：只读方式打开文件。
f, err := os.Open("./newFile.text")

// OpenFile 打开文件：只读、只写、读写方式打开文件。
// 文件路径、打开权限(O_RDONLY只读、O_WRONLY只写、O_RDWR读写)、6
f, err := OpenFile("./newFile.text", os.O_RDWR, 6)
~~~

> 写文件

~~~go
// WriteString 按字符串写入文件
// n写入的字符个数
// 换行:\r\n(windows)、\n(linux)
n, err := f.WriteString("adf\r\nadf")

// 按位置写
// 参数：偏移量,偏移起始位置:io.SeekStart、io.SeekCurrent、io.SeekEnd
// 返回偏移后的位置
import "io"
off, err := f.Seek(-5, io.SeekEnd)
fmt.Println("off:", off)

// 按字节写， 返回写入的字节数
n, _ = f.WriteAt([]byte("12345"), off)
fmt.Println("WriteAt n:", n)
~~~

> 读文件

~~~go
import "bufio"
// 读缓存
reader := bufio.NewReader(f)
for {
    // 按行读
    buf, err := reader.ReadBytes('\n')
    if err != nil && err == io.EOF {
        fmt.Println("read end")
        return
    }else if err != nil {
        fmt.Println("ReadBytes err:", err)
    }
    fmt.Print(string(buf))
}
~~~

> 文件拷贝

~~~go
// 读文件
f_r, err := os.Open("./newFile.text")
if err != nil {
    fmt.Println("Open err:", err)
    return
}
defer f_r.Close()

// 创建文件
f_w, err := os.Create("./copyFile.text")
if err != nil {
    fmt.Println("Create err:", err)
    return
}
defer f_w.Close()

// 读文件，放入缓存，写文件
buf := make([]byte, 4096)

for {
    n, err := f_r.Read(buf)
    if err != nil && err == io.EOF {
        fmt.Printf("read end. n = %d\n", n)
        return
    }
    f_w.Write(buf[:n])
}
~~~

- make([]byte, 4096) # 创建读缓存
- Read([]byte) # 读
- Write([]byte) # 写 

> 打开目录

~~~go
// 打开目录
f, err := os.OpenFile("C:/Users", os.O_RDONLY, os.ModeDir)
if err != nil {
    fmt.Println("Open err:", err)
    return
}
defer f.Close()

// 读目录，-1通常表示全部
f_d, err := f.Readdir(-1)

for _, fileInfo := range f_d {
    if fileInfo.IsDir() {
        fmt.Printf("%s is a dir\n", fileInfo.Name())
    } else {
        fmt.Printf("%s is a file\n", fileInfo.Name())
    }
}
~~~

#### Time

> 格式

~~~
月份 1,01,Jan,January
日　 2,02,_2
时　 3,03,15,PM,pm,AM,am
分　 4,04
秒　 5,05
年　 06,2006
时区 -07,-0700,Z0700,Z07:00,-07:00,MST
周几 Mon,Monday

3 用12小时制表示，去掉前导0
03 用12小时制表示，保留前导0
15 用24小时制表示，保留前导0
03pm 用24小时制am/pm表示上下午表示，保留前导0
3pm 用24小时制am/pm表示上下午表示，去掉前导0
~~~

> 使用

~~~go
import ("time")
~~~

~~~go
func main() {
    // 获取当前时间格式化后的字符串
    // 指定layout，表示年（四位），月日（两位），时分秒（两位）
    time.Now().Format("2006-01-02 15:04:05")
    
}
~~~

### 并发编程

#### 编程技术

> 并行和并发

1s = 1000ms（毫秒）

1ms = 1000us（微秒）

1us = 1000ns（纳秒）

- 并行（parallel）：指在同一时刻，有多条指令在多个处理器上同时执行。（多核CPU）
- 并发（concurrency）：指在同一时刻只能有一条指令执行，但多个指令被快速轮换执行。

- 进程并发：
  - 程序：编译成功的二进制文件。（占用磁盘空间）
  - 进程：运行起来的程序。（占用内存空间）
- 进程状态：初始态、就绪态、运行态、挂起态、终止态。
- 孤儿进程：父进程先于子进程结束。
- 僵尸进程：进程终止，父进程尚未回收，子进程残留资源（PCB）存放于内核中，变成僵尸进程。
- 线程并发：
  - 在linux中线程是LWP轻量级的进程，进程最小的资源分配单位。
  - 在windows中可以忽略进程，线程是最小的可执行单位。
- 同步：
  - 互斥锁：有锁的线程才能访问，否则阻塞等待获取锁。
  - 读写锁：一把锁（读属性、写属性）。写独占，读共享。写锁优先级高。
- 协程并发：
  - 协程（coroutine），也叫轻量级线程。
  - 一个线程可以有任意个协程，但某一时刻只能有一个协程在运行，多个协程共享该线程分配到的计算机资源。
- 总结
  - 进程：稳定性高。
  - 线程：节省资源。
  - 协程：效率高。
- Go并发
  - 支持协程，叫goroutine。
  - 21世纪的C语言，Go从语言层面就支持并行，并提供自动垃圾回收机制。
  - Go语言内置的上层API基于顺序通信进程模型CSP（communicating sequential processes），大大简化了并发程序的编写。
  - Go语言中的并发程序主要使用两种手段实现：goroutine和channel。

#### Goroutine

~~~go
// 产生go程，并发执行
go task()
~~~

> runtime包

~~~go
import runtime

func main() {
    
    go func() {
		for i:=0; i<10; i++ {
			fmt.Println(" this is goroutine test")
			time.Sleep(100*time.Microsecond)
		}
	}()

	for {
        // 让出当前go程所占用的CPU时间片
		runtime.Gosched()
		fmt.Println(" this is main test")
	}
    
}
~~~

~~~go
func test() {
	defer fmt.Println("ccccc")
	return
    // runtime.Goexit()
	fmt.Println("dddddd")
}
func main() {
	go func() {
		fmt.Println("aaaaa")
		test()
		fmt.Println("bbbbbb")
	}()
	for {
		;
	}
}
~~~

- Gosched()：让出当前go程所占用的CPU时间片
- Goexit()：退出当前go程。
- defer：延迟到当前函数结束时执行
- GOMAXPROCS(int)：设置当前进程使用的最大cpu核数，返回上次设置的cpu核数。首次调用返回默认值。

#### channel

channel是go语言中的一个核心类型，可以把它看出管道（FIFO）。并发核心单元通过它发送或者接收数据进行通讯。goroutine奉行通过通信来共享内存，而不是共享内存来通信。

~~~go
// 初始化chan
make(chan Type) // capacity默认0，无缓冲的channel
// 或
make(chan Type, capacity)


var channel = make(chan int)
channel <- 8 // 写入后，等待读（阻塞）
<- channel // 读取后，等待写（阻塞）
~~~

每当有一个进程启动时，系统会自动打开三个文件： 标准输入、标准输出、标准错误。 —— 对应三个文件： stdin、stdout、stderr

当进行运行结束。操作系统自动关闭三个文件。

> 顺序（同步）执行

无缓冲channel：一方执行，另一方等待

~~~go
var channel = make(chan int)
func printer(s string) {
	for _, ch := range s {
		fmt.Printf("%c", ch)
		time.Sleep(300*time.Microsecond)
	}
}
func person1() {
	printer("hello")
	channel <- 8
}
func person2() {
	<- channel
	printer("world")
}
func main() {
	go person1()
	go person2()
	for {
		;
	}
}
~~~

>通信

当子go程执行完，通知主go程，主go程才结束。

~~~go
func main() {
	ch := make(chan string)
    fmt.Println("len(ch)=", len(ch), "cap(ch)=", cap(ch))
	go func() {
		for i:=0;i<2;i++ {
			fmt.Println("i = ", i)
		}
		// 通知
		ch <- "sub goroutine print end"
	}()
	str := <- ch
	fmt.Println("str=", str)
}
~~~

- len(ch)：剩余未读取
- cap(ch)：缓冲容量

>有缓冲channel

异步通信

~~~go
func main() {
	ch := make(chan int, 3)
	fmt.Println("len=", len(ch), "cap=", cap(ch))
	
	go func() {
		for i:=0; i<8; i++ {
			ch <- i
			fmt.Println("sub goroutine write =", i)
		}
	}()
	
	time.Sleep(300*time.Microsecond)
	for i:=0; i<8; i++ {
		num := <- ch
		fmt.Println("main goroutine read =", num)
		
	}
}
~~~

> 关闭channel

- close(ch)
  - 写完了再关闭
  - 关闭了，就不能写了，但可以读

~~~go
func main() {
	ch := make(chan int, 3)
	
	go func() {
		for i:=0; i<8; i++ {
			ch <- i
		}
		close(ch) // 写完了
	}()
	
	for {
		if num, ok := <- ch; ok == true {
			fmt.Println("read =", num)
		} else {
			break
		}
	}
}
~~~

> 使用range读

~~~go
for num := range ch {
}
~~~

> 单向channel

- 默认是双向的：`var ch chan int` 或 `ch := make(chan int)`
  - 转换单向写：`sendCh = ch`
  - 转换单向读：`recvCh = ch`
- 单向写：`var sendCh chan <- int` 或 `sendCh := make(chan <- int)`
- 单向读：`var recvCh <- chan int` 或 `recvCh := make(<- chan int)`

~~~go
func send(out chan <- int) {
	out <- 100
	close(out)
}
func read(input <- chan int) {
	num := <- input
	fmt.Println("read =", num)
}
func main() {
	ch := make(chan int)
	go send(ch)
	read(ch)
}
~~~

> 生产者-消费者模型

- 生产者：发送数据端

- 消费者：接收数据端

- 缓冲区
  - 解耦（降低生产者和消费者之间的耦合度）
  - 并发（生产者和消费者数量不对等时，能保持正常通信）
  - 缓存（生产者和消费者，处理速度不一致时，暂存数据）

#### Timer

定时器

~~~go
func main() {
    // 三种定时方法
	fmt.Println("start time = ", time.Now())
    // time.Sleep
    time.Sleep(time.Second * 2)
	fmt.Println("current time = ", time.Now())
	// Timer.C
    myTimer := time.NewTimer(time.Second * 2)
	nowTime := <- myTimer.C
	fmt.Println("current time = ", nowTime)
    // time.After
	nowTime = <- time.After(time.Second * 2);
	fmt.Println("current time = ", nowTime)
}
~~~

> 停止和重置

~~~go
func main() {
	myTimer := time.NewTimer(time.Second * 2)
	myTimer.Reset(time.Second * 1) // 重置定时时长
	go func() {
		<- myTimer.C
		fmt.Println("sub goroutine time end") // 不会打印
	}()
	myTimer.Stop() // 定时器停止
	for {
		;
	}
}
~~~

- 重置：Timer.Reset(int)
- 停止：Timer.Stop()

> 周期定时

~~~go
func main() {
	quit := make(chan bool)
	fmt.Println("start time = ", time.Now())
	myTimer := time.NewTicker(time.Second * 1)
	go func() {
		i := 0
		for {
			nowTime := <- myTimer.C
			i++
			fmt.Println("nowTime = ", nowTime)
			if i == 5 {
				quit <- true
				break // return // runtime.Goexit
			}
		}
	}()
	<- quit
}
~~~

#### select

select可以监听channel上的数据流动，用法与switch类似。

- case必须是一个IO操作。

- 当多个case条件满足时，随机一个执行。
- 如果没有default，将阻塞，直到case一个执行。
- break可跳出select
- 一般不使用default，避免忙轮询。

~~~go
func main() {
	ch := make(chan int)
	quit := make(chan bool)
	
	go func() {
		for i:=0; i<5; i++ {
			ch <- i
			time.Sleep(time.Second)
		}
		close(ch)
		quit <- true
		runtime.Goexit()
	}()
	
	for {
		select {
		case num := <-ch:
			fmt.Println("read =", num)
		case <-quit:
			return
		}
		fmt.Println("=========")
	}
}
~~~

> 斐波那契数列

~~~go
func fibonacci(ch <- chan int, quit <- chan bool) {
	for {
		select {
		case num := <- ch:
			fmt.Print(num, " ")
		case <-quit:
			runtime.Goexit()
		}
	}
}
func main() {
	ch := make(chan int)
	quit := make(chan bool)
	
	go fibonacci(ch, quit)
	
	x, y := 1, 1
	for i:=0; i<20; i++ {
		ch <- x
		x, y = y, x+y
	}
	quit <- true
}
~~~

> 超时

~~~go
func main() {
	ch := make(chan int)
	quit := make(chan bool)
	go func() {
		for {
			select {
			case num := <- ch: // 2秒就可以读一个
				fmt.Println("num = ", num)
			case <- time.After(3 * time.Second): // 没写了，超时3秒就会退出
				quit <- true
				goto lable // 跳到指定位置，标记在当前函数内有效
			}
		}
		lable:
		fmt.Println("break to lable")
	}()
    // 2秒写一个
	for i:=0; i<2; i++ {
		ch <- i
		time.Sleep(time.Second * 2)
	}
	<- quit
	fmt.Println("finish!")
}
~~~

#### 锁

##### 死锁

单go程死锁

结论：channel至少要在两个以上的go程中通信。

~~~go
func main() {
	ch := make(chan int)
	
	ch <- 100 // 写完阻塞，等待读取，下面语句不会执行。
	num := <- ch
	fmt.Println("num = ", num)
}
~~~

go程间访问顺序导致死锁

结论：先写后读

~~~go
func main() {
	ch := make(chan int)
	num := <-ch
	fmt.Println("num = ", num)
	go func() {
		ch <- 100
	}()
}
~~~

多go程交叉死锁

结论：错误的使用

~~~go
func main() {
	ch1 := make(chan int)
	ch2 := make(chan int)
	
	go func() {
		for {
			select {
			case num := <- ch1:
				ch2 < num
			}
		}
	}()
	for {
		select {
		case num := <- ch2:
			ch1 < num
		}
	}
}
~~~

##### 互斥锁

~~~go
import "sync"

// 互斥锁
var mutex sync.Mutex
func printer(str string) {
	mutex.Lock() // 获取锁
	for _, ch := range str {
		fmt.Printf("%c", ch)
		time.Sleep(time.Millisecond * 300)
	}
	mutex.Unlock() // 释放锁
}
func person1() {
	printer("hello")
}
func person2() {
	printer("world")
}
func main() {
	go person1()
	go person2()
	for {
		;
	}
}
~~~

- `mutex := sync.Mutex`  创建锁
- `mutex.Lock()`  获取锁
- `mutex.Unlock()` 释放锁

建议锁：操作系统提供，建议你在编程时使用

##### 读写锁

读时共享，写时独占。写锁比读锁的优先级高。

~~~go
import "math/rand" // 随机数
import "sync" // 锁
~~~

> 读写锁

~~~go
var rwMutex sync.RWMutex
var value int
func readGo(idx int) {
	rwMutex.RLock()
	num := value
	fmt.Printf("%dth read:%d\n", idx, num)
	rwMutex.RUnlock()
}
func writeGo(idx int) {
	rwMutex.Lock()
	// 生成随机数
	num := rand.Intn(1000)
	value = num
	fmt.Printf("%dth write:%d\n", idx, num)
	time.Sleep(time.Millisecond * 300)
	rwMutex.Unlock()
}
func main() {
	// 随机种子
	rand.Seed(time.Now().UnixNano())
	for i:=0; i<5; i++ {
		go readGo(i)
	}
	for i:=0; i<5; i++ {
		go writeGo(i)
	}
	for {
		;
	}
}
~~~

> channel读写

~~~go
func readGo(in <- chan int, idx int) {
	num := <- in
	fmt.Printf("%dth read:%d\n", idx, num)
}
func writeGo(out chan <- int, idx int) {
	// 生成随机数
	num := rand.Intn(1000)
	out <- num
	fmt.Printf("%dth write:%d\n", idx, num)
	time.Sleep(time.Millisecond * 300)
}
func main() {
	// 随机种子
	rand.Seed(time.Now().UnixNano())
	ch := make(chan int)
	for i:=0; i<5; i++ {
		go readGo(ch, i)
	}
	for i:=0; i<5; i++ {
		go writeGo(ch, i)
	}
	for {
		;
	}
}
~~~

##### 条件变量

条件变量不是锁，但经常跟锁一起使用。

~~~go
// 生产者消费者模型
var cond sync.Cond // 条件变量
func producer(out chan <- int, idx int) {
	for {
		cond.L.Lock() // 加锁
		// 判断缓冲区是否满
		for len(out) == 5 {
			cond.Wait() // 释放锁
		}
		num := rand.Intn(800)
		out <- num
		fmt.Printf("producer%d write:%d\n", idx, num)
		cond.L.Unlock() // 解锁
		cond.Signal() // 唤醒阻塞在条件变量上的消费者
		time.Sleep(time.Millisecond * 200)
	}
	close(out)
}
func consumer(in <- chan int, idx int) {
	for {
		cond.L.Lock() // 加锁
		// 判断缓冲区是否空
		for len(in) == 0 {
			cond.Wait() // 释放锁
		}
		num := <- in
		fmt.Printf("consumer%d read:%d\n", idx, num)
		cond.L.Unlock() // 解锁
		cond.Signal() // 唤醒阻塞在条件变量上的生产者
		time.Sleep(time.Millisecond * 300)
		
	}
}
func main() {
	product := make(chan int, 5)
	rand.Seed(time.Now().UnixNano()) // 随机数种子
	quit := make(chan bool)
	// 指定条件变量 使用的锁
	cond.L = new(sync.Mutex) // 互斥锁 初值0，未加锁状态
	
	for i:=0; i<5; i++ {
		go producer(product, i)
	}
	
	for i:=0; i<5; i++ {
		go consumer(product, i)
	}
	
	<- quit
}
~~~

- `Wait()` 
  - 阻塞等待条件变量满足
  - 释放已获得的互斥锁。相当于`cond.L.Unlock()`
  - 当被唤醒，Wait()函数返回时，解除阻塞并重新获得互斥锁。相当于`cond.L.Lock()`
- `Signal()`
  - 单发通知，给阻塞在该条件变量上的go程发送通知。
- `Broadcast()`
  - 广播通知，给阻塞在该条件变量上的所有go程发送通知。

### 网络编程

#### 概述

##### 协议

通信双方都按相同的规则进行传输数据。

应用层：常见协议有HTTP协议，FTP协议。

传输层：常见协议有TCP/UDP协议。

网络层：常见协议有IP协议、ICMP协议、IGMP协议。

链路层：常见协议有ARP协议、RARP协议。

TCP传输控制协议（Transmission Control Protocol）是一种面向连接的、可靠的、基于字节流的传输层通信协议。

UDP用户数据报协议（User Datagram Protocol）是OSI参考模型中一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务。

HTTP超文本传输协议（Hyper Text Trarnsfer Protocol）是互联网上应用最为广泛的一种网络协议。

FTP文本传输协议（File Transfer Protocol）

IP协议是因特网互联协议（Internet Protocol）

ICMP协议是Internet控制报文协议（Internet Control Message Protocol）它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。

IGMP协议是Internet组管理协议（Internet Group Management Protocol），是因特网协议家族中的一个组播协议。该协议运行在主机和组播路由器之间。

ARP协议是正向地址解析协议（Address Resolution Protocol），通过已知的IP，寻找对应主机的MAC地址。

RARP是反向地址转换协议，通过MAC地址确认IP地址。

##### 分层架构

OSI/RM（理论标准），从高到低：

- [应用层、表示层、会话层]、[传输层]、[网络层]、[数据链路层、物理层]。

TCP/IP（事实标准），从高到低：

- 应用层、传输层、网络层、链路层

每一层利用下一层提供的服务来为上一层提供服务，本层服务的实现细节对上层屏蔽。

- 应用层：应用程序，与用户之间的交互。ftp、http、自定义等协议，对数据进行封装传输。
- 传输层：进程之间的交互。TCP/UDP协议，封装端口标识进程。
- 网络层：主机之间的交互。IP协议，根据IP标识主机
- 链路层：设备之间的交互。ARP协议，根据IP获取网卡mac地址

数据通信过程：层层封装和解封装。

##### Socket

套接字，插座和插头

channel：双向半双工

电话：双向全双工

遥控器：单工通信

网络应用程序设计模式：

- C/S模式：数据存至客户端；协议选择灵活；需开发服务端和客户端，工作量大。
- B/S模式：开发量较小；不受平台限制，受浏览器限制，协议和缓存受限。

#### TCP通信

面向连接的，可靠的数据包传输

> C/S架构

服务器net.Listen()监听客户端连接。

服务器Accept()阻塞等待客户端连接。

客户端net.Dial()连接。

客户端Write()发送数据请求，服务器Read()读取并处理数据，最终Write()响应数据客户端Read()接收。

客户端和服务端通信结束Close()。

> 三次握手

目的：建立连接

- 客户端发送SYN请求包(1000(0))。
  - 客户端：听得见吗？
- 服务端响应ACK应答包(1001)，并发送SYN请求包(8000(0))。
  - 服务端：听得见！你听得见吗？
- 客户端响应ACK应答包(8001)。
  - 客户端：听得见！

> 发送数据

- 客户端发送ACK(8001)，并携带数据(1001(20))
- 服务端应答ACK(1021)，并携带数据(8001(10))

> 四次挥手

目的：断开连接

- 客户端发送FIN断开连接请求(1021(0))，并应答ACK(8011)。
  - 客户端：不说了，挂了吧！
- 服务器应答ACK(1022)。
  - 服务器：我准备好了！
- 服务器发送FIN断开连接请求(8011(0))，并应答ACK(1022)。（半关闭状态）
  - 服务器：你准备好了？我先挂了。
  - 服务器断开连接...
- 客户端发送应答ACK(8012)。
  - 客户端：我准备好了，定时一分钟后自动挂。
  - 客户端等待2MSL后断开连接...
    - MSL（Maximum Segment Lifetime，报文最大生存时间）
    - 在Linux中1MSL等于30秒。

>TCP状态转移

主动发起连接请求端：CLOSED -> 完成三次握手 -> ESTABLISEHED 数据通信状态 -> Dial()函数返回

被动发起连接请求端：CLOSED -> 调用Accept()函数 -> LISTEN -> 完成三次握手 -> ESTABLISEHED（数据通信状态） -> Accept函数返回

主动关闭连接请求：ESTABLISEHED -> FIN_WAT_2（半关闭） -> TIME_WAIT -> 2MSL -> 确认最后一个ACK被对端成功接收 -> CLOSE

被动关闭连接请求：ESTABLISEHED -> CLOSE

~~~bash
# windows
# 启动socket server，等待客户端连接，监听状态LISTENING
netstat -ano | findstr "8000"
  TCP    127.0.0.1:8000         0.0.0.0:0              LISTENING       16748
# 启动socket client，数据通信状态ESTABLISHED
netstat -ano | findstr "8000"
  TCP    127.0.0.1:8000         0.0.0.0:0              LISTENING       16748
  TCP    127.0.0.1:8000         127.0.0.1:52983        ESTABLISHED     16748
  TCP    127.0.0.1:52983        127.0.0.1:8000         ESTABLISHED     12268
# 关闭服务端conn.Close()，客户端立即关闭，服务端等待2MSL关闭
  TCP    127.0.0.1:8000         0.0.0.0:0              LISTENING       16748
  TCP    127.0.0.1:8000         127.0.0.1:52983        TIME_WAIT       0
# 关闭客户端conn.Close()，服务端立即关闭，客户端等待2MSL关闭
  TCP    127.0.0.1:8000         0.0.0.0:0              LISTENING       16748
  TCP    127.0.0.1:51931        127.0.0.1:8000         TIME_WAIT       0
~~~

##### server

~~~go
package main

import (
	"fmt"
	"net"
)

func main() {
	// 指定服务器 通信协议、ip、port
	listener, err := net.Listen("tcp", "127.0.0.1:8000")
	if err != nil {
		fmt.Println("net.Listen err:", err)
		return
	}
	defer listener.Close()

	// 阻塞监听客户端连接请求
	fmt.Println("服务器等待客户端建立连接...")
	conn, err := listener.Accept()
	if err != nil {
		fmt.Println("listener.Accept:", err)
		return
	}
	defer conn.Close()

	// 读取客户端发送的数据
	fmt.Println("服务器与客户端建立连接！")
	buf := make([]byte, 4096)
	n, err := conn.Read(buf)
	if err != nil {
		fmt.Println("conn.Read err:", err)
		return
	}
	conn.Write(buf[:n])
	fmt.Println("服务器读取到数据:", string(buf[:n]))
}
~~~

> 使用netcat模拟客户端

~~~bash
nc 127.0.0.1 8000
hello socket
~~~

> 总结

- net.Listen("tcp", "127.0.0.1:8000")
  - 创建套接字。
- listener.Accept()
  - 阻塞等待客户端连接。
- conn.Read(buf)
  - 读客户端数据。
- conn.Write(buf[:n])
  - 写数据给客户端。

##### client

~~~go
package main

import (
	"fmt"
	"net"
)

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:8000")
	if err != nil {
		fmt.Println("net.Dial err:", err)
		return
	}
	defer conn.Close()

	conn.Write([]byte("Are you Ready?"))

	buf := make([]byte, 4096)
	n, err := conn.Read(buf)
	if err != nil {
		fmt.Println("conn.Read err:", err)
		return
	}
	conn.Write(buf[:n])
	fmt.Println("服务器回发:", string(buf[:n]))
}
~~~

> 总结

- net.Dial("tcp", "127.0.0.1:8000")
  - 连接服务端。
- conn.Write([]byte("Are you Ready?"))
  - 写数据给服务端。
- conn.Read(buf)
  - 读服务端响应的数据。

#### UDP通信

无连接的，不可靠的报文传递。

- 监听：ResolveUDPAddr(network, address string) (*UDPAddr, error)
- 创建socket：ListenUDP(network string, laddr * UDPAddr) (*UDPConn, error)
- 接收数据：ReadFromUDP(b []byte) (int, UDPAddr, error)
- 写入数据：WriteToUDP(b []byte, addr *UDPAddr) (int, error)

##### server

~~~go
package main

import (
	"fmt"
	"net"
	"time"
)

func main() {
	// UDP地址
	sevAddr, err := net.ResolveUDPAddr("udp", "127.0.0.1:8000")
	if err != nil {
		fmt.Println("net.ResolveUDPAddr err:", err)
		return
	}
	fmt.Println("udp 服务器地址结构创建完成！")
	// 创建socket
	udpConn, err := net.ListenUDP("udp", sevAddr)
	if err != nil {
		fmt.Println("net.ListenUDP err:", err)
		return
	}
	defer udpConn.Close()
	fmt.Println("udp 服务器通信Socket创建完成！")

	// 读取客户端数据
	buf := make([]byte, 4096)
	// cltAddr客户端地址
	n, cltAddr, err := udpConn.ReadFromUDP(buf)
	if err != nil {
		fmt.Println("udpConn.ReadFromUDP err:", err)
		return
	}
	fmt.Printf("读取到客户端 %v 的数据：%s\n", cltAddr, string(buf[:n]))

	// 获取系统当前时间
	daytime := time.Now().String()

	// 回写数据给客户端
	_, err = udpConn.WriteToUDP([]byte(daytime), cltAddr)
	if err != nil {
		fmt.Println("udpConn.WriteToUDP err:", err)
		return
	}
}
~~~

> 使用netcat模拟UDP客户端

~~~bash
nc -u 127.0.0.1 8000
~~~

> 总结

- net.ResolveUDPAddr("udp", "127.0.0.1:8000")
  - 创建UDP地址。
- net.ListenUDP("udp", sevAddr)
  - 创建套接字。
- udpConn.ReadFromUDP(buf)
  - 读取客户端数据。
- udpConn.WriteToUDP([]byte(daytime), cltAddr)
  - 写给客户端数据。

##### client

~~~go
package main

import (
	"fmt"
	"net"
)

func main() {
	conn, err := net.Dial("udp", "127.0.0.1:8000")
	if err != nil {
		fmt.Println("net.Dial err:", err)
		return
	}
	defer conn.Close()

	conn.Write([]byte("Are you Ready?"))

	buf := make([]byte, 4096)
	n, err := conn.Read(buf)
	if err != nil {
		fmt.Println("conn.Read err:", err)
		return
	}
	conn.Write(buf[:n])
	fmt.Println("服务器回发:", string(buf[:n]))
}
~~~

> 总结

与TCP连接类似，只需将协议修改为udp即可

- net.Dial("udp", "127.0.0.1:8000")
  - 连接服务器。
- conn.Write([]byte("Are you Ready?"))
  - 写数据给服务端。
- conn.Write(buf[:n])
  - 读取服务端数据。

#### TCP与UDP差异

| TCP               | UDP              |
| ----------------- | ---------------- |
| 面向连接          | 面向无连接       |
| 要求系统资源较多  | 要求系统资源较少 |
| TCP程序结构较复杂 | UDP程序较简单    |
| 使用流式          | 使用数据包式     |
| 保证数据准确性    | 不保证数据准确性 |
| 保证数据顺序      | 不保证数据顺序   |
| 通讯速度较慢      | 通讯速度较快     |

- TCP：对不稳定的网络层，做完全弥补操作。
  - 适用于数据传输安全性、稳定性要求较高的场合。例如网络文件传输，上传、下载。

- UDP：对不稳定的网络层，不作为。
  - 对数据实时传输要求较高的场合，视频直播、在线电话会议、游戏。

#### HTTP

##### Hello World

> 服务端

~~~go
package main

import (
	"net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("hello world"))
}

func main() {
	// 注册回调函数
	http.HandleFunc("/hello", handler)
	// 绑定服务器监听地址
	http.ListenAndServe("127.0.0.1:8000", nil)
}
~~~

- http.HandleFunc("/hello", handler)
  - 定义路径
  - handler(w http.ResponseWriter, r *http.Request) // 回调函数
    - ResponseWriter // 响应
    - *http.Request // 请求
- http.ListenAndServe("127.0.0.1:8000", nil)
  - 服务器地址

> 客户端

~~~go
package main

import (
	"fmt"
	"net"
	"os"
)

func printError(err error, info string) {
	if err != nil {
		fmt.Println(info, err)
		os.Exit(1)
	}
}

func main() {
	conn, err := net.Dial("tcp", "127.0.0.1:8000")
	printError(err, "net.Dial err:")
	defer conn.Close()

	httpRequest := "GET /hello HTTP/1.1\r\nHost:127.0.0.1:8000\r\n\r\n"
	conn.Write([]byte(httpRequest))

	buf := make([]byte, 4096)
	n, _ := conn.Read(buf)
	if n == 0 {
		return
	}
	fmt.Printf("|%s|", string(buf[:n]))
}
~~~

### 爬虫

网络爬虫（web crawler），也叫网络蜘蛛（spider）。

聚焦爬虫：爬取特定网址的数据。

爬取流程：

- 明确目标 URL
- 发送请求，获取应答数据包。
- 保存、过滤数据。提取游泳信息
- 使用、分析得到的数据。

~~~
https://tieba.baidu.com/f?kw=%E8%B5%9B%E5%8D%9A%E6%9C%8B%E5%85%8B2077&ie=utf-8&pn=0
https://tieba.baidu.com/f?kw=%E8%B5%9B%E5%8D%9A%E6%9C%8B%E5%85%8B2077&ie=utf-8&pn=50
https://tieba.baidu.com/f?kw=%E8%B5%9B%E5%8D%9A%E6%9C%8B%E5%85%8B2077&ie=utf-8&pn=100
https://tieba.baidu.com/f?kw=%E8%B5%9B%E5%8D%9A%E6%9C%8B%E5%85%8B2077&ie=utf-8&pn=450
~~~

#### 正则表达式

Go语言使用的RE2标准

`.`：匹配一个字符

`[]`：匹配括号中的字符，可指定范围：a-z、A-Z、0-9

`^`：在[]中使用，取反，排除指定的字符

`?`：匹配前面单元出现0~1次

`+`：匹配前面单元出现1~N次

`*`：匹配前面单元出现0~N次

`{}`：匹配前面单元出现的次数，例如：`{2}`匹配2次，`{2,}`最少匹配2次，`{2,4}`最少2次最多匹配2次

`()`：将一部分正则表达式组成一个单元，可以对其使用数量限定符。

- `func MustCompile(str string) *Regexp`
  - 解析编译正则表达式
- `func (re *Regexp) FindAllStringSubmatch(s string, n int) [][]string`
  - 提取信息，传入要查找的字符串，及匹配的次数，通常传-1表示匹配所有

~~~go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	str := "abc a7c mfc cat 8ca azc cba"

	ret := regexp.MustCompile(`a.c`)

	alls := ret.FindAllStringSubmatch(str, -1)
	fmt.Println("alls:", alls)
    // alls: [[abc] [a7c] [azc]]
}
~~~

> 多行模式

`?s:`

~~~go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	str := `
	<p>
		ddd
		aaa
	</p>
	<div>
		aaaa
		bbb
	</div>
	<div>
		zzz
		xxx
	</div>
`

	ret := regexp.MustCompile(`<div>(?s:(.*?))</div>`)

	alls := ret.FindAllStringSubmatch(str, -1)
	fmt.Println("alls:", alls)
}
/*alls: [[<div>
                aaaa
                bbb
        </div>
                aaaa
                bbb
        ] [<div>
                zzz
                xxx
        </div>
                zzz
                xxx
        ]]*/
// 返回两个元素，一个是匹配到的字符串，第二个是正则匹配部分的字符串
~~~

#### 爬取豆瓣电影

~~~
https://movie.douban.com/top250?start=0&filter=
https://movie.douban.com/top250?start=25&filter=
https://movie.douban.com/top250?start=50&filter=

电影名称：`<img width="100" alt="(?s:(.*?))"`
分数：`<span class="rating_num" property="v:average">(?s:(.*?))</span>`
评分人数：`<span>(?s:(.*?))人评价</span>`
~~~

~~~go
package main

import (
	"fmt"
	"io"
	"net/http"
	"os"
	"regexp"
	"strconv"
)

func HttpGetDB(url string) (result string, err error) {
	resp, err1 := http.Get(url)
	if err1 != nil {
		err = err1
		return
	}
	defer resp.Body.Close()
	// 循环爬取整页数据
	buf := make([]byte, 4096)
	for {
		n, err2 := resp.Body.Read(buf)
		if n == 0 {
			break
		}
		if err2 != nil && err2 != io.EOF {
			err = err2
			return
		}
		result += string(buf[:n])
	}
	return
}

func Save2File(idx int, filmName, filmScore, peopleNum [][]string) {
	f, err := os.Create("douban/第" + strconv.Itoa(idx) + "页.txt")
	if err != nil {
		fmt.Println("os.Create err:", err)
		return
	}

	// 写
	n := len(filmName) // 条目
	f.WriteString("电影名称\t\t\t评分\t\t\t评分人数\n")
	for i := 0; i < n; i++ {
		f.WriteString(filmName[i][1] + "\t\t\t" + filmScore[i][1] + "\t\t\t" + peopleNum[i][1] + "\n")
	}
	f.Close()
}

func SpiderDouban(idx int, page chan<- int) {
	url := "https://movie.douban.com/top250?start=" + strconv.Itoa((idx-1)*25) + "&filter="
	fmt.Println(url)
	result, err := HttpGetDB(url)
	if err != nil {
		fmt.Println("HttpGetDB err:", err)
		return
	}
	fmt.Println("result:", result)
	// 提取 电影名称
	ret := regexp.MustCompile(`<img width="100" alt="(?s:(.*?))"`)
	filmName := ret.FindAllStringSubmatch(result, -1)
	// 提取 分数
	ret2 := regexp.MustCompile(`<span class="rating_num" property="v:average">(?s:(.*?))</span>`)
	filmScore := ret2.FindAllStringSubmatch(result, -1)
	// 提取 评分人数
	ret3 := regexp.MustCompile(`<span>(?s:(.*?))人评价</span>`)
	peopleNum := ret3.FindAllStringSubmatch(result, -1)

	Save2File(idx, filmName, filmScore, peopleNum)

	page <- idx
}

func toWork(start, end int) {
	fmt.Printf("正在爬取 %d 到 %d 页...\n", start, end)

	page := make(chan int)

	for i := start; i <= end; i++ {
		go SpiderDouban(i, page)
	}

	for i := start; i <= end; i++ {
		fmt.Printf("第 %d 页爬取完毕\n", <-page)
	}
}

func main() {
	// 指定爬取起始页、终止页
	var start, end int
	fmt.Print("请输入起始页（>=1）:")
	fmt.Scan(&start)
	fmt.Print("请输入终止页（>=start）:")
	fmt.Scan(&end)

	toWork(start, end)
}
~~~

### 整合

#### mysql

> 配置GOPATH环境变量

> 安装驱动

~~~bash
go get github.com/go-sql-driver/mysql
~~~

>连接数据库

~~~go
package main
import (
	"database/sql"
	"fmt"
	_ "github.com/go-sql-driver/mysql"
)
func main() {
    // 创建连接
    db, err := sql.Open("mysql", "username:password@(127.0.0.1:3306)/dbName")
    if err != nil {
        fmt.Println("sql.Open err:", err)
        return nil
    }
    defer db.Close()
    // 连接数据库
    err = db.Ping()
    if err != nil {
        fmt.Println("db.Ping err:", err)
        return nil
    }
    // 插入
    sql := "INSERT INTO dept(name, source) VALUES(?, ?)"
	//result, err := db.Exec(sql, name, source) // 执行sql
    stmt, err := db.Prepare(sql) // 预处理语句
    if err != nil {
		fmt.Println("sql错误：", err)
		return
	}
    result, err := stmt.Exec(name, source) // 执行sql
	count, err = result.RowsAffected() // 获取受影响的行数
	if err != nil {
		fmt.Println("新增失败：", err)
	}
    
    // 查询单行
    sql = "SELECT * FROM dept WHERE id = ?"
    dept := new(Dept)
	row := conn.db.QueryRow(sql, id)
	err = row.Scan(&dept.Id, &dept.Name, &dept.Source)
	if err != nil {
		fmt.Println("查询失败：", err)
		return nil
	}
	fmt.Printf("result:%v\n", dept)
    
    // 查询多行
    sql = "SELECT * FROM dept"
    depts := make([]Dept, 0)
	rows, err := conn.db.Query(sql)
	for rows.Next() {
		dept := new(Dept)
		rows.Scan(&dept.Id, &dept.Name, &dept.Source)
		depts = append(depts, *dept)
	}
	if err != nil {
		fmt.Println("查询失败：", err)
		return nil
	}
	for i, d := range depts {
		fmt.Printf("row%d:%v\n", i+1, d)
	}
    
}

type Dept struct {
	Id     int    "部门ID"
	Name   string "部门名称"
	Source string "部门所在地"
}
~~~

- sql.Open(driverName, dataSourceName) // 创建连接
- db.Ping() // 连接数据库
- db.Exec(sql, name, source) // 执行sql
- db.Prepare(sql) // 预处理
  - stmt.Exec(name, source) // 执行sql
- result.RowsAffected() // 返回受影响行数
- db.QueryRow(sql, id) // 查询单行
  - row.Scan(&dept.Id, &dept.Name, &dept.Source) // 扫描一行
- db.Query(sql) // 查询多行
  - rows.Next() // 是否有下一行
  - rows.Scan(&dept.Id, &dept.Name, &dept.Source) // 扫描一行

#### Redis

~~~go
import ("github.com/gomodule/redigo/redis")
func main() {
    conn, _ := redis.Dial("tcp", "127.0.0.1:6379")
    defer conn.Close()
    
    // 放入缓冲区，flush后才执行
    conn.Send("set", "name", "zhangsan")
    conn.Flush()
    
    // 获取命令执行的结果
    rel, err := conn.Receive()
    if err != nil {
        fmt.Println("conn.Receive err:", err)
        return
    }
    fmt.Println(rel)
    
    // 直接执行
    rel, err := redis.String(conn.Do("set", "name", "zhangsan"))
    fmt.Println(rel)
    
    rel, err := redis.Values(conn.Do("mget", "name", "age"))
    redis.Scan(rel, &name, &age)
}
~~~

> 序列化、反序列化

~~~go
dest := "1234"

var buffer bytes.Buffer
enc := gob.NewEncode(&buffer) // 编码器
err := enc.Encoder(dest) // 编码


~~~

~~~go
dec := gob.NewDecoder(bytes.NewReader(buffer byte[])) // 解码器
dec.Decode(&src) // 解码
~~~

### Beego

基于MVC的后端框架

官网：https://beego.vip/

#### 下载安装

~~~bash
# 下载beego
go get -u github.com/astaxie/beego
# 下载bee
go get -u github.com/beego/bee
# 查看版本号
bee version
# 创建项目
bee new bee-demo
# 运行项目
bee run


go get github.com/beego/beego/v2@v2.0.0
~~~

> 项目结构

~~~
|- bee-demo
	|-conf # 配置文件
	|-controller # 控制层
	|-models # 依赖的模块
	|-routers # 路由
	|-static # 静态资源
	|-tests # 测试
	|-views # 视图层html
	|-main.go # 程序入口
~~~

#### 路由

~~~go
// 普通路由
beego.Router("/", &controllers.MainController{})

// 高级路由:
// 指定请求方式和方法。多个请求方式用,分割。多个方法用;分割
beego.Router("/", &controllers.MainController{}, "get:GetFunc")
beego.Router("/", &controllers.MainController{}, "get,post:GetFunc")
beego.Router("/", &controllers.MainController{}, "get:GetFunc;post:PostFunc")

// 正则路由：
// 访问/index/1通过。?表示0或多个，去掉?访问/index将不通过
beego.Router("/index/?:id", &controllers.MainController{}, "get:GetFunc")
// 正则放后面匹配至少一个数字
beego.Router("/index/:id([0-9]+)", &controllers.MainController{}, "get:GetFunc")

// 获取前缀和后缀(固定写法)，例如:/index/icon.jpg
beego.Router("/index/*.*", &controllers.MainController{}, "get:GetFunc")
c.getString(":path") // icon
c.getString(":ext") // jpg
// *全匹配，获取路径， 例如:/index/icon.jpg
beego.Router("/index/*", &controllers.MainController{}, "get:GetFunc")
c.getString(":splat") // icon.jpg

~~~

- this.Ctx.Input.Param(":ext") // 获取正则路由参数
- this.Ctx.Input.Bind(&name, "name") // 获取get参数到name变量

#### Controller

~~~go
type MysqlController struct {
	beego.Controller
}
func (this *MysqlController) ShowMysql() {
    id := this.getString("id")
}
~~~

- `this.getString("id")` # 获取请求参数
- `this.Ctx.WriteString("hello world")` # 写入响应
- `this.Redirect("/", 302)` # 重定向
- `c.TplName = "test.html"` # 转发

#### 文件上传

~~~html
<form action="/upload" enctype="multipart/form-data">
    <input type="file" name="file"/>
</form>
~~~

~~~go
// f文件、h文件头信息
f,h,err := this.GetFile("file") // 获取上传的文件
defer f.Close()
// 获取文件后缀 .jpg
ext := path.Ext(h.Filename)
// 获取文件大小
size := h.Size()
// file参数名，存储路径
this.SaveToFile("file", "/D/file/" + h.Filename) // 存储上传文件
~~~

- `path.Ext(h.Filename)` # 根据文件名获取后缀
- 

#### 视图语法

~~~html
<!-- 循环1 -->
{{range .users}}
{{.Name}}
{{end}}

<!-- 循环2 -->
{{range $index, $user := .users}}
{{.Name}}
{{end}}

<!-- if else -->
{{if user.gender == 1}}
{{else}}
{{end}}
~~~

#### 邮箱

~~~go
import ("github.com/astaxie/beego/utils")

config := `{"username": "qq@qq.com", "passwrod", "myPassword", "host":"smtp.qq.com", "port":587}`
email := utils.NewEMail(config)
email.From = "xx@qq.com"
email.To = "xxx@qq.com"
email.Subject = "xxxxx" // 标题
email.Text = "xxxxxxxxxx" // 内容
email.HTML = "<h1>标题</h1>" // HTML内容
email.Send()
~~~



#### ORM

##### 连接数据库

~~~go
package models

import (
	"github.com/astaxie/beego"
	"github.com/astaxie/beego/orm"
)

func init() {
	// 1、连接数据库
	// 数据库别名，驱动名，连接地址
	err := orm.RegisterDataBase("default", "mysql", "root:root@tcp(127.0.0.1:3306)/class1?charset=utf8")
	if err != nil {
		beego.Info("连接数据库错误", err)
		return
	}
	// 2、注册表
	orm.RegisterModel(new(User))
	// 3、生成表
	// 数据库别名，是否强制覆盖，是否打印创表过程
	err = orm.RunSyncdb("default", false, true)
	if err != nil {
		beego.Info("生成表错误", err)
		return
	}
}

type User struct {
	Id   int
	Name string
}
~~~

##### 插入

~~~go
o := orm.NewOrm()

user := models.User{}
user.Name = "张三"
_, err := o.Insert(&user)
if err != nil {
    beego.Info("插入失败", err)
    return
}
~~~

- ormer.Insert

##### 更新

> 先查询再更新

~~~go
o := orm.NewOrm()
user := models.User{Id: 2}
err := o.Read(&user)
if err != nil {
    beego.Info("查询失败", err)
    return
}
user.Name = "张三三"
_, err = o.Update(&user)
if err != nil {
    beego.Info("更新失败", err)
    return
}
~~~

- ormer.Update # 查询后更新

##### 查询

~~~go
o := orm.NewOrm()
user := models.User{Id: 2}
err := o.Read(&user)
if err != nil {
    beego.Info("查询失败", err)
    return
}
fmt.Printf("%T",user)
~~~

- ormer.Read() # 查询单个

> 高级查询

~~~go
o := orm.NewOrm()
// 查询表，返回querySeter
qs := o.queryTable("user")
var users[] models.User
qs.All(&users) // 查询所有
~~~





### GUI for Walk

一个用于Go编程语言的Windows GUI工具包

#### 安装

~~~bash
# 用于在Windows的Go程序中嵌入。ico和manifest资源的工具
go get github.com/akavel/rsrc
# 一个用于Go编程语言的Windows API包装包
go get github.com/lxn/win
# 一个用于Go编程语言的Windows GUI工具包
go get github.com/lxn/walk
~~~

> 使用克隆项目的方式安装

~~~bash
# 在GOPATH/src目录下放置包
cd $GOPATH/src

mkdir -p github.com/akavel
mkdir -p github.com/lxn

cd github.com/akavel
git clone https://github.com/akavel/rsrc.git

cd github.com/lxn
git clone https://github.com/lxn/win.git
git clone https://github.com/lxn/walk.git
~~~

#### Hello World

##### main.go

~~~go
package main

import (
	"github.com/lxn/walk"
	. "github.com/lxn/walk/declarative"
	"strings"
)

func main() {
	var inTE, outTE *walk.TextEdit

	MainWindow{
		Title:   "SCREAMO",
		MinSize: Size{600, 400},
		Layout:  VBox{},
		Children: []Widget{
			HSplitter{
				Children: []Widget{
					TextEdit{AssignTo: &inTE},
					TextEdit{AssignTo: &outTE, ReadOnly: true},
				},
			},
			PushButton{
				Text: "SCREAM",
				OnClicked: func() {
					outTE.SetText(strings.ToUpper(inTE.Text()))
				},
			},
		},
	}.Run()
}
~~~

##### 清单文件

`main.manifest`: 应用程序配置元数据的清单文件。放在main.go的同级目录下，一般不用修改。

~~~xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <assemblyIdentity version="1.0.0.0" processorArchitecture="*" name="SomeFunkyNameHere" type="win32"/>
    <dependency>
        <dependentAssembly>
            <assemblyIdentity type="win32" name="Microsoft.Windows.Common-Controls" version="6.0.0.0" processorArchitecture="*" publicKeyToken="6595b64144ccf1df" language="*"/>
        </dependentAssembly>
    </dependency>
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
            <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">True</dpiAware>
        </windowsSettings>
    </application>
</assembly>
~~~

> 生成main.syso文件

~~~bash
rsrc -manifest main.manifest -o main.syso
~~~

##### 编译运行

~~~bash
# 编译
go build
# 打包
go build -ldflags="-H windowsgui"
~~~

#### 文档

~~~go
import (
    "github.com/lxn/walk"
    . "github.com/lxn/walk/declarative" // 声明式组件
)
~~~

##### 主窗口

~~~go
MainWindow{
    Title: "标题",
    MinSize: Size{600, 400}, // 窗口大小
    Layout:  VBox{}, // 布局
    Children: []Widget{
        HSplitter{ // 一行
            Children: []Widget{ // 两列
                TextEdit{AssignTo: &inTE},
                TextEdit{AssignTo: &outTE, ReadOnly: true},
            },
        },
        PushButton{ // 提交按钮
            Text: "SCREAM",
            OnClicked: func() {
                outTE.SetText(strings.ToUpper(inTE.Text()))
            },
        },
    },
}.Run()
~~~

- 属性
  - Title: "标题" // 窗口标题
  - MinSize: Size{600, 400} // 最小的窗口大小
  - Layout:  VBox{} // 布局
  - Children // 窗口内容 []Widget{} 组件集
  - Visible // 可视化
  - Font // 字体
- 函数
  - Run() // 运行窗口



https://github.com/lxn/walk.git



##### 文本编辑框

```go
var inTE *walk.TextEdit
// AssignTo指派的指针
TextEdit{AssignTo: &inTE}
// 取值
inTE.Text()
```

- 属性
  - AssignTo: &inTE // 指派的指针(*walk.TextEdit)
  - ReadOnly: true // 只读，禁用编辑
  - VScroll: true // 垂直滚动条
  - Background: SolidColorBrush{Color: walk.RGB(255, 191, 0)}, // 背景颜色
  - Margin:     10, // 外边距
- 函数
  - Text() // 取值
  - SetText(text string) // 设值

##### 提交按钮

~~~go
PushButton{
    Text: "提交",
    OnClicked: func() {
        
    }
}
~~~

- 属性
  - Text: "提交" // 按钮名称
  - OnClicked: func(){} // 点击事件

##### 案例总览

- actions：头部功能栏（菜单、多选）
- clipboard：剪贴板
- databinding：表单
- drawing：绘图
- dropfiles：删除文件？？？
- externalwidget：自定义组件
- filebrowser：文件管理器
- gradientcomposite：颜色调节器
- imageicon：icon图片，设置应用图标
- imageview：图片组件
- imageviews：图片选择器
- linklabel：带链接的文字
- listbox：列表框
- listbox_ownerdrawing：列表框，自定义条目
- logview：日志视图
- multiplepages：左侧导航栏
- notifyicon：windows托盘程序
- progressindicator：进程指示器？？？
- radiobutton：单选按钮
- settings：设置app？？？
- slider：滑块
- statusbar：自定义状态栏
- tableview：加载表格
- webview：加载web网页
- webview_events：加载web网页，触发事件

##### 组件总览

- 

~~~
<svg t="1638227933541" class="icon" viewBox="0 0 1049 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2315" width="200" height="200"><path d="M203.299091 17.49311h640.539371a34.98622 34.98622 0 0 1 34.98622 34.986219v26.621598H168.312872V52.479329a34.98622 34.98622 0 0 1 34.986219-34.986219z" fill="#FFFFFF" p-id="2316"></path><path d="M161.66549 54.316106H890.399289a34.98622 34.98622 0 0 1 34.98622 34.98622v26.621597H126.67927V89.302326a34.98622 34.98622 0 0 1 34.98622-34.98622z" fill="#FFFFFF" p-id="2317"></path><path d="M88.923308 934.312826V118.375874c0-12.784548 10.335512-23.184202 23.041341-23.184201h825.65729c12.705829 0 23.041341 10.399654 23.041341 23.184201v758.993964c0 12.784548-10.335512 23.184202-23.041341 23.184202H167.248707l-0.276974 0.11662-78.048425 33.645082z" fill="#FFFFFF" p-id="2318"></path><path d="M111.964649 96.649432c-11.901146 0-21.583582 9.746578-21.583582 21.726442v813.721158l76.013394-32.761679 0.553948-0.239073H937.619024c11.901146 0 21.583582-9.746578 21.583582-21.726442V118.375874c0-11.979865-9.682436-21.726442-21.583582-21.726442H111.964649m0-2.915519h825.65729c13.528005 0 24.4991 11.032321 24.499101 24.641961v758.993964c0 13.609639-10.96818 24.641961-24.499101 24.641961H167.54609L87.465549 936.531535V118.375874c0-13.612555 10.96818-24.644876 24.4991-24.644876z" fill="#D8D8D8" p-id="2319"></path></svg>
~~~



### CGO

####  下载安装

在macOS和Linux下需要安装GCC。

在windows下安装需要安装MinGW工具，设置CGO_ENABLED=1，然后通过import "C"使用。

https://sourceforge.net/projects/mingw-w64/files/mingw-w64/

- 选择Files => MinGW-W64-install.exe
- 安装选择Architecture：x86_64
- 配置系统变量Path，安装目录/bin
- `gcc -v`查看版本

#### 案例

~~~go
package main

import "fmt"
/*
#include <stdio.h>

void printint(int v) {
    printf("printint: %d\n", v);
}
*/
import "C"

func main() {
	fmt.Println("Hello, World!")
	var v = 5
	C.printint(C.int(v))
}
~~~

~~~shell
$ go run demo.go
Hello, World!
printint: 5
~~~















































