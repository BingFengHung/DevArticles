# Rust match 控制流建構子
match 的概念跟其他語言的 swithc 陳述式很像，不過 match 具有模式匹配的功能，可以用一系列的模式來匹配對應到的數值，先來看一下模式匹配的程式碼如何撰寫

```rs
enum BrushColor {
  Red,
  Blue,
  Yellow,
  Green
} 

let color = BrushColor::Red;

match color {
   BrushColor::Red => {
    println!("Red")
   }
   BrushColor::Blue => { 
    println!("Blue"),   
   }
   BrushColor::Yellow => {
    println!("Yellow")
   }
   BrushColor::Green => {
    println!("Green"),   
   }
}
```

這邊有一個重點是，使用 match 裡面所有可能的值都必須被覆蓋，也就是說，像上面的 Enum，如果我的變體在 match 裡面分支少寫了一個變體的處理方式，編譯器就會報錯。

另外，如果說我的 Enum 變體有很多個，但是我只想要針對幾個去特別處理，剩下的想要統一處理可以這樣寫

```rs
let color = BrushColor::Red;

match color {
   BrushColor::Red => {
    println!("Red")
   }
   BrushColor::Blue => { 
    println!("Blue"),   
   }
   _ => println!("Other Colors")
}

只需要使用 `_` 就可以表示其他的可能值。

match 在執行的時候，是由上往下一個一個條件去比較的，所以在使用 `_` 要注意順序，如果把他不小心擺在第一個，這樣程式碼的邏輯可能就會出現問題。

在 Enum 介紹的時候，有說到 Enum 的建構子，如果搭配 match 的話應該如何使用呢

```rs
enum Colors {
  Red,
  Green,
  RGB(u8, u8, u8)
}

let color = Colors::RGB(100, 100, 100);

match color {
  Colors::Red => println!("Red"),
  Colors::Blue => println!("Blue"),
  Colors::RGB(r, g, b) => println!("R: {}, G: {}, B: {}", r, g, b),
}
```

