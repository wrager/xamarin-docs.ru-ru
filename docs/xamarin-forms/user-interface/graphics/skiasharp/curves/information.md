---
title: Сведения о пути и перечисления
description: В этой статье объясняется, как получить сведения о путях SkiaSharp и перечисление содержимого и это демонстрируется с примерами кода.
ms.prod: xamarin
ms.assetid: 8E8C5C6A-F324-4155-8652-7A77D231B3E5
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/12/2017
ms.openlocfilehash: 53d1fce20a0e3bc75ba34ab84b2549211567e222
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243796"
---
# <a name="path-information-and-enumeration"></a>Сведения о пути и перечисления

_Получение сведений о пути и перечисление содержимого_

[ `SKPath` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath/) Класс определяет несколько свойств и методов, которые позволяют получить сведения о пути. [ `Bounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.Bounds/) И [ `TightBounds` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPath.TightBounds/) свойства (и связанные методы) получить metrical измерения пути. [ `Contains` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Contains/p/System.Single/System.Single/) Позволяет определить, если определенный момент в путь.

Иногда полезно определить, общая длина линий и кривых линий, составляющих путь. Это не типовой простая задача, поэтому целый класс с именем [ `PathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) выделено на него.

Он также иногда полезно получить графических операций и точки, составляющие путь. Сначала может показаться ненужной этого средства: Если приложение уже создано путь, программа уже известен содержимое. Тем не менее, вы видели, что пути, также могут создаваться с [эффекты путь](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) и путем преобразования [строки текста в контуры](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md). Можно также получить графических операций и точки, составляющие эти пути. Один из вариантов является применение алгоритма преобразования ко всем точкам. Таким образом, методы, такие как обтекание полушария.

![](information-images/pathenumerationsample.png "Перенос на полушария текста")

## <a name="getting-the-path-length"></a>Получение длины пути

В статье [ **путей и текста** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md) вы узнали, как использовать [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) метод для отображения которого базовые следует курса пути текстовой строки. Но что делать, если вы хотите размер текста, чтобы он точно соответствует пути? Для рисования текста, вокруг круга, это просто, так как простой для вычисления длины окружности. Однако длины окружности эллипс или длина кривую Безье не так просто.

[ `SKPathMeasure` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasure/) Класс может помочь. [Конструктор](https://developer.xamarin.com/api/constructor/SkiaSharp.SKPathMeasure.SKPathMeasure/p/SkiaSharp.SKPath/System.Boolean/System.Single/) принимает `SKPath` аргумента и [ `Length` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPathMeasure.Length/) свойство открывает его длину.

Это показано в **длина пути** пример, который основан на **кривой Безье** страницы. [ **PathLengthPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml) файла является производным от `InteractivePage` и включает сенсорный интерфейс:

```xaml
<local:InteractivePage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       xmlns:local="clr-namespace:SkiaSharpFormsDemos"
                       xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
                       xmlns:tt="clr-namespace:TouchTracking"
                       x:Class="SkiaSharpFormsDemos.Curves.PathLengthPage"
                       Title="Path Length">
    <Grid BackgroundColor="White">
        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface" />
        <Grid.Effects>
            <tt:TouchEffect Capture="True"
                            TouchAction="OnTouchEffectAction" />
        </Grid.Effects>
    </Grid>
</local:InteractivePage>
```

[ **PathLengthPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathLengthPage.xaml.cs) файл кода программной части позволяет перемещать четыре точки касания для определения конечных точек и контрольные точки кривой Безье третьего порядка. Три поля определяют строку текста `SKPaint` объекта и рассчитанную ширину текста:

```csharp
public partial class PathLengthPage : InteractivePage
{
    const string text = "Compute length of path";

    static SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Black,
        TextSize = 10,
    };

    static readonly float baseTextWidth = textPaint.MeasureText(text);
    ...
}
```

`baseTextWidth` Поле является ширина текста, на основе `TextSize` значение 10.

`PaintSurface` Обработчик Строит кривую Безье и изменяет размер текста по размеру вдоль его полную длину:

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

        // Get path length
        SKPathMeasure pathMeasure = new SKPathMeasure(path, false, 1);

        // Find new text size
        textPaint.TextSize = pathMeasure.Length / baseTextWidth * 10;

        // Draw text on path
        canvas.DrawTextOnPath(text, path, 0, 0, textPaint);
    }
    ...
}
```

`Length` Свойств только что созданного `SKPathMeasure` объект получает длину пути. Это получается делением `baseTextWidth` значение (то есть ширину текст, основанный на размер шрифта 10), а затем умножается на размер 10 основной текст. Результатом является новый размер текста для отображения текста по этому пути:

[![](information-images/pathlength-small.png "Тройной снимок экрана со страницей длина пути")](information-images/pathlength-large.png#lightbox "тройной снимок экрана со страницей длина пути")

Кривая Безье увеличиваются длинный или короткий, вы увидите изменение размера текста.

## <a name="traversing-the-path"></a>Обзор пути

`SKPathMeasure` можно сделать больше, чем просто измерения длины пути. Для любого значения от 0 до длины пути `SKPathMeasure` объекта можно получить положение на пути и тангенс кривую путь к этой точке. Тангенс доступен как вектор в виде `SKPoint` объект или как поворот, содержащийся в `SKMatrix` объекта. Ниже приведены методы `SKPathMeasure` , получения этих сведений в разнообразных и гибкие способы:

```csharp
Boolean GetPosition (Single distance, out SKPoint position)

