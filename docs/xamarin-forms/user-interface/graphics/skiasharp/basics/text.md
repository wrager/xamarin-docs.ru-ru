---
title: Интеграция текста и графики
description: Определение размер отображаемого текста строки для интеграции текста с графикой SkiaSharp
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B5AC82-7736-4AD8-AA16-FE43E18D203C
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 1607fe31785b6793175dfb61e1e12e34429aa089
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="integrating-text-and-graphics"></a>Интеграция текста и графики

_Определение размер отображаемого текста строки для интеграции текста с графикой SkiaSharp_

В этой статье показано, как измерять текста, возможно масштабирования текста определенный размер и интегрировать другие графические текст:

![](text-images/textandgraphicsexample.png "Текст, заключены в прямоугольники")

SkiaSharp `Canvas` класс также содержит методы для отрисовки прямоугольника ([`DrawRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRect/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/)) и прямоугольника с закругленными углами ([`DrawRoundRect`](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawRoundRect/p/SkiaSharp.SKRect/System.Single/System.Single/SkiaSharp.SKPaint/)). Эти методы требуют прямоугольник определяется `SKRect` значение.

**Текст в рамке** страницы центров короткая строка текста на странице и его рамка состоит из пары прямоугольников со скругленными углами для окружения. [ `FramedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/FramedTextPage.cs) Класс показано, как это можно сделать.

Используется в SkiaSharp `SKPaint` класс, чтобы задать атрибуты текста и шрифтов, но можно также использовать его для получения отображаемый размер текста. Начало следующие `PaintSurface` обработчик событий вызывает два разных `MeasureText` методы. Первый [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) вызова имеет простую `string` аргумента и возвращает ширину точек текст на основе характеристик текущего шрифта. Затем вычисляется новый `TextSize` свойство `SKPaint` объекта, основанного на этой отображаемую ширину текущего `TextSize` свойство и ширину области отображения. Оно предназначено для задания `TextSize` , чтобы строку текста для отображения на уровне 90% от ширины экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string str = "Hello SkiaSharp!";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Chocolate
    };

    // Adjust TextSize property so text is 90% of screen width
    float textWidth = textPaint.MeasureText(str);
    textPaint.TextSize = 0.9f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(str, ref textBounds);
    ...
}
```

Второй [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/SkiaSharp.SKRect@/) вызов `SKRect` аргумент, поэтому он получает ширину и высоту отображаемого текста. `Height` Это свойство `SKRect` значение зависит от наличия прописных букв, верхний выносной элемент и подстрочные элементы в текстовой строке. Различные `Height` значения передаются текст строки «mom», «cat» и «dog».

`Left` И `Top` свойства `SKRect` структуры указывает координаты верхнего левого угла отображаемого текста, если появится сообщение об ошибке `DrawText` вызов с положения X и Y, 0. Например, если эта программа выполняется на эмулятор iPhone 7 `TextSize` присваивается значение 90.6254 в результате вычисления после первого вызова `MeasureText`. `SKRect` Значение, полученное от второй вызов `MeasureText` имеет следующие значения:

- `Left` = 6
- `Top` = &ndash;68
- `Width` = 664.8214
- `Height` = 88;

Имейте в виду, что координаты X и Y можно передать `DrawText` указать метод в левую часть текста в базовый план. `Top` Значение указывает, что текст выходит 68 пикселей выше эти показатели и (вычитания 68 ИТ-88) 20 пикселей ниже базовой линии. `Left` Значение 6 означает, что текст начинается 6 пикселей справа от значения X в `DrawText` вызова. Это позволяет обычного межзнакового интервала. Если вы хотите отображать текст устроено в левом верхнем углу экрана, передайте отрицательных из этих `Left` и `Top` значений координаты X и Y `DrawText`, в этом примере &ndash;6 и 68.

`SKRect` Структура определяет несколько удобных свойств и методов, некоторые из которых используются в оставшейся части `PaintSurface` обработчика. `MidX` И `MidY` значения указывают координаты центра прямоугольника. (В примере iPhone 7 эти значения являются 338.4107 и &ndash;24.) Следующий код использует эти значения для вычисления простой координат по центру текст с экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(str, xText, yText, textPaint);
    ...
}
```

