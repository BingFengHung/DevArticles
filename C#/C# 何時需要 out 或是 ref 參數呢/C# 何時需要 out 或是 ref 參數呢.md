# C# 何時需要 out 或是 ref 參數呢

## 前言
突然想到為甚麼需要 out 參數呢? 以參考型別來說，印象中應該是用傳遞記憶體位址的方式到方法中，所以當我在方法中所做的物件修改，方法執行完畢之後，原本的參考物件應該也是會修改的。

結果卻與我記憶中的有些差異，下面將做一個完整的說明。

## 傳遞物件到方法中並在方法內修改物件

為了驗證此猜想，寫了一段測試程式碼

```csharp
class Person {
   public string Name { get; set; }
}

public void ChangeName(Person person) {
   person.Name = "Jack";
}

void Main() {
  Person joe = new Person();
  joe.Name = "joe";
  
  ChangeName(joe);

  Console.WriteLine(joe.Name);  // "Jack"
}
```

在上面的測試程式碼中，確實 joe 這個物件的 Name 在經過 ChangeName 函式之後，確實有修改了。

但此時如果是在 ChangeName 方法裡面重新 new 一個 Person 的物件，結果就會不一樣了

```csharp
class Person {
   public string Name { get; set; }
}

public void ChangeName(Person person) {
   person = new Person();
   person.Name = "Jack";
}

void Main() {
  Person joe = new Person();
  joe.Name = "joe";
  
  ChangeName(joe);

  Console.WriteLine(joe.Name);  // "joe"
}
```

原來**C# 的方法參數預設都是使用傳值的方式 (call by value)**，就算是參考型別，再將物件傳遞給方法時，他傳遞的是記憶體位置的副本，所以當我們在方法內對物件做屬性的修改，基本上方法外面的物件也是會跟著修改；但是，當我們在方法裡面重新實例化物件的時候，原本傳遞進來的物件記憶體位置已經不一樣了，所以當方法結束的時候，原本的物件記憶體並沒有被取代還是原本的記憶體位置。

## 結論
當我們想要在方法內重新配置記憶體位置，並且外面的物件也要同時修改的話，就需要在方法參數的地方加上 out 或是 ref 的關鍵字，這樣才有辦法在方法內部做任何的修改，外面的物件也會跟著修改。
