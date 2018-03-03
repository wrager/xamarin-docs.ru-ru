---
title: "3D поворотов"
description: "Используйте неаффинных преобразования для поворота 2D объектов в трехмерном пространстве."
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 04/14/2017
ms.openlocfilehash: 1341cde32778358fbeb7b65045616d5d81623d37
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="3d-rotations"></a>3D поворотов

_Используйте неаффинных преобразования для поворота 2D объектов в трехмерном пространстве._

Одно из распространенных применений-аффинных преобразований имитирует поворот 2D объекта в трехмерном пространстве:

![](3d-rotation-images/3drotationsexample.png "Текстовая строка, поворачивается в трехмерном пространстве")

Это задание включает в себя работа с трехмерных поворотов и затем получая неаффинных `SKMatrix` преобразование, которое выполняет эти 3D поворотов.

Трудно разрабатывать это `SKMatrix` преобразования работают только в пределах двух измерений. Задание становится гораздо проще, если эта матрица 3 x 3 является производным от матрицу 4 x 4, используемую в трехмерной графики. Включает SkiaSharp [ `SKMatrix44` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.PreConcat/p/SkiaSharp.SKMatrix44/) класс для этой цели, но некоторые фона в трехмерной графики необходим для понимания 3D поворотов и Матрица 4 x 4 преобразования.

Под прямым углом на экран трехмерного системы координат добавляет третье оси Z. Концептуально вызывается, оси Z. Координат точек в трехмерном пространстве обозначаются в трех цифр: (x, y, z). В трехмерного системе координат, используемой в данной статье, увеличение значения X справа и возрастающими значениями Y Перейдите вниз, как два измерения. Увеличение положительные значения Z выходить из экрана. Источником является верхнего левого угла, как и в двумерной графики. Можно считать экрана плоскости XY с осью Z под прямым углом для этой плоскости.

Это называется левой системы координат. При наведении указателя мыши указательным для левой руки в направлении положительного X координаты (справа) и палец среднего по оси Y возрастающей координаты (вниз), затем точек thumb в направлении возрастания координаты Z & #x 2014; выходящий за пределы экрана.

В трехмерной графики преобразования основаны на матрицу 4 x 4. Вот единичной матрицы 4 x 4.

<pre>
|  1  0  0  0  |
|  0  1  0  0  |
|  0  0  1  0  |
|  0  0  0  1  |
</pre>

При работе с матрицу 4 x 4, удобный для идентификации ячейки с номерами строк и столбцов:

<pre>
|  M11  M12  M13  M14  |
|  M21  M22  M23  M24  |
|  M31  M32  M33  M34  |
|  M41  M42  M43  M44  |
</pre>

Тем не менее SkiaSharp `Matrix44` класс будет немного отличаться. Единственный способ задать или получить значения отдельных ячеек `SKMatrix44` с помощью [ `Item` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Item/p/System.Int32/System.Int32/) индексатора. Индексы строки и столбца, отсчитываемый от нуля, а не от единицы, а строки и столбцы меняются местами. На схеме выше ячейки M14 осуществляется с помощью индексатора `[3, 0]` в `SKMatrix44` объекта.

В системе с трехмерной графикой трехмерной точки (x, y, z) преобразуется в матрицу 1 по 4 для умножается матрица преобразования 4 x 4:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  z  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Аналогом 2D преобразований, которые происходят в трех измерений, трехмерных преобразований предполагается выполнять в четырех измерений. В четвертом измерении называется W и трехмерном пространстве предполагается, что существует в пространстве 4D, где координаты W, равно 1. Преобразование формул выглядят следующим образом:

x' = M11·x + M21·y + M31·z + M41

y' = M12·x + M22·y + M32·z + M42

z' = M13·x + M23·y + M33·z + M43

w' = M14·x + M24·y + M34·z + M44

