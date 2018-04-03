---
title: Матрица преобразования
description: Подробные сведения SkiaSharp преобразований с матрица универсальный преобразования
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 9EDED6A0-F0BF-4471-A9EF-E0D6C5954AE4
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: d40f898f07077ec2e2466cd5de3cd582636cfb15
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="matrix-transforms"></a>Матрица преобразования

_Подробные сведения SkiaSharp преобразований с матрица универсальный преобразования_

Все преобразований, применяемых к `SKCanvas` объекта объединены в один экземпляр [ `SKMatrix` ](https://developer.xamarin.com/api/type/SkiaSharp.SKMatrix/) структуры. Это стандартная матрицы преобразования 3 x 3 аналогичны во всех системах современных двумерной графики.

Как вы уже видели, преобразования можно использовать в SkiaSharp не зная о преобразовании матрицы, но матрица преобразования, важные с точки зрения теории, так и очень важно при использовании преобразования для изменения пути или для обработки сложных сенсорный ввод, оба которого демонстрируются в этой статье и следующим.

![](matrix-images/matrixtransformexample.png "Битовая карта для аффинные преобразования")

Текущая матрица преобразования, применить к `SKCanvas` доступен в любое время, обратившись к доступной только для чтения [ `TotalMatrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.TotalMatrix/) свойство. Можно задать новый матрицы преобразования с помощью [ `SetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.SetMatrix/p/SkiaSharp.SKMatrix/) метод и можно восстановить данную матрицу преобразования в значения по умолчанию путем вызова [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix/).

Единственным других `SKCanvas` — элемент, который работает непосредственно с canvas матрицы преобразования [ `Concat` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Concat/p/SkiaSharp.SKMatrix@/) которого объединяет две матрицы умножая их друг с другом.

Матрица преобразования по умолчанию — единичная матрица и состоит из 1 в 0 во всех остальных по диагонали ячеек:

<pre>
| 1  0  0 |
| 0  1  0 |
| 0  0  1 |
</pre>

Можно создать с помощью статического единичной матрицей [ `SKMatrix.MakeIdentity` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeIdentity()/) метод:

```csharp
SKMatrix matrix = SKMatrix.MakeIdentity();
```

`SKMatrix` Конструктор по умолчанию не *не* возвращают единичной матрицей. Он возвращает матрицу с все ячейки, равным нулю. Не используйте `SKMatrix` конструктор, если планируется вручную задать эти ячейки.

При SkiaSharp отображает графический объект, каждая точка (x, y) фактически преобразуется в 1 по 3 матрица с 1 в третьем столбце:

<pre>
| x  y  1 |
</pre>

Эта матрица с 1 по 3 представляет точку трехмерного с значение координаты Z, равным 1. Почему Двухмерная матрица преобразования требуется работа в трех измерениях может математических причин (см. описание далее). Можно представить этой матрицы 1 по 3, представляющего точку в системе координат трехмерной, но всегда на двумерной плоскости где Z равно 1.

Эта матрица с 1 по 3 умножается матрица преобразования, и результатом является точка, к просмотру на холсте:

<pre>
              | 1  0  0 |
| x  y  1 | × | 0  1  0 | = | x'  y'  z' |
              | 0  0  1 |
</pre>

С помощью стандартных матрицы умножения, преобразованный точек будет следующим:

x' = x

y "= y

z' = 1

Это преобразование по умолчанию.

При `Translate` метод будет вызван на `SKCanvas` объекта, `tx` и `ty` аргументы `Translate` метод становятся первые две ячейки в третьей строке матрицы преобразования:

<pre>
|  1   0   0 |
|  0   1   0 |
| tx  ty   1 |
</pre>

Умножение является теперь следующим образом:

<pre>
              |  1   0   0 |
| x  y  1 | × |  0   1   0 | = | x'  y'  z' |
              | tx  ty   1 |
</pre>

Ниже приведены формулы преобразования.

x' = x + tx

y "= y + ty

Коэффициенты масштабирования иметь значение по умолчанию 1. При вызове `Scale` новый метод `SKCanvas` объекта содержится матрица преобразования результирующий `sx` и `sy` аргументов в диагонали ячеек:

<pre>
              | sx   0   0 |
| x  y  1 | × |  0  sy   0 | = | x'  y'  z' |
              |  0   0   1 |
</pre>

Преобразование формул выглядят следующим образом:

x' = sx · x

y "= sy · y

Матрица преобразования после вызова `Skew` содержит два аргумента в ячейках матрицы, рядом с коэффициенты масштабирования:

<pre>
              │   1   ySkew   0 │
| x  y  1 | × │ xSkew   1     0 │ = | x'  y'  z' |
              │   0     0     1 │
</pre>

Преобразование формул являются:

x' = x + xSkew · y

y "= ySkew · x + y

Для вызова `RotateDegrees` или `RotateRadians` для углом α матрица преобразования выглядит следующим образом:

<pre>
              │  cos(α)  sin(α)  0 │
| x  y  1 | × │ –sin(α)  cos(α)  0 │ = | x'  y'  z' |
              │    0       0     1 │
</pre>

Ниже приведены формулы преобразования.

x' = cos(α) · x - sin(α) · y

y "= sin(α) · x - cos(α) · y

Когда α 0 градусов, это единичной матрицей. Когда α 180 градусов, матрица преобразования выглядит следующим образом:

<pre>
| –1   0   0 |
|  0  –1   0 |
|  0   0   1 |
</pre>

Поворот на 180 градусов соответствует зеркальное отражение объекта по горизонтали и по вертикали, который также выполняется путем установки коэффициенты масштабирования – 1.

Все эти типы преобразований классифицируются как *аффинного* преобразования. Аффинные преобразования никогда не включать в себя третьего столбца матрицы, остается в значения по умолчанию 0, 0 и 1. Статья [аффинного не преобразует](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md) рассматриваются-аффинных преобразований.

## <a name="matrix-multiplication"></a>Умножение матриц

Другое значительное преимущество с использованием матрицы преобразования — что составного преобразования можно получить, умножение матриц, который часто называют в документации SkiaSharp как *объединение*. Многие методы, связанные с преобразованием в `SKCanvas` см. «перед объединением» или «pre concat». Это относится порядку умножения, что важно, поскольку умножение матриц не является коммутативной.

Например, в документации по [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) метод говорится, что он «Текущая матрица с заданного сдвига Pre-concats» при документации для [ `Scale` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Scale/p/System.Single/System.Single/) метод говорится, что он «Pre-concats Текущая матрица с данным масштабом.»

Это означает, что преобразование, указанный при вызове этого метода — множитель (левый операнд) и текущая матрица преобразования — множитель (правого операнда).

Предположим, что `Translate` вызывается следуют `Scale`:

```csharp
canvas.Translate(tx, ty);
canvas.Scale(sx, sy);
```

`Scale` Умножается на преобразование `Translate` преобразования для матрица составного преобразования:

<pre>
| sx   0   0 |   |  1   0   0 |   | sx   0   0 |
|  0  sy   0 | × |  0   1   0 | = |  0  sy   0 |
|  0   0   1 |   | tx  ty   1 |   | tx  ty   1 |
</pre>

`Scale` может быть вызван перед `Translate` следующим образом:

```csharp
canvas.Scale(sx, sy);
canvas.Translate(tx, ty);
```

В этом случае изменять порядок умножения и эффективно коэффициенты масштабирования применяются коэффициенты преобразования:

<pre>
|  1   0   0 |   | sx   0   0 |   |  sx      0    0 |
|  0   1   0 | × |  0  sy   0 | = |   0     sy    0 |
| tx  ty   1 |   |  0   0   1 |   | tx·sx  ty·sy  1 |
</pre>

Вот `Scale` метод с точки сведения:

```csharp
canvas.Scale(sx, sy, px, py);
```

Это эквивалентно следующие вызовы для преобразования и масштаба:

```csharp
canvas.Translate(px, py);
canvas.Scale(sx, sy);
canvas.Translate(–px, –py);
```

Три матрицы преобразования умножаются в обратном порядке из отображения методы в код:

<pre>
|  1    0   0 |   | sx   0   0 |   |  1   0  0 |   |    sx         0     0 |
|  0    1   0 | × |  0  sy   0 | × |  0   1  0 | = |     0        sy     0 |
| –px  –py  1 |   |  0   0   1 |   | px  py  1 |   | px–px·sx  py–py·sy  1 |
</pre>

### <a name="the-skmatrix-structure"></a>Структура SKMatrix

`SKMatrix` Структура определяет девять свойства чтения/записи типа `float` соответствующий девять ячеек матрицы преобразования:

<pre>
│ ScaleX  SkewY   Persp0 │
│ SkewX   ScaleY  Persp1 │
│ TransX  TransY  Persp2 │
</pre>

`SKMatrix` также определяет свойство с именем [ `Values` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix.Values/) типа `float[]`. Это свойство можно использовать для задания или получения девять значений за один раз в порядке `ScaleX`, `SkewX`, `TransX`, `SkewY`, `ScaleY`, `TransY`, `Persp0`, `Persp1`, и `Persp2`.

`Persp0`, `Persp1`, И `Persp2` ячейки будут рассмотрены в статье [аффинного не преобразует](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md). Если эти ячейки имеют значения по умолчанию 0, 0 и 1, преобразование умножается координат точки следующим образом:

<pre>
              │ ScaleX  SkewY   0 │
| x  y  1 | × │ SkewX   ScaleY  0 │ = | x'  y'  z' |
              │ TransX  TransY  1 │
</pre>

x' = ScaleX · x + SkewX · y + TransX

y "= SkewX · x + ScaleY · y + TransY

z' = 1

Это полный двухмерный аффинного преобразования. Аффинные преобразования сохраняет параллельных линий, это означает, что прямоугольник никогда не преобразуются в ничего, кроме параллелограмм.

`SKMatrix` Структура определяет несколько статических методов для создания `SKMatrix` значения. Они возвращают `SKMatrix` значения:

- [`MakeTranslation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeTranslation/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/)
- [`MakeScale`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeScale/p/System.Single/System.Single/System.Single/System.Single/) с точки вращения
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/) для угол в радианах
- [`MakeRotation`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotation/p/System.Single/System.Single/System.Single/) угол в радианах, содержащих оператор pivot точки
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/)
- [`MakeRotationDegrees`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeRotationDegrees/p/System.Single/System.Single/System.Single/) с точки вращения
- [`MakeSkew`](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.MakeSkew/p/System.Single/System.Single/)

`SKMatrix` также определяет несколько статических методов, осуществляющие сцепление двух матриц, это означает, что их умножения. Эти методы называются `Concat`, `PostConcat`, и `PreConcat`, и существуют две версии. Эти методы имеют без возвращаемого значения; Вместо этого они ссылаются на существующие `SKMatrix` значения через `ref` аргументы. В следующем примере `A`, `B`, и `R` (для «результат»), все `SKMatrix` значения.

Два `Concat` методы вызываются следующим образом:

```csharp
SKMatrix.Concat(ref R, A, B);

SKMatrix.Concat(ref R, ref A, ref B);
```

Эти умножения:

R = B × A

Другие методы имеют только два параметра. Первый параметр изменены и при возврате из вызова метода, содержит произведение двух матриц. Два `PostConcat` методы вызываются следующим образом:

```csharp
SKMatrix.PostConcat(ref A, B);

SKMatrix.PostConcat(ref A, ref B);
```

Эти вызовы выполнить следующую операцию:

A = A × B

Два `PreConcat` аналогичны и методы:

```csharp
SKMatrix.PreConcat(ref A, B);

SKMatrix.PreConcat(ref A, ref B);
```

Эти вызовы выполнить следующую операцию:

A = B × A

Версии этих вызовов метода вызывает со всеми `ref` аргументы немного более эффективны в вызов базовой реализации, но он может ввести в заблуждение кто-то читает этот код и при условии, что что-либо с `ref` аргумент изменить с помощью метода. Кроме того, часто бывает удобнее передать аргумент, который является результатом одной из `Make` методов, например:

```csharp
SKMatrix result;
SKMatrix.Concat(result, SKMatrix.MakeTranslation(100, 100),
                        SKMatrix.MakeScale(3, 3));
```

Это создает следующие матрицы:

<pre>
│   3    0  0 │
│   0    3  0 │
│ 100  100  1 │
</pre>

Это преобразование изменения масштаба, умноженное на преобразование для преобразования. В данном случае `SKMatrix` структура предоставляет ярлык с методом [ `SetScaleTranslate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.SetScaleTranslate/p/System.Single/System.Single/System.Single/System.Single/):

```csharp
SKMatrix R = new SKMatrix();
R.SetScaleTranslate(3, 3, 100, 100);
```

Это несколько раз, когда можно безопасно использовать `SKMatrix` конструктор. `SetScaleTranslate` Метод задает все девять ячеек матрицы. Кроме того, он может использоваться `SKMatrix` статический конструктор `Rotate` и `RotateDegrees` методов:

```csharp
SKMatrix R = new SKMatrix();

SKMatrix.Rotate(ref R, radians);

SKMatrix.Rotate(ref R, radians, px, py);

SKMatrix.RotateDegrees(ref R, degrees);

SKMatrix.RotateDegrees(ref R, degrees, px, py);
```

Эти методы выполняют *не* объединения преобразование, поворот существующее преобразование. Методы задать все ячейки матрицы. Они являются функционально идентичен `MakeRotation` и `MakeRotationDegrees` методы, но они не создать `SKMatrix` значение.

Предположим, что имеется `SKPath` объект, который будет отображаться, но вы предпочитаете, чтобы она имела немного отличаются ориентации или другая центральная точка. Все координаты этот путь можно изменить путем вызова [ `Transform` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.Transform/p/SkiaSharp.SKMatrix/) метод `SKPath` с `SKMatrix` аргумент. **Преобразовать путь** страницы показано, как это сделать. [ `PathTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/PathTransformPage.cs) Класса ссылки `HendecagramPath` объекта в поле только использует его конструктор, чтобы применить преобразование к этому пути:

```csharp
public class PathTransformPage : ContentPage
{
    SKPath transformedPath = HendecagramArrayPage.HendecagramPath;

    public PathTransformPage()
    {
        Title = "Path Transform";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        SKMatrix matrix = SKMatrix.MakeScale(3, 3);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeRotationDegrees(360f / 22));
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(300, 300));

        transformedPath.Transform(matrix);
    }
    ...
}
```

`HendecagramPath` Объект имеет центр на (0, 0) и количеством точек звезды наружу расширения центре по 100 единиц во всех направлениях. Это означает, что путь содержит положительные и отрицательные координаты. **Преобразовать путь** страницы является предпочтительным для работы с звезда трижды как большие и все координаты положительным. Кроме того он не хочет одну точку звездочку, чтобы пункт вверх. Он хочет вместо этого для одной точки звезды точки прямо вниз. (Так как звезды количеством точек, он не может иметь оба.) Требуется смена звезды 360 градусов деленная 22.

Конструктор создает `SKMatrix` объекта из трех отдельных преобразований с помощью `PostConcat` метод с помощью следующего шаблона, где A, B и C являются экземплярами `SKMatrix`:

```csharp
SKMatrix matrix = A;
SKMatrix.PostConcat(ref A, B);
SKMatrix.PostConcat(ref A, C);
```

Это ряд последовательных операции умножения, поэтому результат будет следующим:

A × B × C

Последовательные операции умножения помогают лучше понять назначение каждой преобразования. Преобразование изменения масштаба увеличивает размер путь координат с коэффициентом 3, поэтому координаты в диапазоне от –300 до 300. Преобразования вращения вращается вокруг начала координат звезды. Преобразования для преобразования затем сдвигает его по 300 пикселей вправо и вниз, поэтому все координаты, становятся положительными.

Существуют другие последовательности, соответствующие одной матрицы. Вот еще один:

```csharp
SKMatrix matrix = SKMatrix.MakeRotationDegrees(360f / 22);
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(100, 100));
SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(3, 3));
```

Это сначала поворачивает путь относительно его центра и преобразует его 100 пикселов вправо и вниз, поэтому все координаты имеют положительное значение. Затем звезды увеличивается в размере относительно новых верхнего левого угла, которая является точкой (0, 0).

`PaintSurface` Обработчик может просто преобразовать этот путь:

```csharp
public class PathTransformPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Stroke;
            paint.Color = SKColors.Magenta;
            paint.StrokeWidth = 5;

            canvas.DrawPath(transformedPath, paint);
        }
    }
}

