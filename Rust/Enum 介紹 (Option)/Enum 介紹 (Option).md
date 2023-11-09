# Enum 介紹 (Option)

前一篇介紹基本的 Enum，在 Rust 裡面有一個 Option 的列舉，很常被使用在許多場合中，他主要用來表示一個變數可能有值或是沒有值。

會這樣設計是因為，在 Rust 裡面並沒有 Null 的東西，在 Option 列舉的原始碼裡面是長這樣的。

```rs
enum Option<T> {
  Some(T),
  None
}
```

使用 Option 的好處是，你不會去忘記要去處理 None 的狀況，因為在其他的程式語言中，很可能一不小心就忘記針對 Null 的情況做處裡，導致整個程式崩潰。

由於 Option 很常用，因此他是有預先載入的 (prelude)，我們無須再手動引入，Some 與 None 也是一樣有預先載入。

下面是一個簡單使用 Option 的程式碼

```rs
let some_number = Some(5);
let some_char = Some('a');
let some_number: Option<i32> = None;
```

由於 Rust 可以推論型別，所以在前兩個程式碼，他都可以順利推斷出來是 Option\<i32\>、Option\<char\>，但是最後一行，由於它使是指定 None，Rust 並無法看出他的型別是多少，所以需要再宣告變數的時候，先定義好型別。

**這邊有一個重點要記得，型別 Option\<T\> 與 T (任意型別)，這兩個是不同的型別。**

