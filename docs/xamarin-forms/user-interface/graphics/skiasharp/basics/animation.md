---
title: Базовая анимация
description: Узнайте, как анимация SkiaSharp изображений
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 7435807e77a9a79d7fc3821675c1d959a16caa8f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="basic-animation"></a>Базовая анимация

_Узнайте, как анимация SkiaSharp изображений_

Можно анимировать SkiaSharp графики в Xamarin.Forms, что приводит к `PaintSurface` производится очень часто, будет вызван метод при каждом запуске графики немного по-разному. Вот анимацию, приведенный ниже в этой статье концентрических окружностей, которые внешне разворачиваются в центре.

![](animation-images/animationexample.png "Несколько концентрических окружностей первый взгляд расширение центра")

**Pulsating эллипса** страницы в [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программы анимирует осей эллипса, чтобы он отображался в быть pulsating и даже управления Скорость этого pulsation:


[ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) файл создает Xamarin.Forms `Slider` и `Label` для отображения текущего значения ползунка. Это распространенный способ интеграции `SKCanvasView` с другими представлениями Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Создает файл кода программной `Stopwatch` объект в качестве часов высокой точности. `OnAppearing` Переопределить наборы `pageIsActive` на `true` и вызывает метод с именем `AnimationLoop`. `OnDisappearing` Задает переопределения, которое `pageIsActive` на `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` Запуска метода `Stopwatch` и затем циклы при `pageIsActive` — `true`. Это по сути «бесконечный цикл» страница активна, но оно не вызывает программу зависает, так как цикл завершается с помощью вызова `Task.Delay` с `await` оператор, который позволяет другие части функции программы. Аргумент `Task.Delay` используется для выполнения после 1-30 секунды. Этот параметр определяет частоту кадров анимации.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` Началом цикла, получая времени цикла от `Slider`. Это время в секундах, например, 5. Вторая инструкция вычисляет значение `t` для *время*. Для `cycleTime` 5, `t` увеличивается от 0 до 1 каждые 5 секунд. Аргумент `Math.Sin` функции в второй инструкции в диапазоне от 0 до 2π каждые 5 секунд. `Math.Sin` Функция возвращает значение в диапазоне от 0 до 1 на задней панели 0, а затем в &ndash;1 и 0 каждые 5 секунд, но со значениями, которые изменяют медленнее, если значение равно 1 или -1. Значение 1 добавляется, поэтому значения всегда имеют положительное значение, а затем он делится на 2, поэтому значения в диапазоне от ½ равным 1, чтобы ½ 0, чтобы ½, но медленнее, если значение — около 1 и 0. Эта хранимая в `scale` поля и `SKCanvasView` становится недействительным.

`PaintSurface` Метод использует это `scale` значения для вычисления эллипса осей:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Метод вычисляет максимальное radius, в зависимости от размера области отображения, а Минимальный радиус, исходя из максимального radius. `scale` Значение анимируется от 0 до 1, а значение 0, поэтому метод использует его для вычислений `xRadius` и `yRadius` , лежит в диапазоне между `minRadius` и `maxRadius`. Эти значения используются для рисования и заливка эллипса:

[![](animation-images/pulsatingellipse-small.png "Тройной снимок экрана со страницей пульсирующего эллипс")](animation-images/pulsatingellipse-large.png#lightbox "тройной снимок экрана со страницей пульсирующего эллипса")

Обратите внимание, что `SKPaint` объект создается в `using` блока. Как и во многих классах SkiaSharp `SKPaint` является производным от `SKObject`, который является производным от `SKNativeObject`, который реализует [ `IDisposable` ](https://developer.xamarin.com/api/type/System.IDisposable/) интерфейса. `SKPaint` переопределяет `Dispose` метод для освобождения неуправляемых ресурсов.

 Размещение `SKPaint` в `using` блок гарантирует, что `Dispose` вызывается в конце блока, чтобы освободить эти неуправляемые ресурсы. Это все равно происходит, когда память, используемая `SKPaint` освобождения объекта сборщиком мусора .NET, но в коде анимации, лучше всего несколько заблаговременно при освобождении памяти более упорядоченной способом.

 В этом случае лучшим решением будет создание двух `SKPaint` объектов один раз и сохраните их в качестве полей.

Вот что **расширение круги** анимации. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Класса начинается с определения нескольких полей, включая `SKPaint` объекта:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Эта программа использует другой подход в зависимости от Xamarin.Forms анимацию `Device.StartTimer`. `t` Поле анимируется от 0 до 1 каждые `cycleTime` мс:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` Обработчик рисует 5 концентрических окружностей с анимированных радиусы. Если `baseRadius` переменная вычисляется как 100, затем как `t` анимируется от 0 до 1, радиусы возрастает пять круги от 0 до 100, 100 – 200, 200 – 300, 300 до 400 и 400 до 500. Для большинства круги `strokeWidth` — 50, но для первого круга, `strokeWidth` выполняет анимацию от 0 до 50. Для большинства круги голубой цвет, но для последнего окружности, выполняется анимация цвета от синего прозрачным.

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Результатом является то, что изображение выглядит одинаковыми, если `t` равен 0, когда `t` имеет значение 1, а круги, по-видимому, поочередно раскройте остальные навсегда:

[![](animation-images/expandingcircles-small.png "Тройной снимок экрана со страницей расширение круги")](animation-images/expandingcircles-large.png#lightbox "тройной снимок экрана со страницей круги, развертывание")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