```

Он отображается в левом верхнем углу холста:

[![](matrix-images/pathtransform-small.png "Тройной снимок экрана со страницей преобразовать путь")](matrix-images/pathtransform-large.png#lightbox "тройной снимок экрана со страницей путь преобразования")

Конструктор этой программы применяется матрицы по пути с следующий вызов:

```csharp
transformedPath.Transform(matrix);
```

Путь *не* Сохранить как свойство данной матрицы. Вместо этого он применяет преобразование всех координат пути. Если `Transform` вызывается снова, к которому применяется преобразование еще раз и является единственным способом, можно вернуться к нему, применив другой матрицу, которая отменяет преобразование. К счастью `SKMatrix` структура определяет [ `TryInverse` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix.TryInvert/p/SkiaSharp.SKMatrix/) метод, который получает матрицы, обращает заданной матрицы:

```csharp
SKMatrix inverse;
bool success = matrix.TryInverse(out inverse);
```

Этот метод вызывается `TryInverse` , так как не все матрицы обратимой, но не обратимой матрицы не должна использоваться для преобразования графики.

Также можно применить преобразование матрицы `SKPoint` значение, массив точек, `SKRect`, или хотя бы одно число в программе. `SKMatrix` Структура поддерживает эти операции с коллекцией методов, которые начинаются со слова `Map`, такие:

```csharp
SKPoint transformedPoint = matrix.MapPoint(point);

