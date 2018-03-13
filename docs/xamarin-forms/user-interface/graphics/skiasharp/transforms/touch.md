---
title: "Манипуляции сенсорного ввода"
description: "Преобразует матрица используется для реализации перетаскивания сенсорный ввод, сжатия и поворота"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: A0B8DD2D-7392-4EC5-BFB0-6209407AD650
author: charlespetzold
ms.author: chape
ms.date: 04/12/2017
ms.openlocfilehash: 16e9423c84e591e15a703b4d5bb204a8b642bb40
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="touch-manipulations"></a>Манипуляции сенсорного ввода

_Преобразует матрица используется для реализации перетаскивания сенсорный ввод, сжатия и поворота_

В средах мультисенсорные, таких как браузеры для мобильных устройств пользователи часто используют пальцами для работы с объектами на экране. Общие жестов, например перетащите один пальцем и сжатие два пальца можно переместить и масштабирование объектов или даже поворота. Обычно эти жесты реализуются с помощью матрицы преобразования, и в этой статье показано, как это сделать.

![](touch-images/touchmanipulationsexample.png "Битовая карта для преобразования, масштабирования и поворота")

## <a name="manipulating-one-bitmap"></a>Обработка одного точечного рисунка

**Touch манипуляции** странице демонстрируется манипуляций touch отдельной.
В этом примере используется эффект отслеживания сенсорный ввод, представленные в статье [вызов события из эффекты](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

Другие файлы обеспечивают поддержку **Touch манипуляции** страницы. Во-первых, [ `TouchManipulationMode` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationMode.cs) перечисления, который показывает различные типы обработки touch реализации с помощью кода, читатели смогут увидеть:

```csharp
enum TouchManipulationMode
{
    None,
    PanOnly,
    IsotropicScale,     // includes panning
    AnisotropicScale,   // includes panning
    ScaleRotate,        // implies isotropic scaling
    ScaleDualRotate     // adds one-finger rotation
}
```

`PanOnly` Представляет один пальцем перетаскивания, реализованный с преобразованием. Все последующие параметры также включают панорамирования, но включают двумя пальцами: `IsotropicScale` жестом сжатия операция, которая приводит объект одинаково масштабирование в горизонтальном и вертикальном направлениях. `AnisotropicScale` позволяет равны.

`ScaleRotate` Параметр предназначен для двух пальцев масштабирования и поворота. Масштабирование мощности. Реализация двух пальцев поворота анизотропная масштабирования, затруднительно, поскольку пальцем перемещений по существу одинаковы.

`ScaleDualRotate` Параметр добавляет один пальцем поворота. Когда одним пальцем перетаскивает объект, перетаскиваемый объект сначала вращать вокруг его центра, чтобы центр объекта соответствовал перетаскивания вектор.

[ **TouchManipulationPage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml) файл, содержащий `Picker` с элементами `TouchManipulationMode` перечисления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Transforms.TouchManipulationPage"
             Title="Touch Manipulation">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker Title="Touch Mode"
                Grid.Row="0"
                SelectedIndexChanged="OnTouchModePickerSelectedIndexChanged">
            <Picker.Items>
                <x:String>None</x:String>
                <x:String>PanOnly</x:String>
                <x:String>IsotropicScale</x:String>
                <x:String>AnisotropicScale</x:String>
                <x:String>ScaleRotate</x:String>
                <x:String>ScaleDualRotate</x:String>
            </Picker.Items>
            <Picker.SelectedIndex>
                4
            </Picker.SelectedIndex>
        </Picker>

        <Grid BackgroundColor="White"
              Grid.Row="1">

            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>
    </Grid>
</ContentPage>
```

В нижней — `SKCanvasView` и `TouchEffect` подключенные к одной ячейке `Grid` , в котором он.

[ **TouchManipulationPage.xaml.cs** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationPage.xaml.cs) файл кода содержит `bitmap` поле, но он не относится к типу `SKBitmap`. Тип — `TouchManipulationBitmap` (класс, вскоре вы увидите):

```csharp
public partial class TouchManipulationPage : ContentPage
{
    TouchManipulationBitmap bitmap;
    ...

    public TouchManipulationPage()
    {
        InitializeComponent();

        string resourceID = "SkiaSharpFormsDemos.Media.MountainClimbers.jpg";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            SKBitmap bitmap = SKBitmap.Decode(skStream);
            this.bitmap = new TouchManipulationBitmap(bitmap);
            this.bitmap.TouchManager.Mode = TouchManipulationMode.ScaleRotate;
        }
    }
    ...
}
```

Создает экземпляр конструктора `TouchManipulationBitmap` объект, передаваемый в конструктор `SKBitmap` получен из внедренного ресурса. Конструктор завершается, задав `Mode` свойство `TouchManager` свойство `TouchManipulationBitmap` , который является членом `TouchManipulationMode` перечисления.

`SelectedIndexChanged` Обработчик `Picker` также устанавливает это `Mode` свойство:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    void OnTouchModePickerSelectedIndexChanged(object sender, EventArgs args)
    {
        if (bitmap != null)
        {
            Picker picker = (Picker)sender;
            TouchManipulationMode mode;
            Enum.TryParse(picker.Items[picker.SelectedIndex], out mode);
            bitmap.TouchManager.Mode = mode;
        }
    }
    ...
}
```

