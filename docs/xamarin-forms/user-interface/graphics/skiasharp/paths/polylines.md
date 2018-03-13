---
title: "Ломаных линий и параметрических уравнений"
description: "Используйте SkiaSharp для подготовки к просмотру любой строки, которые можно определить с помощью параметрических уравнений"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85AEBB33-E954-4364-A6E1-808FAB197BEE
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: e40fd215d23e7da6f1356bba17fac84ce91007ae
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="polylines-and-parametric-equations"></a>Ломаных линий и параметрических уравнений

_Используйте SkiaSharp для подготовки к просмотру любой строки, которые можно определить с помощью параметрических уравнений_

В более поздних этапах в этом руководстве, вы увидите различные методы, `SKPath` определяет для отображения определенных типов кривых. Тем не менее, иногда бывает необходимо нарисовать тип кривую, не поддерживается напрямую `SKPath`. В этом случае ломаную линию (коллекция соединенных линий) можно использовать для рисования любой кривой, вы можете определить математически. Если строки достаточно мал, а многочисленные достаточного количества, результат будет выглядеть кривой. Это спирали является фактически 3600 мало строк:

![](polylines-images/spiralexample.png "Спирали")

Обычно лучше для определения кривой с точки зрения пару параметрических уравнений. Это уравнения для, координаты X и Y зависят от третьей переменной, иногда называется `t` раз. Например, следующий параметрических уравнений определить круг с радиусом 1 с центром в точке (0, 0) для *t* от 0 до 1:

 x = cos(2πt) y = sin(2πt)

 Если вы хотите радиусом больше 1, можно просто умножить значения синус и косинус, radius и если необходимо перенести в другое расположение, в центр добавьте эти значения:

 x = xCenter + radius·cos(2πt) y = yCenter + radius·sin(2πt)

Для эллипса с параллельными осей, чтобы горизонтальный и вертикальный участвуют два радиусы.

x = xCenter + xRadius·cos(2πt) y = yCenter + yRadius·sin(2πt)

Затем можно поместить эквивалентный код SkiaSharp в цикл, который вычисляет различные области и добавляет их в пути. В следующем примере кода SkiaSharp создает `SKPath` объекта для эллипса, который заполняет область отображения. Цикл непосредственно продолжаются до 360 градусов. Центр является половины ширины и высоты отображаемой области, а также два радиусы:

```csharp
SKPath path = new SKPath();

for (float angle = 0; angle < 360; angle += 1)
{
    double radians = Math.PI * angle / 180;
    float x = info.Width / 2 + (info.Width / 2) * (float)Math.Cos(radians);
    float y = info.Height / 2 + (info.Height / 2) * (float)Math.Sin(radians);

    if (angle == 0)
    {
        path.MoveTo(x, y);
    }
    else
    {
        path.LineTo(x, y);
    }
}
path.Close();
```

Это приводит к эллипса, определяемого 360 мало линиями. При его отрисовке появляется smooth.

Конечно, не обязательно создавать эллипс, используя ломаной линии, так как `SKPath` включает `AddOval` метод, который делает это для вас. Однако может потребоваться нарисовать визуальный объект, который не предоставляется `SKPath`.

**Archimedean спирали** страница содержит код, аналогичный код эллипса, но с важные различия. Обрабатывает вокруг 360 градусов круга 10 раз постоянно Настройка radius:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float radius = Math.Min(center.X, center.Y);

    using (SKPath path = new SKPath())
    {
        for (float angle = 0; angle < 3600; angle += 1)
        {
            float scaledRadius = radius * angle / 3600;
            double radians = Math.PI * angle / 180;
            float x = center.X + scaledRadius * (float)Math.Cos(radians);
            float y = center.Y + scaledRadius * (float)Math.Sin(radians);
            SKPoint point = new SKPoint(x, y);

            if (angle == 0)
            {
                path.MoveTo(point);
            }
            else
            {
                path.LineTo(point);
            }
        }

        SKPaint paint = new SKPaint
        {
            Style = SKPaintStyle.Stroke,
            Color = SKColors.Red,
            StrokeWidth = 5
        };

        canvas.DrawPath(path, paint);
    }
}
```

Результат также называется *арифметические спирали* поскольку смещение между каждого цикла является константой:

[![](polylines-images/archimedeanspiral-small.png "Тройной снимок экрана со страницей спирали Archimedean")](polylines-images/archimedeanspiral-large.png#lightbox "тройной снимок экрана со страницей Archimedean спирали")

Обратите внимание, что `SKPath` создается в `using` блока. Это `SKPath` использует больше памяти, чем `SKPath` объектов в предыдущей программы, которые предлагает, `using` блок подходит лучше освободить все неуправляемые ресурсы.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