Очевидно из формул преобразования, ячейки `M11`, `M22`, `M33` коэффициенты масштабирования в направлениях X, Y и Z и `M41`, `M42`, и `M43` факторов перевода в X, Y и Z инструкции.

Чтобы преобразовать эти координаты в трехмерном пространстве, где W равно 1, x', y ", и z «координаты все разделены w»:

x» = x "/ w"

y" = y' / w'

z» = z "/ w"

w" = w' / w' = 1

Деление на w "предоставляет перспективы в трехмерном пространстве. Если w "значение 1, то Перспектива не происходит.

Вращения в трехмерном пространстве может быть весьма сложным, но простым поворотов распространяются вокруг оси X, Y и Z. Поворот угла α вокруг оси X является этой матрицы:

<pre>
|  1     0       0     0  |
|  0   cos(α)  sin(α)  0  |
|  0  –sin(α)  cos(α)  0  |
|  0     0       0     1  |
</pre>

Значения X не изменяются при для этого преобразования. Поворота вокруг оси Y оставляет значений Y без изменений:

<pre>
|  cos(α)  0  –sin(α)  0  |
|    0     1     0     0  |
|  sin(α)  0   cos(α)  0  |
|    0     0     0     1  |
</pre>

Поворота вокруг оси Z является такой же, как двумерной графики:

<pre>
|  cos(α)  sin(α)  0  0  |
| –sin(α)  cos(α)  0  0  |
|    0       0     1  0  |
|    0       0     0  1  |
</pre>

Направление поворота подразумевается направление прохождения регистрации системы координат. Это левую систему, поэтому при наведении указателя мыши бегунка левой руки сторону возрастающими значениями для конкретной осью & #x 2014; справа от поворота вокруг оси X down для поворота вокруг оси Y, а также к себе для поворота вокруг оси Z & #x 2014; затем кривой других пальцы указывает направление положительные значения угла поворота.