`TouchAction` Обработчик `TouchEffect` создавать экземпляры в файл XAML не вызывает два метода в `TouchManipulationBitmap` с именем `HitTest` и `ProcessTouchEvent`:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    List<long> touchIds = new List<long>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (bitmap.HitTest(point))
                {
                    touchIds.Add(args.Id);
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    break;
                }
                break;

            case TouchActionType.Moved:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (touchIds.Contains(args.Id))
                {
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    touchIds.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Если `HitTest` возвращает `true` & #x 2014; значение, что палец коснулось экрана в область, занимаемая точечный рисунок & #x 2014; затем touch ID добавлена `TouchIds` коллекции. Этот идентификатор представляет последовательность событий сенсорного ввода для этого пальцем пока отрывает палец с экрана. Если несколько пальцами touch растрового изображения, то `touchIds` коллекции содержит идентификатор сенсорного ввода для каждого пальца.

`TouchAction` Также вызывает обработчика `ProcessTouchEvent` в класс `TouchManipulationBitmap`. Это место, куда некоторых (но не всех) из реальных touch происходит обработка.

[ `TouchManipulationBitmap` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationBitmap.cs) Класса является класс-оболочку для `SKBitmap` , содержащий код для визуализации растрового изображения и обработки событий сенсорного экрана. Оно работает в сочетании с использованием более обобщенный код в `TouchManipulationManager` (в котором вы увидите в ближайшее время).

`TouchManipulationBitmap` Конструктор сохраняет `SKBitmap` и создает два свойства `TouchManager` свойство типа `TouchManipulationManager` и `Matrix` свойство типа `SKMatrix`:

```csharp
class TouchManipulationBitmap
{
    SKBitmap bitmap;
    ...

    public TouchManipulationBitmap(SKBitmap bitmap)
    {
        this.bitmap = bitmap;
        Matrix = SKMatrix.MakeIdentity();

        TouchManager = new TouchManipulationManager
        {
            Mode = TouchManipulationMode.ScaleRotate
        };
    }

    public TouchManipulationManager TouchManager { set; get; }

    public SKMatrix Matrix { set; get; }
    ...
}
```

Это `Matrix` свойство является накопленный преобразования, возникающие в результате все действия касания. Как вы увидите каждого события касания разрешается в матрицу, которое затем объединяется с `SKMatrix` значение, хранящееся в `Matrix` свойство.

`TouchManipulationBitmap` Объекта рисовании его `Paint` метод. Аргумент является `SKCanvas` объекта. Это `SKCanvas` уже может быть преобразование, применяемое к его, поэтому `Paint` метод Сцепляет `Matrix` свойства связанные с растровым изображением, для преобразования существующих и восстанавливает полотно после завершения:

```csharp
class TouchManipulationBitmap
{
    ...
    public void Paint(SKCanvas canvas)
    {
        canvas.Save();
        SKMatrix matrix = Matrix;
        canvas.Concat(ref matrix);
        canvas.DrawBitmap(bitmap, 0, 0);
        canvas.Restore();
    }
    ...
}
```

`HitTest` Возвращает `true` , если пользователь касается экрана с определенной позиции в пределах границ растрового изображения. Как пользователь управляет растрового изображения, растрового изображения могут быть повернуты или даже (с помощью комбинации анизотропная масштабирования и поворота) быть фигуры параллелограмм. Может пугайтесь, `HitTest` метод необходимо реализовать довольно сложной аналитическую геометрию в этом случае.

Однако доступна ярлыка:

Если точка находится в пределах преобразованный прямоугольник является определение таким же, как определить, был ли точка обратный преобразованный находится в пределах границы прямоугольника, подвергнутый. Это гораздо проще вычисления, и можно использовать удобный `Contains` метода, определенного `SKRect`:

```csharp
class TouchManipulationBitmap
{
    ...
    public bool HitTest(SKPoint location)
    {
        // Invert the matrix
        SKMatrix inverseMatrix;

        if (Matrix.TryInvert(out inverseMatrix))
        {
            // Transform the point using the inverted matrix
            SKPoint transformedPoint = inverseMatrix.MapPoint(location);

            // Check if it's in the untransformed bitmap rectangle
            SKRect rect = new SKRect(0, 0, bitmap.Width, bitmap.Height);
            return rect.Contains(transformedPoint);
        }
        return false;
    }
    ...
}
```

Второй открытый метод в `TouchManipulationBitmap` — `ProcessTouchEvent`. При вызове этого метода, он уже установлено, события касания относится к конкретной растрового изображения. Метод поддерживает словарь [ `TouchManipulationInfo` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/TouchManipulationInfo.cs) объектов, которые просто предыдущей точки и новая точка каждого пальца:

```csharp
class TouchManipulationInfo
{
    public SKPoint PreviousPoint { set; get; }

    public SKPoint NewPoint { set; get; }
}
```

Здесь представлен словарь и `ProcessTouchEvent` сам метод:

```csharp
class TouchManipulationBitmap
{
    ...
    Dictionary<long, TouchManipulationInfo> touchDictionary =
        new Dictionary<long, TouchManipulationInfo>();
    ...
    public void ProcessTouchEvent(long id, TouchActionType type, SKPoint location)
    {
        switch (type)
        {
            case TouchActionType.Pressed:
                touchDictionary.Add(id, new TouchManipulationInfo
                {
                    PreviousPoint = location,
                    NewPoint = location
                });
                break;

            case TouchActionType.Moved:
                TouchManipulationInfo info = touchDictionary[id];
                info.NewPoint = location;
                Manipulate();
                info.PreviousPoint = info.NewPoint;
                break;

            case TouchActionType.Released:
                touchDictionary[id].NewPoint = location;
                Manipulate();
                touchDictionary.Remove(id);
                break;

            case TouchActionType.Cancelled:
                touchDictionary.Remove(id);
                break;
        }
    }
    ...
}
```

В `Moved` и `Released` событий, вызывается метод `Manipulate`. В эти моменты времени `touchDictionary` содержит один или несколько `TouchManipulationInfo` объектов. Если `touchDictionary` содержит один элемент, вероятнее всего, `PreviousPoint` и `NewPoint` значения не равны и представляют перемещение пальцем. Если несколько пальца касаются растрового изображения, словарь содержит более одного элемента, но только один из этих элементов имеет другие `PreviousPoint` и `NewPoint` значения. Все остальные имеют одинаковые `PreviousPoint` и `NewPoint` значения.

Это важно: `Manipulate` метода можно предположить, что он обрабатывает перемещение только одним пальцем. Во время данного вызова ни один из других пальцы переходят, и если они действительно перемещается (как, вероятнее всего), эти перемещений будут обрабатываться в будущих вызовах `Manipulate`.

`Manipulate` Сначала копирует словаря в массив для удобства. Пропускает ничего, кроме первых двух записей. Если более чем двумя пальцами пытаетесь манипулирования этим растровым изображением, остальные пропускаются. `Manipulate` Окончательный членом `TouchManipulationBitmap`:

```csharp
class TouchManipulationBitmap
{
    ...
    void Manipulate()
    {
        TouchManipulationInfo[] infos = new TouchManipulationInfo[touchDictionary.Count];
        touchDictionary.Values.CopyTo(infos, 0);
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();

        if (infos.Length == 1)
        {
            SKPoint prevPoint = infos[0].PreviousPoint;
            SKPoint newPoint = infos[0].NewPoint;
            SKPoint pivotPoint = Matrix.MapPoint(bitmap.Width / 2, bitmap.Height / 2);

            touchMatrix = TouchManager.OneFingerManipulate(prevPoint, newPoint, pivotPoint);
        }
        else if (infos.Length >= 2)
        {
            int pivotIndex = infos[0].NewPoint == infos[0].PreviousPoint ? 0 : 1;
            SKPoint pivotPoint = infos[pivotIndex].NewPoint;
            SKPoint newPoint = infos[1 - pivotIndex].NewPoint;
            SKPoint prevPoint = infos[1 - pivotIndex].PreviousPoint;

            touchMatrix = TouchManager.TwoFingerManipulate(prevPoint, newPoint, pivotPoint);
        }

        SKMatrix matrix = Matrix;
        SKMatrix.PostConcat(ref matrix, touchMatrix);
        Matrix = matrix;
    }
}
```

Если одним пальцем с этим растровым изображением, `Manipulate` вызовы `OneFingerManipulate` метод `TouchManipulationManager` объекта. Для двух пальцев вызывает `TwoFingerManipulate`. Аргументы для этих методов совпадают: `prevPoint` и `newPoint` аргументы представляют пальцем, которые перемещаются. Но `pivotPoint` аргумент отличается для двух вызовов:

Для обработки одного пальца `pivotPoint` центра растрового изображения. Это позволяет обеспечить для одного пальца поворота. Для обработки двух пальцев данное событие обозначает, перемещение только одним пальцем, чтобы `pivotPoint` является пальцем, который не перемещается.

В обоих случаях `TouchManipulationManager` возвращает `SKMatrix` значение, которое объединяет метод с текущим `Matrix` свойство, `TouchManipulationPage` используется для подготовки к просмотру растрового изображения.

`TouchManipulationManager` обобщенный и использует никаких других файлов, за исключением `TouchManipulationMode`. Можно будет использовать этот класс без изменений в пользовательских приложениях. Он определяет одно свойство типа `TouchManipulationMode`:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }
    ...
}
```


Тем не менее, вам возможно потребуется избежать `AnisotropicScale` параметр. Это очень простым с этим параметром для манипулирования этим растровым изображением, чтобы один коэффициенты масштабирования становится равным нулю. Это делает исчезают, никогда не получить растрового изображения. Если действительно требуется анизотропная масштабирование, имеет смысл усовершенствовать логику, чтобы избежать нежелательных результатов.

`TouchManipulationManager` использует векторов, но так как она не `SKVector` структуры в SkiaSharp, `SKPoint` вместо него будет использоваться. `SKPoint` поддерживает оператор вычитания и результат могут рассматриваться как вектор. Только вектор определенного логика, необходимая для добавления, `Magnitude` вычисления:

```csharp
class TouchManipulationManager
{
    ...
    float Magnitude(SKPoint point)
    {
        return (float)Math.Sqrt(Math.Pow(point.X, 2) + Math.Pow(point.Y, 2));
    }
}
```

Каждый раз, когда был выбран поворота, методы пальцем одной и двух пальцев обработку сначала обработайте поворот. При обнаружении любого поворота координату поворота фактически удаляется. Что остается интерпретируется как панорамирования и масштабирования.

Вот `OneFingerManipulate` метод. Если один пальцем поворот не включен, логика является простой & #x 2014; он просто использует предыдущей точки и новой точки для создания вектор с именем `delta` , соответствующий точно перевода. Один пальцем поворота включен метод использует углы от точки вращения (центр растрового изображения) к предыдущей точке и новой точки для создания матрицы поворота:

```csharp
class TouchManipulationManager
{
    public TouchManipulationMode Mode { set; get; }

