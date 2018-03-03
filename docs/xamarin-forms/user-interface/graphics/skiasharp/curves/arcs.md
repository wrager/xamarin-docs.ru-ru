---
title: "Три способа, чтобы нарисовать дугу"
description: "Узнайте, как определять дуги тремя разными способами в SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/10/2017
ms.openlocfilehash: 236f5da78022d6f6482ed66ffd439c4cd15766a3
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="three-ways-to-draw-an-arc"></a>Три способа, чтобы нарисовать дугу

_Узнайте, как определять дуги тремя разными способами в SkiaSharp_

Дуга — кривой на окружности эллипса, например со скругленными части это знак бесконечности:

![](arcs-images/arcsample.png "Знак бесконечности")

Несмотря на простоту этому определению, нет возможности для определения функции Рисование окружности, удовлетворяющий все потребности и, следовательно, не согласие между системами графики об оптимальном способе, чтобы нарисовать дугу. По этой причине `SKPath` класса не ограничивает сам лишь одним из подходов.

`SKPath` Определяет `AddArc` метод, пять различных `ArcTo` методы и два относительно `RArcTo` методы. Эти методы делятся на три категории, представляющая три различных подхода к указанию дуги. Используемая один зависит от сведений, доступных для определения дуги и как это дуги сочетается с другими графики, которая рисование.

## <a name="the-angle-arc"></a>Угол дуги

Угол дуги способ рисования дуги, необходимо указать прямоугольник, ограничивающий эллипс. Дуги на окружности этот эллипса, который обозначается углов в центре эллипса, делая начало дуги и его длина. Два метода draw угол дуги. Это [ `AddArc` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.AddArc/p/SkiaSharp.SKRect/System.Single/System.Single/) метод и [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKRect/System.Single/System.Single/System.Boolean/) метод:

```csharp
public void AddArc (SKRect oval, Single startAngle, Single sweepAngle)

public void ArcTo (SKRect oval, Single startAngle, Single sweepAngle, Boolean forceMoveTo)
```

Эти методы идентичны Android [ `AddArc` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.AddArc/p/Android.Graphics.RectF/System.Single/System.Single/) и [ `ArcTo` ](https://developer.xamarin.com/api/member/Android.Graphics.Path.ArcTo/p/Android.Graphics.RectF/System.Single/System.Single/System.Boolean/) методы. IOS [ `AddArc` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArc/p/System.Boolean/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) метода аналогичен, но будет ограничен дуг на длины окружности, а не обобщены, чтобы эллипса.

Оба метода начинается с `SKRect` значение, которое определяет расположение и размер эллипса:

![](arcs-images/anglearcoval.png "Овал начала дугу угол")

Дуга является частью данного эллипса по окружности.

`startAngle` Аргумент — по часовой стрелке в градусах относительно горизонтальной линии, соединяющей центра эллипса справа. `sweepAngle` Аргумент является относительно `startAngle`. Ниже приведены `startAngle` и `sweepAngle` значения 60 и 100 градусов, соответственно:

![](arcs-images/anglearcangles.png "Углы, определяющие дугу угол")

Начальный угол начинается дуги. Его длина регулируется угол поворота:

![](arcs-images/anglearchighlight.png "Выделенная угол дуги")

Добавляется к пути с кривой `AddArc` или `ArcTo` метод является просто этой части эллипса окружности, здесь показаны красным цветом:

![](arcs-images/anglearc.png "Угол дуги сам по себе")

`startAngle` Или `sweepAngle` аргументов может быть отрицательным: дуги по часовой стрелке для положительных значений `sweepAngle` и против часовой стрелки для отрицательных значений.

Тем не менее `AddArc` does *не* определить закрытый профиль. При вызове метода `LineTo` после `AddArc`, линия берет начало от конца дуги к моменту `LineTo` метод и тот же самое справедливо для `ArcTo`.

`AddArc` автоматически запускает новый профиль и функционально эквивалентен вызов `ArcTo` с последним аргументом для `true`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, true);
```

Последний аргумент вызывается `forceMoveTo`, в результате чего он фактически `MoveTo` вызывать в начале дуги. Новый профиль, который начинается. То есть не так с последнего аргумента `false`:

```csharp
path.ArcTo (oval, startAngle, sweepAngle, false);
```

Эта версия `ArcTo` проводит линию, начиная с текущей позиции в начало дуги. Это означает, что дуги может быть где-нибудь середине большего профиль.

**Угол дуги** страница позволяет использовать два ползунки для задания начала и углы поворота. В файле XAML создает два `Slider` элементы и `SKCanvasView`. `PaintCanvas` Обработчик в [ **AngleArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/AngleArcPage.xaml.cs) файл рисует Овал и дуги с использованием двух `SKPaint` объекты, определенные как поля:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKRect rect = new SKRect(100, 100, info.Width - 100, info.Height - 100);
    float startAngle = (float)startAngleSlider.Value;
    float sweepAngle = (float)sweepAngleSlider.Value;

    canvas.DrawOval(rect, outlinePaint);

    using (SKPath path = new SKPath())
    {
        path.AddArc(rect, startAngle, sweepAngle);
        canvas.DrawPath(path, arcPaint);
    }
}
```