`SKMatrix44` должен быть подготовлен статический [ `CreateRotation` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotation/p/System.Single/System.Single/System.Single/System.Single/) и [ `CreateRotationDegrees` ](https://developer.xamarin.com/api/member/SkiaSharp.SKMatrix44.CreateRotationDegrees/p/System.Single/System.Single/System.Single/System.Single/) методы, которые позволяют указать ось, вокруг которой происходит поворот:

```csharp
public static SKMatrix44 CreateRotationDegrees (Single x, Single y, Single z, Single degrees)
```

Значение для поворота вокруг оси X, первые три аргумента 1, 0, 0. Для поворота вокруг оси Y значение 0, 1, 0, а для поворота вокруг оси Z, задать для них значение 0, 0, 1.

Четвертый столбец 4 x 4 — для перспективы. `SKMatrix44` Не имеет методов для создания перспективы преобразования, но можно создать один самостоятельно, используя следующий код:

```csharp
SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
perspectiveMatrix[3, 2] = -1 / depth;
```

Причина имя аргумента `depth` будет заметно чуть ниже. Этот код создает матрицы.

<pre>
|  1  0  0      0     |
|  0  1  0      0     |
|  0  0  1  -1/depth  |
|  0  0  0      1     |
</pre>

Преобразование формул привести следующие вычисления w ":

w "= – z или глубины + 1

Это служит для уменьшения координаты X и Y, если значения Z меньше нуля (концептуально за плоскостью) и повысить координаты X и Y для положительных значений Z. Если значение координаты Z равно `depth`, затем w "равно нулю, а координаты становятся бесконечной. Систем трехмерной графики строятся вокруг метафору камеры и `depth` здесь значение представляет расстояние от камеры из исходной системы координат. Если графический объект имеет Z координации, то есть `depth` единицы от начала координат, по существу касается объектив камеры и становится бесконечном размере.

Следует помнить, что, вероятно, используется это `perspectiveMatrix` значение в сочетании с матрицы поворота. Если графический объект, на который будет сменен имеет координаты X и Y, больше, чем `depth`, то поворота этого объекта в трехмерном пространстве вовлекает больше координаты Z `depth`. Это необходимо избегать! При создании `perspectiveMatrix` нужно задать `depth` значение достаточно большим для всех координат объекта graphics независимо от того, как ее поворот. Это гарантирует, что не все деление на ноль.

Объединение 3D поворотов и перспективы требуется перемножения матрицы 4 x 4. Для этой цели `SKMatrix44` определяет методы для объединения. Если `A` и `B` , `SKMatrix44` объекты, то следующий код задает равенства × б.

```csharp
A.PostConcat(B);
```

При матрицу 4 x 4 преобразования используется в системе двумерной графики, оно применяется к объектам 2D. Эти объекты являются плоскими и предполагается, что имеют координаты Z равно нулю. Умножение преобразование немного проще, чем преобразование, показанного выше:

<pre>
                 |  M11  M12  M13  M14  |
| x  y  0  1 | × |  M21  M22  M23  M24  | = | x'  y'  z'  w' |
                 |  M31  M32  M33  M34  |
                 |  M41  M42  M43  M44  |
</pre>

Это значение 0 для результатов z в формулах преобразования, которые не включают в себя все ячейки в третьей строке матрицы:

x' = M11·x + M21·y + M41

y "= M12·x + M22·y + M42

z' = M13·x + M23·y + M43

w "= M14·x + M24·y + M44

Кроме того z "координат не имеет значения, как здесь. Когда трехмерный объект отображается в системе двумерной графики, он свернут двухмерный объект, игнорируя значения координаты по оси Z. Формулы преобразования — это просто эти два:

x» = x "/ w"

y" = y' / w'

Это означает, что третья строка *и* третьего столбца матрицы 4 x 4 можно игнорировать.

Но, так ли это, почему в первую очередь является даже необходимые матрицы 4 x 4?

Несмотря на то, что третьей строки и третьего столбца 4 x 4 неприменимы для двумерных преобразований, третьей строки и столбца *сделать* играют роль до что при различных `SKMatrix44` умножением значения. Например предположим, умножается поворота вокруг оси Y с точки зрения преобразования:

<pre>
|  cos(α)  0  –sin(α)  0  |   |  1  0  0      0     |   |  cos(α)  0  –sin(α)   sin(α)/depth  |
|    0     1     0     0  | × |  0  1  0      0     | = |    0     1     0           0        |
|  sin(α)  0   cos(α)  0  |   |  0  0  1  -1/depth  |   |  sin(α)  0   cos(α)  -cos(α)/depth  |  
|    0     0     0     1  |   |  0  0  0      1     |   |    0     0     0           1        |
</pre>

В продукте, ячейки `M14` теперь содержит значение перспективы. Если вы хотите применить данную матрицу плоские объекты, преобразовать в матрицу 3 x 3 исключаются третьей строки и столбца:

<pre>
|  cos(α)  0  sin(α)/depth  |
|    0     1       0        |
|    0     0       1        |
</pre>

Теперь его можно использовать для преобразования 2D точки.

<pre>
                |  cos(α)  0  sin(α)/depth  |
|  x  y  1  | × |    0     1       0        | = |  x'  y'  z'  |
                |    0     0       1        |
</pre>

Преобразование формул являются:

x' = cos ·x (α)

y "= y

z' = (sin (α) или глубину) ·x + 1

Теперь разделить все z ":

x» = cos ·x (α) / ((sin (α) или глубину) ·x + 1)

y» = y / ((sin (α) или глубину) ·x + 1)

При плоские объекты поворачиваются с положительный угол вокруг оси Y, а затем положительные значения X направлены в фоновом режиме при отрицательное берутся значения X отображается на переднем плане. Значения X показаться приблизиться к оси Y (определяется косинус значение) как координаты дальше всех от оси Y становится меньшего или большего размера, при перемещении от средство просмотра или ближе к пользователю.

При использовании `SKMatrix44`, выполнять все операции перспективы и трехмерного поворота путем умножения различных `SKMatrix44` значений. Затем можно извлечь Двухмерная матрица 3 x 3 из 4 x 4 с помощью матрицы [ `Matrix` ](https://developer.xamarin.com/api/property/SkiaSharp.SKMatrix44.Matrix/) свойство `SKMatrix44` класса. Это свойство возвращает привычной `SKMatrix` значение.

**Объемные эффекты поворота** страницы позволяет экспериментировать с трехмерного поворота. [ **Rotation3DPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml) файл создает четыре ползунка значение поворота вокруг оси X, Y и Z и задать значение глубины:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Transforms.Rotation3DPage"
             Title="Rotation 3D">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
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
                    <Setter Property="Margin" Value="20, 0" />
                    <Setter Property="Maximum" Value="360" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Slider x:Name="xRotateSlider"
                Grid.Row="0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference xRotateSlider},
                              Path=Value,
                              StringFormat='X-Axis Rotation = {0:F0}'}"
               Grid.Row="1" />

        <Slider x:Name="yRotateSlider"
                Grid.Row="2"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference yRotateSlider},
                              Path=Value,
                              StringFormat='Y-Axis Rotation = {0:F0}'}"
               Grid.Row="3" />

        <Slider x:Name="zRotateSlider"
                Grid.Row="4"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zRotateSlider},
                              Path=Value,
                              StringFormat='Z-Axis Rotation = {0:F0}'}"
               Grid.Row="5" />

        <Slider x:Name="depthSlider"
                Grid.Row="6"
                Maximum="2500"
                Minimum="250"
                ValueChanged="OnSliderValueChanged" />

        <Label Grid.Row="7"
               Text="{Binding Source={x:Reference depthSlider},
                              Path=Value,
                              StringFormat='Depth = {0:F0}'}" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="8"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Обратите внимание, что `depthSlider` инициализируется с `Minimum` значение 250. Это означает, что 2D объект поворачиваемых здесь имеет координаты X и Y, ограничена круга определенного radius 250 пикселей вокруг начала координат. Значения координат меньше, чем 250 всегда приведет к любой поворота этого объекта в трехмерном пространстве.

