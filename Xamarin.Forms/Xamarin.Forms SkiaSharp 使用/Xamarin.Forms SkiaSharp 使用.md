# Xamarin.Forms 使用 SkiaSharp 
## 前言
SkiaSharp 可用於 .NET 與 C# 的 2D 圖形系統，由廣泛使用在 Google 產品的開源 Skia 圖形引擎所支援。SkiaSharp 能夠讓開發人員在 Xamarin.Forms 設計簡單的 2D 圖形。 

要使用 SkiaSharp 繪圖必須先從 Visual Studio 的 Nuget 中下載兩個套件，**SkiaSharp** 與 **SkiaSharp.Views.Forms**。基本上 SkiaSharp 就是用來畫圖的函式庫，主要提供了一些繪圖指 令，而 SkiarSharp.Views.Forms 主要會使用到 SKCanvasView 這個類別。這個類別繼承了 Xamarin.Forms.View 的類別，並且用來裝載 SkiaSharp 的繪製圖形。 

## 使用方法
上述步驟準備好之後，將會說明簡單的繪製步驟。 (為了簡單一點，程式碼主要都以 C# 
呈現，不跳至 XAML) 

這邊主要簡單繪製圓形，有個簡單的概念即可往後延伸。 

首先建立一個 ContentPage 的類別，並且裡面包含了 SKCanvasView 這個類別，此目的主要是要加入 SkiaSharp 繪圖所要呈現在哪邊的容器，如下程式碼所示：
```cs
class DemoSkiaSharp : ContentPage {
  public DemoSkiaSharp() {
    SKCanvasView canvasView = new SKCanvasView();
    
    Content = canvasView;
  }
}
```

再來就是註冊繪圖事件，主要向 SKCanvasView 註冊一個 PaintSurface 的事件，這個事件是用來進行所有繪圖的地方，程式如下所示。 

```cs
class DemoSkiaSharp : ContentPage {
  public DemoSkiaSharp() {
    SKCanvasView canvasView = new SKCanvasView();
    Content = canvasView;
    
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
  }
  
  private void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs e) {
    SKImageInfo info = e.Info;    
    SKSurface surface = e.Surface;
    SKCanvas canvas = surface.Canvas;
  }
}
```

圖2、加入繪圖事件 

其中事件的參數為 OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs 
e)，其中 SKPaintSurfaceEventArgs 有兩個屬性：
- Info：型別為 SKImageInfo 
- Surface：型別為 SKSurface 

SKImageInfo 包含了繪圖面的資訊，最重要的是，**他的寬高是以像素 (Pixel) 為單位。** 

SKSurface 表示繪圖面本身 (當中最重要的屬性為 Canvas 型別為 SKCanvas，這個是實際繪圖的圖形繪製框架) 

SKCanvas 物件包含了圖形的狀態、圖形變換與剪裁。 

完成上面步驟之後，即可開始進入繪圖的環節。這次要畫，一個內部圓心為填滿 (Fill) 藍色，外部輪廓 (Stroke) 為紅色的圓。這需要拆解為兩個步驟： 
1. 先繪出藍色的內圓 
2. 畫出外步輪廓 

要達到這兩步，有一個 SKPaint 的物件，主要是用來繪製顏色與特徵，如下程式碼所示：

```cs
private void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs e) {
  SKImageInfo info = e.Info;    
  SKSurface surface = e.Surface;
  SKCanvas canvas = surface.Canvas;
  
  SKPaint paint = new SKPaint {
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Red,
    StrokeWidth = 20
  }
}
```

SKPaintStyle 這個列舉共有三個成員：
- Fill 填滿 (預設) 
- Stroke 輪廓 
- StrokeAndFill 

設定好繪筆屬性之後，就是要在 Canvas 上面繪圖了，程式碼如下所示。 

```cs
private void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs e) {
  SKImageInfo info = e.Info;    
  SKSurface surface = e.Surface;
  SKCanvas canvas = surface.Canvas;
  
  SKPaint paint = new SKPaint {
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Red,
    StrokeWidth = 20
  }
  
  canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
}
```

前面兩個參數是，X 軸與 Y 軸，座標值是位於畫面的左上角為圓點
- X 座標往右邊是增加
- Y 座標往下是增加 
- 第三個參數是圓形的半徑 
- 第四個參數是剛剛定義的畫筆 

內圓的方法與上面的步驟一樣在重複做一次，然後把 Style 的部分改為 Fill，顏色也稍微改一下即可，即可畫出如下圖 1 的畫面。 

![](./images/01.png)

圖 1、圖形呈現