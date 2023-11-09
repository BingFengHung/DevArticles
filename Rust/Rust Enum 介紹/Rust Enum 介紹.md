# Rust Enum 介紹
Enum (列舉)，許多程式語言都有這個設計，一般來說，列舉是用來將某些特定的值整合再一起，例如，顏色、形狀等。**使用列舉的好處是，比較不會不小心打錯字，並且可讀性也比較高**。

在 Rust 裡面，列舉裡面的列舉值叫做變體 (variant)，宣告方式如下：

```rs
enum BrushColor {
  Red,
  Blue,
  Yello,
  Green
} 

let mut a: Color = BrushColor::Red;
a = BrushColor::Blue
```

這邊要注意 enum 的名稱以及其變體，需要使用駝峰式 (Camel Case) 命名，不然編譯器會報錯。

在使用 Enum 的方式就是用列舉的名稱加上`::` 後面跟著想要的變體即可。

除了上面比較常看到的用法之外，Rust 的 Enum 還能讓變體帶有建構子，程式碼如下所示：

```rs
enum Pet {
  Bird(String, i32),  // 未命名欄位
  Rabbit {name: String, age: i32},  // 命名欄位
  Fox
}

let mut pet: Pet = Pet::Bird("Trace".to_string(), 10);
let mut pet2: Pet = Pet::Rabbit{ name: "Bob".to_string(), age: 2 };
```

這樣做的好處是，不用再額外宣告 Struct，直接使用 enum 就能處理，程式碼也會節省很多。

看一下如果是 Struct 的程式碼宣告會是長哪樣
```rs
struct Bird(String, i32);

struct Rabbit {
  name: String,
  age: i32
}

struct Fox;
```

Enum 還能夠實作方法出來使用，程式碼如下所示：

```rs
impl Pet {
  fn hello(&self) {
    match self {
      Pet::Bird(name, age) => {
        println!("Bird: {}, {}", name, age)
      }
      Pet::Rabbit {name, age} => {
        println!("Rabbit: {}, {}", name, age)
      }
      _ => {
        println!("Unknow")
      }
    }
  }
}

let mut pet: Pet = Pet::Bird("Trace".to_string(), 10);
let mut pet2: Pet = Pet::Rabbit{ name: "Bob".to_string(), age: 2 };

pet.hello();  // Bird: Trace, 10
pet2.hello();  // Rabbit: Bob, 2
```