# 04-3-08 程式語法 結構 Struct

## 前言提要

* Ref. [資料來源](https://michaelchen.tech/golang-programming/struct/)
* 先前程式都是儲存單一變數值，如果有需要表示複合內容，就要使用結構 (Struct)
	* Go 物件導向程式 也會使用結構
* 匿名 Struct 使用方式

## 使用結構

### 基礎知識

* 使用 `struct` 宣告結構定義成新的型別，
	* 使用 `type` 宣告變數
```
package main

import (
	"fmt"
)

type Point struct {
	x float64
	y float64
}

func main() {
	p := Point {
		x: 3.0,
		y: 4.0
	}

	fmt.Println(p.x)
	fmt.Println(p.y)
}
```

### 匿名結構

* 不用創造新的型別，可以使用 匿名結構
```
package main

import (
	"fmt"
)

func main() {
	p := struct {
		x float64
		y float64
	} {
		x: 3.0
		y: 4.0
	}

	fmt.Println(p.x)
	fmt.Println(p.y)
}
```

### 其他例子

* 使用 結構 比較有效率處理資料
```
package main

import (
	"fmt"
	"math"
)

type Point struct {
	x float64
	y float64
}

func main() {
	p := Point {
		x: 3.0,
		y: 4.0
	}

	q := Point {
		x: 0.0
		y: 0.0
	}

	distance := math.Sqrt(math.Pow(p.x - q.x, 2) + math.Pow(p.y - q.y, 2))
	fmt.Println(distance)
}
```

* 結構屬性 不一定要相同型態
* 下面例子 宣告新型別 `Gender` 再定義常數 `Male` 和 `Female`
	* 使用 iota 自動產生常數
	* 再宣告新型別 `Person` 內有三個屬性
```
package main

import (
	"fmt"
)

type Gender int

// 使用 const 定義內容 當作 enum
const (
	Male Gender = iota
	Female
)

type Person struct {
	name string
	gender Gender
	age uint
}

func main() {
	me := Person {
		gender: Male,
		age: 27,
		name: "Mingo Liu"
	}

	fmt.Println(me.name)
	fmt.Println(me.age)

	if me.gender == Male {
		fmt.Println("Male")
	} else {
		fmt.Println("Female")
	}
}
```

### 結構加入另一個結構

* 下面例子 半徑 r 座標 p
	* p 是另一個結構組成
```
package main

import (
	"fmt"
	"math"
)

type Point struct {
	x float64
	y float64
}

type Circle struct {
	r float64
	p Point
}

func main() {
	c := Circle {
		r: 5.0,
		P: Point {
			x: 3.0,
			y: 4.0
		}
	}

	fmt.Println(c.p.x)
	fmt.Println(c.p.y)

	circumstance := 2 * math.Pi * c.r
	fmt.Println(circumstance)
}
```

## 匿名結構

* Ref. [資料來源](https://javasgl.github.io/go-anonymous-struct/)
* 若臨時需要 struct 封裝資料，而這個 struct 結構在其他地方不會使用，
	* 可以實現 匿名結構
* 主要有兩種方式 
	* 透過 `var` 初始化
	* 直接初始化
```
// 第 1 種
var user struct {name string; age int}
user.name = "Mingo"
user.age = 27

// 地 2 種
json.Marshal(struct{name string; age int}{"Mingo", 27})

// 或
json.Marshal (struct {
	name string
	age int
}{"Mingo", 27})
```
