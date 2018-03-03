---
title: "Основные сведения о пути"
description: "Просмотр объектов SkiaSharp SKPath для объединения соединенных линий и кривых"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EDA6C2-3921-4021-89F3-211551E430F1
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: f02c5cfd75fd9d9cd97d28ca276b32808f7a45ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="path-basics"></a>Основные сведения о пути

_Просмотр объектов SkiaSharp SKPath для объединения соединенных линий и кривых_

Один из самых важных возможностей пути графики является возможность определения соединением несколько строк, и если они не должны быть подключены. Разница может быть довольно видимой, как верхние края этих двух треугольников показывают:

![](paths-images/connectedlinesexample.png "Два треугольники с различиями между строками подключенном и отключенном")

Графический контур инкапсулируется [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) объекта. Путь — это коллекция из одной или нескольких *контуров*. Каждый профиль представляет собой коллекцию *подключен* прямых линий и кривых. Профили не подключены друг к другу, но они могут визуально перекрываться. Иногда один профиль может перекрывать сам себя.

Профиль обычно начинается с вызова метода следующие `SKPath`:

- `MoveTo` Чтобы начать новый контур

Аргумент для этого метода является единственной точкой, которого можно выразить как `SKPoint` значение или как отдельный X и Y координаты. `MoveTo` Вызова устанавливает точку в начале контуру и первоначальный *текущей точке*. Можно вызывать следующие методы для продолжения контурной линией или кривой с текущей точки к точке, заданной в методе, которая становится новой текущей точки:

- `LineTo` Добавление прямой путь
- `ArcTo` Чтобы добавить дугу, которая представляет собой строку на длины окружности или эллипса
- `CubicTo` Чтобы добавить сплайна Безье третьего порядка
- `QuadTo` для добавления квадратичной кривой Безье
- `ConicTo` Чтобы добавить rational квадратичной сплайн Безье, которая точно можно визуализировать conic разделы (эллипсы, параболы и гиперболы)

Ни один из этих пяти методов содержит все сведения, необходимые для описания линии или кривой. Каждый из этих пяти методов работает совместно с текущей точки, заданные в вызове метода, непосредственно предшествующий. Например `LineTo` метод добавляет прямую линию в профиль на основе текущей точки таким образом параметра `LineTo` является единственной точкой.

`SKPath` Класса также определяет методы, которые имеют те же имена, что эти шесть методов, но с `R` в начале:

- `RMoveTo`
- `RLineTo`
- `RArcTo`
- `RCubicTo`
- `RQuadTo`
- `RConicTo`

`R` Расшифровывается *относительный*. Они имеют тот же синтаксис, что и соответствующие методы без `R` , но относительно текущей точки. Это удобно для рисования аналогичные части пути в методе, который можно вызывать несколько раз.

Профиль заканчивается другой вызов `MoveTo` или `RMoveTo`, что позволяет начать новый контур, или вызов `Close`, которой закрывает контуру. `Close` Метод автоматически добавляет прямую линию от текущей точки до первой точки контуру и отмечает путь как закрытые, это означает, что он будет изображен без все прописные штриха.

Различие между открытые и закрытые профили поясняет **двух контуров треугольник** страницы, которая использует `SKPath` объекта с помощью двух контуров для подготовки к просмотру двух треугольников. Первый профиль открыт и закрывается за секунду. Вот [ `TwoTriangleContours` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/TwoTriangleContoursPage.cs) класса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create the path
    SKPath path = new SKPath();

    // Define the first contour
    path.MoveTo(0.5f * info.Width, 0.1f * info.Height);
    path.LineTo(0.2f * info.Width, 0.4f * info.Height);
    path.LineTo(0.8f * info.Width, 0.4f * info.Height);
    path.LineTo(0.5f * info.Width, 0.1f * info.Height);

    // Define the second contour
    path.MoveTo(0.5f * info.Width, 0.6f * info.Height);
    path.LineTo(0.2f * info.Width, 0.9f * info.Height);
    path.LineTo(0.8f * info.Width, 0.9f * info.Height);
    path.Close();

    // Create two SKPaint objects
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Magenta,
        StrokeWidth = 50
    };

    SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Cyan
    };

    // Fill and stroke the path
    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

Первый профиль состоит из вызова [ `MoveTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.MoveTo/p/System.Single/System.Single/) с помощью координат X и Y, а не `SKPoint` значения с три вызова [ `LineTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.LineTo/p/System.Single/System.Single/) для рисования три стороны треугольник. Второй профиль содержит только два вызова `LineTo` , но ее завершения профиль с помощью вызова [ `Close` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Close()/), который закрывает контур. Различие важно:

