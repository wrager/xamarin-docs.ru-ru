---
title: "Обрезка с использованием пути и областей"
description: "Использование пути к коллекции графики для конкретной области и для создания областей"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8022FBF9-2208-43DB-94D8-0A4E9A5DA07F
author: charlespetzold
ms.author: chape
ms.date: 06/16/2017
ms.openlocfilehash: e84bce5d4280ded801ed58999a2570d3c6bd327e
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="clipping-with-paths-and-regions"></a>Обрезка с использованием пути и областей

_Использование пути к коллекции графики для конкретной области и для создания областей_

Иногда бывает необходимо ограничить отрисовки графики для конкретной области. Это называется *отсечения*. Можно использовать отсечения для специальные эффекты, такие как этот образ monkey, представляемые стенка с замочной:

![](clipping-images/clippingsample.png "Monkey через стенка с замочной")

*Области обрезки* — это область экрана, в котором отображаются графики. Все, что отображается вне области обрезки не отображается. Области отсечения обычно определяется [ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) объект, но также можно определить области обрезки с помощью [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) объекта. Эти два типа объектов сначала показаться связанные, так как можно создать область из пути. Не удается создать путь из области, но они сильно отличаются внутренне: путь состоит из рядов линий и кривых, пока область определен ряд строк горизонтальной развертки.

