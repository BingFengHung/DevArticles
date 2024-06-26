# C# record 說明

## 前言
record 是用來定義不可變參考類型的資料結構

## record 宣告
先來看看 record 如何宣告

```cs
public record Person(string Name, int Age);
```

上面的 record 如果是用以前的寫法長這樣

```cs
public class Person 
{
   public string Name{ get; }
   public int Age { get; }
   
   public Person(string name, int age)
   {
      Name = name;
      Age = age;
   }

   public override bool Equals(object obj) => 
      obj is Person person && Name == person.Name && Age == person.Age;
   
   public override int GetHashCode() => HashCode.Combine(Name, Age);
}
```

上面兩段程式碼相比，就可以看出差多少了；記得在建立物件時，需要根據參數的位置傳入對應的參數。

## 結論
- 可讀性：語法簡潔容易閱讀與理解
- 重複性降低：減少重複的程式碼，降低錯誤的可能發生
- 提高生產力：程式碼撰寫時間降低