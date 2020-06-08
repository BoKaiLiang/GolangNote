# 04-4-03 Go Debug

## 前言提要

* delve 套件
* gdb 套件

## 使用 delve 除錯 Golang 程式

* Ref. [參考資料](https://www.jishuwen.com/d/2nCv/zh-tw)
* delve 是一款專門針對 Golang 程式除錯而開發的命令列偵錯程式，該工具功能強大，簡易使用。
* Window 環境使用

* 安裝步驟如下
	1. 使用 `go get -u github.com/go-delve/delve/cmd/dlv` 安裝套件
	2. 將 $GOPATH/bin 新增至 環境變數 PATH，即可任意目錄使用 dlv
	3. 主要除錯指令使用
		* `dlv help` 可以看到 dlv 幫助文件。
		* `dlv debug` 可以在 main 函式檔案 所在的資料夾，直接對 main 函式進行除錯，也可以在根目錄以指定包路徑的方式對 main 函式進行除錯。
		* `dlv test` 對 test 包進行除錯
		* `dlv attach` 附加到一個已在執行的程序進行除錯
		* `dlv connect` 可以連線到 debug server 進行除錯
		* `dlv trace` 可以追蹤程式
		* `dlv exec` 可以對編譯好的 二進位制進行除錯
	4. 常用除錯命令
		```
		package main

		import (
	    	"fmt"
	
    		"github.com/golang/example/stringutil"
			)

		func main() {
    		fmt.Println(stringutil.Reverse("!selpmaxe oG ,olleH"))
		}
	
    	// 目標設定斷點1 : main.main()
		// 目標設定斷點2 : stringutil.Reverse()
		```

		* `b main.main` 使用 break 或 b 對 main.main 設定斷點。此時顯示 `Breakpoint 1 set at 0x4af2aa for main.main() E:/go/src/XXX/main.go:25`
		* `c` 使用 continue 或 c 進入斷點 (似 VS F10)
		* `n` 使用 next 或 n 移至下一步 (似 VS F5)
		* `s` 使用 step 或 s 進入函式 (似 VS F11)
		* `b 24` 指定行 設定斷點 在 輸入 `c` 進入斷點
		* `p i` 列印變數，列印 i 變數的數值
		* `locals` 檢視區域變數
		* `bp` 使用 breakpoint 或 bp 檢視所有斷點
		* `on 2 print i` 對於迴圈，想檢視區域性變數值，如果 i 值，每次都 print 較為麻煩，這時可以使用 on 對斷點設定一個執行指令碼。可以在執行 `c` continue 再次進入該段點
		* `cond 2 5==i` 在迴圈中除錯 可以指定條件 使用 cond 設定斷點。再次執行 `c` 直接走到該條件觸發。
		* `stepout` 跳出當前函式 回到上層函式
		* `clear 2` 使用 clear 清除指定斷點
		* `r` 使用 restart 或 r 重新進入除錯。若想重新進入一次新的除錯，無須退出程式再次執行 `dlv debug`

## 使用 gdb 除錯 Golang 程式

* Ref. [參考資料](https://studygolang.com/articles/29168)