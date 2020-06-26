# Go语言基础（二）

<!--more-->
## 流程相关

### 1、for

for在很多编程语言都是有的，golang的for循环与c有点不太一样，倒像是结合了c与python的特性。

```go
for k,v:=range array{
    
}
for i:=0;i>10,i++{
    //i只在此函数内生效
}
sum:=100
for sum<200{
    //可以当作while来使用，
}
```

### 2、if

其实if语言没有什么过多想讲的，但是golang的if有一个特性需要注意一下

```go
if i:=1;i>10{
    //在条件表达式里面可以短变量赋值。所以一般可以直接将需要判断的函数放在表达式里面。
}
if x:=myFunc();x>10{
    
}
```

### 3、switch

switch主要用于判断条件简单的，明确的，切分支多的。switch在每个分支里面找到了是自带break的，如果没有找到则继续往下走。

```go
sum:=2
swith sum{
    case 1:
    	fmt.Println('found',sum)
    case 2,3,4:
    	fmt.Println("found")
    default:
    	fmt.Println("No found")
}
```



### 4、break、continue、goto

似乎每个编程语言都离开不开前两者，goto由于容易破坏语言的流程结构在c语言中被用的很谨慎，当然在go中也是如此，不到万不得已也别使用，不然很难维护，代码可读性也非常差。

```
for i:=1;i>10;i++{
    if i==2{
        break/continue
    }
}

flag:
fmt.Println("goto here)
for i:=1;i<10;i--{
    if i==2{
        goto flag
    }
}
```

## 函数相关

go函数是以关键字`func`标明的。一般格式是`func myFunc(a int,b string)(int,string){}`

可返回多参数内容。例如：

```go
func findMax(a,b int)int{	//返回一个参数内容
    if a>b{
        return a
    }
    return b
}
func findMaxAndMin(a int,b int)(max int,min int){
    if a>b{	//有两个返回值
        return a,b
    }
    return b,a
}
//一般多返回值我们会用在其中一个返回错误标记位
//返回值虽然命名了类型，也还要命名下参数名，这样利于代码可读性

//变参
func variArgs(varibel ...int){
    //变参指的是参数的个数是无法确定的。我们编译程序的时候传入的参数就是一个slice多参数。还有就是在go基础一中数组初始化时的多元素个数也是用...来标示的。
}
```

### 1、传值与传引用

官方说法都采用指针，似乎指针听起来更理解，引用就是对别人的链接，相当于自身并不存有对方的值，而只是可以得到对方值的路径。之前在基础1中我就讲到slice和map都是底层数据类型的引用，相当于在底层结构上面裹了一层，高级数据结构就是这么来的，一层层裹，特性会越来越多，兼容性也越来越强。

现在我们要讲的是函数传参的时候，是传引用还是传自身的值，还是拷贝呢？

首先我么看一个例子：

```go
//简写我略去了包声明和引入包
func changeValue(a int)int{
	a:=5
    return a
}
value:=6
changeValue(value)
---------
结果：value=6
```

这是因为我们传入的a是value的副本(copy)，因此就算在函数内更改了a的值也影响不到value本身。如果要在函数内改变传入的值，那么就得用引用类型。

```go
func changeValue(a *int)int{
    *a:=*a+1
    return *a
}
value:=6
changeValue(&value)
----------
结果：value=7
```

可以看到，通过引用我们改变了value值。可以这么理解，引用链接放到哪里都能找到原有值，但是传值只是传个副本，影响不到原有值本身。而且传引用还可以节省内存开销，但是也有弊端就是需要谨慎用。

### 2、defer

defer是延迟操作，类似于python的finally，就是说无论如何都是会执行此语句。只不过defer是在函数内推出时。它是一个先进后出的操作。

```go
func readFile()bool{
    file.open("file.txt")
    defer file.close()
    file... operation//各种操作
    //但是一旦出现操作错误前，最终都会执行defer的语句。这种就能保证文件能够正常的关闭。
}
//可以试下以下程序,观察现象
func myPrint(){
    for i:=0;i<5;i++{
        defer fmt.Printf("%d",i)
    }
}
---------
结果：
9
8
7
6
5
4
3
2
1
//这是因为，第一个defer是不输出值的，然后进入了第二个defer,到达最后一个的时候。开始准备退出for，这个时候就开始后进先出了
```