    public SKMatrix OneFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        if (Mode == TouchManipulationMode.None)
        {
            return SKMatrix.MakeIdentity();
        }

        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint delta = newPoint - prevPoint;

        if (Mode == TouchManipulationMode.ScaleDualRotate)  // One-finger rotation
        {
            SKPoint oldVector = prevPoint - pivotPoint;
            SKPoint newVector = newPoint - pivotPoint;

            // Avoid rotation if fingers are too close to center
            if (Magnitude(newVector) > 25 && Magnitude(oldVector) > 25)
            {
                float prevAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
                float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

                // Calculate rotation matrix
                float angle = newAngle - prevAngle;
                touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

                // Effectively rotate the old vector
                float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
                oldVector.X = magnitudeRatio * newVector.X;
                oldVector.Y = magnitudeRatio * newVector.Y;

                // Recalculate delta
                delta = newVector - oldVector;
            }
        }

        // Multiply the rotation matrix by a translation matrix
        SKMatrix.PostConcat(ref touchMatrix, SKMatrix.MakeTranslation(delta.X, delta.Y));

        return touchMatrix;
    }
    ...
}
```

В `TwoFingerManipulate` , точки вращения является позицию пальцем, не входящего в этом событии определенного сенсорный ввод. Поворот очень похож на один пальцем поворота и вектор с именем `oldVector` (с учетом к предыдущей точке) настраивается для поворота. Оставшиеся перемещения интерпретируется как масштабирование:

```csharp
class TouchManipulationManager
{
    ...
    public SKMatrix TwoFingerManipulate(SKPoint prevPoint, SKPoint newPoint, SKPoint pivotPoint)
    {
        SKMatrix touchMatrix = SKMatrix.MakeIdentity();
        SKPoint oldVector = prevPoint - pivotPoint;
        SKPoint newVector = newPoint - pivotPoint;

        if (Mode == TouchManipulationMode.ScaleRotate ||
            Mode == TouchManipulationMode.ScaleDualRotate)
        {
            // Find angles from pivot point to touch points
            float oldAngle = (float)Math.Atan2(oldVector.Y, oldVector.X);
            float newAngle = (float)Math.Atan2(newVector.Y, newVector.X);

            // Calculate rotation matrix
            float angle = newAngle - oldAngle;
            touchMatrix = SKMatrix.MakeRotation(angle, pivotPoint.X, pivotPoint.Y);

            // Effectively rotate the old vector
            float magnitudeRatio = Magnitude(oldVector) / Magnitude(newVector);
            oldVector.X = magnitudeRatio * newVector.X;
            oldVector.Y = magnitudeRatio * newVector.Y;
        }

        float scaleX = 1;
        float scaleY = 1;

        if (Mode == TouchManipulationMode.AnisotropicScale)
        {
            scaleX = newVector.X / oldVector.X;
            scaleY = newVector.Y / oldVector.Y;

        }
        else if (Mode == TouchManipulationMode.IsotropicScale ||
                 Mode == TouchManipulationMode.ScaleRotate ||
                 Mode == TouchManipulationMode.ScaleDualRotate)
        {
            scaleX = scaleY = Magnitude(newVector) / Magnitude(oldVector);
        }

        if (!float.IsNaN(scaleX) && !float.IsInfinity(scaleX) &&
            !float.IsNaN(scaleY) && !float.IsInfinity(scaleY))
        {
            SKMatrix.PostConcat(ref touchMatrix,
                SKMatrix.MakeScale(scaleX, scaleY, pivotPoint.X, pivotPoint.Y));
        }

        return touchMatrix;
    }
    ...
}
```

Вы заметите, что имеется без явного преобразования в этом методе. Тем не менее, оба `MakeRotation` и `MakeScale` методы зависят от точки вращения, и включающий неявного преобразования. При использовании двух пальцев на точечный рисунок и перетаскивания их в том же направлении `TouchManipulation` получат последовательность событий сенсорного ввода, переключаясь между двумя пальцами. Перемещении относительно других, масштабирование или поворот результатов, как для каждого пальца, однако сделал перемещения других пальцем и результат преобразования.

Только оставшаяся часть **Touch манипуляции** страница `PaintSurface` обработчик в `TouchManipulationPage` файл кода программной части. Это вызывает `Paint` метод `TouchManipulationBitmap`, применяемое матрицы представляет действие, накопленный сенсорный ввод:

```csharp
public partial class TouchManipulationPage : ContentPage
{
    ...
    MatrixDisplay matrixDisplay = new MatrixDisplay();
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Display the bitmap
        bitmap.Paint(canvas);

