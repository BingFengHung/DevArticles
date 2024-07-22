# Python 運算元重載

Python 要實現基本的加減乘除的方法重載還蠻簡單的，只需要在類別中分別定義 __add__、__sub__ 等方法就能夠重載方法了，接下來將以針對相關的方法提供範例程式碼說明。


## 二元運算子
| Operator | Magic Method                  |
| -------- | ----------------------------- |
| +        | `__add__(self,  other)`       |
| –        | `__sub__(self, other)`        |
| *        | `__mul__(self, other)`        |
| /        | `__truediv__(self, other)`    |
| //       | `__floordiv__(self, other)`   |
| %        | `__mod__(self, other)`        |
| **       | `__pow__(self, other)`        |
| >>       | `__rshfit__(self, other)`     |
| <<       | `__lshift__(self, other)`     |
| &        | `__and__(self, other)`        |
| \|       | `__or__(self, other)`         |
| ^        | `__xor__(self, other)`        |

```py
class Value:
  def __init__(self, value):
    self.value = value
    
  def __add__(self, other):
    return self.value + other.value
    
  def __sub__(self, other):
    return self.value - other.value
    
  def __mul__(self, other):
    return self.value * other.value
  
  def __truediv__(self, other): 
    return self.value / other.value
    

a = Value(10)
b = Value(20)

print(a + b) # 30
print(a - b) # -10
print(a * b) # 200
print(a / b) # 0.5
```

## 比較運算子
| Operator | Magic Method                  |
| -------- | ----------------------------- |
| <        | `__lt__(self, other)`         |
| >        | `__gt__(self, other)`         |
| <=       | `__le__(self, other)`         |
| >=       | `__ge__(self, other)`         |
| ==       | `__eq__(self, other)`         |
| !=       | `__ne__(self, other)`         |

```py
class CompareValue(Value):
  def __init__(self, value):
    super().__init__(value)
  
  def __lt__(self, other):
    return self.value < other.value
  
  def __gt__(self, other):
    return self.value > other.value

a = CompareValue(10)
b = CompareValue(20)
print(a > b) # False
print(a < b) # True
```

## 賦值運算子
| Operator | Magic Method                  |
| -------- | ----------------------------- |
| +=       | `__iadd__(self, other)`       |
| -=       | `__isub__(self, other)`       |
| *=       | `__imul__(self, other)`       |
| /=       | `__idiv___(self, other)`      |
| //=      | `__ifloordiv__(self, other)`  |
| %=       | `__imod__(self, other)`       |
| **=      | `__ipow__(self, other)`       |
| >>=      | `__irshift__(self, other)`    |
| <<=      | `__ilshift__(self, other)`    |
| &=       | `__iand__(self, other)`       |
| \|=      | `__ior__(self, other)`        |
| ^=       | `__ixor__(self, other)`       |

```py
class AssignValue(Value):
  def __init__(self, value):
    super().__init__(value)
  
  def __iadd__(self, other):
    self.value += other.value
    return self
    
  def __isub__(self, other):
    self.value -= other.value
    return self
  
  def __imul__(self, other):
    self.value *= other.value
    return self
  
  def __idiv__(self, other):
    self.value /= other.value
    return self
    
  def __repr__(self):
    return f'{self.value}'

a = AssignValue(10)
b = AssignValue(20)

a += b
print(a) # 30

a -= b
print(a) # 10

a *= b
print(a) # 200

a /= b
print(a) # 10.0
```

## 一元運算子
| Operator | Magic Method                  |
| -------- | ----------------------------- |
| -        | `__neg__(self)`               |
| +        | `__pos__(self)`               |
| ~        | `__invert__(self)`            |

```py
class UnaryValue(Value):
  def __init__(self, value):
    super().__init__(value)
  
  def __neg__(self):
    self.value = -self.value
    return self
    
  def __pos__(self):
    self.value = +self.value
    return self
  
  def __invert__(self):
    self.value = ~self.value 
    return self
  
  def __repr__(self):
    return f'{self.value}'

a = UnaryValue(10)

print(-a) # -10
print(+a) # -10
print(~a) # 9
```