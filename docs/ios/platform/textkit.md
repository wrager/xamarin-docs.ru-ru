---
title: TextKit в Xamarin.iOS
description: В этом документе описывается использование TextKit в Xamarin.iOS. TextKit предоставляет мощные текст функции макета и подготовки к просмотру.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ac80d1d07f5649d377dd6fdefcb4911ba9ec2dcb
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788339"
---
# <a name="textkit-in-xamarinios"></a>TextKit в Xamarin.iOS

TextKit — это новый API, который предлагает мощный текст функции макета и подготовки к просмотру. Является надстройкой низкоуровневые framework основного текста, но гораздо проще по сравнению с основного текста.

Для предоставления возможности TextKit для стандартных элементов управления, несколько текстовых элементов управления iOS были пересоздания использовать TextKit, включая:

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>Архитектура

TextKit предоставляет многоуровневую архитектуру, которая отделяет хранения текста из макета и отображения, включая следующие классы:

-  `NSTextContainer` — Предоставляет систему координат и геометрии, которая используется для макета текста.
-  `NSLayoutManager` — Располагает текст, включив текст в глифов. 
-  `NSTextStorage` — Содержит текстовые данные, а также обрабатывает пакетные обновления свойства текста. Все пакетные обновления будут переданы диспетчера макета для фактической обработки изменения, такие как повторное вычисление макета и перерисовки текста.


Эти три класса, применяются к представление, которое отображает текст. Встроенный текст, обработка представления, такие как `UITextView`, `UITextField`, и `UILabel` уже их установить, но можно создать и применить их к любому `UIView` также экземпляр.

Эта архитектура показана на следующем рисунке:

 ![](textkit-images/textkitarch.png "На рисунке показана архитектура TextKit")

## <a name="text-storage-and-attributes"></a>Хранение текста и атрибуты

`NSTextStorage` Класс содержит текст, отображаемый в представлении. Он также обеспечивает связь любые изменения в текст — например, изменения в символы или их атрибутов — для диспетчера макета для отображения. `NSTextStorage` наследует от `MSMutableAttributed` строки, позволяя изменять для атрибутов текста с помощью пакетов между `BeginEditing` и `EndEditing` вызовов.

Например следующий фрагмент кода указывает изменение на переднем плане и фонового цвета, соответственно и предназначен для определенной диапазонов:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

После `EndEditing` является именем, изменения отправляются в диспетчер макет, который в свою очередь выполняет все необходимые макета и вычисления отрисовки для текста для отображения в представлении.

## <a name="layout-with-exclusion-path"></a>Макет с путь исключения

TextKit также поддерживает макета, а также позволяет для сложных сценариев, например, текст с несколькими столбцами и заполнение текстом вокруг указанных путей называется *пути исключения*. Пути исключения применяются к контейнера текста, которая изменяет геометрию макет текста, в результате текст вокруг указанных путей.

Добавление путь исключения требует параметр `ExclusionPaths` свойства диспетчера макета. При задании этого свойства диспетчера структуры необходимо сделать недействительным макет текста и текста, вокруг путь исключения потока.

### <a name="exclusion-based-on-a-cgpath"></a>Исключения, исходя из CGPath

Рассмотрим следующий `UITextView` подкласс реализации:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Этот код добавляет поддержку для рисования для текстового представления с помощью двухмерной графики. Поскольку `UITextView` с TextKit для отрисовки текста и разметки теперь будет содержать класс, он может использовать все возможности TextKit, например установку исключения пути.

> [!IMPORTANT]
> Этот пример подклассов `UITextView` Добавление сенсорного ввода, поддержка рисования. Создание подклассов `UITextView` не является обязательным для доступа к возможностям TextKit.



После рисования для представления текста, отображаемого `CGPath` применяется к `UIBezierPath` экземпляра, задав `UIBezierPath.CGPath` свойство:

```csharp
bezierPath.CGPath = exclusionPath;
```

Обновление следующая строка кода делает макет текста обновить путь:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

Следующем снимке экрана показано, как макет текста меняется вокруг формируемого путь:

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "На этом снимке экрана показано, как макет текста меняется вокруг формируемого путь")

Обратите внимание, что диспетчер макета `AllowsNonContiguousLayout` свойство имеет значение false в этом случае. В этом случае макета к пересчету во всех случаях, когда изменения текста. Этот параметр может дать преимущества производительности, чтобы избежать обновления макета full, особенно в случае больших документов. Однако задание `AllowsNonContiguousLayout` на true могли бы помешать путь исключения обновление макета в некоторых случаях — например, если ввести текст во время выполнения без возврат каретки конечные до устанавливается путь.


## <a name="related-links"></a>Связанные ссылки

- [Введение в iOS 7 (пример)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Обзор пользовательского интерфейса iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Фоновая обработка](~/ios/app-fundamentals/backgrounding/index.md)
