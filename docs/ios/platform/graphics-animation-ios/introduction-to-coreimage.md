---
title: CoreImage
description: "CoreImage — это новая платформа, появившиеся с iOS 5, чтобы обеспечить обработку изображений и видео расширения функциональности в реальном времени. В этой статье описаны эти возможности с образцами Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 91E0780B-FF8A-E70D-9CD4-419119612B2D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2d9c2b27de7addc0ed1faeed038db81e2470087f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="coreimage"></a>CoreImage

_CoreImage — это новая платформа, появившиеся с iOS 5, чтобы обеспечить обработку изображений и видео расширения функциональности в реальном времени. В этой статье описаны эти возможности с образцами Xamarin.iOS._

CoreImage — это новая платформа, представленные в iOS 5, который предоставляет ряд встроенных фильтров и применяются изображения и видеоролики, включая обнаружение лицевой стороны.

Этот документ содержит простые примеры.

-  Обнаружение начертания.
-  Применение фильтров к изображению
-  Список доступных фильтров.


Эти примеры поможет приступить к работе, включение функции CoreImage в Xamarin.iOS приложения.

## <a name="requirements"></a>Требования

Необходимо использовать последнюю версию Xcode.

## <a name="face-detection"></a>Обнаружение лицевой стороны

Делает функция обнаружения лиц на CoreImage, только то, что оно написано — это предпринимается попытка определить граней в фотографию и возвращает координаты любого лица, распознаваемые им. Эти сведения можно использовать для подсчета числа пользователей на изображении, рисование индикаторы на изображении (например) для «теги» людей в фотографии), и любые другие элементы можно представить.

Этот код CoreImage\SampleCode.cs показано, как создать и использовать обнаружения лиц на внедренного изображения:

```csharp
var image = new UIImage("photoFace.JPG");
var context = CIContext.FromOptions(null);
var detector = CIDetector.CreateFaceDetector (context, true);
var ciImage = CIImage.FromCGImage(image.CGImage);
CIFeature[] features = detector.FeaturesInImage(ciImage);
```

Функции массива будет заполняться `CIFaceFeature` объектов (если все фрагменты были обнаружены). Отсутствует `CIFaceFeature` для каждой грани. `CIFaceFeature` имеет следующие свойства:

-  HasMouthPosition — был ли обнаружен рот для этого грани.
-  HasLeftEyePosition — был ли обнаружен левой глаза для этого грани.
-  HasRightEyePosition — был ли обнаружен правой глаза для этого грани. 
-  MouthPosition — координаты лицу для этого грани.
-  LeftEyePosition — координаты левого глаза для этого грани.
-  RightEyePosition — координаты правой глаза для этого грани.


Координаты для этих свойств иметь их источник в нижнем левом углу — в отличие от UIKit, в котором используется верхнего левого в качестве источника. При использовании координаты на `CIFaceFeature` убедитесь, что «отражение» их. Это представление очень простой пользовательского образа в CoreImage\CoreImageViewController.cs демонстрируется рисование «начертания треугольником» в образе (Примечание `FlipForBottomOrigin` метод):

```csharp
public class FaceDetectImageView : UIView
{
    public Xamarin.iOS.CoreImage.CIFeature[] Features;
    public UIImage Image;
    public FaceDetectImageView (RectangleF rect) : base(rect) {}
    CGPath path;
    public override void Draw (RectangleF rect) {
        base.Draw (rect);
        if (Image != null)
            Image.Draw(rect);

        using (var context = UIGraphics.GetCurrentContext()) {
            context.SetLineWidth(4);
            UIColor.Red.SetStroke ();
            UIColor.Clear.SetFill ();
            if (Features != null) {
                foreach (var feature in Features) { // for each face
                    var facefeature = (CIFaceFeature)feature;
                    path = new CGPath ();
                    path.AddLines(new PointF[]{ // assumes all 3 features found
                        FlipForBottomOrigin(facefeature.LeftEyePosition, 200),
                        FlipForBottomOrigin(facefeature.RightEyePosition, 200),
                        FlipForBottomOrigin(facefeature.MouthPosition, 200)
                    });
                    path.CloseSubpath();
                    context.AddPath(path);
                    context.DrawPath(CGPathDrawingMode.FillStroke);
                }
            }
        }
    }
    /// <summary>
    /// Face recognition coordinates have their origin in the bottom-left
    /// but we are drawing with the origin in the top-left, so "flip" the point
    /// </summary>
    PointF FlipForBottomOrigin (PointF point, int height)
    {
        return new PointF(point.X, height - point.Y);
    }
}
```

Затем в файле SampleCode.cs изображения и функций назначаются перед перерисовке изображения:

```csharp
faceView.Image = image;
faceView.Features = features;
faceView.SetNeedsDisplay();
```

На снимке экрана показан пример выходных данных: расположение обнаруженных черт лица, отображаются в UITextView и на исходный образ с помощью CoreGraphics.

Из-за работы распознавание лиц он иногда обнаружит операции, помимо человека гарнитуры (например, эти игрушек monkeys!).

