---
title: "Основные графики"
description: "В этой статье рассматриваются платформ iOS двухмерной графики. В этом примере показано использование двухмерной графики для рисования геометрии, изображения и PDF-файлов."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: d53494e61d702b83a28534c644f33fb5327b5958
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="core-graphics"></a>Основные графики

_В этой статье рассматриваются платформ iOS двухмерной графики. В этом примере показано использование двухмерной графики для рисования геометрии, изображения и PDF-файлов._

включает iOS [ *двухмерной графики* ](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) framework для обеспечения поддержки низкоуровневые рисования. Эти платформы являются, что включить графический функциональных возможностей в UIKit. 

Основные графики — это платформа низкоуровневые двумерной графики, и позволяющая рисования аппаратно-независимая графика. Все 2D рисование в UIKit внутренне использует основные графики.

Основные графики поддерживает рисование в ряд сценариев, в том числе:

-  [Рисование на экране через `UIView` ](#Drawing_in_a_UIView_Subclass) .
-  [Рисование изображений в памяти или на экране](#Drawing_Images_and_Text).
-  Создание и рисование в PDF-файл.
-  Для чтения и Рисование существующего PDF-файла.


## <a name="geometric-space"></a>Геометрическое места

Независимо от того, сценарий все операции рисования, выполненные с графикой Core выполняется в геометрические места, это означает, что она работает в абстрактный точек, а не в точках. Описания выберите формируемого с точки зрения геометрических и отображения состояния, такими как цвета, стили линий, т. д. и двухмерной графики обрабатывает все преобразования в пикселях. Такие состояния добавляется контексту графики, который можно рассматривать как образцу холста.

Существует несколько преимуществ такого подхода.

-  Код отрисовки становится динамические, а впоследствии можно изменить графики во время выполнения.
-  Снижается необходимость статические изображения в пакете приложения можно уменьшить размер приложения.
-  Графики становятся более устойчивым к изменения разрешения на устройствах.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Рисование в подкласс UIView

Каждый `UIView` имеет `Draw` метода, вызываемого при его отрисовке. Чтобы добавить код прорисовки представление, подкласс `UIView` и Переопределите `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Draw никогда не должен вызываться напрямую. Он вызывается системой во время выполнения цикла обработки. Во время первого выполнения выполнения цикла после добавления представления просмотр иерархии, его `Draw` вызывается метод. Последующие вызовы `Draw` возникают, когда представление является помеченные для рисования с помощью вызова `SetNeedsDisplay` или `SetNeedsDisplayInRect` в представлении.

### <a name="pattern-for-graphics-code"></a>Шаблон для кода графики

Код в `Draw` реализации должны описывать его пожеланий формируемого. Код прорисовки соответствует шаблону, в котором задает некоторое состояние, рисования и вызывает метод для запроса его отображения. Этот шаблон можно обобщить следующим образом:

1. Получите контекст графики.

2. Настройка атрибутов рисования.

3. Создайте некоторые geometry из графических примитивов.

4. Вызовите метод рисования или обводки.

### <a name="basic-drawing-example"></a>Пример базовых средств рисования

Например рассмотрим следующий фрагмент кода:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {
            
    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100), 
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);       
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Давайте разбить этот код:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```
С этой строкой он получает текущий контекст графики, используемый для рисования. Можно считать из графического контекста холста, рисование происходит на, содержащий все данные о состоянии о рисовании, например штриха цвета заливки, а также geometry для отрисовки.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
``` 

После получения графического контекста в коде настраивается некоторые атрибуты, используемый при отображении показано выше. В этом случае устанавливаются цвета линий ширины, обводки и заливки. Все последующие Рисование будет использовать эти атрибуты, поскольку они обслуживаются в состоянии графического контекста.

Создание геометрического объекта в коде используется `CGPath`, что позволяет графический контур описать из линий и кривых. В этом случае путь добавляет линии, соединяющие массиву точек составляет треугольника. Показанные ниже двухмерной графики используются для отображения представления системы координат, где находятся в верхнем левом углу с положительным direct x вправо, а оси y положительные вниз:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100), 
new CGPoint (220, 200)});

path.CloseSubpath ();
``` 

После создания пути, он добавляется графический контекст, чтобы вызов `AddPath` и `DrawPath` соответственно можно создать его.

Ниже приводится полученное представление.

 ![](core-graphics-images/00-bluetriangle.png "Образец вывода треугольник")

## <a name="creating-gradient-fills"></a>Создание градиентные заливки

Также доступны широкие формы отрисовки. Например двухмерной графики позволяет создавать градиентные заливки и применять контура обрезки. Чтобы нарисовать градиентную заливку путь из предыдущего примера, сначала путь должен быть установлен как контура обрезки:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Задание текущего пути как контура обрезки ограничивает все последующие рисования внутри геометрического объекта пути, например, следующий код, который рисует линейного градиента:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient, 
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top), 
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom), 
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Эти изменения позволяют градиентной заливки, как показано ниже:

 ![](core-graphics-images/01-gradient-fill.png "Пример с градиентной заливкой")

