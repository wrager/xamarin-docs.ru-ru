---
title: Интеграция с помощью Xamarin.Forms
description: Создание SkiaSharp графики, отвечающей на сенсорный ввод и элементы Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 288224F1-7AEE-4148-A88D-A70C03F83D7A
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: 67c4330d8e446a407dec7792fe5f40cdd9d23c22
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="integrating-with-xamarinforms"></a>Интеграция с помощью Xamarin.Forms

_Создание SkiaSharp графики, отвечающей на сенсорный ввод и элементы Xamarin.Forms_

SkiaSharp графики можно интегрировать с остальной частью Xamarin.Forms несколькими способами. Можно комбинировать холст SkiaSharp и Xamarin.Forms на одной странице и даже позиции Xamarin.Forms элементы на холсте SkiaSharp:

![](integration-images/integrationexample.png "Выбор цвета с ползунки")

Другой подход к созданию интерактивной SkiaSharp графики в Xamarin.Forms — с помощью сенсорного ввода.
Второй страницы в [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программы имеет право **коснитесь переключателя заполнения**. Он выводит простого круга двумя способами &mdash; без заливки и с заливкой &mdash; переключаться касание. [ `TapToggleFillPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml.cs) Класс показано, как можно изменить SkiaSharp графики в ответ на ввод данных пользователем.

Для этой страницы `SKCanvasView` создается экземпляр класса в [TapToggleFill.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/TapToggleFillPage.xaml) файл, который также задает Xamarin.Forms [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) в представлении:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.TapToggleFillPage"
             Title="Tap Toggle Fill">

    <skia:SKCanvasView PaintSurface="OnCanvasViewPaintSurface">
        <skia:SKCanvasView.GestureRecognizers>
            <TapGestureRecognizer Tapped="OnCanvasViewTapped" />
        </skia:SKCanvasView.GestureRecognizers>
    </skia:SKCanvasView>
</ContentPage>
```

Обратите внимание `skia` объявление пространства имен XML.

`Tapped` Обработчик `TapGestureRecognizer` объект просто Инвертирует логическое поле и вызывает [ `InvalidateSurface` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.SKCanvasView.InvalidateSurface()/) метод `SKCanvasView`:

```csharp
bool showFill = true;
...
void OnCanvasViewTapped(object sender, EventArgs args)
{
    showFill ^= true;
    (sender as SKCanvasView).InvalidateSurface();
}
```

Вызов `InvalidateSurface` фактически создает вызов `PaintSurface` обработчик, который использует `showFill` поля для заполнения или не заполняет круга:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 50
    };
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);

    if (showFill)
    {
        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.Blue;
        canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    }
}
```

`StrokeWidth` Свойство значение 50 для акцентирования разницу. Ширина всей строки с помощью рисования внутренней сначала и затем структуры также видно. По умолчанию графики схемы формируемого далее в `PaintSurface` обработчик событий скрывать те ранее в обработчик.

**Цвет исследовать** страницы показано, как можно интегрировать SkiaSharp графики с другими элементами Xamarin.Forms, а также иллюстрирует различие между два альтернативных способа определения цветов в SkiaSharp. Статический [ `SKColor.FromHsl` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsl/p/System.Single/System.Single/System.Single/System.Byte/) метод создает `SKColor` значение на основе цветового тона, насыщенности, освещенности модели:

```csharp
public static SKColor FromHsl (Single h, Single s, Single l, Byte a)
```

Статический [ `SKColor.FromHsv` ](https://developer.xamarin.com/api/member/SkiaSharp.SKColor.FromHsv/p/System.Single/System.Single/System.Single/System.Byte/) метод создает `SKColor` значение в соответствии с аналогичной модели цветового тона, насыщенности, значение:

```csharp
public static SKColor FromHsv (Single h, Single s, Single v, Byte a)
```

В обоих случаях `h` аргумент лежит в диапазоне от 0 до 360. `s`, `l`, И `v` аргументы в диапазоне от 0 до 100. `a` (Определяющего коэффициент альфа или прозрачность) аргумент лежит в диапазоне от 0 до 255.

[ **ColorExplorePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml) файл создает два `SKCanvasView` объекты в `StackLayout` рядом с `Slider` и `Label` представления, которые позволяют пользователю выбрать HSL и HSV цветовых значений:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Basics.ColorExplorePage"
             Title="Color Explore">
    <StackLayout>
        <!-- Hue slider -->
        <Slider x:Name="hueSlider"
                Maximum="360"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference hueSlider},
                              Path=Value,
                              StringFormat='Hue = {0:F0}'}" />

        <!-- Saturation slider -->
        <Slider x:Name="saturationSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference saturationSlider},
                              Path=Value,
                              StringFormat='Saturation = {0:F0}'}" />

        <!-- Lightness slider -->
        <Slider x:Name="lightnessSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference lightnessSlider},
                              Path=Value,
                              StringFormat='Lightness = {0:F0}'}" />

        <!-- HSL canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hslCanvasView"
                               PaintSurface="OnHslCanvasViewPaintSurface" />

            <Label x:Name="hslLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>

        <!-- Value slider -->
        <Slider x:Name="valueSlider"
                Maximum="100"
                Margin="20, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label HorizontalTextAlignment="Center"
               Text="{Binding Source={x:Reference valueSlider},
                              Path=Value,
                              StringFormat='Value = {0:F0}'}" />

        <!-- HSV canvas view -->
        <Grid VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="hsvCanvasView"
                               PaintSurface="OnHsvCanvasViewPaintSurface" />

            <Label x:Name="hsvLabel"
                   HorizontalOptions="Center"
                   VerticalOptions="Center"
                   BackgroundColor="Black"
                   TextColor="White" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Два `SKCanvasView` элементов находятся в одной ячейке `Grid` с `Label` сидели в верхней части страницы для отображения результирующее значение цвета RGB.

[ **ColorExplorePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/ColorExplorePage.xaml.cs) файл кода программной части относительно проста. Возможно, общий `ValueChanged` обработчик для трех `Slider` элементы просто делает недействительными и `SKCanvasView` элементы. `PaintSurface` Обработчики снимите полотно цветом обозначается `Slider` элементов и также задать `Label` сидели на основе `SKCanvasView` элементов:

```csharp
public partial class ColorExplorePage : ContentPage
{
    public ColorExplorePage()
    {
        InitializeComponent();

        hueSlider.Value = 0;
        saturationSlider.Value = 100;
        lightnessSlider.Value = 50;
        valueSlider.Value = 100;
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        hslCanvasView.InvalidateSurface();
        hsvCanvasView.InvalidateSurface();
    }

