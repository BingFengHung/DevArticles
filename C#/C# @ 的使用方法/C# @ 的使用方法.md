# C# @ 的使用方法
## 前言
本篇主要介紹 **@** 特殊字元，方便使用者對字串進行所需要的操作以及關鍵字識別項的使用，其使用方法主要有以下三種方式：

## 1. 逐字解釋字元
在字串前面加上 @ 就是告訴編譯器，接下來的字串由使用者全權負責

比較一下有使用 @ 與使用跳脫字元的差異

### 使用跳脫字元的字串路徑
在沒有 @ 以前，字串中的 \ 符號代表跳脫字元，因此，在寫路徑的字串就會變得比較麻煩，如下程式碼所示：

```csharp
public static void Main(string[] args) {
  string path = "D:\\NCFILE\\test.nc";
  Console.WriteLine(path);  // D:\NCFILE\test.nc
}
```

#### 使用 @ 在路徑字串前綴的字串路徑
加上 @ 符號之後的字串路徑寫法如下程式碼所示：

```csharp
public static void Main(string[] args) {
  string path = @"D:\NCFILE\test.nc";
  Console.WriteLine(path);  // D:\NCFILE\test.nc
}
```

這邊有一點要注意，如果說你想要在 @ 逐字解釋字串的情況下，加上雙引號，這邊要特別注意，前面要再多加一個雙引號才能順利地將雙引號顯示出來，程式碼如下所示：

```csharp
public static void Main(string[] args) {
  string path = @"This is ""Joe""";
  Console.WriteLine(path);  // This is "Joe"
}
```

## 2. 直接換行
在沒有 @ 之前，如果字串要換行必須使用 **+** 符號進行字串的串接，寫起來稍嫌麻煩一點，程式碼閱讀上也比較不方便，程式碼如下所示：

```csharp
public static void Main(string[] args) {
  string command = "SELECT * FROM student"
          + " WHERE `id` = '123'";
  Console.WriteLine(command);  
  // SELECT * FROM student WHERE `id` = '123';
}
```

使用 @ 開啟換行功能，程式碼如下所示：

```csharp
public static void Main(string[] args) {
  string command = @"SELECT * FROM student
           WHERE `id` = '123'";
  Console.WriteLine(command);  
  /* SELECT * FROM student 
                WHERE `id` = '123';
  */
}
```

這邊要注意字串輸出的時候，@ 符號會讓換行與空白包含在字串內，所以在輸出的時候要注意是否是自己需要的格式。

## 3. 識別項解釋
當需要使用 C# 關鍵字當作識別項的時候使用，程式碼如下所示：

```csharp
public static void Main(string[] args) {
  var @static = "static";
  var @int = 100;
}
```

**這種方法比較不建議使用，因為應該要有用更有意義的命名方式**

## 結論
- 逐字解釋字元： 無需使用跳脫字元，全權由使用者決定
- 直接換行：不須使用 + 符號串接字串
- 識別項解釋：使用關鍵字作為識別項名稱的作法