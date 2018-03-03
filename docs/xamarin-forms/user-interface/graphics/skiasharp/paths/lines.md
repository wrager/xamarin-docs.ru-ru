---
title: "Строки и пунктирные завершения отрезков"
description: "Сведения об использовании SkiaSharp для рисования линий с ограничениями различных штриха"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 9a3090873569db2466db9ab25cc105ea59401df3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="lines-and-stroke-caps"></a>Строки и пунктирные завершения отрезков

_Сведения об использовании SkiaSharp для рисования линий с ограничениями различных штриха_

В SkiaSharp Подготовка одна строка может сильно отличается от обработки последовательность соединенных прямых линий. Даже при рисовании одной строки, тем не менее, часто бывает необходимо предоставить строки определенного толщина, а ширина строки, тем большую важность становится вида конец строки, вызывается *окончаний обводки*:

![](lines-images/strokecapsexample.png "Параметры caps три штриха")

Для рисования одиночных строк `SKCanvas` определяет простой [ `DrawLine` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawLine/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) метода, аргументы которых указать начальное и конечное координаты строку с `SKPaint` объекта:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

По умолчанию `StrokeWidth` свойство вновь созданный экземпляр `SKPaint` объекта равно 0, что действует так же, как значение 1 в визуализации строку на один пиксель в толщину. Отображается слишком тонких на высокое разрешение устройств, таких как телефоны, поэтому возможно, потребуется задать `StrokeWidth` на большее значение. Но после начала Рисование линий изменяемые толщины, вызывает еще одна проблема: как начала и окончания этих строк толстой визуализироваться?

Внешний вид начала и окончания строк называется *отрезка* или в Skia, *окончаний обводки*. Слово «конец» в этом контексте ссылается на тип hat & #x 2014; то, что находится в конце строки. Задать [ `StrokeCap` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeCap/) свойство `SKPaint` объект для одного из следующих членов [ `SKStrokeCap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeCap/) перечисления:

- [`Butt`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Butt/) (по умолчанию)
- [`Square`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeCap.Round/)

Это наиболее наглядно с образцом программы. Во второй части домашней страницы [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) программа начинает со страницей под названием **штриха Caps** на основе [ `StrokeCapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeCapsPage.cs) класса. Эта страница определяет `PaintSurface` обработчик событий, который выполняет цикл по три члена `SKStrokeCap` перечисления отображение имени члена перечисления и Рисование линии с помощью этого окончаний обводки:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Для каждого члена `SKStrokeCap` перечисления, обработчик событий прорисовывает двух строках с толщину обводки 50 пикселей и еще одну строку, размещенная в верхней части с толщину обводки 2 точки. Это вторая строка предназначен для демонстрации геометрические начало и конец строки, независимо от ее толщины и окончаний обводки:

[![](lines-images/strokecaps-small.png "Тройной снимок экрана со страницей Caps штриха")](lines-images/strokecaps-large.png "тройной снимок экрана со страницей пунктирные завершения отрезков")

Как видите, `Square` и `Round` штриха caps эффективно расширить длины строки с размером в половину ширины stroke в начале строки и снова в конце. Это расширение важно в том случае, если необходимо определить размеры объекта отображаемого изображения.

`SKCanvas` Класс также содержит другой метод для отображения нескольких строк, немного необычные:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points` Параметр представляет собой массив `SKPoint` значения и `mode` является членом [ `SKPointMode` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPointMode/) перечисления, который имеет три члена:

- [`Points`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Points/) для отображения отдельных точек
- [`Lines`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Lines/) для каждой пары точек подключения
- [`Polygon`](https://developer.xamarin.com/api/field/SkiaSharp.SKPointMode.Polygon/) для всех последовательных точек подключения

**Несколько строк** страницы демонстрирует этот метод. [ `MultipleLinesPage` XAML-файл](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/MultipleLinesPage.xaml) создаются два `Picker` представления, которые позволяют выбрать членом `SKPointMode` перечисления и членом `SKStrokeCap` перечисления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Points</x:String>
                <x:String>Lines</x:String>
                <x:String>Polygon</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

`SelectedIndexChanged` Обработчик для обоих `Picker` представления просто делает недействительными `SKCanvasView` объекта:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Этот обработчик должен проверить наличие `SKCanvasView` объекта, так как первый обработчик событий вызывается, когда `SelectedIndex` свойство `Picker` имеет значение 0 в файле XAML, и которая появляется перед `SKCanvasView` был создан экземпляр.

`PaintSurface` Обработчик обращается к универсального метода для получения двух выбранных пунктов из `Picker` представления и их преобразования в значения перечисления:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = GetPickerItem<SKStrokeCap>(strokeCapPicker)
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = GetPickerItem<SKPointMode>(pointModePicker);
    canvas.DrawPoints(pointMode, points, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }
    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}
```

На снимке экрана показаны различные `Picker` выбранных элементов на трех платформ:

[![](lines-images/multiplelines-small.png "Тройной снимок экрана со страницей многострочный")](lines-images/multiplelines-large.png "тройной снимок экрана со страницей несколько строк")

IPhone на слева показано как `SKPointMode.Points` вызывает член перечисления `DrawPoints` для каждой точки в визуализации `SKPoint` массив как квадрат, если конец линии — `Butt` или `Square`. Круги подготавливаются к просмотру, если конец линии `Round`.

При использовании вместо `SKPointMode.Lines`, как показано на экране Android в центре `DrawPoints` метод проводит линию между каждой парой `SKPoint` значения, с помощью указанного отрезка, в данном случае `Round`.

Мобильное устройство Windows, отображается результат `SKPointMode.Polygon` значение. Линия между последовательных точек в массиве, но если внимательно очень, вы увидите эти строки не подключены. Каждая из этих отдельных строк начинается и заканчивается указанным отрезка. При выборе `Round` прописные, может появиться строки подключения, но они действительно не подключены.

Являются ли строки, подключен или не подключен является важным аспектом работы с путями графики.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