`PaintSurface` Обработчик завершается два вызова `DrawRoundRect`, для которых требуется аргументов `SKRect`. Это `SKRect` значение аналогично наверняка `SKRect` значение, полученное от `MeasureText` метода, но не могут совпадать. Во-первых, он должен быть немного увеличить, чтобы Скругленный прямоугольник не выводятся над краями текста, а во-вторых, он должен быть перемещен в пространство, чтобы `Left` и `Top` значения соответствуют верхнего левого угла, в которой должен быть прямоугольника разместить. Эти два задания выполняются посредством `Offset` и `Inflate` методов, определенных `SKRect`:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    // Create a new SKRect object for the frame around the text
    SKRect frameRect = textBounds;
    frameRect.Offset(xText, yText);
    frameRect.Inflate(10, 10);

    // Create an SKPaint object to display the frame
    SKPaint framePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 5,
        Color = SKColors.Blue
    };

    // Draw one frame
    canvas.DrawRoundRect(frameRect, 20, 20, framePaint);

    // Inflate the frameRect and draw another
    frameRect.Inflate(10, 10);
    framePaint.Color = SKColors.DarkBlue;
    canvas.DrawRoundRect(frameRect, 30, 30, framePaint);
}
```

Вслед за этим оставшейся части метода тривиальной задачей. Он создает еще один `SKPaint` объекта границ и вызовы `DrawRoundRect` дважды. Второй вызов используется прямоугольник увеличивается за счет другой 10 точек. Первый вызов задает радиус скругления 20 пикселов; второй — скругленных углов 30 пикселей, кажется, не должен быть:

 [![](text-images/framedtext-small.png "Тройной снимок экрана со страницей в рамке текст")](text-images/framedtext-large.png#lightbox "тройной снимок экрана со страницей в рамке текста")

Вы можете включить телефона или симулятор сбоку для просмотра текста и увеличиваются в размере кадра.

Если требуется только некоторые выравнивание текста по центру экрана, это можно сделать приблизительно без измерения текст, задав `TextAlign` свойство `SKPaint` для `SKTextAlign.Center`. Координата X, укажите в `DrawText` метод указывает положение по горизонтали по центру текста. При передаче по центру экрана, чтобы `DrawText` метод, текст будет выравниваться по горизонтали по центру и *почти* вертикально по центру, так как базовый план будет вертикально по центру.

Сам текст может рассматриваться как графические средства намного. Один простой способ состоит в отобразить структуру текстовых символов, а не обычных заполненный отображения:

[![](text-images/outlinedtext-small.png "Тройное снимок экрана: страница описанные текста")](text-images/outlinedtext-large.png#lightbox "Тройное странице описано текст (снимок экрана)")

Для этого простым изменением нормали `Style` свойство `SKPaint` объекта из значения по умолчанию `SKPaintStyle.Fill` для `SKPaintStyle.Stroke` и указав толщину обводки. `PaintSurface` Обработчик **описанные текст** показывается, как это можно сделать:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    string text = "OUTLINE";

    // Create an SKPaint object to display the text
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 1,
        FakeBoldText = true,
        Color = SKColors.Blue
    };

    // Adjust TextSize property so text is 95% of screen width
    float textWidth = textPaint.MeasureText(text);
    textPaint.TextSize = 0.95f * info.Width * textPaint.TextSize / textWidth;

    // Find the text bounds
    SKRect textBounds;
    textPaint.MeasureText(text, ref textBounds);

    // Calculate offsets to center the text on the screen
    float xText = info.Width / 2 - textBounds.MidX;
    float yText = info.Height / 2 - textBounds.MidY;

    // And draw the text
    canvas.DrawText(text, xText, yText, textPaint);
}
```

 В последние несколько статей Вы вами теперь рассмотрено использование SkiaSharp для рисования текста, круги, эллипсы и прямоугольников со скругленными углами. Следующим шагом является [SkiaSharp строки и пути](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md) , в котором будет показано, как рисование линии в графический контур.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
