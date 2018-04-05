---
title: Три типа кривых Безье
description: Показано, как использовать SkiaSharp для подготовки к просмотру кривых Безье третьего, квадратичных и conic
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8FE0F6DC-16BC-435F-9626-DD1790C0145A
author: charlespetzold
ms.author: chape
ms.date: 05/25/2017
ms.openlocfilehash: c5142a3abcc6d461bc277faeb02e3aacd9727bca
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2018
---
# <a name="three-types-of-bzier-curves"></a>Три типа кривых Безье

_Показано, как использовать SkiaSharp для подготовки к просмотру кривых Безье третьего, квадратичных и conic_

Кривая Безье назван Пьер Безье (1910 – 1999 г.), французский инженером в автоматизированных компании Renault, рабочей области для проектирования автоматизированных тел car кривой.

Кривых Безье известны за то, что хорошо подходит для интерактивного дизайна: они являются правильно &mdash; иными словами, не существует, вызывающие кривую, чтобы стать бесконечное или неудобным singularities &mdash; и обычно являются красоту при помощи . Контуров символов на компьютере шрифтов, обычно определяются с кривых Безье:

![](beziers-images/beziersample.png "Образец кривая Безье")

Статья Википедии о [кривой Безье](https://en.wikipedia.org/wiki/B%C3%A9zier_curve) содержит некоторые полезные сведения. Термин *кривой Безье* фактически относится к семейству аналогичные кривых. SkiaSharp поддерживает три типа кривых Безье, называемых *третьего*, *квадратичные*и *conic*. Conic называется также *rational квадратичная*.

## <a name="the-cubic-bzier-curve"></a>Кривая Безье третьего порядка

Кривой Безье третьего порядка — это тип кривой Безье, большинство разработчиков считают когда субъект кривых Безье.

Можно добавить кривую Безье третьего порядка для `SKPath` с помощью [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) метод с тремя `SKPoint` параметры, или [ `CubicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CubicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/System.Single/) перегрузку с отдельными `x` и `y` параметры:

```csharp
public void CubicTo (SKPoint point1, SKPoint point2, SKPoint point3)

public void CubicTo (Single x1, Single y1, Single x2, Single y2, Single x3, Single y3)
```

Кривая начинается в текущей точке контур. Полный кривую Безье третьего порядка определяется четырьмя точками:

- Начальная точка: текущей точки в контур, или (0, 0), если `MoveTo` не вызывается
- Сначала опорную точку: `point1` в `CubicTo` вызова
- Во-вторых, точки управления: `point2` в `CubicTo` вызова
- Конечная точка: `point3` в `CubicTo` вызова

Результирующая Кривая начинается начальную точку и заканчивается в конечной точке. Кривая обычно не проходит через две контрольные точки; Вместо этого они работают много like магниты опрос кривой к их.

Лучший способ почувствовать кривой Безье третьего порядка предусмотрено экспериментов. Это назначение **кривой Безье** страницы, который является производным от `InteractivePage`. [ **BezierCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml) создает файл `SKCanvasView` и `TouchEffect`. [ **BezierCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCurvePage.xaml.cs) файл кода программной части создаются четыре `TouchPoint` объектов в конструкторе. `PaintSurface` Обработчик событий создает `SKPath` для подготовки к просмотру кривую Безье, в зависимости от четыре `TouchPoint` объектов, а также выводит пунктирная касательные из точки управления для конечных точек:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with cubic Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.CubicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     touchPoints[3].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[2].Center.X,
                    touchPoints[2].Center.Y,
                    touchPoints[3].Center.X,
                    touchPoints[3].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
       touchPoint.Paint(canvas);
    }
}
```

Здесь выполняется на всех трех платформах:

[![](beziers-images/beziercurve-small.png "Тройной снимок экрана со страницей кривой Безье")](beziers-images/beziercurve-large.png#lightbox "тройной снимок экрана со страницей кривой Безье")

Математически кривая является кубический полинома. Кривая максимум пересекает прямой линии в трех местах. Начальной точки кривой всегда является касательной, а в направлении прямую линию от начала пункты первой контрольной точки. В конечной точке кривой всегда является точки касательной, а в направлении прямую линию от второго элемента управления в конечную точку.

Кривая Безье третьего порядка, всегда ограничен выпуклая четырехугольника осуществляется плавный четыре точки подключения. Это называется *выпуклая оболочка*. Если контрольные точки находятся на прямую линию между начальной и конечной точки, кривая Безье визуализацию виде прямой линии. Но кривой также может пересекать себя, как показано на снимке экрана устройства Windows Mobile.

Профиль пути может содержать несколько соединенных кривых Безье третьего, но будет использоваться для smooth, только если colinear следующие три точки соединения между двумя кривых Безье третьего порядка (то есть, находящихся на прямую линию):

- второй контрольной точки кривой первый
- Конечная точка первого кривой, которая также является начальной точкой второй кривой
- Первая контрольная точка второй кривой

В следующей статье на [ **данные пути SVG** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md) вы обнаружите средства для упрощения определения smooth соединенных кривых Безье.

Иногда бывает полезно знать базовой параметрического уравнения, которые отрисовки кривой Безье третьего порядка. Для *t* от 0 до 1, параметрических уравнений таковы:

x(t) = (1 – t) ³x₀ + 3t (1 – t) ²x₁ + 3t² (1 – t) x₂ + t³x₃

y(t) = (1 – t) ³y₀ + 3t (1 – t) ²y₁ + 3t² (1 – t) y₂ + t³y₃

Высокий показатель степени 3 подтверждает, что они третьего polynomials. Можно легко убедитесь, что при `t` равен 0, точки (x₀ y₀), который является начальной точкой и когда `t` имеет значение 1, то точка — (x₃ y₃), который является конечной точкой. Вблизи начальную точку (для низкого значения `t`), первой контрольной точки (x₁, y₁) имеет надежный эффект и вблизи конечной точки (высокие значения из 't') второй контрольной точки (x₂, y₂) существенно влияет.

## <a name="bzier-curve-approximation-to-circular-arcs"></a>Приближения кривой Безье в дуги окружности

Иногда бывает удобно использовать для подготовки к просмотру дугу кривой Безье. Кривую Безье третьего могут использоваться для аппроксимации сегмент дуги очень хорошо до круг квартала, четырех соединенных кривых Безье можно определить круг целиком. Такое приближение рассматривается в двух статей, опубликованных более 25 лет назад:

> Tor Dokken, и т. п., «Хорошее приближение кругов, кривых Безье непрерывная кривизны,» *компьютера автоматизированного геометрические разработки 7* (1990 г.), 33 41.

> Майкл Goldapp, «Аппроксимации дуг с третьего Polynomials» *автоматизированного проектирования геометрические 8* 227 238 (1991).

На следующей диаграмме показаны четыре точки с меткой `pto`, `pt1`, `pt2`, и `pt3` определения кривой Безье (показаны красным цветом), соответствующий сегмент дуги:

![](beziers-images/bezierarc45.png "Приближения дуги кривой Безье")

Строк из начальной и конечной точек в контрольные точки тангенса круг и кривую Безье, и они имеют длину *L*. Первой статье, указанных выше указывает, что наиболее кривую Безье соответствующий сегмент дуги при заданной длины *L* вычисляется следующим образом:

L = 4 × tan(α / 4) / 3

На рисунке углом в 45 градусов, поэтому L равно 0.265. В коде это значение будет будет умножено на нужный радиус окружности.

**Безье дуги** страницы позволяет экспериментировать с определением кривую Безье, для приближения дуги для углов диапазоне до 180 градусов. [ **BezierCircularArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml) создает файл `SKCanvasView` и `Slider` для выбора угол. `PaintSurface` Обработчик событий в [ **BezierCircularArgPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierCircularArcPage.xaml.cs) кода преобразования для установки используется точка (0, 0) в центре полотна. Рисование окружности с центром в точке для сравнения и затем вычисляет две контрольные точки кривой Безье:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 3;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate length of control point line
    float length = radius * 4 * (float)Math.Tan(Math.PI * angle / 180 / 4) / 3;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the end points
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point3 = new SKPoint(radius * sin, radius * cos);

    // Find the control points
    SKPoint point0Normalized = Normalize(point0);
    SKPoint point1 = point0 + new SKPoint(length * point0Normalized.Y,
                                          -length * point0Normalized.X);

    SKPoint point3Normalized = Normalize(point3);
    SKPoint point2 = point3 + new SKPoint(-length * point3Normalized.Y,
                                          length * point3Normalized.X);

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);
    canvas.DrawCircle(point3.X, point3.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point3.X, point3.Y, point2.X, point2.Y, dottedStroke);

    // Draw the Bezier curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.CubicTo(point1, point2, point3);
        canvas.DrawPath(path, redStroke);
    }
}

