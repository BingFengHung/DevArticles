# Python 打包與解包
## 變數宣告
先來看一個變數宣告的程式碼

```py
a, b = 1, 2
```

上面的程式碼很常會使用到，其實裡面有一些小細節，程式碼中的 1, 2 會隱式的打包成一個 Tuple。然後這個 Tuple 之後會被解包到變數 a 與 b 中。

`當沒有明確的用括號將 1, 2 包起來的話，Python 會把它當作是 Tuple`

## List 解包
在等號的左邊宣告變數 (使用逗號隔開)，就能夠自動分配等號右邊的串列元素，一一對應的宣告的變數中進行賦值。

```py
# 將串列解包給 3 個變數
a = [1, 2, 3]
one, two, three = a

print(one)
print(two)
print(three)
```

輸出
```sh
1
2
3
```

## Tuple 解包
與 List 一樣，在等號的左邊宣告變數，就可以自動將在等號右邊的 Tuple 進行解包的動作，一一匹配賦值。
```py
# 將元組解包給 3 個變數
a = (1, 2, 3)
one, two, three = a

print(one)
print(two)
print(three)
```

輸出
```sh
1
2
3
```
## Set 解包
與 Tuple 與 List 一樣的解包方式，但由於 set 是一種無順序且元素唯一的資料結構。因此，解包的時候並無法保證順序。

```python
a, b, _, _ = set('hello')
print(a)
print(b)
```

```sh
l
h
```

## Dict 解包
如果是要解包字典的鍵或值，可以使用迭代方法

```py
d = {'a': 1, 'b': 2, 'c': 3 }

# 解包鍵
keys = d.keys()
a, b, c = keys

print(a, b, c)

# 解包值
values = d.values()
x, y, z = values
print(values)
```

輸出
```sh
a b c
1 2 3
```

### 不需要的值用 _ 來跳過
當迭代物件中的項目，有一些並不需要可以使用 _ 來跳過，但要注意最終變數數量一定要一樣，否則會出錯

```py
one, two, _ = [1, 2, 3]
print(one)
print(two)
```
輸出結果

```sh
1
2
```

## 解包運算子 (Unpacking Operator)

解包運算子分為：

> `*` 符號：可用於任何可迭代的物件
> `**` 符號： 只能用於字典物件

## 使用 * 運算符對 List 解包
使用 * 逐一取出 List 的元素值，然後丟到 print 方法中

```py
num = [1, 2, 3]
print(*num) 
```

輸出結果
```sh
1 2 3
```

## Tuple 解包

```py
num = (1, 2, 3)
print(*num)
```
輸出結果
```sh
1 2 3
```


## Set 解包

```py
hello = set('hello')
print(*hello)
```

輸出結果
```sh
l h o e
```

## 字串解包
```py
hello = 'hello'
print(*hello)
```

輸出結果
```sh
h e l l o
```

## Dict 解包
dict 要使用 ** 做解包，並且這邊比較特別的是，當使用 ** 解包 dict 會拿到鍵值對，應該是說會轉換成關鍵字參數的形式。

```py
def my_func(a, b, c):
  print(a, b, c)

num = {'a': 1, 'b': 2, 'c': 3}
my_func(**num)
```

輸出結果
```sh
1 2 3
```

在上面的程式碼中  **num 會將字典轉換為 `a=1, b=2, c=3` 的關鍵字參數的形式，因此，在帶入 my_func 函數時，其函數的參數名稱，必須要與鍵值對的鍵名稱一樣，否則會報錯。

另外，如果對 dict 使用 * 進行解包，拿到的會是 key

```py
my_dict = {'a': 1, 'b': 2, 'c': 3}

# 使用單個星號解包字典
keys = [*my_dict]
print(keys)  
```

輸出 

```sh
a b c
```

## 解包運算子各式用法
list 串接

```py
a = [1, 2, 3]
b = [1, 2, 3]

c = [*a, *b]
```

可迭代物件串接
```py
values = [*range(10)]
```

字典物件串接

```py
dict1 = {'a': 1, 'b': 2, 'c': 3}
dict2 = {'d': 4, 'e': 5, 'a': 3}

mix = {**dict1, **dict2}
```

## 打包剩餘的元素
使用 * 不只可以解包，也能將剩餘元素進行打包

最後一個變數接收所有剩餘元素
```py
t = (1, 2, 3, 4, 5)

a, b, *c = t
print(a)
print(b)
print(c)
```

輸出

```sh
1
2
[3, 4, 5]
```

中間變數接收所有剩餘元素
```py
t = (1, 2, 3, 4, 5)

a, *b, c = t
print(a)
print(b)
print(c)
```

輸出

```sh
1
[2, 3, 4]
5
```

第一個變數接收所有剩餘元素

```py
t = (1, 2, 3, 4, 5)

*a, b, c = t
print(a)
print(b)
print(c)
```

輸出

```sh
[1, 2, 3]
4
5
```

## 搭配 _ 使用 *_
上面有說到當迭代物件中的項目，有一些並不需要可以使用 _ 來跳過，但要注意最終變數數量一定要一樣，否則會出錯

我們能夠搭配 *，這樣就不用寫一堆 _

```py
t = (1, 2, 3, 4, 5)

a, b, *_ = t
print(a)
print(b)
print(_)
```

輸出

```sh
1
2
[2, 3, 4, 5]
```