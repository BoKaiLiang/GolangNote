# 04-3-12 程式語法 Interface

## 前言提要

* Go 缺乏繼承機制，無法透過繼承來達到多型效果，Go 引入介面的機制

## 基礎 Interface

* 介面 (Interface) 只有方法宣告，但缺乏方法實作的型別。
* 以下例子，建立一個以 IPoint 為型別的切片 `points` 並分別加入兩個相異的變數 `p1` 和 `p2`
	* 由於這兩個變數都符合宣告的介面，不會引發程式錯誤，
	* 最後 分別呼叫 兩個變數 `X` 和 `Y` 方法，印出點 所在位置
```
package main

import (
	"fmt"
)

type IPoint interface {
	X() float64
	Y() float64
	SetX(float64)
	SetY(float64)
}

type point struct {
	x float64
	y float64
}

func NewPoint(x float64, y float64) *Point {
	p := new(Point)
	
	p.SetX(x)
	p.SetY(y)

	return P
}

func (p *Point) X() float64 {
	return p.x
} 

func (p *Point) Y() float64 {
	return p.y
}

func (p *Point) SetX(x float64) {
	p.x = x
}

func (p *Point) SetY(y float64) {
	p.y = y
}

// 可以 struct 包 struct
type Point3D struct {
	Point
	z float64
}

func NewPoint3D(x float64, y float64, z float64) *Point3D {
	p := new(Point3D)

	p.SetX(x)
	p.SetY(y)
	p.SetZ(z)

	return p
}

func (p *Point3D) Z() float64 {
	return p.z
}

func (p *Point3D) SetZ(z float64) {
	p.z = z
}

func main() {
	// make a slice of IPoint
	points := make([]IPoint, 0)
	
	p1 := NewPoint(3, 4)
	p2 := NewPoint3D(1, 2, 3)

	// No error !
	points = append(points, p1, p2)

	for _, p := range points {
		fmt.Println(fmt.Sprintf("%.2f %.2f"), p.GetX(), p.GetY())
	}
}
```

## 在介面中嵌入介面

* 下面例子 IPoint3D 繼承 IPoint 所有方法宣告外，也加入自己特有的 `Z` 及 `SetZ`z 方法，
	* 嵌入式介面用來繼承介面的方法。
```
type IPoint interface {
	X() float64
	Y() float64
	SetX(float64)
	SetY(float64)
}

type IPoint3D interface {
	IPoint
	Z() float64
	SetZ(float64)
}
```

## 介面可以滿足多型的需求

* 介面是在沒有繼承，卻能實作子類型 (Subtyping) 的手法。'
	* 在 Java 或 C# 等語言 撰寫 物件導向程式 或 設計模式的書，會大量使用 介面來撰寫 易於維護的程式，反而少用繼承。
	* 以設計模式中 `Builder` 模式中，建立一群相關的物件，將建立過程集中在 同一個 Builder 函式中，使程式易於維護，如下：
```
package main

import (
	"fmt"
)

type IAnimal interface {
	Speak()
}

type AnimalType int

const (
	Duck AnimalType = iota
	Dog
	Tiger
)

type DuckClass struct {}

func NewDuck() *DuckClass {
	return new(DuckClass)
}

func (d *DuckClass) Speak() {
	fmt.Println("Pack pack")
}


type DogClass struct {}

func NewDog() *DogClass {
	return new(DogClass)
}

func (d *DogClass) Speak() {
	fmt.Println("Wow Wow")
}


type TigerClass struct {}

func NewTiger() *TigerClass {
	return new(TigerClass)
}

func (t *TigerClass) Speak() {
	fmt.Println("Halum Halum")
}


func New(t AnimalType) IAnimal {
	switch t {
	case Duck:
		return NewDuck()
	case Dog:
		return NewDog()
	case Tiger:
		return NewTiger()
	default:
		panic("Unknown animal type")
	}
}

func main() {
	animals := make([]IAnimal, 0)

	duck := New(Duck)
	dog := New(Dog)
	tiger := New(Tiger)

	animals = append(animals, duck, dog, tiger)

	for _, a := range animals {
		a.Speak()
	}
}
```

## Golang 中隱藏版的運算子重載

* Overloading 是指物件可以用內建運算子進行操作 => Go 不支援運算子重載
	* 少數是 `String` 函式 即可用 `fmt` 套件內的函式 將物件印出。
	* 以下例子
```
package main

import (
	"fmt"
	"strconv"
)

type Vector []float64

func NewVector(args ...float64) Vector {
	return args
}

func (v Vector) String() string {
	out := "("

	for i, e := range v {
		out  += strconv.FormatFloat(e, 'f', -2, 64)
		if i < len(v) - 1 {
			out += ", "
		}
	}

	out += ")"

	return out
}

func main() {
	v := NewVector(1, 2, 3, 4, 5)

	fmat.Println(v)
}
```

## 用空介面當成萬用型別

* 沒有任何方法宣告的空介面 寫作 `interface {}`
* 空介面也是一種特殊型別，可以放入任何值
* 空介面 和 介面使用時間不同，視為不同概念
* 以下例子 可以編譯
```
package main

import (
	"fmt"
)

func main() {
	var v interface {}

	// v is an integer
	v = 1
	fmt.Println(v)

	// v becomes a string now
	v = "Hello"
	fmt.Println(v)

	// v become a boolean value now
	v = true
	fmt.Println(v)
}
```

* 以下例子 未做 型別判定(type assertion) 會出錯
```
package main

import (
	"fmt"
)

// 錯誤
func main() {
	var m interface{}
	var n interface{}

	m = 3
	n = 2

	// Error
	fmt.Println(m + n)
}


// 正確
func main() {
	var im interface{}
	var in interface{}

	im = 3
	in = 2

	// 型別判定 (type assertion)
	m := im.(int)
	n := in.(int)

	fmt.Println(m + n)
}

// 正確
func main() {
	var im interface{}
	var in interface{}

	im = 3
	in = 2

	// 型別判定 (type assertion)
	m, ok := im.(int)
	if !ok {
		log.Fatal("Wrong type")
	}

	n, ok := in.(int)
	if !ok {
		log.Fatal("Wrong type")
	}

	fmt.Println(m + n)
}

// 若無法預先知道變數型別，可用 `switch` 敘述進行動態判定
func main() {
	var t interface {}
	t = 3
	switch t := t.(type) {
		default:
			fmt.Printf("unexpected type %T\n", t)
		case bool:
			fmt.Printf("boolean %t\n", t)
		case int:
			fmt.Printf("integer %d\n", t)
		case *bool:
			fmt.Printf("pointer to boolean %t\n", *t)
		case *int:
			fmt.Printf("pointer to integer %d\n", *t)
	}
}
```

* Go 標準函式庫也使用 空介面，在 `fmt` 套件的 `Printf` 及 `Sprintf` 函式中，以下是實例
```
package main 

import (
	"fmt"
)

func main() {
	fmt.Println(fmt.Sprintf("%d %s %t", 1, "Hello", true))
}
```