[![](paths-images/twotrianglecontours-small.png "Тройной снимок экрана со страницей двух контуров треугольник")](paths-images/twotrianglecontours-large.png "тройной снимок экрана со страницей двух контуров треугольник")

Как видите, первый профиль очевидно, что представляет собой ряд три соединенных линий, но конца не соединяет с самого начала. В верхней перекрываться две строки. Второй профиль очевидно закрывается и выполнялось с одним меньше `LineTo` вызывает, поскольку `Close` метод автоматически добавляет завершающую строку, чтобы закрыть контур.

`SKCanvas` определяется только один [ `DrawPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawPath/p/SkiaSharp.SKPath/SkiaSharp.SKPaint/) метод, который в этом примере вызывается дважды для заполнения и обводки контура. Все профили заполняется, даже те, которые не закрыты. В целях заполнения незакрытые пути прямую предполагается существует между началом и конечные точки контуров. При удалении последнего `LineTo` из первого профиль или удалите `Close` вызова из второй контуру каждый профиль будет иметь только двух сторон но заполняется, как если бы он был треугольника.

`SKPath` Определяет множество методов и свойств. Следующие методы позволяют добавлять все профили на путь, который может быть закрыт или не закрыт, в зависимости от метода:

- `AddRect`
- [`AddRoundedRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddRoundedRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddCircle`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddCircle/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathDirection/)
- [`AddOval`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddOval/p/SkiaSharp.SKRect/SkiaSharp.SKPathDirection/)
- [`AddArc`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) Чтобы добавить кривой на длины окружности эллипса
- `AddPath` Чтобы добавить другой путь к текущему пути
- [`AddPathReverse`](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddPathReverse/p/SkiaSharp.SKPath/) Чтобы добавить другой путь в обратном порядке

Имейте в виду, что `SKPath` определяет объект типа geometry & #x 2014; последовательность точек и подключений. Только если `SKPath` используется в сочетании с `SKPaint` объект — это путь к просмотру с определенным цветом, толщина и т. д. Кроме того, имейте в виду, что `SKPaint` объект, переданный в `DrawPath` метод определяет характеристики весь путь. Если вы хотите нарисовать что-нибудь требует нескольких цветов, необходимо использовать отдельный путь для каждого цвета.

Определяется так же, как внешний вид начало и конец строки определяется окончаний обводки, внешний вид соединения между двумя строками *штриха соединения*. Укажите это, задав [ `StrokeJoin` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeJoin/) свойство `SKPaint` на член [ `SKStrokeJoin` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStrokeJoin/) перечисления:

- [`Miter`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Miter/) для объединения отделе
- [`Round`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Round/) для соединения со скругленными
- [`Bevel`](https://developer.xamarin.com/api/field/SkiaSharp.SKStrokeJoin.Bevel/) для объединения жесткому off

**Соединения штриха** страниц отображает этих трех вычерчивания соединения с помощью кода, чтобы **штриха Caps** страницы. Это `PaintSurface` обработчик событий в [ `StrokeJoinsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/StrokeJoinsPage.cs) класса:

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
        TextAlign = SKTextAlign.Right
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

    float xText = info.Width - 100;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = 2 * textPaint.FontSpacing;
    string[] strStrokeJoins = { "Miter", "Round", "Bevel" };

    foreach (string strStrokeJoin in strStrokeJoins)
    {
        // Display text
        canvas.DrawText(strStrokeJoin, xText, y, textPaint);

        // Get stroke-join value
        SKStrokeJoin strokeJoin;
        Enum.TryParse(strStrokeJoin, out strokeJoin);

        // Create path
        SKPath path = new SKPath();
        path.MoveTo(xLine1, y - 80);
        path.LineTo(xLine1, y + 80);
        path.LineTo(xLine2, y + 80);

        // Display thick line
        thickLinePaint.StrokeJoin = strokeJoin;
        canvas.DrawPath(path, thickLinePaint);

        // Display thin line
        canvas.DrawPath(path, thinLinePaint);
        y += 3 * textPaint.FontSpacing;
    }
}
```

Вот программу на трех платформ.

[![](paths-images/strokejoins-small.png "Тройной снимок экрана со страницей соединяет штриха")](paths-images/strokejoins-large.png "тройной снимок экрана со страницей соединяет штриха")

Соединения среза состоит из острым точки, где строки подключения. Если две строки соединения небольших углом, среза соединения могут быть довольно длинными. Во избежание слишком длинные среза соединения длину среза соединения ограничивается значение [ `StrokeMiter` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeMiter/) свойство `SKPaint`. Свойства соединения, в которых превышает длину обрезана в качестве соединения багетной рамки.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
