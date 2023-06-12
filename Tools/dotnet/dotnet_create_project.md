新增專案
```sh
# 建立 .net framework 4.0 的專案，並使用 C# 9.0 語法
dotnet new wpf --target-framework-override net40 --langVersion 9.0 -o <project-name>
```
新增解決方案
```sh
dotnet new sln --name <sln-name>
```

加入專案到解決方案中
```sh
dotnet sln add <project-name.csproj>
```

建立專案執行在 x86 
```sh
dotnet build <project-name> --arch x86
```

加入參考

```sh
dotnet new console -o app1
dotnet new classlib -o app1

# 切換到指定專案中再加入類別專案的參考
cd ./app1
dotnet add reference ../cslib/cslib.csproj

# or
# 在跟目錄資料夾直接加入參考
dotnet add app1.csproj reference ./cslib/cslib.csproj
```
