---
title: "Аффинные преобразования"
description: "Создание перспективы и конический эффектов с третьего столбца матрицы преобразования"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 785F4D13-7430-492E-B24E-3B45C560E9F1
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 2e2e83404bc93bd07885008b868c51eba2ff7140
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="non-affine-transforms"></a>Аффинные преобразования

_Создание перспективы и конический эффектов с третьего столбца матрицы преобразования_

Преобразование, масштабирования, поворота и наклон классифицируются как *аффинного* преобразования. Аффинные преобразования сохраняют параллельных линий. Если две строки параллельных до преобразования, остаются параллельных после преобразования. Прямоугольники, всегда преобразуются в parallelograms.

Однако SkiaSharp также способен неаффинных преобразований, которые имеют возможность преобразовать прямоугольник в любой выпуклая четырехугольника осуществляется плавный:

![](non-affine-images/nonaffinetransformexample.png "Преобразовать в выпуклая четырехугольника осуществляется плавный растрового изображения")

Выпуклая четырехугольника осуществляется плавный — это фигура четырех сторон, с внутренних углов всегда меньше, чем 180 градусов, так и которые не пересекаются.

Аффинные не преобразует результат с третьей строки матрицы преобразования имеет значение для значения, отличные от 0, 0 и 1. Полный `SKMatrix` умножение является:

<pre>
              │ ScaleX  SkewY   Persp0 │
| x  y  1 | × │ SkewX   ScaleY  Persp1 │ = | x'  y'  z' |
              │ TransX  TransY  Persp2 │
</pre>

Преобразование итоговые формулы — это:

x' = ScaleX·x + SkewX·y + TransX

y "= SkewY·x + ScaleY·y + TransY

z' = Persp0·x + Persp1·y + Persp2

Основным правилом с помощью матрицы 3 x 3 для преобразований в двумерном — что все содержимое остается на плоскости где Z равно 1. Если не `Persp0` и `Persp1` : 0, и `Persp2` равно 1, то преобразование перемещено координаты Z, плоскость.

Чтобы восстановить двумерного преобразования, координаты должны быть перемещены назад для этой плоскости. Еще один шаг является обязательным. X', y ", и z «значений необходимо разделить на z»:

x" = x' / z'

y" = y' / z'

z" = z' / z' = 1

Эти функции известны как *однородных координаты* и они были разработаны поля Ferdinand Möbius августа, гораздо лучше известных для его топологические oddity Möbius ленты.

Если z "равно 0, результаты деления в бесконечный координатах. На самом деле одной из создавались в Möbius для однородной координаты была возможность представляют собой бесконечные значения с конечным числом.

Для отображения графических объектов, тем не менее, вы хотите избежать визуализации использование координат, преобразовать в бесконечные значения. Эти координаты не будут отображаться. Все, что вблизи эти координаты будет иметь очень большой и визуально, скорее всего, не согласовано.

В этой формуле нужно значение z "становится нулем:

z' = Persp0·x + Persp1·y + Persp2

`Persp2` Ячейки может быть нулевой или не ноль. Если `Persp2` равно нулю, то z "равен нулю, точки (0, 0), и который нежелательно обычно из-за этой точки очень распространены в двумерной графики. Если `Persp2` не равен нулю, и если отсутствует потеря универсальности `Persp2` фиксируется на 1. Например, если выяснится, что `Persp2` должно быть 5, то можно просто разделить все ячейки матрицы, 5, благодаря чему `Persp2` равно 1, а результат будет таким же.

По этим причинам `Persp2` часто фиксируется на 1, это же значение в единичной матрицей.

Как правило `Persp0` и `Persp1` — небольшого числа. Предположим, что вы начинаете работать с единичной матрицей, но набор `Persp0` 0,01:

<pre>
| 1  0   0.01 |
| 0  1    0   |
| 0  0    1   |
</pre>

Преобразование формул являются:

x' = x / (0.01·x + 1)

y "= y-(0.01·x + 1)

Теперь можно используйте это преобразование к просмотру 100 пикселов квадрат располагается в источнике. Вот, как преобразуются в четырех углах.

(0, 0) → (0, 0)