SKPoint transformedPoint = matrix.MapPoint(x, y);

SKPoint[] transformedPoints = matrix.MapPoints(pointArray);

float transformedValue = matrix.MapRadius(floatValue);

SKRect transformedRect = matrix.MapRect(rect);
```

Если используется этот метод, не забывайте, что `SKRect` структура не является возможность представления повернутый прямоугольника. Метод имеет смысл только `SKMatrix` значение, представляющее перевода и масштабирование.

### <a name="interactive-experimentation"></a>Интерактивный экспериментов

Один из способов понять аффинного преобразования является интерактивно перемещение трех углов растрового изображения по экрану и видеть, какие преобразования результатов. Это имеет смысл **Показать аффинное** страницы. Для этой страницы требуется два класса, которые также используются в других демонстрации:

[ `TouchPoint` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchPoint.cs) Класс отображает полупрозрачные кружок, который можно перетаскивать по экрану. `TouchPoint` требуется `SKCanvasView` или элемент, который является родительским для `SKCanvasView` имеют [ `TouchEffect` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/TouchEffect.cs) присоединен. Задайте для свойства `Capture` значение `true`. В `TouchAction` обработчика событий, необходимо вызвать программу `ProcessTouchEvent` метод в `TouchPoint` для каждого `TouchPoint` экземпляр. Метод возвращает `true` Если события касания привело к точке касания перемещение. Кроме того `PaintSurface` необходимо вызвать обработчик `Paint` метод в каждом `TouchPoint` экземпляра, передавая ей `SKCanvas` объекта.

`TouchPoint` показывает общий способ, что SkiaSharp visual можно инкапсулировать в отдельном классе. В классе можно определить свойства для указания характеристики визуального элемента, и метод с именем `Paint` с `SKCanvas` аргумент его подготовки.

`Center` Свойство `TouchPoint` указывает расположение объекта. Это свойство можно задать для инициализации расположение; изменения свойств, когда пользователь перетаскивает кружок вокруг холста.

**Показать страницу аффинного матрицы** также требует [ `MatrixDisplay` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/MatrixDisplay.cs) класса. Этот класс отображает ячейки `SKMatrix` объекта. Он имеет два открытых метода: `Measure` получить размеры подготовленной матрицы и `Paint` для его отображения. Класс содержит `MatrixPaint` свойство типа `SKPaint` , можно заменить другой размер шрифта или цвета.

[ **ShowAffineMatrixPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml) создает файл `SKCanvasView` и присоединяет `TouchEffect`. [ **ShowAffineMatrixPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs) файл кода программной части создает три `TouchPoint` объекты и устанавливает их в место, соответствующее трех углов точечного рисунка, который загружает из внедренного ресурс:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    SKMatrix matrix;
    SKBitmap bitmap;
    SKSize bitmapSize;

    TouchPoint[] touchPoints = new TouchPoint[3];

    MatrixDisplay matrixDisplay = new MatrixDisplay();

    public ShowAffineMatrixPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }

        touchPoints[0] = new TouchPoint(100, 100);                  // upper-left corner
        touchPoints[1] = new TouchPoint(bitmap.Width + 100, 100);   // upper-right corner
        touchPoints[2] = new TouchPoint(100, bitmap.Height + 100);  // lower-left corner

        bitmapSize = new SKSize(bitmap.Width, bitmap.Height);
        matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                           touchPoints[1].Center,
                                           touchPoints[2].Center);
    }
    ...
}
```

