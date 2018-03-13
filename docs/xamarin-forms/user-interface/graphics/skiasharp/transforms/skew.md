---
title: "Наклон преобразования"
description: "В разделе как наклона преобразования может создать наклона графических объектов в SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: FDD16186-E3B7-4FF6-9BC2-8A2974BFF616
author: charlespetzold
ms.author: chape
ms.date: 03/20/2017
ms.openlocfilehash: a18b60d486a911e4a76298fd20a70f16ac392881
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="the-skew-transform"></a>Наклон преобразования

_В разделе как наклона преобразования может создать наклона графических объектов в SkiaSharp_

В SkiaSharp наклона преобразования tilts графических объектов, таких как тени на этом рисунке:

![](skew-images/skewexample.png "Примером наклон из программы наклон тени текста")

Перекос Преобразует прямоугольники в parallelograms, но становятся ассиметричные эллипса по-прежнему эллипса.

Несмотря на то, что Xamarin.Forms определяет свойства для преобразования, масштабирование и поворотов, нет соответствующего свойства в Xamarin.Forms отклонения.

[ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/System.Single/System.Single/) Метод `SKCanvas` принимает два аргумента для наклон горизонтальной и вертикальной наклон:

```csharp
public void Skew (Single xSkew, Single ySkew)
```

Второй [ `Skew` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Skew/p/SkiaSharp.SKPoint/) метод объединяет эти аргументы в одной `SKPoint` значение:

```csharp
public void Skew (SKPoint skew)
```

Однако маловероятно, что вы будете использовать один из этих двух методов в изоляции.

