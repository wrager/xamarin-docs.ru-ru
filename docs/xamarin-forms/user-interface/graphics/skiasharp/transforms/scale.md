---
title: "Преобразование изменения масштаба"
description: "Обнаружение преобразования масштабирования SkiaSharp масштабирование объектов для различных размеров"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 54A43F3D-9DA8-44A7-9AE4-7E3025129A0B
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: 39e2084bf9ca888d6e39fc5f02a455d3500e568c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="the-scale-transform"></a>Преобразование изменения масштаба

_Обнаружение преобразования масштабирования SkiaSharp масштабирование объектов для различных размеров_

Как видно в [перевод преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md) статья преобразования переноса можно переместить графический объект из одного расположения в другое. В отличие от этого преобразования масштабирования изменяет размер графического объекта:

![](scale-images/scaleexample.png "Высота word масштабируется по размер")

Преобразование изменения масштаба также часто вызывает координаты графики для перемещения, в которых были сделаны большего размера.

Вы уже видели два преобразования формулы, которые описаны последствия фактора перевода `dx` и `dy`:

x' = x + dx

y "= y + dy

Масштабирование коэффициентов `sx` и `sy` являются умножения вместо друг к другу:

x' = sx · x

y "= sy · y

Значения по умолчанию для преобразования факторов — 0; коэффициенты масштабирования значения по умолчанию — 1.

`SKCanvas` Класс определяет четыре `Scale` методы. Первый [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/) метод является для вклад в случаях, когда требуется же горизонтальное и вертикальное масштабирование:

```csharp
public void Scale (Single s)
```

Это называется *мощности* масштабирование &mdash; масштабирования, то в обоих направлениях. Масштабирование мощности сохраняет пропорции объекта.

Второй [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) позволяет указывать различные значения для горизонтальное и вертикальное масштабирование:

```csharp
public void Scale (Single sx, Single sy)
```

В результате *анизотропная* масштабирования.
Третий [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/SkiaSharp.SKPoint/) метод объединяет два коэффициенты масштабирования в одном `SKPoint` значение:

```csharp
public void Scale (SKPoint size)
```

Четвертый `Scale` скоро будет описан метод.

**Основные шкалы** странице демонстрируется `Scale` метод. [ **BasicScalePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml) XAML-файл содержит два `Slider` элементы, которые позволяют выбрать коэффициенты вертикального и горизонтального масштабирования от 0 до 10. [ **BasicScalePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicScalePage.xaml.cs) файл кода программной части использует эти значения для вызова `Scale` до отображения прямоугольник с закругленными углами вычерчивании пунктиром уместиться некоторый текст в левом верхнем углу угол холста:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] {  7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        canvas.Scale((float)xScaleSlider.Value,
                     (float)yScaleSlider.Value);

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);

        float margin = 10;
        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Может возникнуть вопрос: как коэффициенты масштабирования влияют на значение, возвращаемое из `MeasureText` метод `SKPaint`? Ответ: не перезаписываются вообще. `Scale` метод `SKCanvas`. Он не влияет на все с `SKPaint` объекта, пока этот объект используется для подготовки к просмотру, что-нибудь на холсте.

Как видите, все, что рисуемые после `Scale` вызова увеличивается пропорционально:

