---
title: Путь данных SVG
description: Определение путей, используя текстовые строки в формате масштабируемой векторной графики
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: 0fcef7ce40a575e015232e1ed5996e6b799dd085
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="svg-path-data"></a>Путь данных SVG

_Определение путей, используя текстовые строки в формате масштабируемой векторной графики_

`SKPath` Класс поддерживает определение объектов весь путь из текстовых строк в формате, заданные в спецификации масштабируемой векторной графикой (SVG). Вы увидите далее в этой статье представления весь путь, подобные следующему в текстовой строке:

![](path-data-images/pathdatasample.png "Образец пути, заданного с данными путь SVG")

SVG-это графики в формате XML, язык программирования для веб-страниц. Поскольку SVG необходимо разрешить пути в разметки, а не в серии вызовов функций, стандартная SVG включает очень краткий способ указания пути всей графики в виде текстовой строки.

В пределах SkiaSharp этот формат называется «SVG путь данных.» Формат также поддерживается в Windows XAML под управлением средах программирования, включая Windows Presentation Foundation и универсальной платформы Windows, где он называется [синтаксис разметки пути](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) или [перемещения и нарисуйте синтаксис команды](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Его также можно использовать в качестве формата обмена для векторных изображений графики, особенно в текстовых файлах, таким как XML.

SkiaSharp определяет два метода со словами `SvgPathData` в их имена:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Статический [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) метод преобразует строку в `SKPath` объекта, пока [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) преобразует `SKPath` объекта в строку.

Вот строка SVG для звездочка указывает пяти центром в точке (0, 0) с радиусом 100.

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Команды, создающие используются буквы `SKPath` объекта. `M` Указывает `MoveTo` вызвать, `L` — `LineTo`, и `Z` — `Close` закрыть профиль. Каждой паре номеров предоставляет точки по оси X и Y. Обратите внимание, что `L` после команды несколько точек, разделенных запятыми. В ряде координат и точки, запятые и пробелы обрабатываются одинаково. Некоторые программисты предпочитают размещать запятые между координаты X и Y, а не между точками, но только необходимые для устранения неоднозначности, запятые и пробелы. Это вполне допустимо:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Синтаксис пути данных SVG формально задокументированных в [8.3 раздел спецификации SVG](http://www.w3.org/TR/SVG11/paths.html#PathData). Здесь приводится сводка.

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Это начинает новый профиль в пути, задав в текущей позиции. Путь данных всегда должен начинаться с `M` команды.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Эта команда добавляет к нему прямой (или строки) и задает новой текущей позиции до конца последней строки. Можно выполнить `L` с нескольких пар *x* и *y* координаты.

## <a name="horizontal-lineto"></a>**Горизонтальная LineTo**

```csharp
H x ...
```

Эта команда добавляет горизонтальная линия в путь и устанавливает новой текущей позиции до конца строки. Можно выполнить `H` команды с несколькими *x* координаты, но он не имеет смысла.

## <a name="vertical-line"></a>**Вертикальная линия**

```csharp
V y ...
```

Эта команда добавляет к нему вертикальной линии и задает новой текущей позиции до конца строки.

## <a name="close"></a>**Закрыть**

```csharp
Z
```

`C` Команда закрывает контуру путем добавления прямую линию от текущей позиции в начало контур.

## <a name="arcto"></a>**ArcTo**

Команду для добавления эллиптическую дугу в профиль является наиболее сложным команды в спецификации весь путь данных SVG. Это только команды, в которых номера может представлять нечто, отличное от значения координат:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

*Rx* и *ыстрой* параметры являются горизонтальный и вертикальный радиусы эллипса. *Угол поворота* — по часовой стрелке в градусах.

Задать *больших дуги флаг* 1 больших дуги или значение 0 для небольших дуги.

Задать *Очистка флаг* 1 по часовой стрелке и значение 0 для против часовой стрелки.

В точке рисуется дуга (*x*, *y*), которая становится новой текущей позиции.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Эта команда добавляет кривую Безье третьего порядка с текущей позицией с целью (*x3*, *y3*), которая становится новой текущей позиции. Точки (*x1*, *y1*) и (*x2*, *y2*) являются контрольными точками.

Можно указать несколько кривых Безье с одной `C` команды. Число точек должно быть кратно 3.

Имеется также команду «smooth» кривой Безье:

```csharp
S x2 y2 x3 y3 ...
```

Эта команда должны выполнить обычной командной Безье (несмотря на то, что, не обязательно). Команда гладкой Безье вычисляет первой контрольной точки, чтобы он является отражением вторую контрольную точку предыдущего Безье вокруг их взаимной точки. Эти три точки, следовательно, colinear и smooth соединение между двумя кривых Безье.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Для квадратичных кривых Безье число точек должно быть кратно 2. Точка управления (*x1*, *y1*) и конечной точки (и новой текущей позиции) является (*x2*, *y2*)

Имеется также smooth квадратичной кривой команды:

```csharp
T x2 y2 ...
```

Контрольная точка, рассчитывается на основании контрольная точка предыдущего квадратичной кривой.

Эти команды также доступны в версиях «относительный», в которых координат точки относительно текущей позиции. Эти команды относительный начинаются с буквы в нижнем регистре, например `c` вместо `C` относительный версии команда Безье третьего порядка.

Это расширение для определения пути данных SVG. Имеется возможность для повторяющейся группы команд или для выполнения любого типа вычисления. Команды для `ConicTo` или других типов спецификаций дуги недоступны.

Статический [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) метод ожидает допустимую строку команд SVG. Если обнаруживается Любая синтаксическая ошибка, метод возвращает `null`. Это указание только ошибки.

[ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Метод удобно использовать для получения данных SVG пути из существующего `SKPath` объекта для передачи в другую программу или для сохранения в формате текстового файла, например XML. ( `ToSvgPathData` Метод не показан в примере кода в этой статье.) Сделать *не* ожидать `ToSvgPathData` для возврата строки, точно соответствующие вызовы методов, которые созданы путь. В частности, вы обнаружите, что дуги преобразуются к нескольким `QuadTo` команды, и как они отображаются в путь данные, возвращенные из `ToSvgPathData`.

**Путь данных Hello** страницу spells слово «HELLO» с использованием данных пути SVG. Как `SKPath` и `SKPaint` объекты определены как поля в [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) класса:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Путь, определение текстовая строка начинается с левого верхнего угла в точке (0, 0). Каждая буква представляет 50 единиц в ширину и высоту 100 единиц и буквы разделяются другой 25 единиц, который означает, что весь путь 350 единиц в ширину.

«H» «Hello» состоит из трех профилей одной строки, пока 'E' два соединенных кривых Безье третьего. Обратите внимание, что `C` шесть точек после команды, и две контрольные точки имеют координаты Y –10 и 110 и помещает их за пределами диапазона координаты Y остальные буквы. «L» представляет две соединенные линии, пока "о — эллипс, отображаемый с `A` команды.

Обратите внимание, что `M` команду, которая начинается с последнего профиль устанавливает положение точки (350, 50), то есть по вертикали по центру левой стороны "о. Как показано ниже первой цифры `A` команды эллипс имеет Горизонтальный радиус 25 и Вертикальный радиус 50. Конечная точка обозначается последнего пара чисел в `A` команду, которая представляет точку (300, 49,9). Это просто намеренно немного отличается от начальной точки. Если конечная точка задана равной начальную точку, дуга не отображается. Чтобы нарисовать полный эллипс, необходимо задать конечную точку близко к (но не равно) начальной точки, или необходимо использовать два или более `A` команд, каждый из них на часть полный эллипс.

Добавьте следующий оператор в конструктор страницы, а затем установите точку останова для изучения итоговые строки может потребоваться:

```csharp
string str = helloPath.ToSvgPathData();
```

Вы обнаружите, что дуги были заменены серию `Q` команды для поэтапного приближения дуги, с помощью кривых Безье второго порядка.

`PaintSurface` Обработчик получает тесной границы пути, который не содержит контрольные точки для «E» и "о кривых. Эти три преобразования перейти в центре путь к точке (0, 0), масштабировать путь размер полотна (но также учете толщину обводки) и затем переместите центра контура в центре полотна с:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Путь заполняется холст, который выглядит более рационального при просмотре в альбомной ориентации.

[![](path-data-images/pathdatahello-small.png "Тройной снимок экрана со страницей пути данных Hello")](path-data-images/pathdatahello-large.png#lightbox "тройной снимок экрана со страницей пути данных Hello")

**Путь данных Cat** аналогичен страницы. Путь и paint объекты определены как поля в [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) класса:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Начало cat отображается в виде круга, а здесь отображается с двумя `A` команд, каждая из которых рисует полукруга. Оба `A` команд для заголовок определяет горизонтальный и вертикальный радиусы 100. Начинается первый дуги (240, 100) и заканчивается в (240, 300), которая становится начальную точку для второй дуги, который заканчивается обратно в (240, 100).

Два глаза также отображаются с двумя `A` команды и с помощью head cat второй `A` команда завершает в той же точке как начало первого `A` команды. Однако эти пары `A` команд не определяют эллипса. С каждой дуги 40 единиц и радиус 40 единиц которой означает, что эти дуги полного semicircles.

`PaintSurface` Обработчик выполняет аналогичные преобразования в предыдущем примере, но задает один `Scale` коэффициент пропорции и обеспечивает немного полей, поэтому cat контакта не нажимайте стороны экрана:

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Вот программу на всех трех платформ.

[![](path-data-images/pathdatacat-small.png "Тройной снимок экрана со страницей данных Cat путь")](path-data-images/pathdatacat-large.png#lightbox "тройной снимок экрана со страницей Cat пути данных")

Как правило, когда `SKPath` объект определен как поле, контуров путь должен быть определен в конструктор или другой метод. При использовании данных SVG пути, однако вы познакомились с пути можно указать полностью в определении поля.

Более ранним **сложный часов аналогом** в [ **преобразование поворота** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) статье отображаются стрелки часов в виде простых строк. **Довольно аналогом часы** следующая программа заменяет эти строки с `SKPath` объекты, определенные в виде полей [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) класса вместе с `SKPaint` объектов:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Стрелки часов и минут теперь вложенных областей, таким образом, чтобы сделать эти руки, отделены друг от друга, они отображаются с черным структуры и серый заполнения с помощью `handStrokePaint` и `handFillPaint` объектов.

В предыдущем **сложный часов аналогом** образца, мало круги, отмечены часы и минуты, созданных в цикле. В этом **довольно аналогом часы** образец, используется совершенно другой подход: метки часы и минуты, пунктирных линий, нарисованных при помощи `minuteMarkPaint` и `hourMarkPaint` объектов:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

[ **Точек и тире** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) руководстве описано, как можно использовать `SKPathEffect.CreateDash` метод для создания пунктирной линией. Первым аргументом является `float` массив, который обычно состоит из двух элементов: первый элемент является длина дефисы, а второй элемент — разрыв между дефисы. Когда `StrokeCap` свойству `SKStrokeCap.Round`, то скругленными концах штриха эффективно увеличить длину штриха на толщину штриха на обеих сторонах штриха. Таким образом при установке первого элемента массива 0 возникает пунктирной линией.

Расстояние между этими точками регулируется второй элемент массива. Как вы увидите, эти два `SKPaint` объекты используются для рисования окружностей с радиусом 90 единиц. Длины окружности этот круг, следовательно, 180π, это означает, что метки 60 минут должно отображаться каждый 3π единиц, это значение второй в `float` массива в `minuteMarkPaint`. Двенадцать час метки должен находиться каждой единицы 15π это значение во втором `float` массива.

`PrettyAnalogClockPage` Класс устанавливает таймер переводимого поверхности каждые 16 миллисекунд и `PaintSurface` обработчик вызывается с заданным уровнем. Более ранние определения из `SKPath` и `SKPaint` объекты позволяют очень качественным код отрисовки:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Что-нибудь специальные выполняются с секундной стрелки, однако. Поскольку часы обновляется каждые 16 миллисекунд `Millisecond` свойство `DateTime` значение потенциально может использоваться для второй стороны анимации Очистка вместо одного, которое перемещается в дискретные перемещается из второго, в секунду. Но этот код не допускает перемещения для сглаживания. Вместо этого он использует Xamarin.Forms [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) и [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) плавности для разных видов перемещение анимации. Эти функции плавности вызвать секундной стрелки для перемещения по принципу jerkier &mdash; отзыва немного перед перемещается, а затем немного чрезмерно Устранение назначению эффект сожалению, невозможно воспроизвести в эти статические снимки экрана:

[![](path-data-images/prettyanalogclock-small.png "Тройной снимок экрана со страницей довольно аналогом часы")](path-data-images/prettyanalogclock-large.png#lightbox "тройной снимок экрана со страницей довольно аналогом часов")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