## <a name="filters"></a>Фильтры

Существует более 50 различных встроенные фильтры, и платформа является расширяемым, чтобы новые фильтры можно реализовать.

## <a name="using-filters"></a>С помощью фильтров

Применение фильтра к изображению состоит из четырех отдельных шагов: загружает изображение, создании фильтра, применения фильтра и сохранении (или отображение) результат.

Во-первых, загрузки образа в `CIImage` объекта.

```csharp
var uiimage = UIImage.FromFile ("photo.JPG");
var ciimage = new CIImage (uiimage);
```

Во-вторых создайте класс фильтра и задать его свойства.

```csharp
var sepia = new CISepiaTone();
sepia.Image = ciimage;
sepia.Intensity = 0.8f;
```

В-третьих, доступ к `OutputImage` свойство и вызвать `CreateCGImage` метод для подготовки к просмотру окончательный результат.

```csharp
CIImage output = sepia.OutputImage;
var context = CIContext.FromOptions(null);
var cgimage = context.CreateCGImage (output, output.Extent);
```

Наконец назначьте изображение представления для просмотра результата. В реальном приложении полученное изображение может быть сохранен в файловой системе, фотоальбома, твит или по электронной почте.

```csharp
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

Эти снимки экрана отобразится результат `CISepia` и `CIHueAdjust` фильтры, которые демонстрируются в CoreImage.zip пример кода.

В разделе [изменить контракт и яркости изображения рецепт](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image) пример `CIColorControls` фильтра.

```csharp
var uiimage = UIImage.FromFile("photo.JPG");
var ciimage = new CIImage(uiimage);
var hueAdjust = new CIHueAdjust();   // first filter
hueAdjust.Image = ciimage;
hueAdjust.Angle = 2.094f;
var sepia = new CISepiaTone();       // second filter
sepia.Image = hueAdjust.OutputImage; // output from last filter, input to this one
sepia.Intensity = 0.3f;
CIFilter color = new CIColorControls() { // third filter
    Saturation = 2,
    Brightness = 1,
    Contrast = 3,
    Image = sepia.OutputImage    // output from last filter, input to this one
};
var output = color.OutputImage;
var context = CIContext.FromOptions(null);
// ONLY when CreateCGImage is called do all the effects get rendered
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

```csharp
var context = CIContext.FromOptions (null);
```

```csharp
var context = CIContext.FromOptions(new CIContextOptions() {
    UseSoftwareRenderer = true  // CPU
});
var cgimage = context.CreateCGImage (output, output.Extent);
var ui = UIImage.FromImage (cgimage);
imgview.Image = ui;
```

### <a name="listing-filters-and-their-properties"></a>Список фильтров и их свойств

Этот код из CoreImage\SampleCode.cs выводит полный список встроенных фильтров и их параметры.

```csharp
var filters = CIFilter.FilterNamesInCategories(new string[0]);
foreach (var filter in filters){
   display.Text += filter +"\n";
   var f = CIFilter.FromName (filter);
   foreach (var key in f.InputKeys){
     var attributes = (NSDictionary)f.Attributes[new NSString(key)];
     var attributeClass = attributes[new NSString("CIAttributeClass")];
     display.Text += "   " + key;
     display.Text += "   " + attributeClass + "\n";
   }
}
```

[Ссылку на класс CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.html) описание 50 встроенных фильтров и их свойств. С помощью кода выше можно запрашивать классов фильтра, включая значения по умолчанию для параметров и минимальное и максимальное допустимые значения (которые могут использоваться для проверки входных данных перед применением фильтра).

Список категорий вывод выглядит следующим образом в симуляторе — можно прокручивать список, чтобы увидеть все фильтры и их параметры.

 [ ![](introduction-to-coreimage-images/coreimage05.png "Список категорий вывод выглядит следующим образом в симуляторе")](introduction-to-coreimage-images/coreimage05.png)

Каждый фильтр в списке предоставленный как класс в Xamarin.iOS, поэтому можно также изучить API Xamarin.iOS.CoreImage в браузере сборку или с помощью автозаполнения в Visual Studio или Visual Studio для Mac. 

## <a name="summary"></a>Сводка

В этой статье показано, как использовать некоторые новые компоненты framework iOS 5 CoreImage как обнаружение лицевой и применения фильтров к изображению. Десятки другое изображение фильтры доступны в платформу для использования.

## <a name="related-links"></a>Связанные ссылки

- [Образ Core (пример)](https://developer.xamarin.com/samples/CoreImage/)
- [Настройка контракта и яркости изображения инструкций](https://developer.xamarin.com/recipes/ios/media/coreimage/adjust_contrast_and_brightness_of_an_image)
- [С помощью фильтров CoreImage](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Conceptual/CoreImaging/ci_tasks/ci_tasks.html)
- [Ссылку на класс CIFilter](https://developer.apple.com/library/prerelease/ios/#documentation/GraphicsImaging/Reference/QuartzCoreFramework/Classes/CIFilter_Class/Reference/Reference.htm)
