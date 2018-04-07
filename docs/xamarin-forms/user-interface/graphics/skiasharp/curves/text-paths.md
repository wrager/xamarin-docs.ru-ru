---
title: Путей и текста
description: Просмотр пересечение путей и текста
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: 9b3f906a23ed0d51237a244f3944104acc76e259
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="paths-and-text"></a>Путей и текста

_Просмотр пересечение путей и текста_

В современных графических систем шрифты текста являются коллекциями контуров символов, обычно определяется кривых Безье второго порядка. Следовательно для многих современных графических систем включают средства для преобразования символов текста в графический контур. 

Вы уже видели, вы может вычерчивания контуров символов текста а также их заполнения. Это позволяет отображать этих контуров символов с определенной толщина и даже эффект, как описано в [ **эффекты путь** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) статьи. Но можно также преобразовать строку символов в `SKPath` объекта. Это означает, что рамку текста можно использовать для обрезки с методами, которые были описаны в [ **Обрезка с использованием пути и областей** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) статьи. 

Помимо использования эффект пути для вычерчивания структуры символов, также можно создать путь эффекты, которые основаны на путях к являются производными от символьных строк и можно даже комбинировать два последствия:

![](text-paths-images/pathsandtextsample.png "Эффект пути текста")

В [предыдущей статьи](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) вы узнали, как [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) метод `SKPaint` можно получить контуром пунктирный путь. Этот метод также можно использовать с путями, производным от контуров символов.

Наконец, в этой статье демонстрируется другой пересечение путей и текста: [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) метод `SKCanvas` позволяет отображать строку текста базовой линии текста после пути кривой.

## <a name="text-to-path-conversion"></a>Текст для преобразования пути

[ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Метод `SKPaint` преобразует строку символов в `SKPath` объекта:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

`x` И `y` аргументы указывает начальную точку показателя в левой части текста. Они играют ту же роль здесь в качестве `DrawText` метод `SKCanvas`. В пути базовых показателей в левой части текста будет иметь координаты (x, y). 

`GetTextPath` Метод является избыточным, если требуется просто добавить заливку или обводку результирующий путь. Нормали `DrawText` метод позволяет это сделать. `GetTextPath` Метод более полезен для других задач, связанных с пути.

Одной из этих задач обрезки. **Обрезки текста** страница создает контура обрезки, основанных на структуре символ слова «CODE». Этот путь растягивается на размер страницы для обрезки точечного рисунка, который содержит изображение **обрезки текста** исходный код:

[![](text-paths-images/clippingtext-small.png "Тройной снимок экрана со страницей обрезки текста")](text-paths-images/clippingtext-large.png#lightbox "тройной снимок экрана со страницей обрезки текста")

[ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Конструктор класса загружает точечного рисунка, который хранится в виде внедренного ресурса в **Media** папки решения:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

`PaintSurface` Обработчик начинается с создания `SKPaint` подходит для текста объекта. `Typeface` Свойству а также `TextSize`, хотя для этого конкретного приложения `TextSize` свойство является исключительно arbirtrary. Также Обратите внимание, не `Style` параметр.

`TextSize` И `Style` не нужны параметры свойств так как это `SKPaint` объект используется исключительно для `GetTextPath` вызовы с использованием текстовая строка «CODE». Обработчик затем измеряет итоговые `SKPath` объекта и применяет три преобразования, которые его по центру и масштабировать его размер страницы. Путь можно затем задать в качестве контура обрезки:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap, 
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

После задания контура обрезки растрового изображения могут быть отображены, и он будет обрезан по ее контур символа. Обратите внимание на использование [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) метод `SKRect` , вычисляет прямоугольник для заполнения страницы, сохраняя пропорции.

**Эффект текста пути** страницы преобразует символ амперсанда в путь для создания эффекта пути 1 D. Объект paint с этого пути используется для вычерчивания контура увеличенную версию того же символа:

[![](text-paths-images/textpatheffect-small.png "Тройной снимок экрана со страницей эффект текста пути")](text-paths-images/textpatheffect-large.png#lightbox "тройной снимок экрана со страницей эффект пути текста")

Большую часть работы в [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) класс происходит в полях и на конструктор. Два `SKPaint` объекты, определенные как поля используются для двух разных целей: первый (с именем `textPathPaint`) используется для преобразования амперсанда с `TextSize` 50 пути для эффект пути 1 D. Второй (`textPaint`) используется для отображения больших версий амперсанд эффект этого пути. По этой причине `Style` этот второй краски, присваивается значение `Stroke`, но `StrokeWidth` свойство не задано, так как это свойство не является обязательным при использовании эффект пути 1 D:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint 
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character, 
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Конструктор сначала использует `textPathPaint` объект для измерения амперсанда с `TextSize` 50. Далее передаются отрицательных результатов координат центра этого прямоугольника `GetTextPath` метод для преобразования текста в контур. Результирующий путь имеет (0, 0) на момент center символа, который идеально подходит для эффекта пути 1D.

Может показаться, что `SKPathEffect` может принимать значение объекта, созданного в конце конструктора `PathEffect` свойство `textPaint` , а не сохранены как поле. Но этот отключен out не очень хорошо работает, поскольку она искажение результаты `MeasureText` вызов в `PaintSurface` обработчика:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Что `MeasureText` используется вызов по центру символов на странице. Чтобы избежать проблем, `PathEffect` свойству рисования объекта после измерить текст, но перед отображением.

## <a name="outlines-of-character-outlines"></a>Контуры символов

Обычно [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) метод `SKPaint` преобразует один путь в другой, применяя свойства paint особенно штриха ширины и путь эффект. При использовании без эффектов путь `GetFillPath` фактически создает путь, описывающий другой путь. Это было продемонстрировано в **коснитесь, чтобы структуры путь** страницы в [ **эффекты путь** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) статьи.

Можно также вызвать `GetFillPath` на путь, возвращаемый из `GetTextPath` , но сначала может быть полностью убедиться, что, должен выглядеть следующим образом.

**Контуров символов структуры** метод продемонстрирован страницы. Соответствующий код находится в `PaintSurface` обработчик [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) класса.

Конструктор начинается с создания `SKPaint` объект с именем `textPaint` с `TextSize` свойство на основе размера страницы. Это преобразуется в пути с помощью `GetTextPath` метод. Координат аргументы `GetTextPath` эффективно центру пути на экране:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

`PaintSurface` Обработчик затем создается новый путь с именем `outlinePath`. Это становится целевой путь в вызове `GetFillPath`. `StrokeWidth` Свойство 25 причины `outlinePath` для описания структуры пути 25 пикселей всей срезом символов текста. Затем этот путь отображается красным цветом толщину обводки 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Тройной снимок экрана со страницей контуров символов структуры")](text-paths-images/characteroutlineoutlines-large.png#lightbox "тройной снимок экрана со страницей контуров символов структуры")

Внимательно посмотрите и вы увидите перекрытия, когда структура пути делает острого угла. Это обычный артефакты этого процесса.

## <a name="text-along-a-path"></a>Текст в пути

Обычно текст отображается по горизонтальной базовой линии. Для запуска по вертикали или по диагонали можно вращать текст, но базового плана по-прежнему является прямая линия.

Бывают случаи, тем не менее, когда требуется текст, пройдите кривой. Это назначение [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) метод `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Текст, указанный в качестве первого аргумента выполняется Пройдите путь, указанный в качестве второго аргумента. Можно начать текст со смещением от начала пути с `hOffset` аргумент. Обычно путь forms линии шрифта: верхний выносной элемент текста — на одной стороне пути, а подстрочные элементы текста — на другом. Но можно смещения базовой линии текста из пути с `vOffset` аргумент.

Этот метод не содержит средств для предоставления рекомендации о параметр `TextSize` свойство `SKPaint` для выравнивания текста, размера идеально для запуска с начала пути до конца. Иногда можно понять, такой размер текста самостоятельно. В других случаях необходимо использовать функции измерения путь для описано в следующей статье.

**Циклические текст** программа переносит текст вокруг круга. Можно легко определить длины окружности, поэтому его легко изменение размера текста по размеру точно. `PaintSurface` Обработчик [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) класс вычисляет радиус окружности, исходя из размера страницы. Становится, вращающимся `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

`TextSize` Свойство `textPaint` затем корректируются таким образом, чтобы ширина текста соответствует окружности круга:

[![](text-paths-images/circulartext-small.png "Тройной снимок экрана со страницей циклические текст")](text-paths-images/circulartext-large.png#lightbox "тройной снимок экрана со страницей циклические текста")

Сам текст был выбран для так же оказаться довольно циклические: слова «circle», как субъект предложения и объект предложная фраза. 

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