        // Display the matrix in the lower-right corner
        SKSize matrixSize = matrixDisplay.Measure(bitmap.Matrix);

        matrixDisplay.Paint(canvas, bitmap.Matrix,
            new SKPoint(info.Width - matrixSize.Width,
                        info.Height - matrixSize.Height));
    }
}
```

`PaintSurface` Завершении работы обработчика, отображая `MatrixDisplay` объекта отображаются в матрице накопленный сенсорный ввод:

[![](touch-images/touchmanipulation-small.png "Тройной снимок экрана со страницей Touch манипуляции")](touch-images/touchmanipulation-large.png#lightbox "тройной снимок экрана со страницей Touch манипуляции")

## <a name="manipulating-multiple-bitmaps"></a>Обработка нескольких растровые изображения

Одно из преимуществ изоляции touch обработки в коде класса, такие как `TouchManipulationBitmap` и `TouchManipulationManager` является возможность повторного использования этих классов в программе, которая позволяет пользователю управлять несколько растровых изображений.

**Точечный рисунок Точечная диаграмма представления** страница показано, как это сделать. Вместо того чтобы определять поле типа `TouchManipulationBitmap`, [ `BitmapScatterPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Transforms/BitmapScatterViewPage.xaml.cs) класс определяет `List` объектов растрового изображения:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    List<TouchManipulationBitmap> bitmapCollection =
        new List<TouchManipulationBitmap>();
    ...
    public BitmapScatterViewPage()
    {
        InitializeComponent();

        // Load in all the available bitmaps
        Assembly assembly = GetType().GetTypeInfo().Assembly;
        string[] resourceIDs = assembly.GetManifestResourceNames();
        SKPoint position = new SKPoint();

        foreach (string resourceID in resourceIDs)
        {
            if (resourceID.EndsWith(".png") ||
                resourceID.EndsWith(".jpg"))
            {
                using (Stream stream = assembly.GetManifestResourceStream(resourceID))
                using (SKManagedStream skStream = new SKManagedStream(stream))
                {
                    SKBitmap bitmap = SKBitmap.Decode(skStream);
                    bitmapCollection.Add(new TouchManipulationBitmap(bitmap)
                    {
                        Matrix = SKMatrix.MakeTranslation(position.X, position.Y),
                    });
                    position.X += 100;
                    position.Y += 100;
                }
            }
        }
    }
    ...
}
```

Конструктор загружает во всех растровых изображений, доступных как внедренные ресурсы и добавляет их в `bitmapCollection`. Обратите внимание, что `Matrix` на каждом инициализируется свойство `TouchManipulationBitmap` объекта, поэтому смещения верхний левый угол каждого растрового изображения на 100 пикселей.

`BitmapScatterView` Страница должна также обрабатывать события касания для нескольких растровых изображений. Вместо того чтобы определять `List` сенсорного манипулировать идентификаторы `TouchManipulationBitmap` этой программы требуется словарь объектов:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    Dictionary<long, TouchManipulationBitmap> bitmapDictionary =
       new Dictionary<long, TouchManipulationBitmap>();
    ...
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        // Convert Xamarin.Forms point to pixels
        Point pt = args.Location;
        SKPoint point =
            new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                        (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));

        switch (args.Type)
        {
            case TouchActionType.Pressed:
                for (int i = bitmapCollection.Count - 1; i >= 0; i--)
                {
                    TouchManipulationBitmap bitmap = bitmapCollection[i];

                    if (bitmap.HitTest(point))
                    {
                        // Move bitmap to end of collection
                        bitmapCollection.Remove(bitmap);
                        bitmapCollection.Add(bitmap);

                        // Do the touch processing
                        bitmapDictionary.Add(args.Id, bitmap);
                        bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                        canvasView.InvalidateSurface();
                        break;
                    }
                }
                break;

            case TouchActionType.Moved:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    canvasView.InvalidateSurface();
                }
                break;

            case TouchActionType.Released:
            case TouchActionType.Cancelled:
                if (bitmapDictionary.ContainsKey(args.Id))
                {
                    TouchManipulationBitmap bitmap = bitmapDictionary[args.Id];
                    bitmap.ProcessTouchEvent(args.Id, args.Type, point);
                    bitmapDictionary.Remove(args.Id);
                    canvasView.InvalidateSurface();
                }
                break;
        }
    }
    ...
}
```

