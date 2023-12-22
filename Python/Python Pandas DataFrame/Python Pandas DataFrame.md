# Python Pandas (Series、DataFrame)
### DataFrame 表格
DataFrame 是 Pandas 中最常用的資料結構

### DataFrame 建立
使用 array + dict 轉為 DataFrame
```py
scores = [
  {"name": "John", "Math": 90, "English": 100},
  {"name": "Amy", "Math": 80, "English": 90},
  {"name": "Tim", "Math": 95, "English": 70},
]

scores_df = pd.DataFrame(scores)

print(scores_df)
"""
   name  Math  English
0  John    90      100
1   Amy    80       90
2   Tim    95       70
"""
```

字典轉 DataFrame
```py
scores = {
  "name": ["John", "Amy", "Tim"],
  "Math": [90, 80, 95],
  "English": [90, 80, 95],
}

scores_df = pd.DataFrame(scores)

print(scores_df)
"""
   name  Math  English
0  John    90      100
1   Amy    80       90
2   Tim    95       70
"""
```

其他方法
```py
John = {'Math': 90, 'English': 100}
Amy = {'Math': 80, 'English': 90}
Tim = {'Math': 95, 'English': 70}

scores_df = pd.DataFrame([John, Amy, Tim])

print(scores_df)
"""
   Math  English
0    90      100
1    80       90
2    95       70
"""
```

### 存取 DataFrame 資料
DataFrame 的資料具有多個欄位與多個列，因此具有`行索引`與`列索引`

因此，操作上需要考量要拿的資料是特定行、特定列

取得特定列
```py
scores = {
  "name": ["John", "Amy", "Tim"],
  "Math": [90, 80, 95],
  "English": [90, 80, 95],
}

scores_df = pd.DataFrame(scores)

print(scores_df[1:3])
""" 
   name  Math  English
1  Amy    80       80
2  Tim    95       95
"""
```

取得特定行，使用欄位名稱
```py
scores = {
  "name": ["John", "Amy", "Tim"],
  "Math": [90, 80, 95],
  "English": [90, 80, 95],
}

scores_df = pd.DataFrame(scores)

print(scores_df[['name']])
""" 
   name
0  John
1   Amy
2   Tim
"""
```

取得表，使用 loc、iloc
1. 使用 loc[列索引值, 欄位名稱]
```py
scores = {
  "name": ["John", "Amy", "Tim"],
  "Math": [90, 80, 95],
  "English": [90, 80, 95],
}

scores_df = pd.DataFrame(scores)

print(scores_df.loc[2, 'name']) # 'Tim'
print(scores_df.loc[[1, 2], ['name', 'English']]) # 'Tim'

""" 
   name  English
1  Amy       80
2  Tim       95
"""
```

2. 使用 iloc[列索引值, 行索引值]
```py
scores = {
  "name": ["John", "Amy", "Tim"],
  "Math": [90, 80, 95],
  "English": [90, 80, 95],
}

scores_df = pd.DataFrame(scores)

print(scores_df.iloc[2, 0]) # 'Tim'
print(scores_df.loc[[1, 2], [0, 2]]) # 'Tim'

""" 
   name  English
1  Amy       80
2  Tim       95
"""
```


DataFrame 屬性

```py
print(scores_df.values) # DataFrame 中的資料
print(scores_df.size) # 元素數量
print(scores_df.index) # 列索引值
print(scores_df.columns) # 行索引值
print(scores_df.dtypes) # 資料型態
print(scores_df.shape) # 數據形狀

"""
[['John' 90 90]
 ['Amy' 80 80]
 ['Tim' 95 95]]

9

RangeIndex(start=0, stop=3, step=1)

Index(['name', 'Math', 'English'], dtype='object')

name       object
Math        int64
English     int64
dtype: object
(3, 3)
"""
```