// Vector methods
SKPoint Normalize(SKPoint v)
{
    float magnitude = Magnitude(v);
    return new SKPoint(v.X / magnitude, v.Y / magnitude);
}

float Magnitude(SKPoint v)
{
    return (float)Math.Sqrt(v.X * v.X + v.Y * v.Y);
}

```

Начальная и конечная точки (`point0` и `point3`) вычисляются на основании обычного параметрических уравнений для окружности. Поскольку круг центрируется на (0, 0), эти точки могут рассматриваться как векторы Радиальный от центра круга для длины окружности. Контрольные точки являются на строки, которые являются касательной круга, поэтому под прямым углом, чтобы эти векторы Радиальный. Вектор под прямым углом является просто исходного вектора с местами координаты X и Y и один из них сделать отрицательное.

Вот программу на трех платформ с тремя различными углами зрения.

[![](beziers-images/beziercirculararc-small.png "Тройной снимок экрана со страницей Безье дуги")](beziers-images/beziercirculararc-large.png#lightbox "тройной снимок экрана со страницей дуги Безье")

Внимательно посмотрите на экране Windows Mobile, и вы увидите, что кривая Безье особенно отличается от полукруга угол равен 180 градусов, когда экран операций ввода-вывода показывает, что видимому помещаются корректно квартал окружность, если угол равен 90 градусов.

Вычисление координаты две контрольные точки является довольно просто, если круг квартал ориентирован следующим образом:

![](beziers-images/bezierarc90.png "Приближение круга квартала кривой Безье")

Если радиус окружности равен 100, затем *L* – 55, и это номеру легко запомнить.

**Возведение в квадрат круга** страницы анимирует рисунок между круг и квадрат. Круг приблизительно оценить по четыре кривых Безье, координаты которого отображаются в первом столбце это определение массива в [ `SquaringTheCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/SquaringTheCirclePage.cs) класса:

```csharp
public class SquaringTheCirclePage : ContentPage
{
    SKPoint[,] points =
    {
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() },
        { new SKPoint(  55,  100), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,   55), new SKPoint( 62.5f,  62.5f), new SKPoint() },
        { new SKPoint( 100,    0), new SKPoint(   125,      0), new SKPoint() },
        { new SKPoint( 100,  -55), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(  55, -100), new SKPoint( 62.5f, -62.5f), new SKPoint() },
        { new SKPoint(   0, -100), new SKPoint(     0,   -125), new SKPoint() },
        { new SKPoint( -55, -100), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,  -55), new SKPoint(-62.5f, -62.5f), new SKPoint() },
        { new SKPoint(-100,    0), new SKPoint(  -125,      0), new SKPoint() },
        { new SKPoint(-100,   55), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint( -55,  100), new SKPoint(-62.5f,  62.5f), new SKPoint() },
        { new SKPoint(   0,  100), new SKPoint(     0,    125), new SKPoint() }
    };
    ...
}
```

Второй столбец содержит координаты четыре кривых Безье, определяющие квадрат, область которого совпадает приблизительно площади круга. (Рисование квадрат с *точное* область заданного круг является классический неразрешимой геометрические проблемы [возведение в квадрат круга](https://en.wikipedia.org/wiki/Squaring_the_circle).) Для подготовки к просмотру квадрат с кривых Безье, две контрольные точки для каждой кривой совпадают, которые colinear с начальной и конечной точками, поэтому кривую Безье отображается в виде прямой линии.

Третий столбец массива равен интерполированные значения для анимации. Страницы устанавливает таймер на 16 миллисекунд и `PaintSurface` обработчик вызывается с заданным уровнем:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    canvas.Translate(info.Width / 2, info.Height / 2);
    canvas.Scale(Math.Min(info.Width / 300, info.Height / 300));

    // Interpolate
    TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
    float t = (float)(timeSpan.TotalSeconds % 3 / 3);   // 0 to 1 every 3 seconds
    t = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;     // 0 to 1 to 0 sinusoidally

    for (int i = 0; i < 13; i++)
    {
        points[i, 2] = new SKPoint(
            (1 - t) * points[i, 0].X + t * points[i, 1].X,
            (1 - t) * points[i, 0].Y + t * points[i, 1].Y);
    }

    // Create the path and draw it
    using (SKPath path = new SKPath())
    {
        path.MoveTo(points[0, 2]);

        for (int i = 1; i < 13; i += 3)
        {
            path.CubicTo(points[i, 2], points[i + 1, 2], points[i + 2, 2]);
        }
        path.Close();

        canvas.DrawPath(path, cyanFill);
        canvas.DrawPath(path, blueStroke);
    }
}
```

Интерполяции точек на основе sinusoidally oscillating значения `t`. Интерполированные точки затем используются для создания ряда из четырех соединенных кривых Безье. Вот анимацию на трех платформах, показывающее ход выполнения из круг в квадрат.

[![](beziers-images/squaringthecircle-small.png "Тройной экрана Squaring странице круг")](beziers-images/squaringthecircle-large.png#lightbox "тройной экрана Squaring круг страницы")

Такие анимации бы невозможно без кривых, типовой гибки, отображаются как дуги и прямых линий.

**Безье бесконечность** страницы также использует возможности кривую Безье для приближения дуги. Вот `PaintSurface` обработчиком [ `BezierInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/BezierInfinityPage.cs) класса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.MoveTo(0, 0);                                // Center
        path.CubicTo(  50,  -50,   95, -100,  150, -100); // To top of right loop
        path.CubicTo( 205, -100,  250,  -55,  250,    0); // To far right of right loop
        path.CubicTo( 250,   55,  205,  100,  150,  100); // To bottom of right loop
        path.CubicTo(  95,  100,   50,   50,    0,    0); // Back to center  
        path.CubicTo( -50,  -50,  -95, -100, -150, -100); // To top of left loop
        path.CubicTo(-205, -100, -250,  -55, -250,    0); // To far left of left loop
        path.CubicTo(-250,   55, -205,  100, -150,  100); // To bottom of left loop
        path.CubicTo( -95,  100,  -50,   50,    0,    0); // Back to center
        path.Close();

        SKRect pathBounds = path.Bounds;
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(0.9f * Math.Min(info.Width / pathBounds.Width,
                                     info.Height / pathBounds.Height));

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 5;

            canvas.DrawPath(path, paint);
        }
    }
}
```

Это может быть хорошим упражнение, чтобы отобразить эти координаты на бумаге график, чтобы увидеть, как они связаны. Знак бесконечности выравнивается по центру относительно точки (0, 0) и двумя циклами имеют центры (–150, 0) и (150, 0) и радиусы 100. В эту серию `CubicTo` команд, вы увидите X координаты точек управления на значения –95 и –205 (эти значения являются –150 плюс и минус 55), 205 и 95 (150 плюс и минус 55), а также 250 и –250 правой и левой сторон. Единственное исключение — когда знак бесконечности пересекает самого в центре. В этом случае контрольные точки имеют координаты вместе с 50 и от – 50, чтобы выровнять кривой рядом с центром.

Вот знак бесконечности на всех трех платформах:

[![](beziers-images/bezierinfinity-small.png "Тройной снимок экрана со страницей бесконечность Безье")](beziers-images/bezierinfinity-large.png#lightbox "тройной снимок экрана со страницей бесконечность Безье")

Это довольно гладкую к центру знак бесконечности, подготовки к просмотру в **дуги бесконечность** страницу [ **три способа, чтобы нарисовать дугу** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md) статьи.

## <a name="the-quadratic-bzier-curve"></a>Кривая Безье

Квадратичной кривой Безье имеет только один контрольной точки и кривой, определяемой требуются три точки: начальную точку, контрольная точка и конечной точки. Параметрических уравнений очень похожи на кривая Безье третьего порядка, за исключением того, высокий показатель степени 2, поэтому полинома квадратичной кривой.

x(t) = (1 – t) ²x₀ + 2t (1 – t) x₁ + t²x₂

y(t) = (1 – t) ²y₀ + 2t (1 – t) y₁ + t²y₂

Для добавления квадратичной кривой Безье в путь, используйте [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/) метода или [ `QuadTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.QuadTo/p/System.Single/System.Single/System.Single/System.Single/) перегрузку с отдельными `x` и `y` координаты:

