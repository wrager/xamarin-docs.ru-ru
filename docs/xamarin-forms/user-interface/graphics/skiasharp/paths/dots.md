---
title: "Точек и тире"
description: "Рисование пунктирная и пунктирных линий в SkiaSharp особенностей-шаблоны"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8E9BCC13-830C-458C-9FC8-ECB4EAE66078
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 32326eb472b1bb8b4fbaf4066edc36255f5948ca
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="dots-and-dashes"></a>Точек и тире

_Рисование пунктирная и пунктирных линий в SkiaSharp особенностей-шаблоны_

SkiaSharp позволяет рисования линий, которые не являются сплошными, но вместо состоят из точек и тире.

![](dots-images/dottedlinesample.png "Пунктирная линия")

Для этого с *эффект пути*, который является экземпляром класса [ `SKPathEffect` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPathEffect/) класс, который задается для [ `PathEffect` ](https://developer.xamarin.com/api/property/SkiaSharp.SKPaint.PathEffect/) свойство `SKPaint`. Можно создать путь эффект (или эффекты объединения пути), с помощью статического `Create` методов, определенных `SKPathEffect`.

Чтобы нарисовать точечных или пунктирных линий, используйте [ `SKPathEffect.CreateDash` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPathEffect.CreateDash/p/System.Single[]/System.Single/) статический метод. Существует два аргумента: это массив `float` значения, указывающие длин точек и тире и длины пробелов между ними. Этот массив должен иметь четное число элементов и должно быть по крайней мере два элемента. (Может быть нуль элементов в массиве, однако, результаты в сплошная линия.) Если имеются два элемента, первый — длина точку или дефис, а второй — длину промежутка до следующей точки или тире. Если имеется более двух элементов, то они находятся в следующем порядке: тире длины, несоответствие длины, длину штриха, длина интервала и т. д.

Как правило имеет смысл сделать длину штриха и интервала кратно толщину обводки. Если ширина штриха 10 точек, например, массив {10, 10} будет нарисуйте пунктирная линия точек и пробелов в которых одинаковую длину поля толщину обводки.

Тем не менее `StrokeCap` параметра `SKPaint` объекта также влияет на эти точки и дефисы. Как вы увидите, что оказывает влияние на элементы исходного массива.

Точечные и пунктирными линиями показаны на **точек и тире** страницы. [ **DotsAndDashesPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml) файл создает два `Picker` просматривает, один для возможности выбирать окончаний обводки, а второй — выберите массив тире:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.DotsAndDashesPage"
             Title="Dots and Dashes">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>Butt</x:String>
                <x:String>Round</x:String>
                <x:String>Square</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="dashArrayPicker"
                Title="Dash Array"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>10, 10</x:String>
                <x:String>30, 10</x:String>
                <x:String>10, 10, 30, 10</x:String>
                <x:String>0, 20</x:String>
                <x:String>20, 20</x:String>
                <x:String>0, 20, 20, 20</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           Grid.Row="1"
                           Grid.Column="0"
                           Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Первые три элемента в `dashArrayPicker` Предположим, что ширина штриха 10 точек. {10, 10} массив — пунктирная линия, {30, 10} — пунктирная линия и {10, 10, 30, 10} для точки штриховой линией. (Остальные три обсуждаются вскоре.)

[ `DotsAndDashesPage` Файл кода программной части](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/DotsAndDashesPage.xaml.cs) содержит `PaintSurface` обработчик событий и ряд вспомогательные процедуры для доступа к `Picker` представления:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = (SKStrokeCap)Enum.Parse(typeof(SKStrokeCap),
                        strokeCapPicker.Items[strokeCapPicker.SelectedIndex]),

        PathEffect = SKPathEffect.CreateDash(GetPickerArray(dashArrayPicker), 20)
    };

    SKPath path = new SKPath();
    path.MoveTo(0.2f * info.Width, 0.2f * info.Height);
    path.LineTo(0.8f * info.Width, 0.8f * info.Height);
    path.LineTo(0.2f * info.Width, 0.8f * info.Height);
    path.LineTo(0.8f * info.Width, 0.2f * info.Height);

    canvas.DrawPath(path, paint);
}

