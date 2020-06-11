# 04-3-09 程式語法 指標 Pointer

## 前言提要

* Ref. [資料來源](https://michaelchen.tech/golang-programming/pointer/)
* Go 支援指標 (Pointer), 且較簡化，不僅沒有指標運算 也不需要手動控制記憶體釋放

## 建立指標

### 基礎知識

* 指標本身的值 就是 指向另一個值得記憶體位置 (Memory Address)
	* 通常不會直接使用 指標的值
	* 而會透過指標間接操作 另一個值
```
package main

import (
	"fmt"
	"log"
)

func main() {
	n := 2
	
	// 參考 變數記憶體位置
	nPtr := &n

	fmt.Println(nPtr)

	if !(*nPtr == 2) {
		log.Fatal ("Wrong value")
	}
}
```

* Ref. 書 <Go 語言核心編程>
```
var a = [...]int{1, 2, 3}
var b = &a
fmt.Println(a[0], a[1])
fmt.Println(b[0], b[1])

for i, v := range b {
	fmt.Println(i, v)
}
```

### 動態配置記憶體

```
package main

improt (
	"fmt"
	"log"
)

func main() {
	// 配置一段記憶體
	nPtr := new(int)

	// 直接配值
	*nPtr = 2

	fmt.Println(nPtr)

	if !(*nPtr == 2) {
		log.Fatal("Wrong value")
	}
}
```

* 以下例子 比較少對基礎型別 進行動態記憶體配子，但可以對結構等複合型別 動態配置記憶體
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
	p := new(Point)

	p.x = 3.0
	p.y = 4.0

	fmt.Println(fmt.Sprintf("(%.2f, %.2f)", p.x, p.y))
}
```

* `make` 函式用來動態份配記憶體的語法，
	* 但是 `make` 回傳的不是指標 而是型別本身
	* Go 語言中 不需要使用指標語法操作陣列、切片、映射 Map

## 結論

* 在 Go 中，指標除了用來和 C函式庫互動外，
	* 當變數較大，傳遞指標 較 傳遞整個變數有效率的多