## <a name="modifying-line-patterns"></a>Изменение строки шаблонов

Атрибуты рисования линии можно изменять основные графики. Это включает в себя изменение ширины и обводки цвет линии, а также шаблон линии, как показано в следующем коде:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Добавление следующий код перед результаты операции рисования в единицах пунктирные обводки 10 long с 4 единиц интервала между тире, как показано ниже:

 ![](core-graphics-images/02-dashed-stroke.png "Добавление следующий код перед результаты операции рисования в штриховой штрихов рукописного ввода")
 
Обратите внимание, что при использовании API единой в Xamarin.iOS, тип массива должна быть `nfloat`, а также должен быть явно приведен к Math.PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Рисование изображений и текста

Помимо рисование контуров в контексте представления графики, двухмерной графики поддерживает рисования текста и изображений. Чтобы нарисовать изображение, просто создайте `CGImage` и передать его в `DrawImage` вызова:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Тем не менее это приводит к созданию изображения, нарисованных сверху вниз, как показано ниже:

 ![](core-graphics-images/03-upside-down-monkey.png "Изображения, нарисованного сверху вниз")

Причина этого — источник графики основных компонентов для рисования изображения находится в левом нижнем, а представление расположен в верхнем левом углу. Таким образом, чтобы правильно отображать изображения, источника должно быть изменено, чего можно выполнить путем изменения *текущей матрицы преобразования* *(CTM)*. CTM определяет, где живут точки, также известный как *пространства пользователя*. Инверсия CTM по оси y и сдвигая его высота границ по оси y отрицательное можно отразить изображение.

Графический контекст содержит вспомогательные методы для преобразования CTM. В этом случае `ScaleCTM` «зеркальное отражение» Рисование и `TranslateCTM` переходит в левый верхний угол, как показано ниже:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
    
    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}   
