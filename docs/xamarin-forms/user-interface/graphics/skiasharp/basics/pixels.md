---
title: Пикселей и аппаратно независимых единицах
description: Ознакомьтесь с различиями между SkiaSharp координаты и Xamarin.Forms
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 26C25BB8-FBE8-4B77-B01D-16A163A16890
author: charlespetzold
ms.author: chape
ms.date: 02/09/2017
ms.openlocfilehash: dd9694a05632cd5f37cb583d15bc93311a49cfdc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="pixels-and-device-independent-units"></a>Пикселей и аппаратно независимых единицах

_Ознакомьтесь с различиями между SkiaSharp координаты и Xamarin.Forms_

В этой статье исследуются различия в системе координат, используемой в SkiaSharp и Xamarin.Forms. Можно получить сведения для преобразования между двумя системами координат и также нарисуйте графики, которая заполнения определенной области.

![](pixels-images/screenfillexample.png "Овал, который заполняет экрана")

Если вы программирования в Xamarin.Forms в течение некоторого времени, может потребоваться поведение в соответствии с Xamarin.Forms координаты и размеры. В двух предыдущих статей круги может показаться слишком маленьким для вас.

Эти круги *,* небольших по сравнению с размерами Xamarin.Forms. По умолчанию SkiaSharp рисует в единицах точек во время Xamarin.Forms сформирует координаты и размеры на аппаратно независимая единица, заданные в базовой платформы. (Дополнительные сведения о системе координат Xamarin.Forms можно найти в [Глава 5. Работа с размерами](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter05.md) книги *Создание мобильных приложений с помощью Xamarin.Forms*.)