Boolean GetTangent (Single distance, out SKPoint tangent)

Boolean GetPositionAndTangent (Single distance, out SKPoint position, out SKPoint tangent)

Boolean GetMatrix (Single distance, out SKMatrix matrix, SKPathMeasureMatrixFlags flag)
```

[ `SKPathMeasureMatrixFlags` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathMeasureMatrixFlags/) Являются:

- [`GetPosition`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPosition/)
- [`GetTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)
- [`GetPositionAndTangent`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathMeasureMatrixFlags.GetPositionAndTangent/)

**Половина канала видел** перемещает фигуру на видел, которая кажется на велосипеде вперед и назад по кривую Безье третьего анимирует страницы:

[![](information-images/unicyclehalfpipe-small.png "Тройной снимок экрана со страницей половина канала видел")](information-images/unicyclehalfpipe-large.png#lightbox "тройной снимок экрана со страницей видел половина-канала")

`SKPaint` Объект, используемый для срезом половина канала и видел определяется как поле в [ `UnicycleHalfPipePage` ]() класса. Также определяется `SKPath` объект для видел:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    SKPaint strokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        StrokeWidth = 3,
        Color = SKColors.Black
    };

    SKPath unicyclePath = SKPath.ParseSvgPathData(
        "M 0 0" +
        "A 25 25 0 0 0 0 -50" +
        "A 25 25 0 0 0 0 0 Z" +
        "M 0 -25 L 0 -100" +
        "A 15 15 0 0 0 0 -130" +
        "A 15 15 0 0 0 0 -100 Z" +
        "M -25 -85 L 25 -85");
    ...
}
```

Этот класс содержит стандартные переопределений `OnAppearing` и `OnDisappearing` методы для анимации. `PaintSurface` Обработчик создает путь для половина канала и затем отображает его. `SKPathMeasure` Объект создается на основе на этот путь:

```csharp
public class UnicycleHalfPipePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath pipePath = new SKPath())
        {
            pipePath.MoveTo(50, 50);
            pipePath.CubicTo(0, 1.25f * info.Height,
                             info.Width - 0, 1.25f * info.Height,
                             info.Width - 50, 50);

            canvas.DrawPath(pipePath, strokePaint);

            using (SKPathMeasure pathMeasure = new SKPathMeasure(pipePath))
            {
                float length = pathMeasure.Length;

                // Animate t from 0 to 1 every three seconds
                TimeSpan timeSpan = new TimeSpan(DateTime.Now.Ticks);
                float t = (float)(timeSpan.TotalSeconds % 5 / 5);

                // t from 0 to 1 to 0 but slower at beginning and end
                t = (float)((1 - Math.Cos(t * 2 * Math.PI)) / 2);

                SKMatrix matrix;
                pathMeasure.GetMatrix(t * length, out matrix,
                                      SKPathMeasureMatrixFlags.GetPositionAndTangent);

                canvas.SetMatrix(matrix);
                canvas.DrawPath(unicyclePath, strokePaint);
            }
        }
    }
}
```

`PaintSurface` Обработчик вычисляет значение `t` , находятся значения от 0 до 1 каждые пять секунд. Затем он использует `Math.Cos` функцию для преобразования, значение `t` диапазоне от 0 до 1, а значение 0, где 0 соответствует видел в начале вверху слева, при 1 соответствует видел вверху справа. Функция косинус приводит скорость порядке канала и быстрым внизу.

Обратите внимание, что это значение `t` необходимо будет умножено на длину пути для первого аргумента `GetMatrix`. Матрица, применяется к `SKCanvas` объект для рисования видел пути.

## <a name="enumerating-the-path"></a>Перечисление путь

Два встроенных классов `SKPath` позволяют перечислить содержимое пути. Эти классы являются [ `SKPath.Iterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+Iterator/) и [ `SKPath.RawIterator` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPath+RawIterator/). Эти два класса похожи, но `SKPath.Iterator` могут исключать элементы в пути с нулевой длиной или близко к длина равна нулю. `RawIterator` Используется в следующем примере.