**Наклон поэкспериментировать** странице позволяет экспериментировать с наклон значений в диапазоне от –10 до 10. Текстовая строка располагается в левом верхнем углу страницы, с отклонение значений, полученных из двух `Slider` элементов. Вот `PaintSurface` обработчик в [ `SkewExperimentPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewExperimentPage.xaml.cs) класса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);

        canvas.Skew((float)xSkewSlider.Value, (float)ySkewSlider.Value);
        canvas.DrawText(text, 0, -textBounds.Top, textPaint);
    }
}
```

Значения `xSkew` аргумент shift нижней части текста справа для положительных значений или влево для отрицательных значений. Значения `ySkew` со сдвигом вправо текст вниз для положительных значений или для отрицательных значений:

[![](skew-images/skewexperiment-small.png "Тройной снимок экрана со страницей наклон эксперимента")](skew-images/skewexperiment-large.png#lightbox "тройной снимок экрана со страницей наклон эксперимента")

Если `xSkew` является отрицательной `ySkew`, результат — поворот, но также растягивания немного отображения указывает Windows.

Преобразование формул выглядят следующим образом:

x' = x + xSkew · y

y "= ySkew · x + y

Например, положительное `xSkew` значение преобразованного `x'` увеличивается значение `y` увеличивается. Это, в каком случае наклона.

Если треугольник 200 пикселей в ширину и 100 пикселей в высоту располагается с его верхнего левого угла в точке (0, 0) и выводится с `xSkew` значение 1,5 параллелограмм следующие результаты:

![](skew-images/skeweffect.png "Влияние преобразования наклона на прямоугольник")

Координаты нижнего края имеют `y` значения 100, поэтому вполне 150 пикселей сдвиг вправо.

Для ненулевых значений из `xSkew` или `ySkew`только точку (0, 0) остается неизменным. Центр наклон могут считаться этой точки. Вам требуется центр наклон быть либо другие (что обычно случается), существует ли не `Skew` метод, который предоставляет. Вам потребуется явно объединить `Translate` вызовы с `Skew` вызова. Чтобы центрировать наклон в `px` и `py`, выполняют следующие вызовы:

```csharp
canvas.Translate(px, py);
canvas.Skew(xSkew, ySkew);
canvas.Translate(-px, -py);
```

Составные преобразования формулы — это:

x' = x + xSkew · (y — py)

y "= ySkew · (x — px) + y

Если `ySkew` равен нулю, и ненулевое значение только задании `xSkew`, затем `px` значение не используется. Значение не играет роли и аналогично для `ySkew` и `py`.

Вам может быть удобнее, указав наклона как угол наклона в градусах, например угол α на этой диаграмме:

![](skew-images/skewangleeffect.png "Указывает влияние на прямоугольник с наклон угол наклона преобразования")

Коэффициент shift 150 пикселей на вертикальную 100 пикселов градуса тангенс угла, в этом примере 56.3.

XAML-файл **эксперимента угол наклона** страница подобна **угол наклона** страницы разницей, что `Slider` элементов в диапазоне от – 90 до 90 градусов. [ `SkewAngleExperiment` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/SkewAngleExperimentPage.xaml.cs) Файл кода программной части Центрирует текст на странице и использует `Translate` для установки центра наклон относительно центральной части страницы. Короткие `SkewDegrees` метод в нижней части код преобразует угол наклона значения:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Blue,
        TextSize = 200
    })
    {
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        string text = "SKEW";
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(text, ref textBounds);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;

        canvas.Translate(xCenter, yCenter);
        SkewDegrees(canvas, xSkewSlider.Value, ySkewSlider.Value);
        canvas.Translate(-xCenter, -yCenter);
        canvas.DrawText(text, xText, yText, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

По мере приближения углом 90 градусов, положительным или отрицательным тангенс приближается бесконечности, однако углы до около 80 градусов, и поэтому могут использоваться:

[![](skew-images/skewangleexperiment-small.png "Тройной снимок экрана со страницей эксперимента угол наклона")](skew-images/skewangleexperiment-large.png#lightbox "тройной снимок экрана со страницей эксперимента угол наклона")

Небольшой отрицательное горизонтальный наклон может имитировать наклонный или курсивом текста, как **наклонный текст** демонстрирует страницы. [ `ObliqueTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ObliqueTextPage.cs) Класс показано, как это можно сделать:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint()
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Maroon,
        TextAlign = SKTextAlign.Center,
        TextSize = info.Width / 8       // empirically determined
    })
    {
        canvas.Translate(info.Width / 2, info.Height / 2);
        SkewDegrees(canvas, -20, 0);
        canvas.DrawText(Title, 0, 0, textPaint);
    }
}

void SkewDegrees(SKCanvas canvas, double xDegrees, double yDegrees)
{
    canvas.Skew((float)Math.Tan(Math.PI * xDegrees / 180),
                (float)Math.Tan(Math.PI * yDegrees / 180));
}
```

`TextAlign` Свойство `SKPaint` равно `Center`. Без преобразования `DrawText` вызов с координатами (0, 0), поместит текста по горизонтали по центру базового плана в левом верхнем углу. `SkewDegrees` Наклон текста по горизонтали 20 градусов относительно базовой линии. `Translate` Вызов перемещает по горизонтали по центру текстового шаблона в центре полотна с:

[![](skew-images/obliquetext-small.png "Тройной снимок экрана со страницей наклонный текст")](skew-images/obliquetext-large.png#lightbox "тройной снимок экрана со страницей наклонный текст")

**Наклон тени текста** страницы показано, как использовать сочетание шкалу наклона и вертикальные 45 градусов, чтобы сделать тень текста, который tilts от текста. Вот нужные часть `PaintSurface` обработчика:

```csharp
using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = info.Width / 6;   // empirically determined

    // Common to shadow and text
    string text = "Shadow";
    float xText = 20;
    float yText = info.Height / 2;

    // Shadow
    textPaint.Color = SKColors.LightGray;
    canvas.Save();
    canvas.Translate(xText, yText);
    canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
    canvas.Scale(1, 3);
    canvas.Translate(-xText, -yText);
    canvas.DrawText(text, xText, yText, textPaint);
    canvas.Restore();

    // Text
    textPaint.Color = SKColors.Blue;
    canvas.DrawText(text, xText, yText, textPaint);
}
```

Тени — сначала отображается, а затем текст:

[![](skew-images/skewshadowtext1-small.png "Тройной снимок экрана со страницей наклон тени текста")](skew-images/skewshadowtext1-large.png#lightbox "тройной снимок экрана со страницей наклон тени текста")

Вертикальная координата передаваемый `DrawText` метод показывает расположение текста относительно базовой линии. Это используется для центра наклон же Вертикальная координата. Этот способ не будет работать, если текстовая строка содержит подстрочные элементы. Например замените слово «причудливым» для «Тень» и здесь в результат:

[![](skew-images/skewshadowtext2-small.png "Тройной снимок экрана со страницей наклон Тень текста с альтернативным ключевое слово with подстрочные элементы")](skew-images/skewshadowtext2-large.png#lightbox "тройной снимок экрана со страницей наклон Тень текста с альтернативным ключевое слово with подстрочные элементы")

Тени и текст по-прежнему были выровнены по базовому плану, но эффект только отображается неправильно. Чтобы устранить проблему, вам необходимо получить границы текста:

```csharp
SKRect textBounds = new SKRect();
textPaint.MeasureText(text, ref textBounds);
```

`Translate` Вызовы должны указываться высотой подстрочные элементы:

```csharp
canvas.Translate(xText, yText + textBounds.Bottom);
canvas.Skew((float)Math.Tan(-Math.PI / 4), 0);
canvas.Scale(1, 3);
canvas.Translate(-xText, -yText - textBounds.Bottom);
```

Теперь тени распространяется в нижней части этих подстрочные элементы:

[![](skew-images/skewshadowtext3-small.png "Тройной снимок экрана со страницей наклон Тень текста с корректировки для подстрочных элементов")](skew-images/skewshadowtext3-large.png#lightbox "тройной снимок экрана со страницей наклон Тень текста с корректировки для подстрочных элементов")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