[![](scale-images/basicscale-small.png "Тройной снимок экрана со страницей основные шкалы")](scale-images/basicscale-large.png#lightbox "тройной снимок экрана со страницей основные шкалы")

Текст, ширина пунктирная линия, длина дефисы в той же строке, округление углов и 10 пикселей между левую и верхнюю границы холста и скругленный прямоугольник подвержены все же коэффициенты масштабирования.

> [!IMPORTANT]
> Универсальная платформа Windows не удалось отобразить правильно anisotropicly масштабируемого текста.

Анизотропная масштабирование причины толщина станут отличается для строки соответствует горизонтальной и вертикальной оси. (Это также видно из первого изображения на этой странице). Если вы не хотите толщину обводки затронет коэффициенты масштабирования, равным 0 и его значение всегда равно одному пикселю, вне зависимости от `Scale` параметр.

Масштабирование — относительно левого верхнего угла холста. Это может оказаться именно вы хотите, но он не может быть. Предположим, что можно разместить текст и прямоугольник в другое место на холсте необходимо масштабировать его относительно его центра. В этом случае можно использовать четвертой версии [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/System.Single/System.Single/) метод, который включает два дополнительных параметра, чтобы указать центр масштабирования:

```csharp
public void Scale (Single sx, Single sy, Single px, Single py)
```

`px` И `py` параметры определяют точку, которая иногда называется *масштабирование center* , но в SkiaSharp документации называется *сводить точки*. Это правило относительно левого верхнего угла холста, масштабирование не затрагивается. Масштабирование все происходит относительно данного центра.

[ **По центру шкалы** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/CenteredScalePage.xaml.cs) показывается, как это работает. `PaintSurface` Обработчик похож **основные шкалы** программы разницей, что `margin` значение вычисляется Центрирование текста по горизонтали, это означает, что программа лучше работает в книжной ориентации:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear(SKColors.SkyBlue);

    using (SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    })
    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 50
    })
    {
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(Title, ref textBounds);
        float margin = (info.Width - textBounds.Width) / 2;

        float sx = (float)xScaleSlider.Value;
        float sy = (float)yScaleSlider.Value;
        float px = margin + textBounds.Width / 2;
        float py = margin + textBounds.Height / 2;

        canvas.Scale(sx, sy, px, py);

        SKRect borderRect = SKRect.Create(new SKPoint(margin, margin), textBounds.Size);
        canvas.DrawRoundRect(borderRect, 20, 20, strokePaint);
        canvas.DrawText(Title, margin, -textBounds.Top + margin, textPaint);
    }
}
```

Верхнего левого угла прямоугольника с закругленными углами располагается `margin` пикселей от левого края холста и `margin` пикселей от верхнего. В последних двух аргументов `Scale` метод присваиваются эти значения, а также ширину и высоту текста, который также является ширину и высоту прямоугольника с закругленными углами. Это означает, что все масштабирования относительно центральной этого прямоугольника:

[![](scale-images/centeredscale-small.png "Тройной снимок экрана со страницей по центру шкалы")](scale-images/centeredscale-large.png#lightbox "тройной снимок экрана со страницей по центру шкалы")

`Slider` Элементы в этой программе имеется широкий &ndash;10 до 10. Как видите, отрицательные значения по вертикали, масштабирование (например, на Android экране в центре) приводят к зеркального отображения относительно горизонтальной оси, проходящей через Центр масштабирования объектов. Отрицательные значения горизонтальное масштабирование (например, как экран Windows справа) приводят к зеркального отображения относительно вертикальной оси, проходящей через Центр масштабирования объектов.

Четвертая версия `Scale` метод является фактически ярлык. Вы можете увидеть, как это работает, заменив `Scale` метод в этот код следующим:

```csharp
canvas.Translate(-px, -py);
```

Это отрицательных результатов координат точек pivot.

Запустите программу повторно. Вы увидите, что «прямоугольник» и «текст сдвигаются, чтобы центр находится в левом верхнем углу холста. Едва видно его. Ползунки не работают Конечно, так как теперь программа плохо масштабируется во всех.

Теперь добавьте базовый `Scale` вызова (без масштабирования center) *перед* , `Translate` вызова:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Если вы знакомы с этого упражнения в других системах программированию графики, может показаться, что это неверно, но он не будет. Skia обрабатывает преобразования последовательные вызовы немного по-разному из что вы не знакомы с.

С помощью последовательных `Scale` и `Translate` вызовов, центр прямоугольника с закругленными углами по-прежнему в левом верхнем углу, а вы теперь может масштабировать относительно левого верхнего угла холста, который также является центр прямоугольника с закругленными углами.

Теперь, перед этим `Scale` вызова добавьте еще один `Translate` вызова центровки значениями:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Масштабированный результат перемещается обратно в исходное положение. Эти три вызова эквивалентны:

```csharp
canvas.Scale(sx, sy, px, py);
```

Преобразования отдельных сочетаются таким образом, общая transform формулу:

 x' = sx · (x — px) + пкс

 y "= sy · (y — py) + py

Имейте в виду, что значения по умолчанию для `sx` и `sy` равны 1. Можно легко, чтобы убедиться, что точки вращения (px, py) не преобразуется этих формул. Он остается в том же расположении относительно полотна.

При объединении `Translate` и `Scale` вызовы, порядок имеет значение. Если `Translate` следует после `Scale`, коэффициенты преобразования эффективно масштабируется с коэффициенты масштабирования. Если `Translate` предшествует `Scale`, коэффициенты преобразования не масштабируются. Этот процесс немного яснее (хотя и более математическое) Если вводится субъект матрицы преобразования.

`SKPath` Класса определяет только для чтения [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) свойство, которое возвращает `SKRect` определение пространства координат в пути. Например, если `Bounds` свойства извлекается из hendecagram путь, созданный ранее, `Left` и `Top` Свойства прямоугольника, – приблизительно 100 `Right` и `Bottom` свойства приблизительно 100 и `Width` и `Height` свойства — около 200. (Большинство фактические значения являются немного меньше, поскольку точки звездочек определяются круг с радиусом 100, но только в верхней точке параллельных с горизонтальной или вертикальной оси.)

Доступность этой информации означает, возможно, наследования масштабирования и перевести факторов, подходящий для масштабирования путь к размер полотна. [ **Анизотропная масштабирование** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicScalingPage.cs) страницы это демонстрируется с 11 указывает звездочкой. *Анизотропная* шкалы означает, что равны в горизонтальном и вертикальном направлениях, это означает, что звезды не будет хранить исходные пропорции. Ниже приведен соответствующий код `PaintSurface` обработчика:

```csharp
SKPath path = HendecagramPage.HendecagramPath;
SKRect pathBounds = path.Bounds;