```csharp
public void QuadTo (SKPoint point1, SKPoint point2)

public void QuadTo (Single x1, Single y1, Single x2, Single y2)
```

Методы добавления кривой с текущей позицией с целью `point2` с `point1` в качестве точки управления.

Можно поэкспериментировать с кривых Безье второго порядка с **квадратичной кривой** страницу, которая очень похожа на **кривой Безье** страницы, отличаясь только наличием только три точки касания. Вот `PaintSurface` обработчик в [ **QuadraticCurve.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/QuadraticCurvePage.xaml.cs) файл кода программной части:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with quadratic Bezier
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.QuadTo(touchPoints[1].Center,
                    touchPoints[2].Center);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

И здесь он выполняется на всех трех платформах:

[![](beziers-images/quadraticcurve-small.png "Тройной снимок экрана со страницей квадратичной кривой")](beziers-images/quadraticcurve-large.png#lightbox "тройной снимок экрана со страницей квадратичной кривой")

Пунктирные линии, касательной к кривой в начальной и конечной точки и в точке управления.

Безье второго удобен, если требуется кривой общие фигуры, но вы предпочитаете удобство одной контрольной точки, а не два. Безье второго отрисовывает более эффективно, чем любой другой кривой, почему он используется системой в Skia для подготовки к просмотру эллиптической дуги.

Однако форма квадратичной кривой Безье не эллипса, поэтому несколько квадратичной Béziers необходимы для аппроксимации эллиптической дуги. Вместо этого Безье второго находится сегмент параболы.

## <a name="the-conic-bzier-curve"></a>Conic кривую Безье

Conic кривую Безье &mdash; также называется rational квадратичной кривой Безье &mdash; — относительно новых дополнение к семейству кривых Безье. Как квадратичной кривой Безье rational квадратичной кривой Безье включает в себя начальной точкой, конечной точкой и одной контрольной точкой. Но также требует rational квадратичной кривой Безье *вес* значение. Он вызывается *rational* квадратичной тем, что коэффициенты параметрического формулы.

Параметрических уравнений для X и Y являются коэффициенты, которые совместно используют того же знаменатель. Вот уравнение для знаменателя для *t* от 0 к 1 и значение веса *w*:

d(t) = (1 – t) ² + 2wt(1 – t) + t²

В теории rational квадратичная может включать в себя три значения отдельный вес, по одному для каждого из трех условий, но их может быть упрощено только одно значение веса в середине термина.

Параметрических уравнений для координат X и Y аналогичны параметрических уравнений для второго Безье, среднего члена также включает значение веса и выражения получается делением знаменатель:

x(t) = ((1 – t) ²x₀ + 2wt (1 – t) x₁ + t²x₂)) ÷ d(t)

