# Python 函式的可變參數 (**kwargs)

可變參數 **kwargs (keyword arguments) 指的是有關鍵字參數，用來處理帶有關鍵字的不定數量參數。

**如果有跟 *args 一起使用的話，**kwargs 必須要排在函式參數的最後面**

先來把 kwargs 印出來看看接收的參數會長哪樣，以及其型別

程式碼:
```py
def pair(**kwargs):
   print(kwargs)
   print(type(kwargs))

pair(first='Hi', second='Hello')
```

輸出結果:

```sh
{'first': 'Hi', 'second': 'Hello'}
<class 'dict'>
```

知道他是 dict 的型別之後，試著使用迭代 dict 的方式將 key 與 value 輸出

程式碼:
```py
def pair(**kwargs): 
  for key, value in kwargs.items(): 
    print(f'{key}: {value}')

pair(first='Hi', second='Hello')
```

輸出結果:
```sh
first: Hi      
second: Hello  
```