using (SKPaint fillPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Pink
})
using (SKPaint strokePaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 3,
    StrokeJoin = SKStrokeJoin.Round
})
{
    canvas.Scale(info.Width / pathBounds.Width,
                 info.Height / pathBounds.Height);
    canvas.Translate(-pathBounds.Left, -pathBounds.Top);

    canvas.DrawPath(path, fillPaint);
    canvas.DrawPath(path, strokePaint);
}
```

`pathBounds` Извлекается в верхней части этого кода и затем использовать позже в ширину и высоту холста в прямоугольник `Scale` вызова. Что координаты пути при его отрисовке масштабируется вызов сама по себе `DrawPath` вызов, но звезды будет выравниваться по центру в правом верхнем углу холста. Он должен быть перемещен вниз и влево. Это задание из `Translate` вызова. Эти два свойства `pathBounds` находятся – приблизительно 100, поэтому коэффициенты преобразования около 100. Поскольку `Translate` вызывается после `Scale` вызвать, эти значения масштабируется с коэффициенты масштабирования, эффективно перемещать их центре звезды относительно центральной части холста:

[![](scale-images/anisotropicscaling-small.png "Тройной снимок экрана со страницей анизотропная масштабирование")](scale-images/anisotropicscaling-large.png#lightbox "тройной снимок экрана со страницей анизотропная масштабирование")

Другим способом, можно сравнить `Scale` и `Translate` вызовы является определение эффект в обратном порядке: `Translate` вызов сдвигает путь, она станет видимой но направлены в верхнем левом углу холста. `Scale` Метод затем увеличивает, звезда относительно верхнего левого угла.

На самом деле похоже, что звезды немного больше, чем холста. Проблема заключается в толщину обводки. `Bounds` Свойство `SKPath` указывает размеры координаты кодируются в пути, и это используется программой для масштабирования. При подготовке к просмотру путь с определенной толщина, готовый для просмотра пути больше, чем холста.

Для устранения этой проблемы необходимо компенсировать. Один простой подход в этой программе является добавление следующей инструкции прямо перед `Scale` вызова:

```csharp
pathBounds.Inflate(strokePaint.StrokeWidth / 2,
                   strokePaint.StrokeWidth / 2);
