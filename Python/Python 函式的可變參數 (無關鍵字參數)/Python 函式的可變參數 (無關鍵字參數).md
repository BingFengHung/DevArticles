# Python 函式的可變參數 (*args)

可變參數 *args 顧名思義，也就是一個函式，可以接收任一的參數，只需要在函式定義的地方，加上 *args 關鍵字，即可讓這個函式在被呼叫的時候，能夠接收 0 到多個參數 (未被命名的位置參數)。

先來把 args 印出來看看接收的參數會長哪樣，以及其型別

程式碼:
```py
def add(*args):
   print(args)
   print(type(args))

add(1, 2, 3, 4, 5, ['a', 'b', 'c'])
```
輸出結果:
```sh
(1, 2, 3, 4, 5, ['a', 'b', 'c'])
<class 'tuple'>
```

知道他是 tuple 的型別之後，我們可透過 for 迴圈來對資料進行迭代

程式碼:
```py
def add(*args): 
  sum = 0
  for i in args:
    sum += i
  return sum

sum = add(1, 2, 3, 4, 5)
print(f'sum = {sum}')
```

輸出結果:
```sh
sum = 15
```