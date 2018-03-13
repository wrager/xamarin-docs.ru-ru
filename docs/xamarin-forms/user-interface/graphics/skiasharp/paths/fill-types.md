---
title: "Типы заполнения путь"
description: "Обнаружение различных эффектов возможных типов заполнения путь SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: 57103A7A-49A2-46AE-894C-7C2664682644
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: cee93f082386e78a643b8c07dd48d0196e8ecd6c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="the-path-fill-types"></a>Типы заполнения путь

_Обнаружение различных эффектов возможных типов заполнения путь SkiaSharp_

Могут пересекаться двух контуров в пути и строки, которые составляют один профиль может пересекаться с другими. Любой замкнутую область потенциально может быть заполнен, но может не потребоваться заполнения замкнутые области. Ниже приведен пример:

![](fill-types-images/filltypeexample.png "Указывает пяти звездочек частично filles")

У вас есть немного контролировать. Алгоритм заполнение регулируется [ `SKFillType` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.FillType/) свойство `SKPath`, необходимо присвоить значение является членом [ `SKPathFillType` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathFillType/) перечисления:

- [`Winding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.Winding/), значение по умолчанию
- [`EvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.EvenOdd/)
- [`InverseWinding`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseWinding/)
- [`InverseEvenOdd`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathFillType.InverseEvenOdd/)

Вращения и четный алгоритмы определить, является любой замкнутой области заполнения или не заполнено на основании гипотетической линия, проведенная из этой области до бесконечности. Эту строку пересекает одну или несколько границ строк, составляющих путь. С режимом заполнения режимом Если число границ линий, нарисованных в одном направлении баланс из количество линий, нарисованных в обратном направлении, а затем области не заполнены. В противном случае заполнения области. Алгоритм четный Заливка области, если нечетное количество линий границ.

Для многих сопоставление путей поворота алгоритма часто заполняет все замкнутые области пути. Четный алгоритм обычно возвращает более интересные результаты.

Классический пример — звезды пяти указывает, как показано в **Five-Pointed звезда** страницы. [FivePointedStarPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FivePointedStarPage.xaml) файл создает два `Picker` представления, чтобы выбрать путь заполнения тип и путь задан вычерчивании или заполнения или оба и в каком порядке:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.FivePointedStarPage"
             Title="Five-Pointed Star">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="fillTypePicker"
                Title="Path Fill Type"
                Grid.Row="0"
                Grid.Column="0"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Winding</x:String>
                <x:String>EvenOdd</x:String>
                <x:String>InverseWinding</x:String>
                <x:String>InverseEvenOdd</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="drawingModePicker"
                Title="Drawing Mode"
                Grid.Row="0"
                Grid.Column="1"
                Margin="10"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Fill only</x:String>
                <x:String>Stroke only</x:String>
                <x:String>Stroke then Fill</x:String>
                <x:String>Fill then Stroke</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Файл кода использует оба `Picker` значения Нарисуйте звезду пяти указывает:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPath path = new SKPath
    {
        FillType = (SKPathFillType)Enum.Parse(typeof(SKPathFillType),
                        fillTypePicker.Items[fillTypePicker.SelectedIndex])
    };
    path.MoveTo(info.Width / 2, info.Height / 2 - radius);

    for (int i = 1; i < 5; i++)
    {
        // angle from vertical
        double angle = i * 4 * Math.PI / 5;
        path.LineTo(center + new SKPoint(radius * (float)Math.Sin(angle),
                                        -radius * (float)Math.Cos(angle)));
    }
    path.Close();

    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 50,
        StrokeJoin = SKStrokeJoin.Round
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue
    };

    switch (drawingModePicker.SelectedIndex)
    {
        case 0:
            canvas.DrawPath(path, fillPaint);
            break;

        case 1:
            canvas.DrawPath(path, strokePaint);
            break;

        case 2:
            canvas.DrawPath(path, strokePaint);
            canvas.DrawPath(path, fillPaint);
            break;

        case 3:
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, strokePaint);
            break;
    }
}
```

Как правило, должна влиять на тип заливки путь только заливки не штрихов рукописного ввода, но для двух `Inverse` режимы влияют на заливки и обводки. Для заливки, два `Inverse` типы противоположно заполнения областей, чтобы заполнить область за пределами звезды. Для штрихов, два `Inverse` типы цвета все, кроме штриха. С помощью этих типов обратные заполнения может привести некоторые нечетное эффекты, как показано на снимке экрана iOS:

[![](fill-types-images/fivepointedstar-small.png "Тройной снимок экрана со страницей звезда Five-Pointed")](fill-types-images/fivepointedstar-large.png#lightbox "тройной снимок экрана со страницей Five-Pointed звезда")

Снимки мобильных устройств Android и Windows показаны типичные эффекты четный и поворота, но порядок обводки и заливки также влияет на результаты.

Алгоритм поворота является зависимым от оси отображаются линии. Обычно при создании пути, можно управлять, направление задаются нарисовать линии из одной точки в другую. Тем не менее `SKPath` класс также определяет методы, такие как `AddRect` и `AddCircle` , нарисуйте все профили. Чтобы контролировать способ рисования эти объекты, методы включают параметр типа [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/), который имеет два члена:

- [`Clockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.Clockwise/)
- [`CounterClockwise`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathDirection.CounterClockwise/)

Методы в `SKPath` , которые включают `SKPathDirection` параметр ей следует присвоить значение по умолчанию `Clockwise`.

**Перекрывающиеся круги** страница создает путь с четыре круга с типом заполнения четный путь:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(info.Width, info.Height) / 4;

    SKPath path = new SKPath
    {
        FillType = SKPathFillType.EvenOdd
    };

    path.AddCircle(center.X - radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X - radius / 2, center.Y + radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y - radius / 2, radius);
    path.AddCircle(center.X + radius / 2, center.Y + radius / 2, radius);

    SKPaint paint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    canvas.DrawPath(path, paint);

    paint.Style = SKPaintStyle.Stroke;
    paint.StrokeWidth = 10;
    paint.Color = SKColors.Magenta;

    canvas.DrawPath(path, paint);
}
```

Очень интересный образе, созданном с минимумом кода:

[![](fill-types-images/overlappingcircles-small.png "Тройной снимок экрана со страницей перекрывающихся круги")](fill-types-images/overlappingcircles-large.png#lightbox "тройной снимок экрана со страницей перекрывающихся кругов")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