```

Это увеличивает `pathBounds` прямоугольник на 1,5 единиц по всем четырем сторонам. Это приемлемым решением только в том случае, когда соединение штриха округляется. Соединения среза может быть больше времени и сложен для вычисления.

Можно также использовать подобный прием с текстом, как **анизотропная текст** демонстрирует страницы. Вот соответствующая часть `PaintSurface` обработчиком [ `AnisotropicTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AnisotropicTextPage.cs) класса:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    Color = SKColors.Blue,
    StrokeWidth = 0.1f,
    StrokeJoin = SKStrokeJoin.Round
})
{
    SKRect textBounds = new SKRect();
    textPaint.MeasureText("HELLO", ref textBounds);

    // Inflate bounds by the stroke width
    textBounds.Inflate(textPaint.StrokeWidth / 2,
                       textPaint.StrokeWidth / 2);

    canvas.Scale(info.Width / textBounds.Width,
                 info.Height / textBounds.Height);
    canvas.Translate(-textBounds.Left, -textBounds.Top);

    canvas.DrawText("HELLO", 0, 0, textPaint);
}
```

Это аналогичная логика, и дополняет текст по размеру страницы, на основании прямоугольник границы текст, возвращаемый из `MeasureText` (то есть немного больше, чем фактический текст):

[![](scale-images/anisotropictext-small.png "Снимок экрана тройной анизотропная тестовой страницы")](scale-images/anisotropictext-large.png#lightbox "тройной экрана анизотропная тестовой страницы")

Если необходимо сохранить соотношение графических объектов, нужно использовать мощности масштабирование. **Мощности масштабирование** это демонстрируется страницы указывает 11 звезды. По существу для отображения графического объекта в центре страницы с мощности масштабирования, необходимо:

- Перевод центр графического объекта в левом верхнем углу.
- Масштабирование объекта, на основании минимального значения размеров страницы горизонтальные и вертикальные, деленное графический объект измерения.
- Перевод центра масштабируемого объекта относительно центральной части страницы.

[ `IsotropicScalingPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/IsotropicScalingPage.cs) Выполняет следующие действия перед отображением звезды в обратном порядке:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPath path = HendecagramArrayPage.HendecagramPath;
    SKRect pathBounds = path.Bounds;

    using (SKPaint fillPaint = new SKPaint())
    {
        fillPaint.Style = SKPaintStyle.Fill;

        float scale = Math.Min(info.Width / pathBounds.Width,
                               info.Height / pathBounds.Height);

        for (int i = 0; i <= 10; i++)
        {
            fillPaint.Color = new SKColor((byte)(255 * (10 - i) / 10),
                                          0,
                                          (byte)(255 * i / 10));
            canvas.Save();
            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(scale);
            canvas.Translate(-pathBounds.MidX, -pathBounds.MidY);
            canvas.DrawPath(path, fillPaint);
            canvas.Restore();

            scale *= 0.9f;
        }
    }
}
```

Код также отображает звезды десять несколько раз, каждый раз, уменьшение масштабирования вклад в 10% и постепенно изменение красный цвет на синий цвет:

[![](scale-images/isotropicscaling-small.png "Тройной снимок экрана со страницей мощности масштабирование")](scale-images/isotropicscaling-large.png#lightbox "тройной снимок экрана со страницей мощности масштабирование")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
