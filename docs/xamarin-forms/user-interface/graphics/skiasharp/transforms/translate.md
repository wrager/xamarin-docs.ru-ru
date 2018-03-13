---
title: "Преобразования для преобразования"
description: "Узнайте, как использовать преобразования для преобразования для сдвига SkiaSharp графики"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: BD28ADA1-49F9-44E2-A548-46024A29882F
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: cac2479af2778af6043a85583f9d7b518748d7da
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="the-translate-transform"></a>Преобразования для преобразования

_Узнайте, как использовать преобразования для преобразования для сдвига SkiaSharp графики_

Является простейшим типом преобразования в SkiaSharp *преобразовать* или *перевода* преобразования. Это преобразование сдвигает графических объектов в горизонтальном и вертикальном направлениях. С точки зрения перевод — наиболее ненужные преобразования, так как обычно, можно выполнить тот же эффект, просто изменив координат, которые используются в функции рисования. При подготовке к просмотру путь, тем не менее, все координаты инкапсулированным в пути, что намного упрощает применение преобразования для преобразования для сдвига весь путь.

Перевод удобно использовать для анимации и простые текстовые эффекты:

![](translate-images/translateexample.png "Тень текста Утопленный текст; и тиснение с преобразованием")

[ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/System.Single/System.Single/) Метод в `SKCanvas` имеет два параметра, которые приводят к сдвиг по горизонтали и вертикали объектов впоследствии графических элементов:

```csharp
public void Translate (Single dx, Single dy)
```

Эти аргументы может быть отрицательным. Второй [ `Translate` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Translate/p/SkiaSharp.SKPoint/) метод объединяет два преобразования значения в одном `SKPoint` значение:

```csharp
public void Translate (SKPoint point)
```

**Накапливаются преобразовать** страница [ **SkiaSharpForms** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/) образце демонстрируется, нескольких вызовах `Translate` метод являются накопительными. [ `AccumulatedTranslate` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/AccumulatedTranslatePage.cs) Класс отображает 20 версии того же прямоугольника, каждый из них смещение от предыдущего прямоугольника достаточно, поэтому они растянуть по диагонали. Вот `PaintSurface` обработчик событий:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint strokePaint = new SKPaint())
    {
        strokePaint.Color = SKColors.Black;
        strokePaint.Style = SKPaintStyle.Stroke;
        strokePaint.StrokeWidth = 3;

        int rectangleCount = 20;
        SKRect rect = new SKRect(0, 0, 250, 250);
        float xTranslate = (info.Width - rect.Width) / (rectangleCount - 1);
        float yTranslate = (info.Height - rect.Height) / (rectangleCount - 1);

        for (int i = 0; i < rectangleCount; i++)
        {
            canvas.DrawRect(rect, strokePaint);
            canvas.Translate(xTranslate, yTranslate);
        }
    }
}
```

Последовательные прямоугольники тонкая вниз по странице:

[![](translate-images/accumulatedtranslate-small.png "Тройной снимок экрана со страницей накапливаются преобразовать")](translate-images/accumulatedtranslate-large.png#lightbox "тройной экрана накапливаются перевод страницы")

Если коэффициенты накопленный преобразования `dx` и `dy`, и точка, укажите в функцию рисования (`x`, `y`), графический объект отображается в точке, а затем (`x'`, `y'`), где:

x' = x + dx

y "= y + dy

Эти функции известны как *Преобразование формул* для перевода. Значение по умолчанию `dx` и `dy` новый `SKCanvas` равны 0.

Обычно использовать преобразования для преобразования для эффекта теней и сходные методы, как **преобразования текстовых эффектов** демонстрирует страницы. Вот соответствующая часть `PaintSurface` обработчик в [ `TranslateTextEffectsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TranslateTextEffectsPage.cs) класса:

```csharp
float textSize = 150;

using (SKPaint textPaint = new SKPaint())
{
    textPaint.Style = SKPaintStyle.Fill;
    textPaint.TextSize = textSize;
    textPaint.FakeBoldText = true;

    float x = 10;
    float y = textSize;

    // Shadow
    canvas.Translate(10, 10);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("SHADOW", x, y, textPaint);
    canvas.Translate(-10, -10);
    textPaint.Color = SKColors.Pink;
    canvas.DrawText("SHADOW", x, y, textPaint);

    y += 2 * textSize;

    // Engrave
    canvas.Translate(-5, -5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("ENGRAVE", x, y, textPaint);
    canvas.ResetMatrix();
    textPaint.Color = SKColors.White;
    canvas.DrawText("ENGRAVE", x, y, textPaint);

    y += 2 * textSize;

    // Emboss
    canvas.Save();
    canvas.Translate(5, 5);
    textPaint.Color = SKColors.Black;
    canvas.DrawText("EMBOSS", x, y, textPaint);
    canvas.Restore();
    textPaint.Color = SKColors.White;
    canvas.DrawText("EMBOSS", x, y, textPaint);
}
```

В каждом из трех примерах `Translate` вызывается для отображения текста для его смещение от расположении, указанном `x` и `y` переменных. Затем сообщение об ошибке снова другим цветом, не влияя при трансляции.

[![](translate-images/translatetexteffects-small.png "Тройной снимок экрана со страницей преобразования текстовых эффектов")](translate-images/translatetexteffects-large.png#lightbox "тройной снимок экрана со страницей преобразования текстовых эффектов")

Каждый из трех примерах показан другой способ Инверсия `Translate` вызова:

Первый пример просто вызывает `Translate` еще раз, но с отрицательными значениями. Поскольку `Translate` вызовы являются накопительными, это последовательность вызовов просто восстанавливает Общее преобразование значения по умолчанию равно нулю.

Второй пример вызывает [ `ResetMatrix` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.ResetMatrix()/). В этом случае все правила преобразования для возврата к значениям по умолчанию.

Третий пример сохраняет состояние из `SKCanvas` объекта с помощью вызова [ `Save` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Save()/) и восстанавливать состояние с помощью вызова [ `Restore` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Restore/). Это наиболее универсальных способ управления преобразований для ряда операций рисования. Эти `Save` и `Restore` вызывает функцию как стек: вы можете вызвать `Save` несколько раз, а затем вызовите `Restore` в обратном последовательности, чтобы вернуться на предыдущие состояния. `Save` Метод возвращает значение типа integer, и можно передать целого числа для [ `RestoreToCount` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.RestoreToCount/) фактически вызывается `Restore` несколько раз. [ `SaveCount` ](https://developer.xamarin.com/api/property/SkiaSharp.SKCanvas.SaveCount/) Свойство возвращает число состояний, в настоящее время сохранены в стеке.

Тем не менее, можно не беспокоиться о преобразованиях, переносятся из одного вызова из `PaintSurface` обработчика к следующему. Каждый новый вызов `PaintSurface` предоставляет новый `SKCanvas` объект с преобразования по умолчанию.

Другим распространенным применением `Translate` преобразование — для отрисовки визуального объекта, который был изначально создан с помощью координат, удобны для рисования. Например может потребоваться задать координаты аналогично часам с центром в точке (0, 0). Затем можно использовать преобразования для его отображения нужное место. Это показано в [**Hendecagram массива**] страницы. [ `HendecagramArrayPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramPage.cs) Класса начинается с создания `SKPath` объекта для звездочка указывает 11. `HendecagramPath` Объекта определено как открытый, статический и только для чтения таким образом, может осуществляться из других программ демонстрации. Он создается в статическом конструкторе:

```csharp
public class HendecagramArrayPage : ContentPage
{
    ...
    public static readonly SKPath HendecagramPath;

    static HendecagramArrayPage()
    {
        // Create 11-pointed star
        HendecagramPath = new SKPath();
        for (int i = 0; i < 11; i++)
        {
            double angle = 5 * i * 2 * Math.PI / 11;
            SKPoint pt = new SKPoint(100 * (float)Math.Sin(angle),
                                    -100 * (float)Math.Cos(angle));
            if (i == 0)
            {
                HendecagramPath.MoveTo(pt);
            }
            else
            {
                HendecagramPath.LineTo(pt);
            }
        }
        HendecagramPath.Close();
    }
}
```

Если центре звезды точки (0, 0), все точки звезды, круг вокруг этой точки. Каждая точка представляет собой комбинацию значений синус и косинус угла, увеличивающейся на 5/11ths 360 градусов. (Это также можно создать звездочка указывает 11, увеличивая угол на 2/11, 3/11ths или 4/11 круга.) Радиус окружности, имеет значение 100.

Если этот путь отображается без преобразования, центр будет располагаться в левом верхнем углу `SKCanvas`и отображаются только квартала. `PaintSurface` Обработчик `HendecagramPage` использовал `Translate` мозаичного полотно несколько копий звезды, каждый из них случайным образом цвет:

```csharp
public class HendecagramArrayPage : ContentPage
{
    Random random = new Random();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            for (int x = 100; x < info.Width + 100; x += 200)
                for (int y = 100; y < info.Height + 100; y += 200)
                {
                    // Set random color
                    byte[] bytes = new byte[3];
                    random.NextBytes(bytes);
                    paint.Color = new SKColor(bytes[0], bytes[1], bytes[2]);

                    // Display the hendecagram
                    canvas.Save();
                    canvas.Translate(x, y);
                    canvas.DrawPath(HendecagramPath, paint);
                    canvas.Restore();
                }
        }
    }
}

```

Ниже приведен результат:

[![](translate-images/hendecagramarray-small.png "Тройной снимок экрана со страницей массива Hendecagram")](translate-images/hendecagramarray-large.png#lightbox "тройной снимок экрана со страницей Hendecagram массива")

Анимация часто требует преобразования. **Hendecagram анимации** страницы перемещаются звездочка указывает 11 в кружке. [ `HendecagramAnimationPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/HendecagramAnimationPage.cs) Класс начинается с некоторые поля и переопределений `OnAppearing` и `OnDisappearing` методы для запуска и остановки таймера Xamarin.Forms:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    const double cycleTime = 5000;      // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float angle;

    public HendecagramAnimationPage()
    {
        Title = "Hedecagram Animation";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            double t = stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime;
            angle = (float)(360 * t);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }

            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`angle` Поле анимируется от 0 до 360 градусов каждые 5 секунд. `PaintSurface` Обработчик использует `angle` свойство двумя способами: для указания цветового тона цвета в `SKColor.FromHsl` метода и в качестве аргумента для `Math.Sin` и `Math.Cos` методы для управления расположение звезды:

```csharp
public class HendecagramAnimationPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();
        canvas.Translate(info.Width / 2, info.Height / 2);
        float radius = (float)Math.Min(info.Width, info.Height) / 2 - 100;

        using (SKPaint paint = new SKPaint())
        {
            paint.Style = SKPaintStyle.Fill;
            paint.Color = SKColor.FromHsl(angle, 100, 50);

            float x = radius * (float)Math.Sin(Math.PI * angle / 180);
            float y = -radius * (float)Math.Cos(Math.PI * angle / 180);
            canvas.Translate(x, y);
            canvas.DrawPath(HendecagramPage.HendecagramPath, paint);
        }
    }
}
```

`PaintSurface` Вызовов обработчика `Translate` метод дважды, чтобы сначала преобразуются в центре полотна, а затем преобразовывать длины окружности по центральным (0, 0). Радиус окружности задано быть как можно большего размера, при этом оставляя звезды в пределах страницы:

[![](translate-images/hendecagramanimation-small.png "Тройной снимок экрана со страницей анимации Hendecagram")](translate-images/hendecagramanimation-large.png#lightbox "тройной снимок экрана со страницей Hendecagram анимации")

Обратите внимание, что звезды сохраняет ту же ориентацию, как она строится на центре страницы. Объект не поворачивается вообще. Это задание для преобразования вращения.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
