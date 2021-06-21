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