(0, 100) → (0, 100)

(100, 0) → (50, 0)

(100, 100) → (50, 50)

Если x — 100, а затем z "знаменатель равен 2, поэтому координаты x и y являются фактически вдвое. Справа от поля становится меньше, чем левой стороны:

![](non-affine-images/nonaffinetransform.png "Поле, подвергались-аффинные преобразования")

`Persp` Часть из этих имен ячейки ссылается на «перспективы», поскольку foreshortening предполагает, что поле теперь наклонена с правой стороны от средства просмотра.

**Перспективы "Тест"** страницы позволяет вам поэкспериментировать со значениями `Persp0` и `Pers1` чтобы понять, как они работают. Приемлемые значения этих ячеек матрицы так малы, `Slider` в универсальной платформы Windows не может правильно их обработки. Для размещения проблемы UWP, два `Slider` элементов в [ **TestPerspective.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml) должны быть инициализированы в диапазоне от – 1 до 1:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.TestPerspectivePage"
             Title="Test Perpsective">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Label">
                    <Setter Property="HorizontalTextAlignment" Value="Center" />
                </Style>

                <Style TargetType="Slider">
                    <Setter Property="Minimum" Value="-1" />
                    <Setter Property="Maximum" Value="1" />
                    <Setter Property="Margin" Value="20, 0" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="persp0Slider"
                Grid.Row="0"
                ValueChanged="OnPersp0SliderValueChanged" />

        <Label x:Name="persp0Label"
               Text="Persp0 = 0.0000"
               Grid.Row="1" />

        <Slider x:Name="persp1Slider"
                Grid.Row="2"
                ValueChanged="OnPersp1SliderValueChanged" />

        <Label x:Name="persp1Label"
               Text="Persp1 = 0.0000"
               Grid.Row="3" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="4"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Обработчики событий для ползунков в [ `TestPerspectivePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TestPerspectivePage.xaml.cs) файл кода программной части разделить значения 100, чтобы они в диапазоне между –0.01 и 0,01. Кроме того конструктор загружает в точечном рисунке:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    SKBitmap bitmap;

    public TestPerspectivePage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }

    void OnPersp0SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp0Label.Text = String.Format("Persp0 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }

    void OnPersp1SliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        Slider slider = (Slider)sender;
        persp1Label.Text = String.Format("Persp1 = {0:F4}", slider.Value / 100);
        canvasView.InvalidateSurface();
    }
    ...
}
```

`PaintSurface` Вычисляет обработчик `SKMatrix` значение с именем `perspectiveMatrix` на основе значений этих двух ползунки, разделенное на 100. Используется вместе с двумя перевести преобразований, которые помещают center этого преобразования в центре растрового изображения:

```csharp
public partial class TestPerspectivePage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Calculate perspective matrix
        SKMatrix perspectiveMatrix = SKMatrix.MakeIdentity();
        perspectiveMatrix.Persp0 = (float)persp0Slider.Value / 100;
        perspectiveMatrix.Persp1 = (float)persp1Slider.Value / 100;

        // Center of screen
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);
        SKMatrix.PostConcat(ref matrix, perspectiveMatrix);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(xCenter, yCenter));

        // Coordinates to center bitmap on canvas
        float x = xCenter - bitmap.Width / 2;
        float y = yCenter - bitmap.Height / 2;

        canvas.SetMatrix(matrix);
        canvas.DrawBitmap(bitmap, x, y);
    }
}
```

Ниже приведены некоторые примеры.

[![](non-affine-images/testperspective-small.png "Тройной снимок экрана со страницей перспективы "Тест"")](non-affine-images/testperspective-large.png#lightbox "тройной снимок экрана со страницей перспективы "Тест"")

Во время экспериментов с ползунки, вы обнаружите, что значения за пределы 0.0066 или ниже –0.0066 привести к внезапно становятся части и сочетаются изображения. Преобразуемые точечного рисунка — квадрат 300 пикселей. Преобразования относительно его центра, поэтому координаты растрового изображения в диапазоне от –150 до 150. Помните, что значение z "является:

z' = Persp0·x + Persp1·y + 1

Если `Persp0` или `Persp1` больше, чем 0.0066 или ниже –0.0066, затем всегда есть некоторые координаты точечного рисунка, который приводит к z "нулевое значение. Вызывающий деление на ноль, а отрисовку становится карточки. При использовании-аффинных преобразований, вы хотите избежать создания любого другого элемента с координатами, вызывающие деления на ноль.

Как правило, вы не параметр `Persp0` и `Persp1` в изоляции. Также часто бывает необходимо задать других ячеек в матрице для достижения определенных типов, отличных от аффинных преобразований.

Одно из таких неаффинных преобразование является *конический преобразования*. Этот тип не аффинное преобразование сохраняет общие размеры прямоугольника, но истончается одной стороны:

![](non-affine-images/tapertransform.png "Поле, которое подвергается конический преобразования")

[ `TaperTransform` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransform.cs) Класс выполняет обобщенный вычисления на основе этих параметров не аффинного преобразования:

- размер прямоугольной преобразуемые, изображения
- Перечисление, указывающее стороны прямоугольника, истончается,
- другого перечисления, указывающее, каким образом истончается, и
- Ослабление экстент.

Ниже приведен код:

```csharp
enum TaperSide { Left, Top, Right, Bottom }

enum TaperCorner { LeftOrTop, RightOrBottom, Both }

static class TaperTransform
{
    public static SKMatrix Make(SKSize size, TaperSide taperSide, TaperCorner taperCorner, float taperFraction)
    {
        SKMatrix matrix = SKMatrix.MakeIdentity();

        switch (taperSide)
        {
            case TaperSide.Left:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp0 = (taperFraction - 1) / size.Width;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        matrix.TransY = size.Height * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Top:
                matrix.ScaleX = taperFraction;
                matrix.ScaleY = taperFraction;
                matrix.Persp1 = (taperFraction - 1) / size.Height;

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction);
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        matrix.TransX = size.Width * (1 - taperFraction) / 2;
                        break;
                }
                break;

            case TaperSide.Right:
                matrix.ScaleX = 1 / taperFraction;
                matrix.Persp0 = (1 - taperFraction) / (size.Width * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewY = size.Height * matrix.Persp0;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewY = (size.Height / 2) * matrix.Persp0;
                        break;
                }
                break;

            case TaperSide.Bottom:
                matrix.ScaleY = 1 / taperFraction;
                matrix.Persp1 = (1 - taperFraction) / (size.Height * taperFraction);

                switch (taperCorner)
                {
                    case TaperCorner.RightOrBottom:
                        break;

                    case TaperCorner.LeftOrTop:
                        matrix.SkewX = size.Width * matrix.Persp1;
                        break;

                    case TaperCorner.Both:
                        matrix.SkewX = (size.Width / 2) * matrix.Persp1;
                        break;
                }
                break;
        }
        return matrix;
    }
}
```

Этот класс используется в **конический преобразования** страницы. В файле XAML создает два `Picker` элементов для выбора значений перечисления и `Slider` по выбору конический дробной части. [ `PaintSurface` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TaperTransformPage.xaml.cs#L55) Обработчик объединяет конический преобразования с двумя перевести преобразования вносить преобразование относительно верхнего левого угла изображения:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    TaperSide taperSide = (TaperSide)taperSidePicker.SelectedIndex;
    TaperCorner taperCorner = (TaperCorner)taperCornerPicker.SelectedIndex;
    float taperFraction = (float)taperFractionSlider.Value;

    SKMatrix taperMatrix =
        TaperTransform.Make(new SKSize(bitmap.Width, bitmap.Height),
                            taperSide, taperCorner, taperFraction);

    // Display the matrix in the lower-right corner
    SKSize matrixSize = matrixDisplay.Measure(taperMatrix);

    matrixDisplay.Paint(canvas, taperMatrix,
        new SKPoint(info.Width - matrixSize.Width,
                    info.Height - matrixSize.Height));

    // Center bitmap on canvas
    float x = (info.Width - bitmap.Width) / 2;
    float y = (info.Height - bitmap.Height) / 2;

    SKMatrix matrix = SKMatrix.MakeTranslation(-x, -y);
    SKMatrix.PostConcat(ref matrix, taperMatrix);
    SKMatrix.PostConcat(ref matrix, SKMatrix.MakeTranslation(x, y));

    canvas.SetMatrix(matrix);
    canvas.DrawBitmap(bitmap, x, y);
}
```

Далее приводятся некоторые примеры.

[![](non-affine-images/tapertransform-small.png "Тройной снимок экрана со страницей конический преобразования")](non-affine-images/tapertransform-large.png#lightbox "тройной снимок экрана со страницей конический преобразования")

Другой тип обобщенный-аффинных преобразований — трехмерного поворота, как показано в следующей статье [вращения трехмерной](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md).

Аффинного преобразования может преобразовать прямоугольник в любой выпуклая четырехугольника осуществляется плавный. Это продемонстрировано на **Показать Non-аффинное** страницы. Это очень похоже на **Показать аффинное** страницу [матрица преобразует](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md) статьи, за исключением того, что он имеет четвертый `TouchPoint` для работы в четвертой углу растрового изображения:

[![](non-affine-images/shownonaffinematrix-small.png "Тройной снимок экрана со страницей Показать Non-аффинное")](non-affine-images/shownonaffinematrix-large.png#lightbox "тройной снимок экрана со страницей Показать Non-аффинное")

При условии, что не пытайтесь создайте внутреннюю угол один из углов точечного рисунка больше 180 градусов или две стороны пересекать друг друга, успешно вычисляется преобразование с помощью данного метода из [ `ShowNonAffineMatrixPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowNonAffineMatrixPage.xaml.cs) класса:

```csharp
static SKMatrix ComputeMatrix(SKSize size, SKPoint ptUL, SKPoint ptUR, SKPoint ptLL, SKPoint ptLR)
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

    // Non-Affine transform
    SKMatrix inverseA;
    A.TryInvert(out inverseA);
    SKPoint abPoint = inverseA.MapPoint(ptLR);
    float a = abPoint.X;
    float b = abPoint.Y;

    float scaleX = a / (a + b - 1);
    float scaleY = b / (a + b - 1);

    SKMatrix N = new SKMatrix
    {
        ScaleX = scaleX,
        ScaleY = scaleY,
        Persp0 = scaleX - 1,
        Persp1 = scaleY - 1,
        Persp2 = 1
    };

    // Multiply S * N * A
    SKMatrix result = SKMatrix.MakeIdentity();
    SKMatrix.PostConcat(ref result, S);
    SKMatrix.PostConcat(ref result, N);
    SKMatrix.PostConcat(ref result, A);

    return result;
}
```

Для упрощения расчетов этот метод получает общая transform как произведение три отдельных преобразования, в которых являются отображаемый здесь со стрелками, показывающий, как эти преобразования изменяют четыре угла растрового изображения:

(0, 0) → → (0, 0) → (0, 0) (x 0, y0) (верхнего левого)

(0, H) → → (0, 1) → (0, 1) (x1, y1) (слева)

(W, 0) → → (1, 0) → (1, 0) (x 2, y2) (верхний правый)

("W", "H") → → (a, b) → (1, 1) (x 3 y3) (правый нижний)

Окончательный координаты справа указываются четыре точки, связанные с точками четыре касания. Это окончательный координаты углов растрового изображения.

W и H представляют ширина и Высота растрового изображения. Первое (`S`) просто масштабирует точечный рисунок в 1 пиксель квадрат. Второе преобразование является преобразование неаффинных `N`, а третий — аффинного преобразования `A`. Это аффинное преобразование основана на три точки, поэтому это просто как ранее аффинного [ `ComputeMatrix` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/ShowAffineMatrixPage.xaml.cs#L68) метод и не включает четвертой строки с (a, b) точки.

`a` И `b` значения вычисляются так, чтобы аффинное преобразование третьего. Код получает аффинное преобразование, обратное, а затем использует, чтобы сопоставить нижнего правого угла. Это точка (a, b).

Другой-аффинных преобразований используется для имитации трехмерной графики. В следующей статье [вращения трехмерной](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md) описано, как поворачивать двумерной графики в трехмерном пространстве.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
