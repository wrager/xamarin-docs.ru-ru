---
title: "Преобразования вращения"
description: "Изучите эффекты и при наличии преобразования вращения SkiaSharp анимации"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/23/2017
ms.openlocfilehash: c87f9a561ac2f7a8c3da1c1e4ab839431073fcb9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="the-rotate-transform"></a>Преобразования вращения

_Изучите эффекты и при наличии преобразования вращения SkiaSharp анимации_

С помощью преобразования вращения SkiaSharp графических объектов прервать свободного ограничения выравнивания с горизонтальной и вертикальной оси:

![](rotate-images/rotateexample.png "Текст, повернутый относительно центра")

Для поворота вокруг точки (0, 0), SkiaSharp поддерживает как графический объект [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/) метод и [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/) метод:

```csharp
public void RotateDegrees (Single degrees)

public Void RotateRadians (Single radians)
```

Круг 360 градусов совпадает со значением радианах 2π, поэтому его легко для преобразования между двумя единицами измерения. Используется удобным. Тригонометрические функции в статическом [ `Math` ](https://developer.xamarin.com/api/type/System.Math/) класс использовать единицы радиан.

Поворот по часовой стрелке, нацеленное на повышение углов. (Хотя поворота в декартовой системе координат против часовой стрелки в соответствии с соглашением, поворот по часовой стрелке согласуется с координаты Y увеличение переход вниз.) Отрицательное значение угла и угла, больше, чем разрешено 360 градусов.

Преобразование формул для поворота являются более сложными, чем для преобразования и масштаб. В прежнем α формулы преобразования используется:

x' = x•cos(α) — y•sin(α)   

y "= x•sin(α) + y•cos(α)

**Основные Поворот** странице демонстрируется `RotateDegrees` метод. [ `BasicRotate.xaml.cs` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BasicRotatePage.xaml.cs) Файл выводит текст с базового плана в центре страницы и поворачивает ее на основе `Slider` в диапазоне от –360 до 360. Вот соответствующая часть `PaintSurface` обработчика:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Так как поворот, собранных вокруг верхнего левого угла холста, для большинства углов, задайте в этой программе текст поворачивается уместиться на экране:

[![](rotate-images/basicrotate-small.png "Тройной снимок экрана со страницей основные Поворот")](rotate-images/basicrotate-large.png "тройной экрана основные Поворот страницы")

Очень часто имеет смысл поворот нечто по центру относительно указанной основной точки указанных версий [ `RotateDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateDegrees/p/System.Single/System.Single/System.Single/) и [ `RotateRadians` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RotateRadians/p/System.Single/System.Single/System.Single/) методов:

```csharp
public void RotateDegrees (Single degrees, Single px, Single py)

public void RotateRadians (Single radians, Single px, Single py)
```

**По центру Поворот** страница является так же, как **основные Поворот** за исключением того, что расширенная версия `RotateDegrees` используется для задания центра вращения на одну точку для позиционирования текста:

```csharp
using (SKPaint textPaint = new SKPaint
{
    Style = SKPaintStyle.Fill,
    Color = SKColors.Blue,
    TextAlign = SKTextAlign.Center,
    TextSize = 100
})
{
    canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
    canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
}
```

Теперь текст вращается вокруг точки, используемой для позиционирования текста, который является по горизонтали по центру текстового шаблона:

[![](rotate-images/centeredrotate-small.png "Тройной снимок экрана со страницей по центру Поворот")](rotate-images/centeredrotate-large.png "тройной экрана поворот по центру страницы")

Как и в случае с версией по центру `Scale` метод, по центру версию `RotateDegrees` вызова представляет собой ярлык:

```csharp
RotateDegrees (degrees, px, py);
```

Оно эквивалентно следующему выражению:

```csharp
canvas.Translate(px, py);
canvas.RotateDegrees(degrees);
canvas.Translate(-px, -py);
```

Вы обнаружите, что иногда может объединить `Translate` вызовы с `Rotate` вызовов. Например, вот `RotateDegrees` и `DrawText` вызывает в **по центру Поворот** страницы;

```csharp
canvas.RotateDegrees((float)rotateSlider.Value, info.Width / 2, info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`RotateDegrees` Вызов эквивалентен два `Translate` вызовы и не подразумевают `RotateDegrees`:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.DrawText(Title, info.Width / 2, info.Height / 2, textPaint);
```

`DrawText` Вызова для отображения текста в определенном месте эквивалентно `Translate` вызвать для этого расположения, за которым следует `DrawText` в точке (0, 0):

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.Translate(-info.Width / 2, -info.Height / 2);
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.DrawText(Title, 0, 0, textPaint);
```

Два последовательных `Translate` вызовы, отменяют друг друга:

```csharp
canvas.Translate(info.Width / 2, info.Height / 2);
canvas.RotateDegrees((float)rotateSlider.Value);
canvas.DrawText(Title, 0, 0, textPaint);
```

По существу два преобразования применяются в том порядке, в отличие от их отображения в коде. `DrawText` Вызов отображает текст в верхнем левом углу холста. `RotateDegrees` Вызов поворачивает этого текста относительно верхнего левого угла. Затем `Translate` вызов перемещает текст в центре полотна.

Как правило, существует несколько способов для объединения вращения и перемещения. **Поворачивать текст** страница создает следующий результат:

[![](rotate-images/rotatedtext-small.png "Тройной снимок экрана со страницей поворачивать текст")](rotate-images/rotatedtext-large.png "тройной снимок экрана со страницей поворачивать текст")

Вот `PaintSurface` обработчик [ `RotatedTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/RotatedTextPage.cs) класса:

```csharp
static readonly string text = "    ROTATE";
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 72
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float yText = yCenter - textBounds.Height / 2 - textBounds.Top;

        for (int degrees = 0; degrees < 360; degrees += 30)
        {
            canvas.Save();
            canvas.RotateDegrees(degrees, xCenter, yCenter);
            canvas.DrawText(text, xCenter, yText, textPaint);
            canvas.Restore();
        }
    }
}

```

`xCenter` И `yCenter` значения указывают центре полотна. `yText` Значение — немного смещение от обнаружения. Указывает, необходимые для размещения текста, таким образом, чтобы он действительно вертикально по центру на странице по оси Y. `for` Цикл затем задает поворот по центру в центре полотна. Смена — с шагом 30 градусов. Текст рисуется с помощью `yText` значение. Количество пустых значений перед словом «ВРАЩЕНИЯ» в `text` значение было определено эмпирически для установления соединения между эти 12 текстовые строки могут быть dodecagon.

Чтобы упростить этот код является увеличивается при каждом прохождении цикла после угол поворота на 30 градусов `DrawText` вызова. Это устраняет необходимость для вызовов `Save` и `Restore`. Обратите внимание, что `degrees` переменная больше не используется в теле `for` блока:

```csharp
for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, xCenter, yText, textPaint);
    canvas.RotateDegrees(30, xCenter, yCenter);
}