T GetPickerItem<T>(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return default(T);
    }

    return (T)Enum.Parse(typeof(T), picker.Items[picker.SelectedIndex]);
}

float[] GetPickerArray(Picker picker)
{
    if (picker.SelectedIndex == -1)
    {
        return new float[0];
    }

    string[] strs = picker.Items[picker.SelectedIndex].Split(new char[] { ' ', ',' },
                                                             StringSplitOptions.RemoveEmptyEntries);
    float[] array = new float[strs.Length];

    for (int i = 0; i < strs.Length; i++)
    {
        array[i] = Convert.ToSingle(strs[i]);
    }
    return array;
}
```

На следующих снимках экрана экрана операций ввода-вывода в крайней левой показано пунктирной линией.

[![](dots-images/dotsanddashes-small.png "Тройной снимок экрана со страницей точек и тире")](dots-images/dotsanddashes-large.png#lightbox "тройной снимок экрана со страницей точек и тире")

Однако Android экрана также должен показать пунктирной линией, с помощью массива {10, 10}, но вместо сплошной линии. Что произошло? Проблема заключается в Android экрана, также, имеет значение caps штриха `Square`. При этом расширяется все дефисы на половине толщины обводки, доступ к ним для заполнения пропусков.

Чтобы обойти эту проблему, при использовании окончаний обводки из `Square` или `Round`, необходимо уменьшить тире длины массива расстоянием stroke (иногда в результате чего тире длиной 0) и увеличения длины разрыв на длину штриха. Это как окончательный трех штрих массивов в `Picker` вычисляются в XAML-файле:

- {10, 10} становится {0, 20} пунктирная линия
- {30, 10} становится {20, 20} для пунктирная линия
- {10, 10, 30, 10} становится {0, 20, 20, 20} пунктирная и штриховой линии

Ограничения максимального экрана показывает Windows, точечные и пунктирных линий для штриха из `Round`. `Round` Окончаний обводки часто обеспечивает лучший вида точек и тире толстой линии.

До сих не указываются не выполнена для второго параметра `SKPathEffect.CreateDash` метод. Этот параметр называется `phase` и он ссылается на смещение в шаблон и пунктирная для начало строки. Например, если массив тире {10, 10} и `phase` равно 10, то строка начинается с пробел, а не точки.

Одно из интересных применений `phase` параметр находится в анимации. **Анимированы спирали** страница подобна **спирали Archimedean** страницы, за исключением случаев, [ `AnimatedSpiralPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/LinesAndPaths/AnimatedSpiralPage.cs) класс анимирует `phase` параметр. Этой странице также показан другой подход для анимации. Предыдущий пример из [ `PulsatingEllipsePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml.cs) используется `Task.Delay` способом управления анимации. В этом примере используется вместо Xamarin.Forms `Device.Timer` метод:


```csharp
const double cycleTime = 250;       // in milliseconds

SKCanvasView canvasView;
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float dashPhase;

public AnimatedSpiralPage()
{
    Title = "Animated Spiral";

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
        dashPhase = (float)(10 * t);
        canvasView.InvalidateSurface();

        if (!pageIsActive)
        {
            stopwatch.Stop();
        }

        return pageIsActive;
    });
}
```

Конечно вы получите фактически работающих с программой, чтобы увидеть анимацию.

[![](dots-images/animatedspiral-small.png "Тройной снимок экрана со страницей анимированы спирали")](dots-images/animatedspiral-large.png#lightbox "тройной снимок экрана со страницей спирали анимации")

Теперь вы знаете, как для рисования линий и кривых с помощью параметрических уравнений определения. Раздел должен быть опубликован в более поздней версии будут рассмотрены различные типы кривых, `SKPath` поддерживает.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
