# 04-3-21 Go strconv 包 (未完成)

* Ref. [資料來源](https://www.itread01.com/content/1540663406.html)

## 數據類型轉換 strconv 包

* 簡單轉換操作 `valueOfTypeB = typeB(valueofTypeA)`
	* 例如：
```
// 浮點數
a := 5.0

// 轉換為 int 類型
b := int(a)
```

* Go 允許在底層結構相同的兩個類型之間互轉。
```
// IT 類型的底層是 int 類型
type IT int

// a 的類型是 IT 底層是 int
var a IT = 5

// 將 a(IT) 轉換成 int,  b 現在是 int 類型
b := int(5)

// 將 b(int) 轉換為 IT,  c 現在是 IT 類型
c := IT(b)
```

* 注意
	* 不是所有資料類型都能轉換，string 類型 "abcd" 轉換成 int 肯定失敗
	* 高精度值 轉換 低精度 會丟失精度
	* 這種簡單轉換方式，不能對 int(float) 和 string 進行互換，
		* 要跨大類型轉換，可以使用 strconv 包

## strconv

* `strconv` 包提供了簡單數據類型 之間的類型轉換功能。
	* 可以將簡單類型轉換成 字串，
	* 也可以將 字串轉換成其他簡單類型
* 提供很多函數，大概分成幾類：
	* string 轉 int : `Atoi()`
	* int 轉 string : `itoa()`
	* ParseTP 類函數將 string 轉換成 TP 類型：`ParseBool(), ParseFloat(), ParseInt(), ParseUint()`
		* 因為 string 轉其他類型 可能會失敗，所以這些函數都有第二個返回值 表示是否轉換成功。
	* 其他類型轉 string 類型：`FormatBool(), FormatFloat(), FormatInt(), FormatUint()`
	* AppendTP類函數 用於將 TP 轉換成字串後 append 到一個 slice 中：`AppendBool(), AppendFloat(), AppendInt(), AppendUint()`
* 轉型會報錯誤有兩種
	* `var ErrRange = errors.New("value out of range")`
	* `var ErrSyntax = errors.New("invalid syntax)`

### string 和 int 的轉換

* int 轉換為 string : `Itoa()`
	* `println("a" + strconv.Itoa(32))          // a32`
* string 轉換為 int : `Atoi()`
	* `func Atoi(s string) (int, error)`
		* 第一個返回值 是轉成 int
		* 第二個返回值 判斷是否轉換成功
		```
		// Atoi() : string > int
		i, _ := strconv.Atoi("3")
		Println(3 + i)       // 6

		// Atoi() 轉換失敗
		i, werr := strconv.Atoi("a")
		if err != nill {
			println("converted failed")
		}
		```

### Parse 類函數

* 用於轉換字串給定類型的值
	* `ParseBool()`
	* `ParseFloat()`
	* `ParseInt()`
	* `ParseUint()`


### Format 類函數

* 將給定類型格式化為 string 類型：
	* `FormatBool()`
	* `FormatFloat()`
	* `FormatInt()`
	* `FormatUint()`

* formatFloat() 參數眾多 `func FormatFloat(f float64, fmt byte, prec, bitSize int) string`
	* bitSize 轉換成多少為 (32 或 64) 的浮點數對應的字串 
	* `strconv.FormatFloat(e, 'f', -2, 64)`
	
### Append 類函數

* AppendTP類函數 用於將 TP 轉換成字串後 append 到一個 slice 中：
	* `AppendBool()`
	* `AppendFloat()`
	* `AppendInt()`
	* `AppendUint()`