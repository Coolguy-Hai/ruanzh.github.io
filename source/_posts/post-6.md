---
title: 【Go】Go基础
date: 2021-06-21 09:59:54
tags: Go
---

## Go语言结构

- 包声明
- 引入包
- 函数
- 变量
- 语句&表达式
- 注释

~~~GO
package main
import "fmt"
//与其他语言一样，Go的每个可执行程序都需要一个main函数
func main(){
    fmt.Println("Hello world!")
}
~~~

> 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）。

**{ 不能单独放在一行，以下代码会产生错误**

~~~go
package main

import "fmt"

func main()  
{  // 错误，{ 不能在单独的行上
    fmt.Println("Hello, World!")
}
~~~

## Go语言基础语法

#### 行分隔符

在 Go 程序中，一行代表一个语句结束。每个语句不需要像 C 家族中的其它语言一样以分号 ; 结尾，因为这些工作都将由 Go 编译器自动完成。

如果打算将多个语句写在同一行，它们则必须使用 ; 人为区分。

#### 字符串拼接

Go 语言的字符串可以通过 **+** 实现：

~~~go
package main
import "fmt"
func main(){
    fmt.Println("Google"+"Runoob")
}
~~~

#### 格式化字符串

Go 语言中使用 **fmt.Sprintf** 格式化字符串并赋值给新串：

~~~go
package main
import (
	"fmt"
)
func main(){
    var stockcode=123
    var enddate="2020-12-31"
    var url="Code=%d&endDate=%s"
    var target_url=fmt.Sprintf(url,stockcode,enddate)
    fmt.Println(target_url)
}
/*
output:Code=123&endDate=2020-12-31
*/
~~~

## Go语言变量

声明变量的一般形式是使用 var 关键字。

**变量声明**

第一种，指定变量类型，如果没有初始化，则变量默认为零值。

~~~go
var v_name v_type
v_name = value
~~~

第二种，根据值自行判定变量类型。

~~~go
var v_name = value
~~~

第三种，省略 var, 注意 **:=** 左侧如果没有声明新的变量，就产生编译错误，格式：

~~~go
v_name := v_value
//注意，这种不带声明的格式只能出现在函数体中
//如下形式将会产生编译错误，因为:=左边变量已经声明，无须重新声明
var v_name int
v_name := v_value
~~~

**如果你想要交换两个变量的值，则可以简单地使用 a, b = b, a，两个变量的类型必须是相同。**

## Go语言常量

常量中的数据类型只可以是布尔型、数字型（整数型、浮点型和复数）和字符串型。

常量的定义格式：

~~~go
const identifier [type] = value
~~~

~~~go
package main
import "fmt"
func main(){
    const LENGTH int = 10
    const WIDTH int = 5
    var area int
    area = LENGTH * WIDTH
    fmt.Printf("面积为：%d", area)
}
~~~

**iota**

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中每新增一行常量声明将使 iota 计数一次(iota 可理解为 const 语句块中的行索引)。

iota 可以被用作枚举值：

```go
const (
	a = itoa //0
    b = itoa //1
    c = itoa //2
)
```

~~~go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
    //0 1 2 ha ha 100 100 7 8
}
~~~

```go
package main

import "fmt"
const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
/*
i= 1
j= 6
k= 12
l= 24
*/
```

## Go语言条件语句

*注意：Go 没有三目运算符，所以不支持* **?:** *形式的条件判断。*

## Go语言循环语句

#### for循环

Go 语言的 For 循环有 3 种形式，只有其中的一种使用分号。

**和C的for一样**

~~~go
for init; condition; post { }
~~~

~~~go
package main

import "fmt"

func main() {
        sum := 0
        for i := 0; i <= 10; i++ {
                sum += i
        }
        fmt.Println(sum)
}
~~~

**和C的while一样**

~~~go
for condtion { }
~~~

~~~go
package main

import "fmt"

func main() {
        sum := 1
        for ; sum <= 10; {
                sum += sum
        }
        fmt.Println(sum)

        // 这样写也可以，更像 While 语句形式
        for sum <= 10{
                sum += sum
        }
        fmt.Println(sum)
}
~~~

**和C的for(;;)一样**

~~~go
for { }
~~~

~~~go
package main

import "fmt"

func main() {
        sum := 0
        for {
            sum++ // 无限循环下去
        }
        fmt.Println(sum) // 无法输出
}
~~~

**for-each range 循环**

这种格式的循环可以对字符串、数组、切片等进行迭代输出元素。

~~~go
package main
import "fmt"