```

Полученное изображение отображается вертикально:

 ![](core-graphics-images/04-upright-monkey.png "Вертикальное положение изображения, отображаемого образца")

> [!IMPORTANT]
>  **Примечание:** изменения графического контекста применяются для всех последующих операций рисования. Таким образом когда CTM преобразуется, это повлияет на все дополнительные рисования. Например если после преобразования CTM перетащенной треугольника, отображаемое сверху вниз.

### <a name="adding-text-to-the-image"></a>Добавление текста в образ

Как пути и рисунки, рисование текста с графикой Core включает же базовый шаблон настройки какое-либо состояние графики и вызов метода для рисования. В случае текста, метод для отображения текста является `ShowText`. При добавлении рисования примере изображения, следующий код выводит текст с помощью двухмерной графики:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);
    
    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Как видите, установив состояние графики для отображения текста похоже на рисование geometry. Однако рисования текста, также применяются Рисование режим и шрифт текста. В этом случае тень также применяется, несмотря на то, что применение shadows работает так же, для отображения пути.

Получившийся текст отображается со значком, как показано ниже:

 ![](core-graphics-images/05-text-on-image.png "Получившийся текст отображается с изображением")

## <a name="memory-backed-images"></a>Изображения с поддержкой памяти

Помимо рисования для графического представления контекста двухмерной графики поддерживает рисование памяти резервные образов, также известной как рисование за пределы экрана. Для этого необходимо:

-  Создание графического контекста, поддерживаемый в памяти растрового изображения
-  Рисование состояние установки и выполнения команд рисования
-  Получение изображения из контекста
-  Удаление контекста


В отличие от `Draw` метод, где предоставляется контекст представления, в этом случае вы создания контекста одним из двух способов:

1. Путем вызова `UIGraphics.BeginImageContext` (или `BeginImageContextWithOptions`)

2. При создании нового `CGBitmapContextInstance`

 `CGBitmapContextInstance` полезно при работе непосредственно с биты изображения, например, для случаев, когда используется алгоритм обработки пользовательского образа. Во всех остальных случаях следует использовать `BeginImageContext` или `BeginImageContextWithOptions`.

Получив контекст образа, добавления код отрисовки — так же, как он находится в `UIView` подкласс. Например, в примере кода, которые ранее использовались для Рисование треугольника, использоваться для рисования изображения в памяти, а не в `UIView`, как показано ниже:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100), 
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }
    
    return triangleImage;
}
```

Обычно отрисовки к растровому изображению поддержкой памяти используется для записи образа из любого `UIView`. Например, следующий код подготавливают слой представления контекст растрового изображения и создает `UIImage` из него:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Рисование PDF-файлов

В дополнение к изображения двухмерной графики поддерживает рисование PDF. Подобно изображениям, вы можете отрисовки PDF-файла в памяти, а также чтения PDF-ФАЙЛ для подготовки к просмотру в `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF-файла в UIView

Графики Core также поддерживает чтение PDF-файла из файла и отображения его в представлении с помощью `CGPDFDocument` класса. `CGPDFDocument` Представляет PDF в коде и может использоваться для чтения и отображения страниц.

Например, в следующем коде в `UIView` подкласс считывает PDF-файла из файла в `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }
  
     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

`Draw` Можно затем использовать метод `CGPDFDocument` необходимо считать страницу в `CGPDFPage` и отображать его путем вызова `DrawPDFPage`, как показано ниже:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);
        
    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);
        
        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {
            
        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);
        
        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);
        
        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>Память резервного PDF

Для PDF-документ в памяти, необходимо создать PDF контекст путем вызова `BeginPDFContext`. Документ в формат PDF более детальные на страницы. Каждая страница запускается путем вызова `BeginPDFPage` и завершается путем вызова метода `EndPDFContent`, с помощью графического кода между ними. Кроме того как с помощью рисования изображения памяти скопированный PDF Рисование использует источник в левом нижнем, который может быть включен, только изменив CTM как с изображениями.

Ниже показано, как для рисования текста в PDF-файл:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();
       
using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);      
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }
    
//complete a PDF page
UIGraphics.EndPDFContent ();
```

Получившийся текст рисуется в формат PDF, то содержится в `NSData` , могут быть сохранены, отправленного, полученный по электронной почте, и т. д.


## <a name="summary"></a>Сводка

В этой статье мы рассмотрели графические возможности, предоставляемые через *двухмерной графики* framework. Мы рассмотрели используемый двухмерной графики для отрисовки геометрии, изображения и PDF-файлов в контексте `UIView,` а также с контексты графических поддержкой памяти.

## <a name="related-links"></a>Связанные ссылки

- [Пример основных графики](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Графики и анимации Пошаговое руководство](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Основы рецепты анимации](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