Обратите внимание как `Pressed` логика просматривает `bitmapCollection` в обратном порядке. Точечные рисунки часто пересекаться друг с другом. Растровые изображения, более поздней версии в коллекции визуально находятся поверх точечные рисунки ранее в коллекции. Если есть несколько растровых изображений в списке пальца, нажимает на экране, верхний должен быть тем, управляется этой пальцем.

Также Обратите внимание, что `Pressed` помещает это растровое изображение в конец коллекции, чтобы визуально перемещается в верхнюю часть накопление других точечных рисунков.

В `Moved` и `Released` события, `TouchAction` вызовов обработчика `ProcessingTouchEvent` метод в `TouchManipulationBitmap` так же, как программа ранее.

Наконец `PaintSurface` вызовов обработчика `Paint` метод каждого `TouchManipulationBitmap` объекта:

```csharp
public partial class BitmapScatterViewPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKCanvas canvas = args.Surface.Canvas;
        canvas.Clear();

        foreach (TouchManipulationBitmap bitmap in bitmapCollection)
        {
            bitmap.Paint(canvas);
        }
    }
}
```

Код выполняет цикл по коллекции и отображает накопление точечных рисунков с начала коллекции в конец:

[![](touch-images/bitmapscatterview-small.png "Тройной снимок экрана со страницей представление Точечная диаграмма растрового изображения")](touch-images/bitmapscatterview-large.png#lightbox "тройной снимок экрана со страницей представление Точечная диаграмма растрового изображения")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [Вызов события из эффектов](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
