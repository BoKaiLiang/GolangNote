# 04-4-02 Go Test 測試

## GO 測試套件

* Ref. [資料來源](https://openhome.cc/Gossip/Go/Testing.html)
* Go 本身附帶了 testing 套件，搭配 `go test` 指令，可以自動對套件中的程式碼進行測試。
	* 在套件中，測試程式碼必須是 XXXX_test.go 結尾
	* 一個套件中可以有多個 XXXX_test.go
	* 例如 [官方 fmt 套件原始碼](https://golang.org/src/fmt/) 中，可以看到 export_test.go、fmt_test.go 等都是 測試程式碼
* 功能測試
	* 在 src/myTest 目錄下 寫個 Oooo_test.go
	* 在 Oooo_test.go 中需要 `import "testing"` 
	* 撰寫 `func TestXxxx(t *testing.T)` 的函式
		* Xxxx 任意名稱
	* 在 myTest 目錄下，執行 `go test` 會自動尋找所有 Oooo_test.go 裡的 Test 開頭函式並執行
	```
	package mytest

	improt "testing"

	func TestSomething(t *testing.T) {
		t.Skip()
	}

	func TestAdd(t *testing.T) {
    	if Add(1, 2) == 3 {
   	    	t.Log("mymath.Add PASS")
    	} else {
    	    t.Error("mymath.Add FAIL")
    	}
	}
	```
	
	> $ go test <br>
	> ok      mytest   0.189s
	
	* 如果想看到 Log 訊息需要 加上 `-v` 代表 verbose
		* `go test -v`
	* 若 TestXxxx 函式中使用 testing 的 Error、Fail 等與失敗相關的方法，測試就會失敗。
	* 想留下些訊息，可使用 Error 方法。
	* 略過測試，可使用 Skip
	```
	func TestSomething(t *testing.T) {
		t.Fail()                    // 直接失敗
		t.Error("something wrong")  // 留下錯誤訊息 => 先 Log 再 Fail 的意義
		t.Skip()                    // 直接略過
	}
	```

	* 想指定某個測試，可以使用 `-run` 引數，這接受一個正規表示式，例如只想執行 Add function
		* `go test -v -run=Add`
	
* 效能評測 (Benchmark)
	* 如果想進行效能評測 撰寫 `func BenchmarkXxxx(b *testing.B)` 的函式
	```
	func BenchmarkAdd(b *testing.B) {
		for i := 0; i < b.N; i++ {
			Add(1, 2)
		}	
	}
	```

	* 為了進行評測，被測試的函式要執行多次，以求得每次執行的平均時間，
		* 要執行多次 可以使用迴圈，
		* 以 b.N 作為邊界，若目標預設 1000000000, 以越來越大 b.N 執行迴圈，這是為了讓評測進入穩定狀態，以收集可靠的評測資料，
		* 若運行時間到了，b.N 目標值仍未達成，就以現有的收集到的資料來回報評測結果。
	* 再執行 go test 時加上 `-bench` 引數，這個引述可以使用正規表示式，指定符合的評測函數名稱。
		* `go test -v -bench="."` 若想執行所有評測函式，可以使用 `-bench="."`
	* `-benchtime` 引數，可以設定評測運行時間
		* `go test -v -bench="." -benchtime=2s`
		* `-benchtime=1h30s`
		* `-benchtime=100000000000x` 指定次數
	* `-count` 可以指定評測重啟幾次
		* `go test -bench="*" -count=3`