func main() {
        strings := []string{"google", "runoob"}
        for i, s := range strings {
                fmt.Println(i, s)
        }


        numbers := [6]int{1, 2, 3, 5}
        for i,x:= range numbers {
                fmt.Printf("第 %d 位 x 的值 = %d\n", i,x)
        }  
}
~~~

## Go语言函数

#### 函数定义

Go 语言函数定义格式如下：

~~~go
func function_name( [parameter list] ) [return_types] {
   函数体
}
~~~

#### 函数返回多个值

~~~go
package main
import "fmt"
func main(){
	str1, str2 := "1", "2"
	fmt.Println(exchange(str1, str2))
}
func exchange(str1, str2 string) (string, string){
	return str2, str1
}
~~~

#### 通过引用传参

实际就是通过指针传参

~~~go
func swap(x *int, y *int){
    var temp int
    temp = *x
    *x = *y
    *y = temp
}
~~~

#### Go 语言函数闭包

~~~go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
//函数对象还未销毁，则其中的变量也仍旧存在
/*
1
2
3
1
2
*/
~~~

## Go语言数组

#### 声明数组

~~~go
var variable_name [SIZE] variable_type
var balance [10] float32
~~~

#### 初始化数组

~~~go
var balance = [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
balance := [5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
~~~

如果数组长度不确定，可以使用 **...** 代替数组的长度，编译器会根据元素个数自行推断数组的长度：

~~~go
var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}
~~~

如果设置了数组的长度，我们还可以通过指定下标来初始化元素：

~~~go
//  将索引为 1 和 3 的元素初始化
balance := [5]float32{1:2.0,3:7.0}
~~~

#### 访问数组元素

~~~go
var salary float32 = balance[9]
~~~

## Go语言指针

Go中指针与C用法相同，通过&取地址，使用*进行解引用。

#### 空指针

当一个指针被定义后没有分配到任何变量时，它的值为 nil。

nil 指针也称为空指针。

nil在概念上和其它语言的null、None、nil、NULL一样，都指代零值或空值。

空指针判断：

~~~go
if(ptr != nil)     /* ptr 不是空指针 */
if(ptr == nil)    /* ptr 是空指针 */
~~~

## Go语言结构体

#### 定义结构体

~~~go
type struct_variable_type struct {
   member definition
   member definition
   ...
   member definition
}
~~~

结构体声明：

~~~go
variable_name := structure_variable_type {value1, value2...valuen}
或
variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}
~~~

#### 访问结构体成员

如果要访问结构体成员，需要使用点号 **.** 操作符。

#### 结构体指针

~~~go
var struct_pointer *Books
struct_pointer = &Book1
//结构体指针也是通过 . 访问成员
struct_pointer.title
~~~

## Go语言切片

Go 语言切片是对数组的抽象。

Go 数组的长度不可改变，在特定场景中这样的集合就不太适用，Go 中提供了一种灵活，功能强悍的内置类型切片("动态数组")，与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

#### 定义切片

~~~go
var identifier []type
//使用make()创建切片
var slice1 []type = make([]type, len)
//简写为 slice1 := make([]type, len)
~~~

#### 切片初始化

~~~go
s := [] int{1,2,3}
s := arr[:]
s := arr[startIndex:endIndex]
s := arr[startIndex:]
s := arr[:endIndex]
~~~

#### len()函数和cap()函数

切片是可索引的，并且可以由 len() 方法获取长度。

切片提供了计算容量的方法 cap() 可以测量切片最长可以达到多少。

~~~go
package main
import "fmt"
func main(){
    var slice1 = make([]int, 3, 5)
    printSlice(slice1)
}
func printSlice(x []int){
    fmt.Printf("len:%d, cap:%d", len(x), cap(x))
}
~~~

#### append() 和 copy() 函数

如果想增加切片的容量，我们必须创建一个新的更大的切片并把原分片的内容都拷贝过来。

下面的代码描述了从拷贝切片的 copy 方法和向切片追加新元素的 append 方法。

~~~go
package main
import "fmt"
func main(){
    var numbers []int
    printSlice(numbers)
    
    numbers = append(numbers, 0)
    printSlice(numbers)
    
    numbers = append(numbers, 1)
    printSlice(numbers)
    
    numbers = append(numbers, 2,3,4)
    printSlice(numbers)
    
    number1 := make([]int, len(numbers), 2*cap(numbers))
    copy(numbers1, numbers)
    
    printSlice(numbers1)
}
func printSlice(x []int){
    fmt.Printf("len=%d, cap=%d, slice=%v\n", len(x), cap(x), x)
}
~~~

## Go语言范围（Range）

Go 语言中 range 关键字用于 for 循环中迭代数组(array)、切片(slice)、通道(channel)或集合(map)的元素。在数组和切片中它返回元素的索引和索引对应的值，在集合中返回 key-value 对。