    void OnHslCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsl((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)lightnessSlider.Value);
        args.Surface.Canvas.Clear(color);

        hslLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }

    void OnHsvCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKColor color = SKColor.FromHsv((float)hueSlider.Value,
                                        (float)saturationSlider.Value,
                                        (float)valueSlider.Value);
        args.Surface.Canvas.Clear(color);

        hsvLabel.Text = String.Format(" RGB = {0:X2}-{1:X2}-{2:X2} ",
                                      color.Red, color.Green, color.Blue);
    }
}
```

В моделях цвет HSL и HSV значение оттенка лежит в диапазоне от 0 до 360 и указывает главным цветового тона цвета. Это традиционный цвета радуги: красный, оранжевый, желтый, зеленый, синий, indigo фиолетовый и обратно в кружке на красный.

В модели HSL нулевое значение для яркости всегда является black и 100 значением всегда является белым. При нулевом значении насыщенность светлые значения в диапазоне от 0 до 100 являются оттенков серого. Увеличение насыщенности добавляет дополнительные цвета. Чистые цвета (которые значения RGB одного компонента, равными 255, другой равно 0, а третий от 0 до 255) возникают, когда насыщенность равно 100, а яркость — 50.

В модели HSV чистых цветов привести при насыщенность и значение 100. Если значение равно 0, независимо от других параметров, черный цвет. Оттенки серого возникают, когда насыщенность имеет значение 0, а значение лежит в диапазоне от 0 до 100.

Но чтобы почувствовать для двух моделей рекомендуется поэкспериментировать с ними:

[![](integration-images/colorexplore-large.png "Тройной снимок экрана со страницей исследовать цвет")](integration-images/colorexplore-small.png#lightbox "тройной экрана просмотра цвет страницы")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