```

Можно также использовать простой формой `RotateDegrees` с предшествующим цикла с помощью вызова `Translate` перенести все центре полотна с:

```csharp
float yText = -textBounds.Height / 2 - textBounds.Top;

canvas.Translate(xCenter, yCenter);

for (int degrees = 0; degrees < 360; degrees += 30)
{
    canvas.DrawText(text, 0, yText, textPaint);
    canvas.RotateDegrees(30);
}
```

Измененный `yText` вычисления больше не включает в себя `yCenter`. Теперь `DrawText` вызов Центрирование текста по вертикали в верхней части холста.

Поскольку преобразования применяются концептуально отличие от их отображения в коде, часто бывает возможных начинается с более глобальные преобразования, следуют дополнительные локальные преобразования. Часто это самый простой способ объединения вращения и перемещения.

Например предположим, что необходимо нарисовать графический объект, который вращается вокруг его центра подобно вращается вокруг его оси планеты. Однако необходимо опираться на центре экрана очень напоминает планеты, вращение вокруг Солнца этого объекта.

Это можно сделать путем размещение объекта в верхнем левом углу холста, а затем с помощью анимации вращения, угол. Далее перевод объекта по горизонтали как orbital radius. Теперь можно примените второй анимированного поворота также вокруг начала координат. Это делает объект опираться на угол. Теперь преобразуются в центре полотна.

Вот `PaintSurface` обработчик, который содержит эти преобразования вызовы в обратном порядке:

```csharp
float revolveDegrees, rotateDegrees;
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint fillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Red
    })
    {
        // Translate to center of canvas
        canvas.Translate(info.Width / 2, info.Height / 2);

        // Rotate around center of canvas
        canvas.RotateDegrees(revolveDegrees);

        // Translate horizontally
        float radius = Math.Min(info.Width, info.Height) / 3;
        canvas.Translate(radius, 0);

        // Rotate around center of object
        canvas.RotateDegrees(rotateDegrees);

        // Draw a square
        canvas.DrawRect(new SKRect(-50, -50, 50, 50), fillPaint);
    }
}
```

`revolveDegrees` И `rotateDegrees` анимированных поля. Эта программа использует метод другой анимации, Xamarin.Forms `Animation` класса. (Этот класс описан в [главе 22 *Создание мобильных приложений с помощью Xamarin.Forms*](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)) `OnAppearing` переопределение создает два `Animation` объектов с помощью методов обратного вызова, а затем вызывает `Commit` на них анимации время:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    new Animation((value) => revolveDegrees = 360 * (float)value).
        Commit(this, "revolveAnimation", length: 10000, repeat: () => true);

    new Animation((value) =>
    {
        rotateDegrees = 360 * (float)value;
        canvasView.InvalidateSurface();
    }).Commit(this, "rotateAnimation", length: 1000, repeat: () => true);
}
```