y(t) = ((1 – t)²y₀ + 2wt(1 – t)y₁ + t²y₂)) ÷ d(t)

Также называются Rational кривых Безье второго порядка *conics* , так как они могут точно представлять сегменты любого раздела conic &mdash; гиперболы, эллипсы, параболы и кругов.

Чтобы добавить путь rational квадратичной кривой Безье, используйте [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) метода или [ `ConicTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ConicTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) перегрузку с отдельными `x` и `y` координаты:

```csharp
public void ConicTo (SKPoint point1, SKPoint point2, Single weight)

public void ConicTo (Single x1, Single y1, Single x2, Single y2, Single weight)
```

Обратите внимание, конечное `weight` параметра.

**Conic кривой** страницы позволяет экспериментировать с этих кривых. Класс `ConicCurvePage` является производным от класса `InteractivePage`. [ **ConicCurvePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml) создает файл `Slider` установите значение веса от – 2 до 2. [ **ConicCurvePage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCurvePage.xaml.cs) файл кода программной части создает три `TouchPoint` объектов и `PaintSurface` обработчик просто отображает результирующий кривой с касательные к элементу управления точки:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Draw path with conic curve
    using (SKPath path = new SKPath())
    {
        path.MoveTo(touchPoints[0].Center);
        path.ConicTo(touchPoints[1].Center,
                     touchPoints[2].Center,
                     (float)weightSlider.Value);

        canvas.DrawPath(path, strokePaint);
    }

    // Draw tangent lines
    canvas.DrawLine(touchPoints[0].Center.X,
                    touchPoints[0].Center.Y,
                    touchPoints[1].Center.X,
                    touchPoints[1].Center.Y, dottedStrokePaint);

    canvas.DrawLine(touchPoints[1].Center.X,
                    touchPoints[1].Center.Y,
                    touchPoints[2].Center.X,
                    touchPoints[2].Center.Y, dottedStrokePaint);

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}
```

Здесь выполняется на всех трех платформах:

[![](beziers-images/coniccurve-small.png "Тройной снимок экрана со страницей Conic кривой")](beziers-images/coniccurve-large.png#lightbox "тройной снимок экрана со страницей Conic кривой")

Как видите, контрольная точка кажется, что если вес выше по запросу кривой в сторону более. Если вес равен нулю, кривой становится прямую линию от начальной точки к конечной точке.

В теории отрицательных весов, а также вызвать кривую, чтобы Сгибание *немедленно* из точки управления. Тем не менее, весовые коэффициенты – 1 или ниже причина знаменатель в параметрических уравнений стать отрицательным для конкретного значения *t*. Возможно, по этой причине отрицательных весов учитываются в `ConicTo` методы. **Conic кривой** программы позволяет задать отрицательных весов, но как видите, поэкспериментировав отрицательных весов имеют тот же эффект, как вес равно нулю и вызвать прямую должен быть подготовлен к просмотру.

Очень просто для получения контрольной точки и плотность `ConicTo` метод, чтобы нарисовать дугу до (но не включая) полукруга. На следующей диаграмме касательные из начальной и конечной точек соответствует точке управления.

![](beziers-images/conicarc.png "Подготовки conic дуги в дугу окружности")

Тригонометрические функции можно использовать для определения расстояния контрольной точки из центра окружности: это радиус окружности, деленное косинус угла половину α. Чтобы нарисовать дугу между начальной и конечной точек, задайте вес для этой же косинус половину угла. Обратите внимание, что если угол 180 градусов, затем касательные никогда не соответствовать вес равно нулю. Однако для углов меньше, чем 180 градусов, математические работает отлично.

**Conic дуги** страницы это показано. [ **ConicCircularArc.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml) создает файл `Slider` для выбора угол. `PaintSurface` Обработчик в [ **ConicCircularArc.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ConicCircularArcPage.xaml.cs) файл кода вычисляет контрольную точку и вес:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Translate to center
    canvas.Translate(info.Width / 2, info.Height / 2);

    // Draw the circle
    float radius = Math.Min(info.Width, info.Height) / 4;
    canvas.DrawCircle(0, 0, radius, blackStroke);

    // Get the value of the Slider
    float angle = (float)angleSlider.Value;

    // Calculate sin and cosine for half that angle
    float sin = (float)Math.Sin(Math.PI * angle / 180 / 2);
    float cos = (float)Math.Cos(Math.PI * angle / 180 / 2);

    // Find the points and weight
    SKPoint point0 = new SKPoint(-radius * sin, radius * cos);
    SKPoint point1 = new SKPoint(0, radius / cos);
    SKPoint point2 = new SKPoint(radius * sin, radius * cos);
    float weight = cos;

    // Draw the points
    canvas.DrawCircle(point0.X, point0.Y, 10, blackFill);
    canvas.DrawCircle(point1.X, point1.Y, 10, blackFill);
    canvas.DrawCircle(point2.X, point2.Y, 10, blackFill);

    // Draw the tangent lines
    canvas.DrawLine(point0.X, point0.Y, point1.X, point1.Y, dottedStroke);
    canvas.DrawLine(point2.X, point2.Y, point1.X, point1.Y, dottedStroke);

    // Draw the conic
    using (SKPath path = new SKPath())
    {
        path.MoveTo(point0);
        path.ConicTo(point1, point2, weight);
        canvas.DrawPath(path, redStroke);
    }
}
```

Как видите, нет визуального различия между `ConicTo` путь показаны красным цветом и базовой круг, отображаемый для ссылки:

[![](beziers-images/coniccirculararc-small.png "Тройной снимок экрана со страницей Conic дуги")](beziers-images/coniccirculararc-large.png#lightbox "тройной снимок экрана со страницей Conic дуги")

Но угол 180 градусов и сбоя математических операций.

Она сожалению таким образом, `ConicTo` не поддерживает отрицательных весов, поскольку теоретически (с учетом параметрических уравнений), можно выполнить круга другим вызовом `ConicTo` с одной точки, но отрицательное значение веса. Это позволит создание всего круг с только два `ConicTo` на любой угол между (но не включая) основе кривых нуля градусов до 180 градусов.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