Вы можете получить объект типа `SKPath.RawIterator` путем вызова [ `CreateRawIterator` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.CreateRawIterator()/) метод `SKPath`. Перечисление путь достигается посредством вызова многократно [ `Next` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.Next/p/SkiaSharp.SKPoint[]/) метод. Передать ему массив из четырех `SKPoint` значения:

```csharp
SKPoint[] points = new SKPoint[4];
...
SKPathVerb pathVerb = rawIterator.Next(points);
```

`Next` Метод возвращает член [ `SKPathVerb` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathVerb/) перечисления. Эти значения указывают определенной команды рисования в пути. Число действительных точек вставлять в массив зависит от этой командой:

- [`Move`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Move/) с точки
- [`Line`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Line/) с двумя точками
- [`Cubic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Cubic/) с четырьмя точками
- [`Quad`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Quad/) с тремя точками
- [`Conic`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Conic/) с тремя точками (а также вызвать [ `ConicWeight` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath+RawIterator.ConicWeight/) метод веса)
- [`Close`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Close/) с одной точкой
- [`Done`](https://developer.xamarin.com/api/field/SkiaSharp.SKPathVerb.Done/)

`Done` Команда указывает, что перечисление завершено.

Обратите внимание, что не `Arc` команд. Это означает, что все дуги преобразуются в кривых Безье, когда добавляется к пути.

Некоторые сведения в `SKPoint` массива является избыточным. Например если `Move` следуют команда `Line` команду, а затем первый из двух точек, которые сопровождают `Line` совпадает со значением `Move` точки. На практике эта избыточность очень полезным. При получении `Cubic` глагол, сопровождаемый все четыре точки, определяющие кривой Безье третьего порядка. Не нужно хранить в текущей позиции, заданные в предыдущей команды.

Проблемный команда, однако является `Close`. Эта команда рисует прямую линию от текущей позиции в начало контуру установлено ранее с помощью `Move` команды. В идеальном случае `Close` команды должен предоставить эти две точки, а не всего одну точку. Что еще хуже является то, что сопутствующий точки `Close` команда всегда является (0, 0). Это означает, что при перечислении через путь, скорее всего, необходимо сохранить `Move` точки и текущей позиции.

Статический [ `PathExtensions` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathExtensions.cs) класс содержит несколько методов, которые преобразуют три вида кривых Безье в ряд очень мала прямые линии, которые приближения кривой. (Параметрического формулы были представлены в статье [ **трех типов кривых Безье**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md).) `Interpolate` Метод разбивает прямую линию в многочисленные коротких строк, которые только одна единица длина:

```csharp
static class PathExtensions
{
    ...
    static SKPoint[] Interpolate(SKPoint pt0, SKPoint pt1)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * pt0.X + t * pt1.X;
            float y = (1 - t) * pt0.Y + t * pt1.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenCubic(SKPoint pt0, SKPoint pt1, SKPoint pt2, SKPoint pt3)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2) + Length(pt2, pt3));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * (1 - t) * pt0.X +
                        3 * t * (1 - t) * (1 - t) * pt1.X +
                        3 * t * t * (1 - t) * pt2.X +
                        t * t * t * pt3.X;
            float y = (1 - t) * (1 - t) * (1 - t) * pt0.Y +
                        3 * t * (1 - t) * (1 - t) * pt1.Y +
                        3 * t * t * (1 - t) * pt2.Y +
                        t * t * t * pt3.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenQuadratic(SKPoint pt0, SKPoint pt1, SKPoint pt2)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            points[i] = new SKPoint(x, y);
        }

        return points;
    }

    static SKPoint[] FlattenConic(SKPoint pt0, SKPoint pt1, SKPoint pt2, float weight)
    {
        int count = (int)Math.Max(1, Length(pt0, pt1) + Length(pt1, pt2));
        SKPoint[] points = new SKPoint[count];

        for (int i = 0; i < count; i++)
        {
            float t = (i + 1f) / count;
            float denominator = (1 - t) * (1 - t) + 2 * weight * t * (1 - t) + t * t;
            float x = (1 - t) * (1 - t) * pt0.X + 2 * weight * t * (1 - t) * pt1.X + t * t * pt2.X;
            float y = (1 - t) * (1 - t) * pt0.Y + 2 * weight * t * (1 - t) * pt1.Y + t * t * pt2.Y;
            x /= denominator;
            y /= denominator;
        }

        return points;
    }

    static double Length(SKPoint pt0, SKPoint pt1)
    {
        return Math.Sqrt(Math.Pow(pt1.X - pt0.X, 2) + Math.Pow(pt1.Y - pt0.Y, 2));
    }
}
```

Все эти методы ссылается на метод расширения `CloneWithTransform` показано ниже. Этот метод копирует путь, перечисление команды пути и создав новый путь на основе данных. Тем не менее, новый путь состоит только из `MoveTo` и `LineTo` вызовов. Все прямые линии и кривые уменьшаются до набора строк очень мала.

При вызове `CloneWithTransform`, передается методу `Func<SKPoint, SKPoint>`, который является функцией, `SKPaint` параметр, возвращающий `SKPoint` значение. Эта функция вызывается для каждой точки применить пользовательское преобразование алгоритма:

```csharp
static class PathExtensions
{
    public static SKPath CloneWithTransform(this SKPath pathIn, Func<SKPoint, SKPoint> transform)
    {
        SKPath pathOut = new SKPath();

        using (SKPath.RawIterator iterator = pathIn.CreateRawIterator())
        {
            SKPoint[] points = new SKPoint[4];
            SKPathVerb pathVerb = SKPathVerb.Move;
            SKPoint firstPoint = new SKPoint();
            SKPoint lastPoint = new SKPoint();

            while ((pathVerb = iterator.Next(points)) != SKPathVerb.Done)
            {
                switch (pathVerb)
                {
                    case SKPathVerb.Move:
                        pathOut.MoveTo(transform(points[0]));
                        firstPoint = lastPoint = points[0];
                        break;

                    case SKPathVerb.Line:
                        SKPoint[] linePoints = Interpolate(points[0], points[1]);

                        foreach (SKPoint pt in linePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[1];
                        break;

                    case SKPathVerb.Cubic:
                        SKPoint[] cubicPoints = FlattenCubic(points[0], points[1], points[2], points[3]);

                        foreach (SKPoint pt in cubicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[3];
                        break;

                    case SKPathVerb.Quad:
                        SKPoint[] quadPoints = FlattenQuadratic(points[0], points[1], points[2]);

                        foreach (SKPoint pt in quadPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Conic:
                        SKPoint[] conicPoints = FlattenConic(points[0], points[1], points[2], iterator.ConicWeight());

                        foreach (SKPoint pt in conicPoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        lastPoint = points[2];
                        break;

                    case SKPathVerb.Close:
                        SKPoint[] closePoints = Interpolate(lastPoint, firstPoint);

                        foreach (SKPoint pt in closePoints)
                        {
                            pathOut.LineTo(transform(pt));
                        }

                        firstPoint = lastPoint = new SKPoint(0, 0);
                        pathOut.Close();
                        break;
                }
            }
        }
        return pathOut;
    }
    ...
}
```

Поскольку клонированного путь сокращается до очень мала прямых линий, функция преобразования имеет возможность выполнения перевод прямых линий и кривых.

Обратите внимание, что метод сохраняет первая точка каждого профиль в переменной с именем `firstPoint` и текущую позицию после каждого Рисование команды в переменной `lastPoint`. Они необходимы для создания последней закрывающей строку при `Close` обнаружил команды.

**GlobularText** образец использует этот метод расширения, на первый взгляд обтекание полушария в объемные эффекты:

[![](information-images/globulartext-small.png "Тройной снимок экрана со страницей Globular текст")](information-images/globulartext-large.png#lightbox "тройной снимок экрана со страницей Globular текста")

[ `GlobularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/GlobularTextPage.cs) Конструктор класса выполняет это преобразование. Он создает `SKPaint` объекта для текста, а затем происходит получение `SKPath` объекта из `GetTextPath` метод. — Это путь, переданный `CloneWithTransform` метод расширения, а также функцию преобразования:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;

    public GlobularTextPage()
    {
        Title = "Globular Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        using (SKPaint textPaint = new SKPaint())
        {
            textPaint.Typeface = SKTypeface.FromFamilyName("Times New Roman");
            textPaint.TextSize = 100;

            using (SKPath textPath = textPaint.GetTextPath("HELLO", 0, 0))
            {
                SKRect textPathBounds;
                textPath.GetBounds(out textPathBounds);

                globePath = textPath.CloneWithTransform((SKPoint pt) =>
                {
                    double longitude = (Math.PI / textPathBounds.Width) *
                                            (pt.X - textPathBounds.Left) - Math.PI / 2;
                    double latitude = (Math.PI / textPathBounds.Height) *
                                            (pt.Y - textPathBounds.Top) - Math.PI / 2;

                    longitude *= 0.75;
                    latitude *= 0.75;

                    float x = (float)(Math.Cos(latitude) * Math.Sin(longitude));
                    float y = (float)Math.Sin(latitude);

                    return new SKPoint(x, y);
                });
            }
        }
    }
    ...
}
```

Функция преобразования сначала вычисляет двух значений с именем `longitude` и `latitude` диапазоне от-π/2, в верхней и левой части текста, чтобы π/2 на справа и снизу от текста. Диапазон этих значений не визуально подходит, поэтому они сокращаются путем умножения 0,75. (Попробуйте код без корректировок. Текст, становится слишком запутанные в Северной и южно полюса и слишком тонких по сторонам.) Эти трехмерного координаты на сферической преобразуются в двухмерной `x` и `y` координаты по стандартной формулы.

Новый путь хранится в виде поля. `PaintSurface` Обработчик просто должна центрирование и масштабирование путь для его отображения на экране:

```csharp
public class GlobularTextPage : ContentPage
{
    SKPath globePath;
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint pathPaint = new SKPaint())
        {
            pathPaint.Style = SKPaintStyle.Fill;
            pathPaint.Color = SKColors.Blue;
            pathPaint.StrokeWidth = 3;
            pathPaint.IsAntialias = true;

            canvas.Translate(info.Width / 2, info.Height / 2);
            canvas.Scale(0.45f * Math.Min(info.Width, info.Height));     // radius
            canvas.DrawPath(globePath, pathPaint);
        }
    }
}
```

Это очень универсальный метод. Если массив эффекты путь описан в [ **эффекты путь** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) статьи не совсем включают в себя нечто представляется, должны быть включены, этот способ для заполнения промежутков.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
