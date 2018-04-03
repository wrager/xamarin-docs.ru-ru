---
title: Рисование пальцем
description: Пальцы используются для закрашивания на холсте.
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 56929D74-8F2C-44C6-90E6-3FBABCDC0A4B
author: charlespetzold
ms.author: chape
ms.date: 04/05/2017
ms.openlocfilehash: dacb9f399ad044d2d5e9c960bce398092766020c
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="finger-painting"></a>Рисование пальцем

_Пальцы используются для закрашивания на холсте._

`SKPath` Постоянно обновляться и отображаемых объектов. Эта функция позволяет путь используется для интерактивного рисования, таких как finger-painting программы.

![](finger-paint-images/fingerpaintsample.png "Это операция в Рисование пальцем")

Поддержка сенсорного ввода в Xamarin.Forms не позволяет отслеживания отдельных пальцев на экране, поэтому эффекта отслеживания touch Xamarin.Forms были разработаны для поддержки дополнительных сенсорный ввод. Этот эффект, описанное в статье [ **вызов события из эффекты**](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md). Образец программы [ **демонстрации эффекта отслеживания Touch** ](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/) включает две страницы, использующие SkiaSharp, включая finger-painting программы.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) решение включает в себя это событие отслеживания сенсорный ввод. Включает проекта переносимой библиотеки классов `TouchEffect` класса `TouchActionType` перечисления, `TouchActionEventHandler` делегата и `TouchActionEventArgs` класса. Проекты каждой из платформ включают `TouchEffect` класса для этой платформы; iOS проект также содержит `TouchRecognizer` класса.

**Paint пальцем** страницы в **SkiaSharpFormsDemos** — это упрощенная реализация Рисование пальцем. Не выбора цвета и ширины обводки, нет возможности снимите на холст и Конечно не удается сохранить иллюстрации.

[ **FingerPaintPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml) файл помещает `SKCanvasView` в одной ячейке `Grid` и присоединяет `TouchEffect` , `Grid`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Paths.FingerPaintPage"
             Title="Finger Paint">

    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</ContentPage>
```

Присоединение `TouchEffect` непосредственно к `SKCanvasView` не работает в группе всех платформ.

[ **FingerPaintPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/FingerPaintPage.xaml.cs) файл кода определяет две коллекции для хранения `SKPath` объектов, а также `SKPaint` объектов для подготовки к просмотру эти пути:

```csharp
public partial class FingerPaintPage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };

    public FingerPaintPage()
    {
        InitializeComponent();
    }
    ...
}
```

Как имени `inProgressPaths` словарь хранит путями, которые в данный момент отображаются с одного или нескольких пальцами. Ключ словаря — идентификатор сенсорного ввода, прилагаемый к события касания. `completedPaths` Поля — это набор путей, которые были завершены наблюдают палец Рисование путь снята с экрана.

`TouchAction` Обработчик управляет этих двух коллекций. При первом касании пальцем экрана, новый `SKPath` добавляется `inProgressPaths`. После перемещения, пальцем дополнительных точек добавляются к пути. При освобождении палец передается путь `completedPaths` коллекции. Можно рисовать несколько пальцами одновременно. После каждого изменения к одному из путей или коллекциям `SKCanvasView` становится недействительным:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                           (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }
}
```

Точки, сопровождающие события касания отслеживания являются координаты Xamarin.Forms; их необходимо преобразовать в SkiaSharp координаты, что пикселей. Это назначение `ConvertToPixel` метод.

`PaintSurface` Обработчик просто выводит обе коллекции путей. Ранее завершенного путей отображаются под путей выполняется:

```csharp
public partial class FingerPaintPage : ContentPage
{
    ,,,
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (SKPath path in completedPaths)
        {
            canvas.DrawPath(path, paint);
        }

        foreach (SKPath path in inProgressPaths.Values)
        {
            canvas.DrawPath(path, paint);
        }
    }
    ...
}
```

К работе с рисунками пальцем ограничены только вашей talent:

[![](finger-paint-images/fingerpaint-small.png "Тройной снимок экрана со страницей Paint пальцем")](finger-paint-images/fingerpaint-large.png#lightbox "тройной снимок экрана со страницей пальцем рисования")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Демонстрации эффекта отслеживания сенсорного ввода (пример)](https://developer.xamarin.com/samples/xamarin-forms/Effects/TouchTrackingEffectDemos/)
- [Вызов события из эффектов](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