[ **Rotation3DPage.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/Rotation3DPage.xaml.cs) загружает файл кода программной части в точечном рисунке квадратный 300 пикселей:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }
    ...
}
```

Если 3D-преобразование центрируется на это растровое изображение, затем координаты X и Y диапазон от –150 до 150, пока углов являются 212 пикселей в центре, поэтому все, что находится в пределах радиуса 250 пикселей.

`PaintSurface` Создает обработчик `SKMatrix44` объекты на основании ползунки и умножает их друг с другом с помощью `PostConcat`. `SKMatrix` Значение, извлеченное из последней `SKMatrix44` заключается в объект путем перевода преобразований по центру поворота в центре экрана:

```csharp
public partial class Rotation3DPage : ContentPage
{
    SKBitmap bitmap;

    public Rotation3DPage()
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

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (canvasView != null)
        {
            canvasView.InvalidateSurface();
        }
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Use 3D matrix for 3D rotations and perspective
        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, (float)xRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, (float)yRotateSlider.Value));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, (float)zRotateSlider.Value));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / (float)depthSlider.Value;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the bitmap
        canvas.SetMatrix(matrix);
        float xBitmap = xCenter - bitmap.Width / 2;
        float yBitmap = yCenter - bitmap.Height / 2;
        canvas.DrawBitmap(bitmap, xBitmap, yBitmap);
    }
}
```

Экспериментируя с четвертого ползунок, вы увидите глубина различные параметры не переместить объект дальнейшей от него, что вместо этого alter степень влияния перспективы:

[![](3d-rotation-images/rotation3d-small.png "Тройной снимок экрана со страницей трехмерного поворота")](3d-rotation-images/rotation3d-large.png "тройной снимок экрана со страницей трехмерного поворота")

**Анимация поворота 3D** также использует `SKMatrix44` для анимации текстовую строку в трехмерном пространстве. `textPaint` Набор объектов, так как поле используется в конструкторе для определения границ текста:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    SKCanvasView canvasView;
    float xRotationDegrees, yRotationDegrees, zRotationDegrees;
    string text = "SkiaSharp";
    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        TextSize = 100,
        StrokeWidth = 3,
    };
    SKRect textBounds;

    public AnimatedRotation3DPage()
    {
        Title = "Animated Rotation 3D";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Measure the text
        textPaint.MeasureText(text, ref textBounds);
    }
    ...
}
```