Первый `Animation` анимирует объект `revolveDegrees` от 0 до 360 градусов более 10 секунд. Во втором анимирует `rotateDegrees` от 0 до 360 градусов каждые 1 секунды, а также делает недействительными область, чтобы создать другой вызов `PaintSurface` обработчика. `OnDisappearing` Переопределение отменяет эти два анимации:

```csharp
protected override void OnDisappearing()
{
    base.OnDisappearing();
    this.AbortAnimation("revolveAnimation");
    this.AbortAnimation("rotateAnimation");
}
```

**Сложный аналогично часам** программы (так называется из-за более привлекательным аналогично часам будут описаны в статье более поздней версии) использует поворота для минуты и часы знаки часов и повернуть на разработчике. Программа выводит часы, с использованием произвольного системы координат на основании кружок, который выравнивается по центру в точке (0, 0) с радиусом 100. Она использует преобразования и масштабирование для разверните и центра круга, что на странице.

`Translate` И `Scale` вызовы применяются глобально в часы, поэтому это первый из них должна вызываться после инициализации `SKPaint` объектов:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    using (SKPaint fillPaint = new SKPaint())
    {
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.Color = SKColors.Black;
        strokePaint.StrokeCap = SKStrokeCap.Round;

        fillPaint.Style = SKPaintStyle.Fill;
        fillPaint.Color = SKColors.Gray;

        // Transform for 100-radius circle centered at origin
        canvas.Translate(info.Width / 2f, info.Height / 2f);
        canvas.Scale(Math.Min(info.Width / 200f, info.Height / 200f));
        ...
    }
}

```csharp
There are 60 marks of two different sizes that must be drawn in a circle around the clock. The `DrawCircle` call draws that circle at the point (0, –90), which relative to the center of the clock corresponds to 12:00. The `RotateDegrees` call increments the rotation angle by 6 degrees after every tick mark. The `angle` variable is used solely to determine if a large circle or a small circle is drawn:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        // Hour and minute marks
        for (int angle = 0; angle < 360; angle += 6)
        {
            canvas.DrawCircle(0, -90, angle % 30 == 0 ? 4 : 2, fillPaint);
            canvas.RotateDegrees(6);
        }
    ...
    }
}
```

Наконец `PaintSurface` обработчик получает текущего времени и вычисляет градусов поворота для часа, минуты и второй руки. Таким образом, угол поворота относительно, каждой стрелки изображением в позицию 12:00:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
        DateTime dateTime = DateTime.Now;

        // Hour hand
        strokePaint.StrokeWidth = 20;
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawLine(0, 0, 0, -50, strokePaint);
        canvas.Restore();

        // Minute hand
        strokePaint.StrokeWidth = 10;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawLine(0, 0, 0, -70, strokePaint);
        canvas.Restore();

        // Second hand
        strokePaint.StrokeWidth = 2;
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Second);
        canvas.DrawLine(0, 10, 0, -80, strokePaint);
        canvas.Restore();
    }
}
```

Часы определенно работает, несмотря на то, что стрелки вместо грубый:

[![](rotate-images/uglyanalogclock-small.png "Тройное снимок экрана со страницей сложный текст аналогом часов")](rotate-images/uglyanalogclock-large.png "Triple screenshot of the Ugly Analog page")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
