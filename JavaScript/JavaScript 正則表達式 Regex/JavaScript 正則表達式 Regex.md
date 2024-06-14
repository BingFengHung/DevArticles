# JavaScript 正則表達式 Regex
## 前言
正則表達式 (Regex Expression) 是用於在字串中查找指定內容的工具

> 最常應用到的場景是：
>
> 1. 驗證使用者輸入是否符合規範
> 2. 大量文字內容的處理，如檔案中內容的查詢與替換等

正則表達式在很多程式語言中都有支援其功能，因此，只要學會正則表達式就能在其他語言中快速的上手。

## JavaScript 正則表達式
以 JavaScript 來說，搭配 Regex 語法，並使用 **match** 或 **replace** 語法能夠快速針對指定的字串進行操作

```js
// match 
const matches = "Jack's jacket is black".match(/ack/)

// replace with
const result = 'I like cats'.replace(/cat/, 'dog')
```

在使用上面的方法之前，要先知道如何撰寫 Regex

在 JavaScript 中，正則表達式是以兩個斜線作為開始撰寫 Regex 語法，在兩個斜線之間，就是定義語法的地方

```js
const reg = / abc /
```

預設情況下，正則表達式只會找到第一個符合的項目

如果想要改變這一種匹配模式，可以在結束斜線的後面，加上修飾符

```js
const reg = / abc /g
// g (global) 表示全域匹配
```

另外，還有其他種修飾符

```js
const reg = / abc /gimuys
```

### 特殊功能字串
如果要建立更加豐富的查找模式，可以加入一些特殊功能的字串

> | [] ^ $ ? * + {,} [] \

#### `|` 邏輯或
可用來匹配一個字串或是另外一個字串

```js
/jack|lucy/g
```

`jack` is a good boy
`lucy` is a good girl
where is `jack`
`jack`

#### `()` 邏輯分組  
這讓你能夠與模式中的其他部分進行隔離開

```js
/(jack/lucy) is a /g
```

**jack is a **good boy  
**lucy is a **good girl  
where is jack  
jack  

#### `^` 開始位置   
用來批配模式開頭的字串

```js
/^jack/g
```

**jack** is a good boy  
lucy is a good girl  
where is jack  
jack  

#### `$` 結束位置  
用來批配模式結束位置的字串

```js
/girl$/g
```

jack is a good boy  
lucy is a good **girl**  
where is jack  
jack  

### 使用量詞控制出現的頻率

#### `?` 0 或 1  
? 表示允許模式出現 0 或 1 次

```js
/colou?rs/g
// u 可以出現 0 次或是 1 次
```

`color`  
`colour`  
`color`s  
`color`sss  
`color`ssssss  

#### `*` 0 次或多次
* 表示允許模式出現 0 或 n 次

```js
/colou?rs*/g
// u 可以出現 0 次或是 1 次
// s 可以出現 0 次或是多次
```

**color**    
**colour**    
**colors**    
**colorsss**    
**colorssssss**    

#### `+` 1 次或多次

+ 表示允許模式出現 1 或 n 次

```js
/colou?rs+/g
// u 可以出現 0 次或是 1 次
// s 可以出現 1 次或是多次
```

color    
colour  
**colors**
**colorsss**  
**colorssssss**

如果要更加明確控制頻率，使用 {} 控制範圍

#### `{}` 規定次數範圍
{} 準確控制次數的最大與最小值

```js
/colou?rs{2,}/g
// u 可以出現 0 次或是 1 次
// {2,} 可以出現 2 次或是多次
```

color  
colour  
colors  
**colorsss**  
**colorssssss**  

```js
/colou?rs{2,5}/g
// u 可以出現 0 次或是 1 次
// {2,5} 可以出現 2 次或 5 次
```

color  
colour  
colors  
**colorsss**   
**colorsssss**s  

### 模式匹配多種字串條件

#### `[]` 定義字串集合
有兩種方式

1. 指定可能出現的字串
2. 指定想要的字串範圍

### 1. 指定可能出現的字串

```js
/[REC]/g
```

`R`eg `E`xr was created by gskinner.com.

`E`dit the `E`xpression & Text to see matches. `R`oll over matches or the expression for details. P `CRE` & JavaScript flavers of `R`eg `E`x are supported. Validate your expression with Tests mode.

### 2. 指定字串出現的範圍

```js
/[A-Z]/g
```

`R`eg `E`xr was created by gskinner.com.

`E`dit the `E`xpression & `T`ext to see matches. `R`oll over matches or the expression for details. P `CRE` & JavaScript flavers of `R`eg `E`x are supported. `V`alidate your expression with Tests mode.

##### `^` 取得其他字串

如果說在在指定的範圍模式前面加上 `^`，可以匹配除了中括號內字元以外的任意字元

```js
/[^A-Z]/g
```

R `eg`E `xr was created by gskinner.com.`

E `dit the `E `xpression & `T `ext to see matches. `R `oll over matches or the expression for details. `PCRE `&`J `ava`S `cript flavers of `R `eg`E `x are supported. `V `alidate your expression with `T `ests mode.`

##### `\` 特殊符號匹配
如果希望匹配字串中出現特殊符號，可以使用 `\` 作為開頭，然後接上想要的符號

```js
/\[/g
```

He purchahsed tickets for `[`Section A] of the stadium.

除了特殊匹配之外，反斜線還能觸發一些預定義的模式

##### `\d` 數字字元
\d 可以代表任何數字

```js
/\d/g
```

She arrived at the party around `9` pm.

##### `\w` 單詞字元

```js
/\w/g
```

`She` `arrived` `at` `the` `party` `around` `9` `pm`.

如果把它變成大寫

##### `\D` 數字字元  = [^0-9]

代表除了數字以外的所有字元

`She arrived at the party around `9 ` pm.`

### 參考連結

[150秒学会正则 (youtube.com)](https://www.youtube.com/watch?v=Pc5DQGiH5bk&ab_channel=%E5%90%B4%E6%82%A0%E8%AE%B2%E5%89%8D%E7%AB%AF)
