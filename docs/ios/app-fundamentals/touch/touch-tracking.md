---
title: Мультисенсорные пальцем отслеживания в Xamarin.iOS
description: В этом документе описывается отслеживать отдельные пальцев в мультисенсорные жесты в приложении Xamarin.iOS. Оно основано на finger-painting пример приложения.
ms.prod: xamarin
ms.assetid: 48E8B20D-0833-43D2-976A-0605DDB386E3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 85dbd3c158408026f4ecef5fb2b01c265747140e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784407"
---
# <a name="multi-touch-finger-tracking-in-xamarinios"></a>Мультисенсорные пальцем отслеживания в Xamarin.iOS

_В этом документе показано, как отслеживать события касания из нескольких пальцами_

Бывают случаи, когда приложения мультисенсорные должна отслеживать отдельных пальцами при перемещении одновременно на экране. Одно приложение обычно представляет собой программу finger-paint. Необходимо, чтобы пользователь могли для рисования с одним пальцем, но также для рисования сразу несколько пальцами. Как приложение обрабатывает несколько событий сенсорного экрана, ему необходимо различать эти пальцами.

При первом касании пальцем экрана, создает iOS [ `UITouch` ](https://developer.xamarin.com/api/type/UIKit.UITouch/) объект для этого пальцем. Этот объект остается равным палец перемещается на экране и убирает с экрана, после чего удаляется объект. Для отслеживания пальцами, программы следует избегать хранения это `UITouch` объекта напрямую. Вместо этого можно использовать [ `Handle` ](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) свойство типа `IntPtr` для уникальной идентификации этих `UITouch` объектов.

Почти всегда программу, которая отслеживает отдельных пальцами поддерживают словарь отслеживания сенсорного ввода. Для приложения iOS ключ словаря представляет `Handle` значение, определяющее определенного палец. Значение словаря зависит от приложения. В [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) программы, мазка пальцем (из касания для освобождения) связан с объектом, который содержит все сведения, необходимые для подготовки к просмотру линия, проведенная с этой пальцем. Программа определяет небольшой `FingerPaintPolyline` класса для этой цели:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new CGPath();
    }

    public CGColor Color { set; get; }

    public float StrokeWidth { set; get; }

    public CGPath Path { private set; get; }
}
```

Каждой ломаной линии имеет цвет, толщину обводки и операций ввода-вывода графики [ `CGPath` ](https://developer.xamarin.com/api/type/CoreGraphics.CGPath/) объекта накапливаются и просмотру нескольких точек линии его написания.


Все остальные приведенный ниже код содержится в `UIView` производный класс с именем `FingerPaintCanvasView`. Что класса поддерживают словарь объектов типа `FingerPaintPolyline` во время их активно отображаются с одного или нескольких пальцами:

```csharp
Dictionary<IntPtr, FingerPaintPolyline> inProgressPolylines = new Dictionary<IntPtr, FingerPaintPolyline>();
```

Этот словарь позволяет быстро получить представление `FingerPaintPolyline` на основе сведений, связанных с каждого пальца `Handle` свойство `UITouch` объекта.

`FingerPaintCanvasView` Также поддерживает класс `List` объекта для завершенных многоугольника:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Объекты в этом `List` находятся в том же порядке, что они были отображены.

`FingerPaintCanvasView` переопределяет пять методов, определенных `View`:

- [`TouchesBegan`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesBegan/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesMoved`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesMoved/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesEnded`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesEnded/p/Foundation.NSSet/UIKit.UIEvent/)
- [`TouchesCancelled`](https://developer.xamarin.com/api/member/UIKit.UIResponder.TouchesCancelled/p/Foundation.NSSet/UIKit.UIEvent/)
- [`Draw`](https://developer.xamarin.com/api/member/UIKit.UIView.Draw/p/CoreGraphics.CGRect/)

Различные `Touches` переопределения накапливаться точки, которые составляют многоугольника.

[`Draw`] Переопределение рисует завершенного многоугольника, а затем выполняющиеся многоугольника:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (CGContext context = UIGraphics.GetCurrentContext())
    {
        // Stroke settings
        context.SetLineCap(CGLineCap.Round);
        context.SetLineJoin(CGLineJoin.Round);

        // Draw the completed polylines
        foreach (FingerPaintPolyline polyline in completedPolylines)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }

        // Draw the in-progress polylines
        foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
        {
            context.SetStrokeColor(polyline.Color);
            context.SetLineWidth(polyline.StrokeWidth);
            context.AddPath(polyline.Path);
            context.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Каждый из `Touches` переопределения потенциально сообщает действия несколько пальцами, указанного одним или несколькими `UITouch` объектов, хранящихся в `touches` аргумента метода. `TouchesBegan` Переопределения цикл через эти объекты. Для каждого `UITouch` объекта, метод создает и инициализирует новую `FingerPaintPolyline` объекта, включая сохранение исходного положения пальцем, полученный от `LocationInView` метод. Это `FingerPaintPolyline` добавляется объект `InProgressPolylines` словаря с помощью `Handle` свойство `UITouch` объекта в качестве ключа словаря:

```csharp
public override void TouchesBegan(NSSet touches, UIEvent evt)
{
    base.TouchesBegan(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Create a FingerPaintPolyline, set the initial point, and store it
        FingerPaintPolyline polyline = new FingerPaintPolyline
        {
            Color = StrokeColor,
            StrokeWidth = StrokeWidth,
        };

        polyline.Path.MoveToPoint(touch.LocationInView(this));
        inProgressPolylines.Add(touch.Handle, polyline);
    }
    SetNeedsDisplay();
}
```

Метод завершается путем вызова `SetNeedsDisplay` для создания вызова `Draw` переопределить и обновление экрана.

Движении пальца или пальца на экране `View` возвращает несколько вызовов его `TouchesMoved` переопределения. Это переопределение аналогично циклически `UITouch` объектов, хранящихся в `touches` аргумента и добавляет текущее расположение пальцем графический контур:

```csharp
public override void TouchesMoved(NSSet touches, UIEvent evt)
{
    base.TouchesMoved(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Add point to path
        inProgressPolylines[touch.Handle].Path.AddLineToPoint(touch.LocationInView(this));
    }
    SetNeedsDisplay();
}
```

`touches` Коллекция содержит только те `UITouch` объектов для пальцами, которые были перемещены с момента последнего вызова `TouchesBegan` или `TouchesMoved`. В дальнейшем `UITouch` объектов, соответствующий *все* пальцами, которые в настоящее время подключенные к экрана, эти сведения можно получить через `AllTouches` свойство `UIEvent` аргумента метода.

`TouchesEnded` Переопределения имеет два задания. Его необходимо добавить последней точки графический контур и передачи `FingerPaintPolyline` объекта из `inProgressPolylines` словарь для `completedPolylines` списка:

```csharp
public override void TouchesEnded(NSSet touches, UIEvent evt)
{
    base.TouchesEnded(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        // Get polyline from dictionary and remove it from dictionary
        FingerPaintPolyline polyline = inProgressPolylines[touch.Handle];
        inProgressPolylines.Remove(touch.Handle);

        // Add final point to path and save with completed polylines
        polyline.Path.AddLineToPoint(touch.LocationInView(this));
        completedPolylines.Add(polyline);
    }
    SetNeedsDisplay();
}
```

`TouchesCancelled` Переопределение обрабатывается просто прерывания `FingerPaintPolyline` объекта в словаре:

```csharp
public override void TouchesCancelled(NSSet touches, UIEvent evt)
{
    base.TouchesCancelled(touches, evt);

    foreach (UITouch touch in touches.Cast<UITouch>())
    {
        inProgressPolylines.Remove(touch.Handle);
    }
    SetNeedsDisplay();
}
```

В принципе, такая обработка позволяет [FingerPaint](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint) программы, отслеживать отдельные пальцев и отображать результаты на экране:

[![](touch-tracking-images/image01.png "Отслеживание отдельных пальцев и отображения результатов на экране")](touch-tracking-images/image01.png#lightbox)

Теперь вы знаете, как можно отслеживать отдельные пальца на экране и различать их.



## <a name="related-links"></a>Связанные ссылки

- [Руководство по эквивалентные Xamarin Android](~/android/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (пример)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