### 3、函数作为类型

`type myFunc func(int)bool`

可用于实现多态，例如：

```go
type myFunc func(int)bool
func isOdd(value int)bool{
    if value %2!=0{
        return true
    }
    return false
}
func isEven(value int)bool{
    if value%2==0{
        return true
    }
    return false
}
//check函数可以把myFunc当作类型传入
func check(a int,f myFunc)bool{
    return f(a)
}
```

## main和init函数

### 1、main函数

main函数是一个程序的入口，不需要自己调用。package main包必须只能包含一个main函数入口。

### 2、init函数

go语言中init函数用于包(package)的初始化，该函数是go语言的一个重要特性，

有下面的特征：

1. init函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等

2. 每个包可以拥有多个init函数

3. 包的每个源文件也可以拥有多个init函数

4. 同一个包中多个init函数的执行顺序go语言没有明确的定义(说明)

5. 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序

6. init函数不能被其他函数调用，而是在main函数执行之前，自动被调用
7. 当出现嵌套包的情况，会把所有根源的包都导入进来。例如：a1导入包b1,但b1包倒入c1，这样一级一级的把所有包都导入进来

**但是使用init函数也有以下几个原则**：

1. 一个package或者是go文件可以包含多个init函数
2.  init函数是在main函数之前执行的
3. init函数被自动调用，不能在其他函数中显示调用

```go
package main

import (
    "fmt"
)

func main(){
    fmt.Println("do main")
}

func init() {
    fmt.Println("do in init1")
}

func init() {
    fmt.Println("do in init2")
}
-----------
结果：do in init1
	do in init2
	do main
	
```

可以看到上面代码执行的顺序，init是先于main执行的。一个包里面可以有多个init函数。

## 模块导入import

模块导入有两种方式，一种是`import "fmt"` 绝对路径，另外一种是相对路径。

### 1、点操作

`import . "fmt"`这样导入后，在代码中fmt.Println可以写成Println。这是将导入符略去了，但是不建议大家这样，如果出现多个模块中相同的函数，那么会有冲突。

### 2、别名替换

可以给要导入的模块加上别名，这有点类似于python中as的做法。`import  f "fmt"`，所以可以缩写成`f.Println()`

### 3、单纯导入操作

有些时候你只是想用包里面的初始变量名，并不需要用到函数。这个时候可以用到我们之前在基础一中说到的`_`下划线占位符，可以用下划线当作别名占位。`import _ "fmt"`

## struct类型

struct类型和c/c++里面的struct结构体有点类似，但是定义和声明的方式还有些不同。我们可以定义申明struct如下：

```go
type person struct{	//定义
    name string
    age int
}
//初始化方式1
var Person1 person
Person1.name="leezhu"
Person1.age=10
fmt.Printf("name is %s,age=%d",Person1.name,Person1.age)

//初始化方式2---顺序初始化
Person2:=person{"leezhu",10}

//初始化方式3---通过k:v方式初始化,任意顺序
Person3:=person{name:"leezhu",age:10}

//初始化方式3--初始化*Person4 指针类型
Person4:=new(person)
*Person4.name="leezhu"
*Person4.age=10
```

以上是struct类型的一般运用，但是struct也有高级应用，例如struct匿名结构。

```go
type person struct{
    name string
    age int
    human	//匿名对象，就是包含另外一个结构体,嵌入字段，不写类型
}

type human struct{
    eat string
    talk string
    age int 
}

var Person5 person
person.human=human{eat:"noodle",talk:"chinese"}

Person6:=person{"leezhu",10,human{"noodle","chinese"}}

//访问字段，可以当作当前类型直接使用,这是因为匿名字段是实现了字段的继承。
fmt.Println("eat =",Person5.eat)

//有人注意到human 和student都有age，那当Person5.age是访问哪一个呢？Go里面很简单的解决了这个问题，最外层的优先访问，也就是访问student里面的age
```