Аффинное однозначно определяется тремя точками. Три `TouchPoint` соответствующие объекты в левый верхний, правый верхний и левый нижний углы растрового изображения. Так как только аффинное способен преобразования прямоугольник в параллелограмм, остальные три подразумевается четвертой точке. Конструктор завершается с помощью вызова `ComputeMatrix`, которая вычисляет ячейки `SKMatrix` объекта из этих трех точек.

`TouchAction` Вызовов обработчика `ProcessTouchEvent` метод каждого `TouchPoint`. `scale` Значение преобразуется из Xamarin.Forms координат точек:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        bool touchPointMoved = false;

        foreach (TouchPoint touchPoint in touchPoints)
        {
            float scale = canvasView.CanvasSize.Width / (float)canvasView.Width;
            SKPoint point = new SKPoint(scale * (float)args.Location.X,
                                        scale * (float)args.Location.Y);
            touchPointMoved |= touchPoint.ProcessTouchEvent(args.Id, args.Type, point);
        }

        if (touchPointMoved)
        {
            matrix = ComputeMatrix(bitmapSize, touchPoints[0].Center,
                                               touchPoints[1].Center,
                                               touchPoints[2].Center);
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

При наличии `TouchPoint` был перемещен, а затем вызывает метод `ComputeMatrix` еще раз и делает недействительным рабочей области.

`ComputeMatrix` Метод определяет, содержится в разрешении этих трех точек матрицы. Матрица называется `A` преобразований квадратный прямоугольник один пиксель в параллелограмм на основе трех точек, а вызывается преобразование изменения масштаба `S` масштабирование растрового изображения в прямоугольник квадратный один пиксель. Матрица составного `S` × `A`:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL)
    {
        // Scale transform
        SKMatrix S = SKMatrix.MakeScale(1 / size.Width, 1 / size.Height);

        // Affine transform
        SKMatrix A = new SKMatrix
        {
            ScaleX = ptUR.X - ptUL.X,
            SkewY = ptUR.Y - ptUL.Y,
            SkewX = ptLL.X - ptUL.X,
            ScaleY = ptLL.Y - ptUL.Y,
            TransX = ptUL.X,
            TransY = ptUL.Y,
            Persp2 = 1
        };

        SKMatrix result;
        SKMatrix.Concat(ref result, A, S);
        return result;
    }
    ...
}
```

Наконец `PaintSurface` метод отображает растровое изображение на основе на данную матрицу, отображает матрицу, в нижней части экрана и подготавливает к просмотру точки касания на трех углов растрового изображения:

```csharp
public partial class ShowAffineMatrixPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap using the matrix
        canvas.Save();
        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(matrix);

        matrixDisplay.Paint(canvas, matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));

        // Display the touchpoints
        foreach (TouchPoint touchPoint in touchPoints)
        {
            touchPoint.Paint(canvas);
        }
    }
  }