Как видите, начальный угол и угол поворота может принимать отрицательные значения.

[![](arcs-images/anglearc-small.png "Тройной снимок экрана со страницей угол дуги")](arcs-images/anglearc-large.png "тройной снимок экрана со страницей угол дуги")

Такой подход к созданию дугу является типовой простым и легко производные параметрического уравнения, которые описывают дуги. Знание размера и расположения эллипса и угла начала и очистка, начальная и конечная точки дуги можно вычислить с помощью простого тригонометрические функции:

x = oval. MidX + (овал. Ширина / 2) * cos(angle)

y = oval. MidY + (овал. Высота / 2) * sin(angle)

`angle` Значение `startAngle` или `startAngle + sweepAngle`.

Использование двух углов для определения дугу лучше всего подходит для случаев, когда вы знаете углового длина окружности, необходимого для отрисовки, например, чтобы убедиться в круговой диаграмме. **Разрезанная круговая диаграмма** страницы это показано. [ `ExplodedPieChartPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ExplodedPieChartPage.cs) Класс использует внутренний класс для определения некоторых данных создано и цвета:

```csharp
class ChartData
{
    public ChartData(int value, SKColor color)
    {
        Value = value;
        Color = color;
    }

    public int Value { private set; get; }

    public SKColor Color { private set; get; }
}

ChartData[] chartData =
{
    new ChartData(45, SKColors.Red),
    new ChartData(13, SKColors.Green),
    new ChartData(27, SKColors.Blue),
    new ChartData(19, SKColors.Magenta),
    new ChartData(40, SKColors.Cyan),
    new ChartData(22, SKColors.Brown),
    new ChartData(29, SKColors.Gray)
};

```

`PaintSurface` Обработчик сначала просматривает элементы для вычисления `totalValues` номер. Таким образом он может определить размер каждого элемента как часть общего количества и преобразовать их значение угла:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    int totalValues = 0;

    foreach (ChartData item in chartData)
    {
        totalValues += item.Value;
    }

    SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
    float explodeOffset = 50;
    float radius = Math.Min(info.Width / 2, info.Height / 2) - 2 * explodeOffset;
    SKRect rect = new SKRect(center.X - radius, center.Y - radius,
                             center.X + radius, center.Y + radius);

    float startAngle = 0;

    foreach (ChartData item in chartData)
    {
        float sweepAngle = 360f * item.Value / totalValues;

        using (SKPath path = new SKPath())
        using (SKPaint fillPaint = new SKPaint())
        using (SKPaint outlinePaint = new SKPaint())
        {
            path.MoveTo(center);
            path.ArcTo(rect, startAngle, sweepAngle, false);
            path.Close();

            fillPaint.Style = SKPaintStyle.Fill;
            fillPaint.Color = item.Color;

            outlinePaint.Style = SKPaintStyle.Stroke;
            outlinePaint.StrokeWidth = 5;
            outlinePaint.Color = SKColors.Black;

            // Calculate "explode" transform
            float angle = startAngle + 0.5f * sweepAngle;
            float x = explodeOffset * (float)Math.Cos(Math.PI * angle / 180);
            float y = explodeOffset * (float)Math.Sin(Math.PI * angle / 180);

            canvas.Save();
            canvas.Translate(x, y);

            // Fill and stroke the path
            canvas.DrawPath(path, fillPaint);
            canvas.DrawPath(path, outlinePaint);
            canvas.Restore();
        }

        startAngle += sweepAngle;
    }
}
```

Новый `SKPath` объекта создается для каждого среза круговой диаграммы. Путь состоит из строки в центре то `ArcTo` обратно нарисовать дуги, а другая строка результаты center `Close` вызова. Эта программа отображает «Разрезанная» сегментами, перемещая их все в центре на 50 пикселей. Эта задача требует вектор в направлении среднюю угол поворота для каждого среза:

[![](arcs-images/explodedpiechart-small.png "Тройной снимок экрана со страницей Разрезанная круговая диаграмма")](arcs-images/explodedpiechart-large.png "тройной снимок экрана со страницей Разрезанная круговая диаграмма")

Чтобы увидеть, как оно выглядит без «развертывание», просто закомментировать `Translate` вызова:

[![](arcs-images/explodedpiechartunexploded-small.png "Тройной снимок экрана со страницей Разрезанная круговая диаграмма без развертывания")](arcs-images/explodedpiechartunexploded-large.png "тройной снимок экрана со страницей Разрезанная круговая диаграмма без развертывания")

## <a name="the-tangent-arc"></a>Касательной дуги

Второй тип дуги, поддерживаемых `SKPath` — *касательной дуги*, так называемые, так как длина окружности по касательной две соединенные линии, дуги.

Касательной дуги добавляется путь с помощью вызова [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/SkiaSharp.SKPoint/System.Single/) метод с двумя `SKPoint` параметры, или [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/System.Single/System.Single/) перегрузку с отдельными `Single` параметры для точки:

```csharp
public void ArcTo (SKPoint point1, SKPoint point2, Single radius)

public void ArcTo (Single x1, Single y1, Single x2, Single y2, Single radius)
```

Это `ArcTo` аналогичен методу PostScript [ `arct` ](https://www.adobe.com/products/postscript/pdfs/PLRM.pdf) функции (страницы 532 в документе PDF) и iOS [ `AddArcToPoint` ](https://developer.xamarin.com/api/member/CoreGraphics.CGPath.AddArcToPoint/p/System.nfloat/System.nfloat/System.nfloat/System.nfloat/System.nfloat/) метод.

`ArcTo` Метод состоит из трех точек:

- Текущий точки контура или точку (0, 0), если `MoveTo` не вызывается
- Первый аргумент точки `ArcTo` метод с именем *углов точки*
- Второй аргумент точки `ArcTo`, который называется *конечная точка*:

![](arcs-images/tangentarcthreepoints.png "Три точки, начинающиеся касательной дуги")

Эти три точки определяют две соединенные линии:

![](arcs-images/tangentarcconnectinglines.png "Линии, соединяющие три точки тангенса дуги")

Если три точки являются colinear & #x 2014; то есть, если они находятся на одной прямой линии & #x 2014; отображается не дуги.

`ArcTo` Также включает `radius` параметра. Этот параметр определяет радиус окружности:

![](arcs-images/tangentarccircle.png "Касательной дуги окружности")

Касательной дуги не обобщенная для эллипса.

Если две строки соответствует углом, круг, может быть вставлен между этими строками, чтобы оно было тангенс для обеих строк:

![](arcs-images/tangentarctangentcircle.png "Круг касательной дугу между двумя строками")

Кривой, которая добавляется в профиль не затрагивает либо точек, заданных в `ArcTo` метод. Он состоит из прямую линию от текущей точки первой точки тангенса и дугу, которая заканчивается на второй точке касания.

![](arcs-images/tangentarchighlight.png "Выделенная касательной дугу между двумя строками")

Ниже приведен последний прямой линии и дуги, который добавляется в профиль.

![](arcs-images/tangentarc.png "Выделенная касательной дугу между двумя строками")

Профиль может быть продолжен из второй точке касания.

**Дуги тангенс** страницы позволяет экспериментировать с касательной дуги. Это первое из нескольких страниц, которые являются производными от [ `InteractivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/InteractivePage.cs), который определяет несколько удобных `SKPaint` объектов и выполняет `TouchPoint` обработки:

```csharp
public class InteractivePage : ContentPage
{
    protected SKCanvasView baseCanvasView;
    protected TouchPoint[] touchPoints;

    protected SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3
    };

    protected SKPaint redStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Red,
        StrokeWidth = 15
    };

    protected SKPaint dottedStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        PathEffect = SKPathEffect.CreateDash(new float[] { 7, 7 }, 0)
    };

    protected void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = baseCanvasView.CanvasSize.Width / (float)baseCanvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            baseCanvasView.InvalidateSurface();
        }
    }
}
```

Класс `TangentArcPage` является производным от класса `InteractivePage`. Конструктор в [ **TangentArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml.cs) файла отвечает за создание и инициализация `touchPoints` массива, а параметр `baseCanvasView` (в `InteractivePage`) для `SKCanvasView` создать экземпляр объекта в [ **TangentArcPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/TangentArcPage.xaml) файла:

```csharp
public partial class TangentArcPage : InteractivePage
{
    public TangentArcPage()
    {
        touchPoints = new TouchPoint[3];

        for (int i = 0; i < 3; i++)
        {
            TouchPoint touchPoint = new TouchPoint
            {
                Center = new SKPoint(i == 0 ? 100 : 500,
                                     i != 2 ? 100 : 500)
            };
            touchPoints[i] = touchPoint;
        }

        InitializeComponent();

        baseCanvasView = canvasView;
        radiusSlider.Value = 100;
    }

