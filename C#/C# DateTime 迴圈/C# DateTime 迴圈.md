# C# DateTime 迴圈
## 前言
本篇將介紹如何使用迴圈走訪某段時間區間；將以每日日期走訪的範例，進行說明。

## 使用 for 迴圈走訪指定的日期範圍
當需要以天作為迭代的次數，可以使用以下方法，程式碼如下所示：

```csharp
DateTime from = new DateTime(2099, 01, 31);
DateTime from = new DateTime(2099, 12, 31);

for (var day = from.Date; day.Date <= to.Date; day = day.AddDays(1)) {
  Console.WriteLine(day.ToString("yyyy-MM-dd HH:mm:ss"));
}
```

這邊主要的重點是，day = day.AddDays(1)，使用這個方法將日期變數加一天，但是要記住，不能只寫 day.AddDays(1)，因為 day.AddDays 是回傳一個新的實例，並不是將原本的 day 進修改行。

## 使用 foreach 迴圈改寫
這邊，可將上面的方法，更進一步的寫成 foreach 的形式，程式碼如下所示：

```csharp
public IEnumerable<DateTime> EachDay(DateTime from, DateTime to) {
  for (var day = from.Date; day.Date <= to.Date; day = day.AddDays(1))
     yield return day;
}
```

寫成 IEnumerable<DateTime> yield return 形式後，就能如下程式碼中的方法一樣，使用便利的 foreach 寫法了。

```csharp
public void PrintDate(DateTime start, DateTime end) {
  foreach (DateTime day in EachDay(start, end)) {
    Console.WriteLine(day.ToString("yyyy-MM-dd HH:mm:ss"));
  }
}
```

## 結論
用迴圈走訪一個時間區間內的 DateTime，駐點在於判斷當前的 Date 是否已經到達，並且每次迭代時，用 date 物件內的 AddDays 方法，一次加一天。

- Date 指的是 DateTime 物件的日期部分 (包含日期與時間)
- Day 是 DateTime 物件的一個屬性，會返回日期中日的部分