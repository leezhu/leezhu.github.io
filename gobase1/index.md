# Go语言基础（一）

<!--more-->
## 关键字

```go
break    default      func    interface    select
case     defer        go      map          struct
chan     else         goto    package      switch
const    fallthrough  if      range        type
continue for          import  return       var
```

*注意*:关键字不可以使用

## 变量声明

### 1、一般声明

​	1、显示声明：var variable int ,变量名是在后面

​	2、隐式声明：var variable = 1, 没有类型

​	3、简短声明：varible:=1	.

​	4、多变量声明：varible1,varible2:=1,2,

​	5、_,varible:=1,2. 下划线 _一般用于占位符，不进行使用，可以用于索引等

### 2、分组声明

```go
const (
	i=100
	j=200
)
var (
	a=100
	b=200
)
```

**注意：**go语言是可以不声明类型直接用，但是简短声明是必须在函数内生效，不可用于包内。go和python 有很多类似的地方。分组声明针对于同一种类型的变量可以放在一起声明

## 常量

​	**const**:是常量的标志，可以定义为多个类型，例如：

​	1.const constName =value

​	2.const pi float=3.14

​	3.const Max="prefix" 

## 变量类型

​	1、Boolean类型：true/false

​	2、rune/int/int8/int16/int32/...等等，具体可以百度，会根据编译器的32/64位来决定长度。int和int32类型长度一致，但是不可通用

​	3、float32/float64:无float类型

​	4、string:字符型。例如：var varible="1233",或者多行数据用反斜引号。var hello =\`wowoowowo\`

## 变量可用性



- 变量命名规则遵循驼峰原则

- 大写字母开头的变量/函数名是`public`，可以被其他包直接用。只能在当前包可用

- 小写字母开头的变量/函数名是`private`,不可被其他包直接用

  ------

## 高级数据结构

### 1、Array数组

`var varible [10]int`,array长度不可变，默认赋值是0

数组初始化：`var varible[10]int`

​			`varible[0]=1`

​			`a:=[3]int{1,2,3}//指定个数初始化`

​			`a:=[...]int{1,2,3,4,5}//可自动计算元素个数`

多维数组初始化：

```go
doubleArray:=[2][4]int{[4]int{1,2,3,4},[3]int{1,2,3}}
doubleArray:=[2][4]int{{1,2,3},{3,4,5}}
```

### 2、slice-动态数组

​	**声明**：`var flslice[]int`可以没有长度。底层实现还是array,只是是引用而已。

​     **声明并初始化数据**：flslice:=[]int{1,2,3,4,5}

​     **取值**：flslice[i:j],遵循左闭右开的原则，即[1，2)这样。

​     **切片**：flslice[1:2],flslice[2:3]

*提示：*slice和数组切片操作可参照python的list操作，基本类似

### 3、map-映射

map与python的dict是类似的，k-v结构，key可以是int或者string等类型。map和slice一样都是引用类型，没有长度限制。make函数用于内建类型（map, slice,channel）进行内存分配

​	**声明：**`var numbers map[string] int`,key是string,value是int类型

​     		`numbers:=make(map [string]int)`

​	    `numbers['1']=2 //直接赋值`

​     **初始化：**`rating:=map [string]float32{"c":23,"b":2}`	

​     **安全取值：**

```go
value,ok := rating['c']
if ok{
    fmt.Println('have found ',value)
}else{
    fmt.Println('not found')
}
```

### 4、make与new的区别

- make是指定slice、map、channal这三种类型分配内容，返回的是类型的零值
- new是建立内存，返回的是指针。

### 5、零值

是类型变量填充前的默认值，`int类型`的都是0，字节或者16进制值会有点不一样。布尔型是false,string类型是"".

*注意*：引用类型会被另外一个引用改变，想想可不可以加const就不会被改变了。