~~~go
package main
import "fmt"
func main(){
    nums := []int{2,3,4}
    sum := 0
    for _, num := range nums{
        sum += num
    }
    fmt.Println("Sum:", sum)
    for i, num := range nums{
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    kvs := map[string]string{"a":"apple", "b":"banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s", k, v)
    }
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
~~~

## Go语言Map（集合）

Map 是一种无序的键值对的集合。Map 最重要的一点是通过 key 来快速检索数据，key 类似于索引，指向数据的值。

Map 是一种集合，所以我们可以像迭代数组和切片那样迭代它。不过，Map 是无序的，我们无法决定它的返回顺序，这是因为 Map 是使用 hash 表来实现的。

#### 定义map

~~~go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
~~~

~~~go
package main
import "fmt"
func main(){
    var countryCapitalMap map[string]string
    countryCapitalMap = make(map[string][string])
    
    countryCapitalMap [ "France" ] = "巴黎"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"
    
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }
    capital, ok = countryCapitalMap ["American"]
    if (ok) {
        
    } else {
        
    }
}
~~~

#### delete()函数

delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key。实例如下：

~~~go
package main
import "fmt"
func main(){
    countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}
    fmt.Println("原始地图")
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }
    delete(countryCapitalMap, "France")
	fmt.Println("法国条目被删除")
	fmt.Println("删除元素后地图")
    for country := range countryCapitalMap {
    	fmt.Println(country, "首都是", countryCapitalMap [country])
    }
}
~~~

## Go语言递归函数

#### 阶乘

~~~go
package main
import "fmt"
func Factorial(n uint64)(result uint64){
    if(n > 0) {
        result = n * Factorial(n-1)
        return result
    }
    return 1
}
func main(){
    var i int = 5
    fmt.Printf("%d的阶乘为：%d\n", i, Factorial(uint64(i)))
}
~~~

## Go语言类型转换

类型转换用于将一种数据类型的变量转换为另外一种类型的变量。Go 语言类型转换基本格式如下：

~~~go
type_name(expression)
~~~

~~~go
package main

import "fmt"

func main() {
   var sum int = 17
   var count int = 5
   var mean float32
   
   mean = float32(sum)/float32(count)
   fmt.Printf("mean 的值为: %f\n",mean)
}
~~~

## Go并发

Go 语言支持并发，我们只需要通过 go 关键字来开启 goroutine 即可。

goroutine 是轻量级线程，goroutine 的调度是由 Golang 运行时进行管理的。

goroutine 语法格式：

~~~go
go 函数名( 参数列表 )
go f(x, y, z)
//开启一个线程
f(x, y, z)
~~~

~~~go
package main
import (
	"fmt"
    "time"
)
func say(s string){
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}
func main(){
    go say("world")
    say("hello")
}
~~~

#### 通道（Channel）

通道（channel）是用来传递数据的一个数据结构。

通道可用于两个 goroutine 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，发送或接收。如果未指定方向，则为双向通道。

~~~go
ch <- v    // 把 v 发送到通道 ch
v := <-ch  // 从 ch 接收数据
           // 并把值赋给 v
~~~

声明一个通道很简单，我们使用chan关键字即可，通道在使用前必须先创建：

~~~go
ch := make(chan int)
~~~

**注意**：默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须有接收端相应的接收数据。

以下实例通过两个 goroutine 来计算数字之和，在 goroutine 完成计算后，它会计算两个结果的和：

~~~go
package main
import "fmt"
func sum(s []int, c chan int){
    sum := 0
    for _, num := range s {
        sum += num
    }
    c <- sum
}
func main(){
    s := []int{1,2,3,4,5,6,7,8,9,10}
    c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y = <-c, <-c
    fmt.Println(x, y, x+y)
}
~~~

#### 通道缓冲区

通道可以设置缓冲区，通过 make 的第二个参数指定缓冲区大小：

~~~go
ch = make(chan int, 100)
~~~



带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于异步状态，就是说发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据。

不过由于缓冲区的大小是有限的，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

**注意**：如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞。

#### Go 遍历通道与关闭通道

Go 通过 range 关键字来实现遍历读取到的数据，类似于与数组或切片。格式如下：

~~~go
v, ok := <-ch
~~~

~~~go
package main
import "fmt"
func fibonacci(n int, c chan int){
    x, y= 0, 1
    for i := 0; i < n; i++ {
        c <- x
        x, y = y, x+y
    }
    close(c)
}
func main(){
    c := make(chan int, 10)
    go fibonacci(cap(c), c)
    for i := range c {
        fmt.Println(i)
    }
}
~~~

