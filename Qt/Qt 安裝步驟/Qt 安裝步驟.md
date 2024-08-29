# Qt 安裝步驟

先到官方網站下載 Online installer [Qt Framework and Tools](https://www.qt.io/download-dev)

由於我的電腦是 Windows，當然就是下載對應的安裝檔。

![](images/2024-08-28-08-36-30.png)

接著就可以開始安裝了，不過他需要有帳號，因此要先去申請一個。

![](images/2024-08-28-08-48-53.png)

接著就是一步一步的安裝

![](images/2024-08-28-08-50-23.png)

![](images/2024-08-28-08-50-43.png)

![](images/2024-08-28-08-50-55.png)

![](images/2024-08-28-08-51-11.png)

選擇安裝資料夾，並且我這邊選擇自定義安裝

![](images/2024-08-28-08-56-34.png)

選擇 Qt -> Qt 6.7.2，因為我預計使用 Visual Studio 開發，因此下面選單中多選擇 MSVC 2019 64bit 的選項

![](images/2024-08-28-09-02-25.png)

![](images/2024-08-28-09-13-19.png)

![](images/2024-08-28-09-13-35.png)

![](images/2024-08-28-09-13-43.png)

![](images/2024-08-28-09-13-53.png)

等他安裝完成就完成安裝了

## 安裝 Visual Qt 套件
Qt Visual Studio Tools 套件安裝
![](images/2024-08-29-08-38-12.png)

![](images/2024-08-29-08-38-28.png)

https://marketplace.visualstudio.com/items?itemName=TheQtCompany.QtVisualStudioTools2022

## Visual Studio 2022 配置 Qt6
![](images/2024-08-29-08-40-55.png)

開啟 Visual Studio 2022 選擇延伸套件 -> Qt VS Tool -> Qt Versions

![](images/2024-08-29-15-49-22.png)

在左邊選單中，Qt -> Versions，然後再右邊點選 + 號加入配置

![](images/2024-08-29-15-49-49.png)

選擇 Qt 安裝的路徑，C:\Qt\6.7.2\msvc2019_64\bin\qmake.exe
![](images/2024-08-29-15-53-30.png)

## 建立 Qt 專案

![](images/2024-08-29-16-01-44.png)

![](image/1724918576369.png)

![](images/2024-08-29-16-04-08.png)

![](images/2024-08-29-16-05-00.png)

![](image/1724918754886.png)


## 其他配置方式
https://www.cnblogs.com/xiaqiuchu/p/16716276.html

https://www.youtube.com/watch?v=cZfFlk09uGU