На рисунке выше созданные **Monkey через стенка с замочной** страницы. [ `MonkeyThroughKeyholePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/MonkeyThroughKeyholePage.cs) Класс определяет путь, используя данные SVG и использует конструктор для загрузки растрового изображения из ресурсов программы:

```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    SKBitmap bitmap;
    SKPath keyholePath = SKPath.ParseSvgPathData(
        "M 300 130 L 250 350 L 450 350 L 400 130 A 70 70 0 1 0 300 130 Z");

    public MonkeyThroughKeyholePage()
    {
        Title = "Monkey through Keyhole";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Несмотря на то что `keyholePath` объект, который описывает структуру стенка с замочной, полностью произвольны и отражать, что было удобно, если был разработан данных пути. По этой причине `PaintSurface` обработчик получает границы этого пути и вызовы `Translate` и `Scale` перемещение путь к центру экрана, а также почти как высота экрана:


```csharp
public class MonkeyThroughKeyholePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set transform to center and enlarge clip path to window height
        SKRect bounds;
        keyholePath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.98f * info.Height / bounds.Height);
        canvas.Translate(-bounds.MidX, -bounds.MidY);

        // Set the clip path
        canvas.ClipPath(keyholePath);

        // Reset transforms
        canvas.ResetMatrix();

        // Display monkey to fill height of window but maintain aspect ratio
        canvas.DrawBitmap(bitmap,
            new SKRect((info.Width - info.Height) / 2, 0,
                       (info.Width + info.Height) / 2, info.Height));
    }
}
```

Но путь не отображаются. Вместо этого после преобразования, чтобы задать область обрезки при использовании этой инструкции используется путь:

```csharp
canvas.ClipPath(keyholePath);
```

`PaintSurface` Обработчик затем сбрасывает преобразований с помощью вызова `ResetMatrix` и рисуется растровое изображение, чтобы расширить до полного размера экрана. Данный код предполагает точечного рисунка квадратом, являющегося конкретной растрового изображения. Растровое изображение отображается только в пределах области, определяемой контура обрезки:

[![](clipping-images/monkeythroughkeyhole-small.png "Снимок экрана тройной Monkey через страницу стенка с замочной")](clipping-images/monkeythroughkeyhole-large.png#lightbox "тройной экрана Monkey через страницу стенка с замочной")

Контура обрезки действует подчиняется преобразования при `ClipPath` вызове метода, а не для преобразования в силе при графического объекта (например, битовая карта) отображается. Контур обрезки — часть состояния холст, сохраняется вместе с `Save` метод и восстанавливаться с `Restore` метод.

## <a name="combining-clipping-paths"></a>Объединение контура обрезки

Строго говоря, область обрезки не» задается» `ClipPath` метод. Вместо этого он объединяется с существующие контура обрезки, начинается в виде прямоугольника размер равен экрана. Вы можете получить прямоугольник, ограничивающий область обрезки с помощью [ `ClipBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipBounds/) свойство или [ `ClipDeviceBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.ClipDeviceBounds/) свойство. `ClipBounds` Возвращает `SKRect` значение, отражающее любого преобразования, могут применяться. `ClipDeviceBounds` Возвращает `RectI` значение. Это представляет собой прямоугольник с измерениями целое число со знаком и описывает области отсечения в фактические пикселах.

Каждый вызов `ClipPath` уменьшает области отсечения путем объединения области отсечения с новой областью. Полный синтаксис [ `ClipPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipPath/p/SkiaSharp.SKPath/SkiaSharp.SKClipOperation/System.Boolean/) является метод:

```csharp
public void ClipPath(SKPath path, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

Имеется также [ `ClipRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRect/p/SkiaSharp.SKRect/SkiaSharp.SKClipOperation/System.Boolean/) метод, который объединяет прямоугольником отсечения области:

```csharp
public Void ClipRect(SKRect rect, SKClipOperation operation = SKClipOperation.Intersect, Boolean antialias = false);
```

По умолчанию области отсечения итоговые является пересечение существующей области обрезки и `SKPath` или `SKRect` , указанным в `ClipPath` или `ClipRect` метод. Это показано в **Intersect-Clip четыре круги** страницы. `PaintSurface` Обработчик в [ `FourCircleInteresectClipPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourCircleIntersectClipPage.cs) класс использует же `SKPath` объекта создаваемого четыре круга, каждый из которых уменьшает области отсечения через последовательные вызовы `ClipPath`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float size = Math.Min(info.Width, info.Height);
    float radius = 0.4f * size;
    float offset = size / 2 - radius;

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    using (SKPath path = new SKPath())
    {
        path.AddCircle(-offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(-offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, -offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        path.Reset();
        path.AddCircle(offset, offset, radius);
        canvas.ClipPath(path, SKClipOperation.Intersect);

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColors.Blue;
            canvas.DrawPaint(paint);
        }
    }
}
```

Остается пересечение этих четырех кругов:

[![](clipping-images//fourcircleintersectclip-small.png "Тройной снимок экрана со страницей Intersect-Clip четыре круг")](clipping-images/fourcircleintersectclip-large.png#lightbox "тройной снимок экрана со страницей Intersect-Clip четыре окружности")

[ `SKClipOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKClipOperation/) Перечисление имеет только два члена:

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Difference/) Удаляет указанный путь или прямоугольник из существующей области обрезки

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKClipOperation.Intersect/) пересекает указанный путь или прямоугольник с существующей области обрезки

Если заменить четыре `SKClipOperation.Intersect` аргументов в `FourCircleIntersectClipPage` класса `SKClipOperation.Difference`, вы увидите следующее:

[![](clipping-images//fourcircledifferenceclip-small.png "Тройной снимок экрана со страницей Intersect-Clip четыре круг с операцией разницы")](clipping-images/fourcircledifferenceclip-large.png#lightbox "тройной снимок экрана со страницей Intersect-Clip четыре круг с операцией разницы")

Четыре круга были удалены из области отсечения.

**Clip операций** страницы показано различие между эти две операции с парой кругов. Первый круг в левой части добавляется в область обрезки с операцией обрезки по умолчанию из `Intersect`, а второй круг справа добавляется области отсечения с операцией обрезки, обозначенном текстовую метку:

[![](clipping-images//clipoperations-small.png "Тройной снимок экрана со страницей Clip операции")](clipping-images/clipoperations-large.png#lightbox "тройной снимок экрана со страницей Clip операций")

[ `ClipOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ClipOperationsPage.cs) Класс определяет два `SKPaint` объектов как поля, а затем разделяет экрана на две прямоугольной области. Эти области различаются в зависимости от того, является ли телефона в книжном или альбомном режиме. `DisplayClipOp` Класс затем отображает текст и вызовы `ClipPath` кружок с двумя путями для иллюстрации каждой операцией обрезки:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;

    foreach (SKClipOperation clipOp in Enum.GetValues(typeof(SKClipOperation)))
    {
        // Portrait mode
        if (info.Height > info.Width)
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width, y + info.Height / 2), clipOp);
            y += info.Height / 2;
        }
        // Landscape mode
        else
        {
            DisplayClipOp(canvas, new SKRect(x, y, x + info.Width / 2, y + info.Height), clipOp);
            x += info.Width / 2;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKClipOperation clipOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(clipOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    canvas.Save();

    using (SKPath path1 = new SKPath())
    {
        path1.AddCircle(xCenter - radius / 2, yCenter, radius);
        canvas.ClipPath(path1);

        using (SKPath path2 = new SKPath())
        {
            path2.AddCircle(xCenter + radius / 2, yCenter, radius);
            canvas.ClipPath(path2, clipOp);

            canvas.DrawPaint(fillPaint);
        }
    }

    canvas.Restore();
}
```

Вызов `DrawPaint` обычно вызывает весь холст, заполняемый `SKPaint` объекта, но в этом случае метод просто закрашивает внутри области обрезки.

## <a name="exploring-regions"></a>Просмотр областей

Если вы изучили документацию по API для `SKCanvas`, возможно, вы заметили перегрузки `ClipPath` и `ClipRect` методы, которые похожи на методы, описанные выше, но вместо этого имеют параметр с именем [ `SKRegionOperation` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegionOperation/) вместо `SKClipOperation`. `SKRegionOperation` имеет шесть членов, обеспечивающий более гибко объединения путей к областям формы обрезки.

- [`Difference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Difference/)

- [`Intersect`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Intersect/)

- [`Union`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Union/)

- [`XOR`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.XOR/)

- [`ReverseDifference`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.ReverseDifference/)

- [`Replace`](https://developer.xamarin.com/api/field/SkiaSharp.SKRegionOperation.Replace/)

Тем не менее перегруженные версии `ClipPath` и `ClipRect` с `SKRegionOperation` параметры устарели и не может использоваться.

Вы можете продолжать использовать `SKRegionOperation` перечисления, но он требует определения область обрезки в виде [ `SKRegion` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRegion/) объекта.

Только что созданный объект `SKRegion` объект, который описывает пустую область. Обычно является первый вызов в объекте [ `SetRect` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetRect/p/SkiaSharp.SKRectI/) , чтобы прямоугольную область описания области. Параметр, чтобы `SetRect` — `SKRectI` значение &mdash; значение прямоугольника со свойствами целое число со знаком. Затем можно вызвать [ `SetPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.SetPath/p/SkiaSharp.SKPath/SkiaSharp.SKRegion/) с `SKPath` объекта. При этом создается область, совпадает со значением внутреннюю часть пути, но ограничивается начальной прямоугольной области.

`SKRegionOperation` Перечисления только вступает в действие при вызове одного из [ `Op` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRegion.Op/p/SkiaSharp.SKRegion/SkiaSharp.SKRegionOperation/) перегрузки метода, подобные следующему:

```csharp
public Boolean Op(SKRegion region, SKRegionOperation op)
```

Регион, вы вносите `Op` вызов объединяется с областью, указанное в качестве параметра на основе `SKRegionOperation` член. Получив наконец область подходит для обрезки, можно задать в качестве области отсечения холст, используя [ `ClipRegion` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ClipRegion/p/SkiaSharp.SKRegion/SkiaSharp.SKClipOperation/) метод `SKCanvas`:

```csharp
public void ClipRegion(SKRegion region, SKClipOperation operation = SKClipOperation.Intersect)
```

На следующем рисунке показан области обрезки, в зависимости от операций шесть области. Левой круг — регион, `Op` метод будет вызван на, и правой круг области передаваемый `Op` метод:

[![](clipping-images//regionoperations-small.png "Тройной снимок экрана со страницей операций области")](clipping-images/regionoperations-large.png#lightbox "тройной снимок экрана со страницей операций области")

Эти все возможные сочетания этих двух кругов такое Рассматривать как сочетание трех компонентов, что сами по себе, видны в итоговый образ `Difference`, `Intersect`, и `ReverseDifference` операций. Общее число сочетаний является в третьей степени, так и до восьми. Отсутствующие они первоначальном регионе (получаемое в результате вызова не `Op` вообще) и полностью пустая область.

Трудно области можно использовать для обрезки, поскольку необходимо сначала создать путь, а затем область с помощью этого пути, а затем объедините несколько областей. Общая структура **операций области** страница очень похожа на **Clip операции** , но [ `RegionOperationsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionOperationsPage.cs) класс делит, экрана на шесть категорий и Показывает дополнительную работу, необходимые для использования области для этого задания:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float x = 0;
    float y = 0;
    float width = info.Height > info.Width ? info.Width / 2 : info.Width / 3;
    float height = info.Height > info.Width ? info.Height / 3 : info.Height / 2;

    foreach (SKRegionOperation regionOp in Enum.GetValues(typeof(SKRegionOperation)))
    {
        DisplayClipOp(canvas, new SKRect(x, y, x + width, y + height), regionOp);

        if ((x += width) >= info.Width)
        {
            x = 0;
            y += height;
        }
    }
}

void DisplayClipOp(SKCanvas canvas, SKRect rect, SKRegionOperation regionOp)
{
    float textSize = textPaint.TextSize;
    canvas.DrawText(regionOp.ToString(), rect.MidX, rect.Top + textSize, textPaint);
    rect.Top += textSize;

    float radius = 0.9f * Math.Min(rect.Width / 3, rect.Height / 2);
    float xCenter = rect.MidX;
    float yCenter = rect.MidY;

    SKRectI recti = new SKRectI((int)rect.Left, (int)rect.Top,
                                (int)rect.Right, (int)rect.Bottom);

    using (SKRegion wholeRectRegion = new SKRegion())
    {
        wholeRectRegion.SetRect(recti);

        using (SKRegion region1 = new SKRegion(wholeRectRegion))
        using (SKRegion region2 = new SKRegion(wholeRectRegion))
        {
            using (SKPath path1 = new SKPath())
            {
                path1.AddCircle(xCenter - radius / 2, yCenter, radius);
                region1.SetPath(path1);
            }

            using (SKPath path2 = new SKPath())
            {
                path2.AddCircle(xCenter + radius / 2, yCenter, radius);
                region2.SetPath(path2);
            }

            region1.Op(region2, regionOp);

            canvas.Save();
            canvas.ClipRegion(region1);
            canvas.DrawPaint(fillPaint);
            canvas.Restore();
        }
    }
}
```

Вот Существенная разница между `ClipPath` метод и `ClipRegion` метод:

> [!IMPORTANT]
> В отличие от `ClipPath` метода `ClipRegion` метод не зависит от преобразования.

Чтобы понять обоснование это различие, необходимо понять, какие область. Если предполагается о как клип операции или операций области может быть реализована внутренне, скорее всего, кажется очень сложным. Объединяются несколько путей потенциально очень сложными и структуре результирующий путь, скорее всего, алгоритмической кошмаром.

Но это задание упрощено значительно, если каждый путь сокращается до ряд строк горизонтальной развертки, такие как в традиционные вакуумных трубку телевизоры. Каждая строка сканирования является просто горизонтальную линию с начальной и конечной точки. Например круг с радиусом 10 может быть разложено на 20 строк горизонтальной развертки, каждый из которых начинается с левой части круга и заканчивается в правой части. Объединение двух кругов с любой области операции становится очень простой, так как он является просто проверка координаты начальной и конечной каждой пары соответствующих строк развертки.

Это то, что область: набора строк-горизонтальной проверки, определяющий область.

Тем не менее, когда область сокращается до ряд сканирования линий, эти строки основаны на измерении пикселя сканирования. Строго говоря область не графический объект vector. Она будет ближе по своей природе сжатых монохромный точечный рисунок чем пути. Следовательно областей нельзя масштабировать или поворачивать без потери точности и поэтому они не преобразуются при использовании для области обрезки.

Тем не менее можно применить преобразования в области рисования в целях. **Область рисования** программе наглядно показано внутреннее характер регионов. [ `RegionPaintPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RegionPaintPage.cs) Класс создает `SKRegion` объекта, основанного на `SKPath` окружности radius блок-10. Затем преобразование разворачивается, вращающимся для заполнения страницы.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int radius = 10;

    // Create circular path
    using (SKPath circlePath = new SKPath())
    {
        circlePath.AddCircle(0, 0, radius);

        // Create circular region
        using (SKRegion circleRegion = new SKRegion())
        {
            circleRegion.SetRect(new SKRectI(-radius, -radius, radius, radius));
            circleRegion.SetPath(circlePath);

            // Set transform to move it to center and scale up
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(Math.Min(info.Width / 2, info.Height / 2) / radius);

            // Fill region
            using (SKPaint fillPaint = new SKPaint())
            {
                fillPaint.Style = SKPaintStyle.Fill;
                fillPaint.Color = SKColors.Orange;

                canvas.DrawRegion(circleRegion, fillPaint);
            }

            // Stroke path for comparison
            using (SKPaint strokePaint = new SKPaint())
            {
                strokePaint.Style = SKPaintStyle.Stroke;
                strokePaint.Color = SKColors.Blue;
                strokePaint.StrokeWidth = 0.1f;

                canvas.DrawPath(circlePath, strokePaint);
            }
        }
    }
}
```

`DrawRegion` Вызов заполняет область оранжевым цветом, хотя `DrawPath` вызов обводки исходный путь синего цвета для сравнения:

[![](clipping-images//regionpaint-small.png "Тройной снимок экрана со страницей области рисования")](clipping-images/regionpaint-large.png#lightbox "тройной снимок экрана со страницей области рисования")

Область — это именно ряд дискретных координаты.

Если не требуется использовать в связи с вашей области обрезки преобразования, можно использовать области отсечения, как **клевера конечные – четырех** демонстрирует страницы. [ `FourLeafCloverPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/FourLeafCloverPage.cs) Класса составного регион из четырех областей циклические конструкции, задает в качестве области отсечения этого составного региона, а затем выводит ряд 360 прямых линий, при в центре страницы:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float xCenter = info.Width / 2;
    float yCenter = info.Height / 2;
    float radius = 0.24f * Math.Min(info.Width, info.Height);

    using (SKRegion wholeScreenRegion = new SKRegion())
    {
        wholeScreenRegion.SetRect(new SKRectI(0, 0, info.Width, info.Height));

        using (SKRegion leftRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion rightRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion topRegion = new SKRegion(wholeScreenRegion))
        using (SKRegion bottomRegion = new SKRegion(wholeScreenRegion))
        {
            using (SKPath circlePath = new SKPath())
            {
                // Make basic circle path
                circlePath.AddCircle(xCenter, yCenter, radius);

                // Left leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, 0));
                leftRegion.SetPath(circlePath);

                // Right leaf
                circlePath.Transform(SKMatrix.MakeTranslation(2 * radius, 0));
                rightRegion.SetPath(circlePath);

                // Make union of right with left
                leftRegion.Op(rightRegion, SKRegionOperation.Union);

                // Top leaf
                circlePath.Transform(SKMatrix.MakeTranslation(-radius, -radius));
                topRegion.SetPath(circlePath);

                // Combine with bottom leaf
                circlePath.Transform(SKMatrix.MakeTranslation(0, 2 * radius));
                bottomRegion.SetPath(circlePath);

                // Make union of top with bottom
                bottomRegion.Op(topRegion, SKRegionOperation.Union);

                // Exclusive-OR left and right with top and bottom
                leftRegion.Op(bottomRegion, SKRegionOperation.XOR);

                // Set that as clip region
                canvas.ClipRegion(leftRegion);

                // Set transform for drawing lines from center
                canvas.Translate(xCenter, yCenter);

                // Draw 360 lines
                for (double angle = 0; angle < 360; angle++)
                {
                    float x = 2 * radius * (float)Math.Cos(Math.PI * angle / 180);
                    float y = 2 * radius * (float)Math.Sin(Math.PI * angle / 180);

                    using (SKPaint strokePaint = new SKPaint())
                    {
                        strokePaint.Color = SKColors.Green;
                        strokePaint.StrokeWidth = 2;

                        canvas.DrawLine(0, 0, x, y, strokePaint);
                    }
                }
            }
        }
    }
}
```

Он не действительно выглядит клевера конечные – четырех, но это изображение, которое может быть трудно отрисовки без усечения:

[![](clipping-images//fourleafclover-small.png "Тройной снимок экрана со страницей четырех – конечного клевера")](clipping-images/fourleafclover-large.png#lightbox "тройной снимок экрана со страницей четырех – конечного клевера")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