`OnAppearing` Переопределение определяет три Xamarin.Forms `Animation` объектов анимации `xRotationDegrees`, `yRotationDegrees`, и `zRotationDegrees` поля с разной интенсивностью. Обратите внимание, что периоды эти анимации присвоено простых чисел & #x 2014; 5 секунд, 7 секунд и 11 секунд & #x 2014; Поэтому общий сочетания только повторяется каждые 385 секунд или более 10 минут:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();

        new Animation((value) => xRotationDegrees = 360 * (float)value).
            Commit(this, "xRotationAnimation", length: 5000, repeat: () => true);

        new Animation((value) => yRotationDegrees = 360 * (float)value).
            Commit(this, "yRotationAnimation", length: 7000, repeat: () => true);

        new Animation((value) =>
        {
            zRotationDegrees = 360 * (float)value;
            canvasView.InvalidateSurface();
        }).Commit(this, "zRotationAnimation", length: 11000, repeat: () => true);
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        this.AbortAnimation("xRotationAnimation");
        this.AbortAnimation("yRotationAnimation");
        this.AbortAnimation("zRotationAnimation");
    }
    ...
}
```

Как в предыдущей программе `PaintCanvas` создает обработчик `SKMatrix44` значений для смены и перспективы и их Перемножает:

```csharp
public class AnimatedRotation3DPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Find center of canvas
        float xCenter = info.Width / 2;
        float yCenter = info.Height / 2;

        // Translate center to origin
        SKMatrix matrix = SKMatrix.MakeTranslation(-xCenter, -yCenter);

        // Scale so text fits
        float scale = Math.Min(info.Width / textBounds.Width,
                               info.Height / textBounds.Height);
        SKMatrix.PostConcat(ref matrix, SKMatrix.MakeScale(scale, scale));

        // Calculate composite 3D transforms
        float depth = 0.75f * scale * textBounds.Width;

        SKMatrix44 matrix44 = SKMatrix44.CreateIdentity();
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(1, 0, 0, xRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 1, 0, yRotationDegrees));
        matrix44.PostConcat(SKMatrix44.CreateRotationDegrees(0, 0, 1, zRotationDegrees));

        SKMatrix44 perspectiveMatrix = SKMatrix44.CreateIdentity();
        perspectiveMatrix[3, 2] = -1 / depth;
        matrix44.PostConcat(perspectiveMatrix);

        // Concatenate with 2D matrix
        SKMatrix.PostConcat(ref matrix, matrix44.Matrix);

        // Translate back to center
        SKMatrix.PostConcat(ref matrix,
            SKMatrix.MakeTranslation(xCenter, yCenter));

        // Set the matrix and display the text
        canvas.SetMatrix(matrix);
        float xText = xCenter - textBounds.MidX;
        float yText = yCenter - textBounds.MidY;
        canvas.DrawText(text, xText, yText, textPaint);
    }
}
```

Это трехмерного поворота окружено нескольких 2D преобразований для перемещения к центру экрана центра вращения и для изменения размера текста строки так, чтобы он ту же ширину, что экрана:

[![](3d-rotation-images/animatedrotation3d-small.png "Тройной снимок экрана со страницей 3D анимация поворота")](3d-rotation-images/animatedrotation3d-large.png "тройной снимок экрана со страницей анимация поворота 3D")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
