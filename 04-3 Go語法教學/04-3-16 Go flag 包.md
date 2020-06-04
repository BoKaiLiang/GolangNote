# 04-3-16 Go flag 包 (尚未寫完)

## 前言

* Ref [資料來源](https://www.jianshu.com/p/f9cf46a4de0e)
* 寫命令行程式(工具、server) 時，對命令參數進行解析是常見需求，
	* Go 在標準庫中提供 flag 包，方便進行命令行解析
	* flag 實作了命令行參數的解析。

## flag 包概述

### 例子

```
type group struct {
	name string
	address string
	age string
}

func main() {
	flag.Parse()
	newgroup := group {
		name: flag.Arg(0),
		address: flag.Arg(1),
		age: flag(2),
	}
}
```

### 定義 flags 有兩種方式

* `flag.Xxx()`, 其中 Xxx 可以是 Int, String, Bool 等。返回一個相應類型的指標
	* `var ip = flag.Int("flagname", 1234, "help message for flagname")`
		* 第 1 參數 : flag 名稱為 flagname
		* 第 2 參數 : flagname 預設值 1234
		* 第 3 參數 : flagname 提示信息
		* 返回的 ip 是指標類型，所以這種方式獲取 ip 的值應該 `fmt.Println(*ip)`
* `flag.XxxVar()`, 將 flag 綁定到一個變數上。
	* 下方例子
	```
	var flagValue int
	flag.IntVar(&flagValue, "flagname", 1234, "help message for flagname")
	```
		* 第 1 參數 : 接收 flagname 的實際值
		* 第 2 參數 : flag 名稱為 flagname
		* 第 3 參數 : flagname 預設值 1234
		* 第 4 參數 : flagname 提示信息
		* 這種方式獲取 ip 的值 : `fmt.Println(ip)`
### 自訂義 Value
* `flag.Value` (需求 receiver 是指標) 接口可實現自定義 flag
	* ``
	*flag.Var(&flagVal, "name", "help message for flagname")
* 例子：自訂義 sliceValue, 實現 Value 接口。
	* 透過 `-slice "go,php"` 這樣形式傳遞參數
	* `languages` 得到就是 [go, php]
	* 如果不加 `-slice` 參數則打印默認值 `default is me`
	* flag 中對 `Duration` 這種非基本類型的支持，使用的就是類似這樣的方式，即同樣實現了 `Value` 接口

```
package main
import(
	"flag"
	"fmt"
	"strings"
)

// 定義一個類型 用於增加該類型
type sliceValue []string

// new 一個存放命令行參數值的 slice
func newSliceValue(vals []string, p *[]string) *sliceValue {
	*p = vals
	return (*sliceValue)(p)
}

//// value 介面
//type Value interface {
//	String() string
//	Set(string) error
//}

// 實作 flag 包中的 Value 介面，將命令行接收到值用 ，分隔存到 slice 裡

func (s *sliceValue) Set(val string) error {
	*s = sliceValue(string.Split(val, ","))
	return nil
}

// flag 為 slice 的預設值 default is me, 和 return 返回值沒有關係
func (s *sliceValue) String() string {
	*s = sliceValue(strings.Split("default is me", ","))
	return "It's none of my business"
}

// 可執行文件名 -slice="java,go"  最後將輸出[java,go]
// 可執行文件名 最後將輸出[default is me]
	
func main() {
	var languages []string
	flag.Var(newSliceValue([]string{}, &languages), "slice", "I like programming `languages`")
	flag.Parse()

	// 打印結果 slice 接收到值
	fmt.Println(language)
}
``` 

### 解析 flag

* 所有 flag 定義完成後，可以通過 `flag.Parse()` 進行解析。
* 命令行 flag 的語法有三種型式
	* `-flag`     // 只支持 bool 類型
	* `-flag=x`   
	* `-flag x`   // 只支持非 bool 類型

