---
title: Рисование окружности простой
description: Основные сведения о документе SkiaSharp, включая полотна и рисования
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 402470c3a27ba4327afa6e77336d60748abad436
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="drawing-a-simple-circle"></a>Рисование окружности простой

_Основные сведения о документе SkiaSharp, включая полотна и рисования_

В этой статье описаны основные понятия графики в Xamarin.Forms с помощью SkiaSharp, включая создание `SKCanvasView` объектов для размещения графики, обработка `PaintSurface` событий и использование `SKPaint` объект для задания цвета и другие графические атрибуты.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программа содержит все образцы кода для этой серии статей SkiaSharp. Имеет право на первой странице **круг простой** и вызывает класс страницы [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Этот код показано, как Рисование окружности в центре страницы с радиусом 100 пикселей. Структура круга красного цвета, и внутреннюю часть круга — синим.

![](circle-images/circleexample.png "Голубой круг выделено красным цветом")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Страницы класс является производным от `ContentPage` и содержит два `using` директивы для пространства имен SkiaSharp:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Создает следующий конструктор класса [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/) объекта, присоединяет обработчик [ `PaintSurface` ](https://developer.xamarin.com/api/event/SkiaSharp.Views.Forms.SKCanvasView.PaintSurface/) событий и наборы `SKCanvasView` объект как содержимое страницы:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Занимает всю область содержимого страницы. Кроме того, можно объединять `SKCanvasView` с другими Xamarin.Forms `View` производные, как можно будет увидеть в других примерах.

`PaintSurface` Обработчик событий — которых выполняется в документ. Этот метод обычно вызывается несколько раз во время выполнения программы, поэтому он должен поддерживать все данные необходимые для воссоздания отображения графики:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs/) Объект, который сопровождает событие имеет два свойства:

- [`Info`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info/) типа [`SKImageInfo`](https://developer.xamarin.com/api/type/SkiaSharp.SKImageInfo/)
- [`Surface`](https://developer.xamarin.com/api/property/SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface/) типа [`SKSurface`](https://developer.xamarin.com/api/type/SkiaSharp.SKSurface/)

`SKImageInfo` Структура содержит сведения о поверхности рисования важнее всего, это ширину и высоту в пикселях. `SKSurface` Представляет поверхность рисования сам объект. В этой программе поверхности рисования является дисплея, но в других программах `SKSurface` также может представлять объект, который позволяет рисовать на SkiaSharp растрового изображения.

Наиболее важным свойством `SKSurface` — [ `Canvas` ](https://developer.xamarin.com/api/property/SkiaSharp.SKSurface.Canvas/) типа [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/). Этот класс представляет собой графики, рисование контекст, который можно использовать для выполнения фактического рисования. `SKCanvas` Объект инкапсулирует состояние графики, включая преобразования графики и обрезки.

Ниже приведен типичный начала `PaintSurface` обработчик событий:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[ `Clear` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.Clear()/) Метод очищает холст и прозрачный цвет. Перегрузка позволяет указать цвет фона для canvas.

Цель — для рисования синюю заливку красный кружок. Из-за этого конкретного изображения содержит две различные цвета, задание необходимо сделать в два этапа. Первым шагом является Чтобы нарисовать контур круга. Чтобы задать цвет и другие характеристики линии, создайте и инициализируйте [ `SKPaint` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaint/) объекта:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[ `Style` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Style/) Свойство указывает, что вы хотите *штриха* строки (в данном случае структуры круга) вместо *заполнения* внутренней. Три члена [ `SKPaintStyle` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPaintStyle/) перечисления, как показано ниже:

- [`Fill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Fill/)
- [`Stroke`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.Stroke/)
- [`StrokeAndFill`](https://developer.xamarin.com/api/field/SkiaSharp.SKPaintStyle.StrokeAndFill/)

Значение по умолчанию — `Fill`. Третий параметр обводки линии и с тем же цветом заливки.

Задать [ `Color` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.Color/) свойства со значением типа [ `SKColor` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColor/). Один из способов получения `SKColor` значение — преобразование Xamarin.Forms `Color` значение `SKColor` значение с помощью метода расширения [ `ToSKColor` ](https://developer.xamarin.com/api/member/SkiaSharp.Views.Forms.Extensions.ToSKColor/p/Xamarin.Forms.Color/). [ `Extensions` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.Extensions/) В класс `SkiaSharp.Views.Forms` пространство имен включает другие методы, которые преобразуют между Xamarin.Forms и SkiaSharp значениями.

[ `StrokeWidth` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.StrokeWidth/) Свойство указывает толщину линии. Здесь устанавливается значение 25 пикселей.

Используется, `SKPaint` для рисования круга:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Координаты указываются относительно верхнего левого угла отображаемой области. Увеличение справа и выхода из строя увеличение координаты Y координаты X. Часто дискуссию о графики математической нотации (x, y) используется для обозначения точки. Точка (0, 0) верхнего левого угла отображаемой области и часто называется *источника*.

Первые два аргумента из `DrawCircle` указывают координаты X и Y центра окружности. Они назначаются половины ширины и высоты отображаемой области для размещения центра круга по центру в отображаемой области. Третий аргумент задает радиус окружности, а последний аргумент является `SKPaint` объекта.

Для заливки круга, его можно изменить два свойства `SKPaint` и вызовите `DrawCircle` еще раз. Данный код также отображает альтернативный способ получения `SKColor` значение из одного из многих поля [ `SKColors` ](https://developer.xamarin.com/api/type/SkiaSharp.SKColors/) структуры:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
На этот раз `DrawCircle` вызов заполняет круг с помощью новых свойств `SKPaint` объекта.

Вот программу на iOS, Android и универсальной платформы Windows.

[![](circle-images/simplecircle-small.png "Тройной снимок экрана со страницей круг простой")](circle-images/simplecircle-large.png#lightbox "тройной снимок экрана со страницей простой окружности")

При выполнении программы самостоятельно, можно включить номера телефона или симулятор сбоку чтобы увидеть, как перерисовке рисунок. Каждый раз, рисунок должен быть перерисованы `PaintSurface` вызывается обработчик событий.

`SKPaint` Больше, чем коллекцию рисования свойства объекта. Эти объекты являются очень простая. Можно повторно использовать `SKPaint` объектов как реализовано этой программы, или можно создать несколько `SKPaint` объектов для отображения свойств различных комбинаций. Можно создать и инициализировать эти объекты за пределами `PaintSurface` обработчик событий и сохраните их как поля в классе страницы.

Несмотря на то, что ширина контура круга указывается как 25 пикселей &mdash; или части радиус окружности &mdash; похоже на более тонкие и дает веские причины для этого: голубой круг замещается размером в половину ширины линии. Аргументы для `DrawCircle` метод определения абстрактный координаты геометрического окружности. Синий внутреннюю подбирается к такому измерению к ближайшей точке, но 25 пикселей всей структуры захватывающая геометрические круг &mdash; половина на внутренние и половина с внешней стороны.

Следующий пример в [интеграции с помощью Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) статье демонстрируется это визуально.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
