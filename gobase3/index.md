# Go语言基础（三）

<!--more-->
## 面向对象
```go
//method.go
package main

import (
	"fmt"
	"math"
)

type Rectangle struct {
	width, height float64
}

type Cicle struct {
	radius float64
}

func (r Rectangle) area() float64 {
	return r.width * r.height
}

func (r Cicle) area() float64 {
	return r.radius * r.radius * math.Pi
}

func main() {
	r := Rectangle{12.4, 12.4}
	c := Cicle{12.4}
	fmt.Println("rectangle is:", r.area())
	fmt.Println("cicle is:", c.area())
}
------------------------
$ go run method.go
rectangle is: 153.76000000000002
cicle is: 483.0512864159667

```

method方法其实就是类似于java中需要实现方法，将不同的类定义传过去。只是在struct中不要声明方法。



```go
//method2.go test method change struct content
package main

import (
	"fmt"
)

const (
	WHITE = iota
	BLACK
	BLUE
	RED
	YELLO
)

type Color byte

type Box struct {
	width, height, depth float64
	color                Color
}

type BoxList []Box

func (b Box) Volume() float64 {
	return b.width * b.height * b.depth
}

//指针类型可直接更改传入类型的值
func (b *Box) SetColor(c Color) {
	b.color = c	//这个地方go已经做优化，不需要显示的作为指针去调用方法
}

func (b1 BoxList) BiggestColor() Color {
	v := 0.0
	k := Color(WHITE)
	for _, b := range b1 {
		if bv := b.Volume(); bv > v {
			v = bv
			k = b.color
		}
	}
	return k
}

func (b1 BoxList) PaintInBlack() {
	for i, _ := range b1 {
		b1[i].SetColor(BLACK)
	}
}

func (c Color) String() string {
	strings := []string{"WHITE", "BLACK", "BLUE", "RED", "YELLOW"}
	return strings[c] //c color是int索引
}

func main() {
	boxes := BoxList{
		Box{10, 1, 4, WHITE},
		Box{4, 4, 4, RED},
	}
	fmt.Println("box`s volume is=", boxes[1].Volume())
	fmt.Println("color of last one is ", boxes[len(boxes)-1].color.String())
}

```

## method方法继承

匿名字段实现的方法可以被包含它的struct调用

```go
type  Student struct{
    age int
    name string
    Human
}
type  Employee struct{
    age int
    name string
    Human
}
type Human struct{
    talk string
}

func (h *Human)SayHi(){	//匿名字段定义的方法
    fmt.Println("i can say hi")
}

func main(){
    student:=Student{Human{"Yes"},12,"leezhu"}
    employee:=Employee{Human{"No"},20,"leezhu"}
    student.SayHi()
    employee.SayHi()	//可以继承匿名字段的方法
}
```