    void sliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

`PaintSurface` Обработчик использует `ArcTo` Чтобы нарисовать дугу на основании точки касания и `Slider`, но также типовой вычисляет круг, угол основана на:

```csharp
public partial class TangentArcPage : InteractivePage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Draw the two lines that meet at an angle
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.LineTo(touchPoints[1].Center);
            path.LineTo(touchPoints[2].Center);
            canvas.DrawPath(path, dottedStrokePaint);
        }

        // Draw the circle that the arc wraps around
        float radius = (float)radiusSlider.Value;

        SKPoint v1 = Normalize(touchPoints[0].Center - touchPoints[1].Center);
        SKPoint v2 = Normalize(touchPoints[2].Center - touchPoints[1].Center);

        double dotProduct = v1.X * v2.X + v1.Y * v2.Y;
        double angleBetween = Math.Acos(dotProduct);
        float hypotenuse = radius / (float)Math.Sin(angleBetween / 2);
        SKPoint vMid = Normalize(new SKPoint((v1.X + v2.X) / 2, (v1.Y + v2.Y) / 2));
        SKPoint center = new SKPoint(touchPoints[1].Center.X + vMid.X * hypotenuse,
                                     touchPoints[1].Center.Y + vMid.Y * hypotenuse);

        canvas.DrawCircle(center.X, center.Y, radius, this.strokePaint);

        // Draw the tangent arc
        using (SKPath path = new SKPath())
        {
            path.MoveTo(touchPoints[0].Center);
            path.ArcTo(touchPoints[1].Center, touchPoints[2].Center, radius);
            canvas.DrawPath(path, redStrokePaint);
        }

        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
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
}
```

Вот **дуги тангенс** страница, выполняемая на всех трех платформах:

[![](arcs-images/tangentarc-small.png "Тройной снимок экрана со страницей дуги тангенс")](arcs-images/tangentarc-large.png "тройной снимок экрана со страницей тангенс дуги")

На устройстве с Windows Mobile почти colinear тремя точками и дуги очень мал.

Касательной дуги идеально подходит для создания прямоугольника с закругленными углами, например прямоугольник с закругленными углами. Поскольку `SKPath` уже включает в себя `AddRoundedRect` метод, **округленное Heptagon** страницы демонстрируется использование `ArcTo` для округления углов семь сторонний многоугольник. (Для любого правильного многоугольника обобщенный код.)

`PaintSurface` Обработчик [ `RoundedHeptagonPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/RoundedHeptagonPage.cs) класса содержит один `for` цикл для вычисления координат семь вершины heptagon и второй для вычисления средние семь сторон этих вершины. Затем эти средние точки используются для построения пути:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float cornerRadius = 100;
    int numVertices = 7;
    float radius = 0.45f * Math.Min(info.Width, info.Height);

    SKPoint[] vertices = new SKPoint[numVertices];
    SKPoint[] midPoints = new SKPoint[numVertices];

    double vertexAngle = -0.5f * Math.PI;       // straight up

    // Coordinates of the vertices of the polygon
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        vertices[vertex] = new SKPoint(radius * (float)Math.Cos(vertexAngle),
                                       radius * (float)Math.Sin(vertexAngle));
        vertexAngle += 2 * Math.PI / numVertices;
    }

    // Coordinates of the midpoints of the sides connecting the vertices
    for (int vertex = 0; vertex < numVertices; vertex++)
    {
        int prevVertex = (vertex + numVertices - 1) % numVertices;
        midPoints[vertex] = new SKPoint((vertices[prevVertex].X + vertices[vertex].X) / 2,
                                        (vertices[prevVertex].Y + vertices[vertex].Y) / 2);
    }

    // Create the path
    using (SKPath path = new SKPath())
    {
        // Begin at the first midpoint
        path.MoveTo(midPoints[0]);

        for (int vertex = 0; vertex < numVertices; vertex++)
        {
            SKPoint nextMidPoint = midPoints[(vertex + 1) % numVertices];

            // Draws a line from the current point, and then the arc
            path.ArcTo(vertices[vertex], nextMidPoint, cornerRadius);

            // Connect the arc with the next midpoint
            path.LineTo(nextMidPoint);
        }
        path.Close();

        // Render the path in the center of the screen
        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Blue;
            paint.StrokeWidth = 10;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.DrawPath(path, paint);
        }
    }
}

```

Вот программу на трех платформ.

[![](arcs-images/roundedheptagon-small.png "Тройной снимок экрана со страницей округленное Heptagon")](arcs-images/roundedheptagon-large.png "тройной снимок экрана со страницей округленное Heptagon")

## <a name="the-elliptical-arc"></a>Эллиптической дуги

Эллиптической дуги добавляется путь с помощью вызова [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/SkiaSharp.SKPoint/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/SkiaSharp.SKPoint/) метод, который имеет два `SKPoint` параметры, или [ `ArcTo` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ArcTo/p/System.Single/System.Single/System.Single/SkiaSharp.SKPathArcSize/SkiaSharp.SKPathDirection/System.Single/System.Single/) перегрузка с отдельной X и Y координаты:

```csharp
public void ArcTo (SKPoint r, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, SKPoint xy)

public void ArcTo (Single rx, Single ry, Single xAxisRotate, SKPathArcSize largeArc, SKPathDirection sweep, Single x, Single y)
```

Эллиптической дуги согласуется с [эллиптической дуги](http://www.w3.org/TR/SVG11/paths.html#PathDataEllipticalArcCommands) в составе масштабируемой векторной графикой (SVG), а также универсальная платформа Windows [ `ArcSegment` ](/uwp/api/Windows.UI.Xaml.Media.ArcSegment/) класса.

Эти `ArcTo` методы нарисовать дугу между двумя точками, является текущей точкой контуру, и последний параметр `ArcTo` метод ( `xy` параметр или отдельной `x` и `y` параметров):

![](arcs-images/ellipticalarcpoints.png "Две точки, определенные эллиптическую дугу")

Первый параметр точки `ArcTo` метод (`r`, или `rx` и `ry`) вообще не является точкой, но вместо этого задает горизонтальный и вертикальный радиусы эллипса;

![](arcs-images/ellipticalarcellipse.png "Эллипса, который определен эллиптической дуги")

`xAxisRotate` Представляет собой число градусов по часовой стрелке поворот эллипса это:

![](arcs-images/ellipticalarctiltedellipse.png "Tilted эллипса, который определен эллиптической дуги")

Если этот tilted эллипса, который располагается нажмите, чтобы он соприкасается две точки, точки соединяются два разных дуги:

![](arcs-images/ellipticalarcellipse1.png "Первый набор эллиптической дуги.")

Можно отличить эти две дуги двумя способами: верхний дуги больше, чем нижней дуги и как дуги слева направо, top дуги по часовой стрелке пока нижней дуги в направлении против часовой стрелки.

Можно также в соответствии с эллипса между двумя точками другим способом:

![](arcs-images/ellipticalarcellipse2.png "Второй набор эллиптической дуги.")

Теперь имеется меньших дуг в верхней части, которая рисуется по часовой стрелке, а большего размера дуги в нижней, которая рисуется против часовой стрелки.

Эти две точки может быть подключено дугой определяется tilted эллипса всего из четырех способов:

![](arcs-images/ellipticalarccolors.png "Все четыре эллиптической дуги")

Эти четыре дуги различаются по четыре комбинации [ `SKPathArcSize` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathArcSize/) и [ `SKPathDirection` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathDirection/) перечисления аргументов типа для `ArcTo` метод:

- красный: SKPathArcSize.Large и SKPathDirection.Clockwise
- Зеленый: SKPathArcSize.Small и SKPathDirection.Clockwise
- Синий: SKPathArcSize.Small и SKPathDirection.CounterClockwise
- Пурпурный: SKPathArcSize.Large и SKPathDirection.CounterClockwise

Если tilted эллипс не является достаточно большой между двумя точками, затем он равномерно масштабируется до достаточный размер. Только две уникальные дуги в этом случае подключение двумя точками. Их можно отличить с `SKPathDirection` параметра.

Несмотря на то, что этот подход к определению дугу звучит сложных при первом возникновении, это единственный подход, который позволяет определить дуги с поворотом эллипса и часто является простой подход при необходимости интегрировать с другими частями контуру дуги.

**Эллиптической дуги** страница позволяет интерактивно установить две точки и размер и поворот эллипса. `EllipticalArcPage` Класс является производным от `InteractivePage`и `PaintSurface` обработчик в [ **EllipticalArcPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/EllipticalArcPage.xaml.cs) четыре дуги строит файл кода программной части:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        int colorIndex = 0;
        SKPoint ellipseSize = new SKPoint((float)xRadiusSlider.Value,
                                          (float)yRadiusSlider.Value);
        float rotation = (float)rotationSlider.Value;

        foreach (SKPathArcSize arcSize in Enum.GetValues(typeof(SKPathArcSize)))
            foreach (SKPathDirection direction in Enum.GetValues(typeof(SKPathDirection)))
            {
                path.MoveTo(touchPoints[0].Center);
                path.ArcTo(ellipseSize, rotation,
                           arcSize, direction,
                           touchPoints[1].Center);

                strokePaint.Color = colors[colorIndex++];
                canvas.DrawPath(path, strokePaint);
                path.Reset();
            }
    }

    foreach (TouchPoint touchPoint in touchPoints)
    {
        touchPoint.Paint(canvas);
    }
}

```

Здесь выполняется на трех платформах:

[![](arcs-images/ellipticalarc-small.png "Тройной снимок экрана со страницей эллиптической дуги")](arcs-images/ellipticalarc-large.png "тройной снимок экрана со страницей эллиптической дуги")

**Дуги бесконечность** страница использует эллиптической дуги для рисования знак бесконечности. Знак бесконечности основана на двух кругов с радиусы 100 единиц измерения, разделенные 100 единиц.

![](arcs-images/infinitycircles.png "Два круга")

Две строки, пересекающие друг с другом, касательной обоих круги:

![](arcs-images/infinitycircleslines.png "Два круга с касательные")

Значок «бесконечность» представляет собой сочетание части этих окружностей и две строки. Использование эллиптической дуги для рисования знак бесконечности, необходимо определить координаты, где две строки представляют касательной кругов.

Создайте правый прямоугольник в одном из кругов.

![](arcs-images/infinitytriangle.png "Два круга с касательные и внедренные окружности")

Радиус окружности 100 единиц, который гипотенузы треугольник 150 единиц, поэтому угол α арксинус (арксинус) 100 разделить по 150 или 41,8 градусов. Длина стороны треугольника — 150 раз косинус 41,8 градусов или 112, который также может быть рассчитана теоремы Пифагоров.

Координаты точки тангенса затем может быть вычислено с помощью этих сведений:

x = 112·cos(41.8) = 83

y = 112·sin(41.8) = 75

Четыре точки тангенса приведены все необходимое для рисования круга радиусы 100 знак бесконечности по центру в точке (0, 0).

![](arcs-images/infinitycoordinates.png "Два круга с касательные и координаты")

`PaintSurface` Обработчик в [ `ArcInfinityPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/ArcInfinityPage.cs) класс размещает знак бесконечности, чтобы (0, 0) точка располагается по центру страницы и масштабирует путь, по размеру экрана:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPath path = new SKPath())
    {
        path.LineTo(83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.CounterClockwise, 83, -75);
        path.LineTo(-83, 75);
        path.ArcTo(100, 100, 0, SKPathArcSize.Large, SKPathDirection.Clockwise, -83, -75);
        path.Close();

        // Use path.TightBounds for coordinates without control points
        SKRect pathBounds = path.Bounds;

        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / pathBounds.Width,
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

Код использует `Bounds` свойство `SKPath` определяет размеры синус бесконечность его масштабированию размер полотна:

[![](arcs-images/arcinfinity-small.png "Тройной снимок экрана со страницей бесконечность дуги")](arcs-images/arcinfinity-large.png "тройной снимок экрана со страницей бесконечность дуги")

Результат кажется слишком маленьким, подразумевает, что `Bounds` свойство `SKPath` сообщает размером, превышающим путь.

На внутреннем уровне Skia предусматривает аппроксимацию дуги, используя несколько кривых Безье второго порядка. Эти кривые (как можно будет увидеть в следующем разделе) содержат контрольные точки, которые определяют способ рисования кривой, но не являются частью отображаемой кривой. `Bounds` Свойства включает эти контрольные точки.

Чтобы получить более строгого соответствия, используйте `TightBounds` свойство, которое исключает контрольные точки. Ниже приведен в альбомном режиме и с помощью программы `TightBounds` , чтобы получить границы пути:

[![](arcs-images/arcinfinitytightbounds-small.png "Тройной снимок экрана со страницей бесконечность дуги с границами тесной")](arcs-images/arcinfinitytightbounds-large.png "тройной снимок экрана со страницей бесконечность дуги с тесной границами")

Несмотря на то, что соединения между дуги, а также прямые линии, математически smooth, может показаться немного внезапные изменение дуги на прямой линии. На следующей странице представлен лучше знак бесконечности.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