Страницы в [ **SkewSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программы под названием **размер поверхности** используется SkiaSharp выходного текста для отображения размер отображаемой области из трех различных источников:

- Обычный Xamarin.Forms [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) и [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) свойства `SKCanvasView` объекта.
- [ `CanvasSize` ](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKCanvasView.CanvasSize/) Свойство `SKCanvasView` объекта.
- [ `Size` ](https://developer.xamarin.com/api/property/SkiaSharp.SKImageInfo.Size/) Свойство `SKImageInfo` значение, которое соответствует `Width` и `Height` свойства, используемые на двух предыдущих страницах.

[ `SurfaceSizePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SurfaceSizePage.cs) Класс показывает порядок отображения этих значений. Конструктор сохраняет `SKCanvasView` объекта в поле, чтобы он был доступен в `PaintSurface` обработчик событий:

```csharp
SKCanvasView canvasView;

public SurfaceSizePage()
{
    Title = "Surface Size";

    canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvas` включает шесть разных `DrawText` методы, но это [ `DrawText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawText/p/System.String/System.Single/System.Single/SkiaSharp.SKPaint/) метод является наиболее простым:

```csharp
public void DrawText (String text, Single x, Single y, SKPaint paint)
```

Указать текстовую строку, в котором начинается, текст координаты X и Y и `SKPaint` объекта. Координата X указывает, расположены в левой части текста, но Контрольные значения ожидания: координату по оси y определяет положение *базовые* текста. Если когда-либо написания вручную на подчеркнутый бумаги, базового плана — это линия, на какие sit символов и ниже опускаются какие подстрочных элементов (например, на буквы g, p, q и y).

`SKPaint` Позволяет указать цвет текста, семейство шрифтов и размер текста. По умолчанию [ `TextSize` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.TextSize/) свойство имеет значение 12, что приводит к очень мало текста на устройствах с высоким разрешением, например телефонов. В простой приложений, кроме вам потребуются некоторые сведения о размере текста, который будет отображаться. `SKPaint` Класс определяет [ `FontMetrics` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontMetrics/) свойств и несколько [ `MeasureText` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.MeasureText/p/System.String/) методы, но меньше правило, понимаем потребностям [ `FontSpacing` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.FontSpacing/) свойство предоставляет Рекомендованное значение для интервала последовательных строк текста.

Следующие `PaintSurface` создает обработчик `SKPaint` для объекта `TextSize` 40 пикселей, в которых находится нужное высоты текста из верхней части верхний выносной элемент в конец подстрочные элементы. `FontSpacing` Значением, которое `SKPaint` возвращает объект немного больше, чем, о 47 пикселей.

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 40
    };

    float fontSpacing = paint.FontSpacing;
    float x = 20;               // left margin
    float y = fontSpacing;      // first baseline
    float indent = 100;

    canvas.DrawText("SKCanvasView Height and Width:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(String.Format("{0:F2} x {1:F2}",
                                  canvasView.Width, canvasView.Height),
                    x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKCanvasView CanvasSize:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(canvasView.CanvasSize.ToString(), x + indent, y, paint);
    y += fontSpacing * 2;
    canvas.DrawText("SKImageInfo Size:", x, y, paint);
    y += fontSpacing;
    canvas.DrawText(info.Size.ToString(), x + indent, y, paint);
}
```

Метод начинает первой строки текста с координатами X 20 (для мало поле слева) и координату Y `fontSpacing`, который немного более, чем нужно для отображения полной высотой первой строки текста в верхней части области отображения. После каждого вызова `DrawText`, координаты Y увеличивается на один или два шагом `fontSpacing`.

Вот программу на всех трех платформ.

[![](pixels-images/surfacesize-small.png "Тройной снимок экрана со страницей размер поверхности")](pixels-images/surfacesize-large.png#lightbox "тройной снимок экрана со страницей размер рабочей области")

Как видите, `CanvasSize` свойство `SKCanvasView` и `Size` свойство `SKImageInfo` значение согласованы в отчетности в пикселах. `Height` И `Width` свойства `SKCanvasView` являются свойства Xamarin.Forms и отчет, размер представления в аппаратно независимых единицах, определенные платформой.

Имитатор iOS 7 слева имеет 2 пикселя в аппаратно независимая единица, Android 5 хранилища в центре имеет 3 точки на единицу и 925 Lumia Nokia справа имеет 2,25 пикселей на единицу. Что почему простой circle показано ранее выглядит о одинакового размера для iPhone и Windows phone, но меньше на телефоне Android.

Если вы предпочитаете работать полностью в аппаратно независимых единицах, это можно сделать, задав `IgnorePixelScaling` свойство `SKCanvasView` для `true`. Тем не менее могут не устроит результат. SkiaSharp отображает графики на поверхности меньшего размера устройства равен размеру представления в аппаратно независимых единицах размера пикселей. (Например, SkiaSharp будет использоваться отображаемой области 360 x 512 пикселей на 5 хранилища.) Затем масштабировании изображения в размер, что jaggies заметно растрового изображения.

Для поддержания же разрешение изображения, лучшим решением заключается в написании простых функций для преобразования между двумя системами координат.

В дополнение к `DrawCircle` метода `SKCanvas` также определяет два `DrawOval` методы, которые нарисовать эллипс. Эллипс определяется двумя радиусы вместо одного radius. Эти функции известны как *основных radius* и *дополнительный radius*. `DrawOval` Метод Рисование эллипса с двумя радиусы параллельно осей X и Y. Это ограничение можно обойти с преобразованиями или использование графический контур (Чтобы рассматриваться позже), но [это `DrawOval` метод](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/System.Single/System.Single/System.Single/System.Single/SkiaSharp.SKPaint/) имена двух радиусы аргумент `rx` и `ry` указывает, что параллельно осей X и Y:

```csharp
public void DrawOval (Single cx, Single cy, Single rx, Single ry, SKPaint paint)
```

Существует возможность нарисовать эллипс, который заполняет область отображения? **Заполнения эллипса** страницы показано, как. `PaintSurface` Обработчик событий в [ **EllipseFillPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/EllipseFillPage.xaml.cs) класс вычитает размером в половину ширины обводки из `xRadius` и `yRadius` значения по размеру всей эллипса и его структуры в отображаемой области:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float strokeWidth = 50;
    float xRadius = (info.Width - strokeWidth) / 2;
    float yRadius = (info.Height - strokeWidth) / 2;

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = strokeWidth
    };
    canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
}
```

Здесь выполняется на трех платформах:

[![](pixels-images/ellipsefill-small.png "Тройной снимок экрана со страницей размер поверхности")](pixels-images/ellipsefill-large.png#lightbox "тройной снимок экрана со страницей размер рабочей области")

[Других `DrawOval` метод](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawOval/p/SkiaSharp.SKRect/SkiaSharp.SKPaint/) имеет [ `SGRect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKRect/) аргументом, который представляет собой прямоугольник, определенные в терминах координаты X и Y верхнего левого угла и нижнего правого угла. Овал заполняет этот прямоугольник, который предполагает, что могут быть доступны для использования в **заполнения эллипса** страницы следующим образом:

```csharp
SKRect rect = new SKRect(0, 0, info.Width, info.Height);
canvas.DrawOval(rect, paint);
```

Тем не менее, который усекает всех краев контура эллипса четыре стороны. Необходимо настроить все `SKRect` на основе аргументов конструктора `strokeWidth` для этого справа:

```csharp
SKRect rect = new SKRect(strokeWidth / 2,
                         strokeWidth / 2,
                         info.Width - strokeWidth / 2,
                         info.Height - strokeWidth / 2);
canvas.DrawOval(rect, paint);
```


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