```

IOS экрана ниже показано растрового изображения при первой загрузке страницы, а два других экранах показывает его после обработки некоторые:

[![](matrix-images/showaffinematrix-small.png "Тройной снимок экрана со страницей Показать аффинное")](matrix-images/showaffinematrix-large.png#lightbox "тройной снимок экрана со страницей Показать аффинное")

Хотя кажется, как если бы точки касания перетащите углы растрового изображения, которые являются только иллюзии. Матрица, вычисленный на основе точки касания преобразует растровое изображение так, чтобы углов совпадало с точки касания.

Более естественно для пользователей для перемещения, изменения размера и поворота растровые изображения не перетаскиванием углов, но с помощью одним или двумя пальцами непосредственно в объекте, чтобы перетащить, сжатие и поворот. Это рассматривается в следующей статье [Touch манипуляции](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md).

### <a name="the-reason-for-the-3-by-3-matrix"></a>Причина матрицу 3 x 3

Можно ожидать, что система двумерной графики потребует только матрицу преобразования 2 x 2:

<pre>
           │ ScaleX  SkewY  │
| x  y | × │                │ = | x'  y' |
           │ SkewX   ScaleY │
</pre>

Для масштабирования, поворота и даже Наклон это работает, но не способен основная преобразований, являющийся перевода.

Проблема заключается в том, что матрица 2 x 2 представляет *линейной* преобразования в двух измерений. Преобразование линейного сохраняет некоторые основные арифметические операции, но одно из следствий является то, что линейного преобразования никогда не изменяет точку (0, 0). Линейного преобразования делают преобразование невозможно.

В трех измерениях Матрица линейного преобразования выглядит следующим образом:

<pre>
              │ ScaleX  SkewYX  SkewZX │
| x  y  z | × │ SkewXY  ScaleY  SkewZY │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ  ScaleZ │
</pre>

Ячейки с меткой `SkewXY` означает, что значение наклона на основе значений Y по оси X; ячейки `SkewXZ` означает, что значение наклона на основе значений Z; по оси X, а значения наклон аналогично другим `Skew` ячеек.

Можно ограничить этой матрицы преобразования 3D на двумерной плоскости, задав `SkewZX` и `SkewZY` 0, и `ScaleZ` 1:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  z | × │ SkewXY  ScaleY   0 │ = | x'  y'  z' |
              │ SkewXZ  SkewYZ   1 │
</pre>

Если двумерной графики рисуются полностью на плоскости в трехмерном пространстве, где значение Z равно 1, умножение преобразования выглядит следующим образом:

<pre>
              │ ScaleX  SkewYX   0 │
| x  y  1 | × │ SkewXY  ScaleY   0 │ = | x'  y'  1 |
              │ SkewXZ  SkewYZ   1 │
</pre>

Все остается на двумерной плоскости где Z равно 1, но `SkewXZ` и `SkewYZ` ячейки эффективно станут двумерный перевод факторов.

Это как трехмерного линейного преобразования служит двухмерный нелинейного преобразования. (Используя аналогию, преобразований в трехмерной графики основаны на матрицу 4 x 4.)

`SKMatrix` Структуры в SkiaSharp определяет свойства для этой строки третий:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z` |
              │ TransX  TransY  Persp2 │
</pre>

Ненулевые значения `Persp0` и `Persp1` привести преобразований, которые перемещают объекты вне двумерной плоскости, где значение Z равно 1. Что произойдет, если эти объекты перемещаются назад в этой плоскости рассматривается в статье на [аффинного не преобразует](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md).


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
