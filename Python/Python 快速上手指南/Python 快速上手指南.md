# Python 快速上手指南
## 前言
由於常常在不同的程式語言做切換，有時候可能會誤用其他語言的語法或是撰寫風格；為此，特別整理一篇快速上手指南，幫助自己快速刷新 Python 的語法風格，避免誤用 😂。

## 基本語法
### 變數與類型
```py
x = 3  # 整數
y = 123.123  # 浮點數
person = "Joe"  # 字串
is_open = True  # 布林值
```

### 流程控制
Python 縮排是以 4 格空白為單位

**if-else 語法**

```py
if x > 10:
    print("大於 10")
else: 
    print("小於等於 10")
```

**for 迴圈**

```py
for i in range(10):
    print(i)
```

**while 迴圈**

```py
index = 0
while index < 10:
    print(index)
    index += 1
```

### 函數
```py
def sum(a, b):
    return a + b

result = sum(1, 1)
print(result)  # 2
```

## 資料結構
### 串列
```py
letters = ["a", "b", "c"]
letters.append("d")
print(letters)  # ["a", "b", "c", "d"]
```
### 字典
```py
person = {
    "name": "Joe",
    "age": 18
}

print(person["age"])  # 18
```

### 集合
```py
numbers = { 1, 2, 3 }
numbers.add(4)
print(numbers)  # { 1, 2, 3, 4 }
```

### 元組
```py
points = (0.0, 0.0)
print(points)  # (0.0, 0.0)
```

## 模組引入與導出
### import 模組

1. import 整個模組

```py
import math

print(math.sqrt(4))  # 使用模組中的函式
```

2. import 模組中特定的函式或變數
```py
from math import sqrt

print(sqrt(4))  # 直接使用函式
```

3. 給 import 的模組或是函式命名
```py
import pandas as pd
df = pd.DataFrame({"name": ["Joe", "Lucy"], "age": [18, 18]})

from math import sqrt as square_root
print(square_root(16))
```

### export 模組
1. 單一模組
在 python 中每一個 .py 的檔案就是一個模組

```py
# greet.py
def hi(name):
    return f"Hi, {name}"

# main.py
import greet
print(greet.hi("Joe")) # Hi, Joe
```

2. Package
Package 是一組模組的集合，資料夾會包含一個 **`__init__.py`** 的檔案

```
pokemon/
    __init__.py
    ability.py
    species.py
```

```py
# ability.py
def show():
    print("Show ability")
    
# species
def show():
    print("Show Species")
    
# __init__.py
from .ability import show as ability_show 
from .species import show as species_show 

# main.py
from pokemon import ability_show, species_show
ability_show()
species_show()
```