# JavaScript Statement VS Expression

## 前言

有時候往往會搞不清楚，這一段或是這一行程式碼到底是陳述式 (statement) 還是表達式 (expression)。為此，本篇將針對這兩個項目進行說明。

## 簡易判別方法

一個非常簡單判別是 `expression` 或是 `statement` 的方法是，在 JavaScript 執行環境下，如果用 console.log() 可以順利打印東西出來就是表達式，如果沒辦法就是陳述式。

### console.log() 表達式輸出

```js
console.log("123");
```

### console.log() 陳述式無法輸出

```js
console.log(if (true) {})  // Uncaught SyntaxError: Unexpected token 'if'
```

## 表達式 (Expression)

會回傳值的程式碼片段都算是表達式

ex.

> 1 -> 1
> "hello" -> "hello"
> 5 * 10 -> 50
> 3 > 2  -> true
> [1, 2, 3].pop() -> 回傳 3

表達式能夠在包含表達式，像是 (5 + 1) * 2

Expression 1: 整個表達式

> (5+1)*2

Expression 2: 小括號

> (5 + 1)

Expression 3: 小括號內的數字 5

> 5

Expression 4: 小括號內的 1

> 1

Expression 5: 乘以 2 的 2

> 2

## 陳述式 (Statements)

陳述式一定會執行動作，但不會有回傳值或是結果

下面是一些陳述式的範例

```js
let hi = 'Hello';
```

```js
if (true) {
   // 更多 statemetn
}

```

```js
throw new Error('Something wrong!');
```

## 總結

- Expression：會回傳一個值，並且可以內嵌到其他表達式中
- Statement：會執行動作，但不會回傳值。

## 參考資料

- https://www.joshwcomeau.com/javascript/statements-vs-expressions/
