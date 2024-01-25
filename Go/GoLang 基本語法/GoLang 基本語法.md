# Go Language 基本語法
## 基本結構
檔案名稱為 main.go

```go
package main  // 可執行的程式必須使用 main 封包

import "fmt"  // 載入內建的 fmt 封包，用來作為基本的輸出輸入

// 建立 main 函式，程式的進入點
func main() {
    // 輸出訊息到終端機: fmt.Println(data, data, ...)
    fmt.Println("Hello World");
}
```

## 程式編譯與執行
程式編譯

go build (編譯 packages 以及相依的套件)

```sh
go build main.go
```

執行程式，編譯後會產生 main.exe 的程式，直接執行即可

如果想要編譯後執行可以下
 
```sh
go run main.go
```

## 資料型態

基本語法

```go
package main  

import "fmt" 

func main() {
    fmt.Println(3)  // 整數 int
    fmt.Println(3.1415)  // 浮點數 float64
    fmt.Println("測試中文")  // 字串 string
    fmt.Println(true)  // 布林值 bool
    fmt.Println('a')  // 字符 rune
}
```

輸出 

```sh
3
3.1415
測試中文 
true
97
```
