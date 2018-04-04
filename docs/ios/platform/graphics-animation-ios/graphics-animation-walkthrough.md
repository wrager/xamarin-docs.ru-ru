---
title: Пошаговое руководство. Использование CoreGraphics и CoreAnimation
description: В этой статье шаг за шагом показано, как создать приложение, использующее Core графики и анимации Core. Показано, как рисовать на экране в ответ на сенсорный ввод пользователя, а также как анимация изображения, которое перемещаются по пути.
ms.prod: xamarin
ms.assetid: 4B96D5CD-1BF5-4520-AAA6-2B857C83815C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f857accfcdec4cb60e781936d1d0836dbf8d6ffb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="drawing-and-animating-along-a-path"></a>Рисование и анимации в пути

В данном пошаговом руководстве мы собираемся рисование контура с помощью двухмерной графики в ответ на сенсорный ввод. Затем мы добавим `CALayer` содержит изображение, мы будет анимировать вдоль пути.

На следующем снимке экрана показано завершенное приложение:

![](graphics-animation-walkthrough-images/00-final-app.png "Завершенное приложение")

Перед началом загрузки *GraphicsDemo* , которая входит в этом руководстве. Ее можно загрузить [здесь](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/) и находится в **GraphicsWalkthrough** каталог запуска проекта с именем **GraphicsDemo_starter** , дважды щелкнув ее, и Откройте `DemoView` класса.

## <a name="drawing-a-path"></a>Рисование контура


1. В `DemoView` добавить `CGPath` переменной класса и создать его экземпляр в конструктор. Также объявить два `CGPoint` переменных, `initialPoint` и `latestPoint`, что мы будем использовать для захвата точки касания, из которой мы создаем путь:
    
    ```csharp
    public class DemoView : UIView
    {
        CGPath path;
        CGPoint initialPoint;
        CGPoint latestPoint;
    
        public DemoView ()
        {
            BackgroundColor = UIColor.White;
    
            path = new CGPath ();
        }
    }
    ```

2. Добавьте следующие директивы using:

    ```csharp
    using CoreGraphics;
    using CoreAnimation;
    using Foundation;
    ```

3. Затем Переопределите `TouchesBegan` и `TouchesMoved,` и добавьте следующие реализации для захвата сенсорный ввод начальной точки и в каждой точке касания последующих соответственно:

    ```csharp
    public override void TouchesBegan (NSSet touches, UIEvent evt){
    
        base.TouchesBegan (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }
    
    public override void TouchesMoved (NSSet touches, UIEvent evt){
    
        base.TouchesMoved (touches, evt);
    
        UITouch touch = touches.AnyObject as UITouch;
        
        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }
    ```

    `SetNeedsDisplay` будет вызываться каждый раз, штрихи перемещаться в порядке для `Draw` должен быть вызван в следующей проверки выполнения цикла.

4. Мы добавим строки в путь в `Draw` метод и использование красной пунктирной линией для рисования с. [Реализуйте `Draw` ](~/ios/platform/graphics-animation-ios/core-graphics.md) кодом, показанным ниже:

    ```csharp
    public override void Draw (CGRect rect){
    
        base.Draw (rect);
    
        if (!initialPoint.IsEmpty) {
    
            //get graphics context
            using(CGContext g = UIGraphics.GetCurrentContext ()){
                    
                //set up drawing attributes
                g.SetLineWidth (2);
                UIColor.Red.SetStroke ();
    
                //add lines to the touch points
                if (path.IsEmpty) {
                    path.AddLines (new CGPoint[]{initialPoint, latestPoint});
                } else {
                    path.AddLineToPoint (latestPoint);
                }
            
                //use a dashed line
                g.SetLineDash (0, new nfloat[] { 5, 2 * (nfloat)Math.PI });
                                
                //add geometry to graphics context and draw it
                g.AddPath (path);       
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
    ```

Если мы запустим приложение, мы могут соприкасаться для рисования на экране, как показано на следующем снимке экрана:

![](graphics-animation-walkthrough-images/01-path.png "Изображения на экране")

## <a name="animating-along-a-path"></a>Анимация вдоль контура

Теперь, когда реализован код, чтобы разрешить пользователям отобразить путь, добавим код, чтобы анимировать слоя формируемого пути.

1. Сначала добавьте [ `CALayer` ](~/ios/platform/graphics-animation-ios/core-animation.md) переменных в класс и создайте его в конструкторе:

    ```csharp
    public class DemoView : UIView
        {
            …
    
            CALayer layer;
    
            public DemoView (){
                …
    
                //create layer
                layer = new CALayer ();
                layer.Bounds = new CGRect (0, 0, 50, 50);
                layer.Position = new CGPoint (50, 50);
                layer.Contents = UIImage.FromFile ("monkey.png").CGImage;
                layer.ContentsGravity = CALayer.GravityResizeAspect;
                layer.BorderWidth = 1.5f;
                layer.CornerRadius = 5;
                layer.BorderColor = UIColor.Blue.CGColor;
                layer.BackgroundColor = UIColor.Purple.CGColor;
            }
    ```

2. Далее мы добавим слой как уровне слоя представления, когда пользователь отрывает копирование пальцев на экране. Затем мы создадим ключевой кадр анимации, используя путь, анимация слоя `Position`.

    Для этого необходимо переопределить `TouchesEnded` и добавьте следующий код:

    ```csharp
    public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            base.TouchesEnded (touches, evt);

            //add layer with image and animate along path

            if (layer.SuperLayer == null)
                Layer.AddSublayer (layer);

            // create a keyframe animation for the position using the path
            layer.Position = latestPoint;
            CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
            animPosition.Path = path;
            animPosition.Duration = 3;
            layer.AddAnimation (animPosition, "position");
        }
    ```

3. Запустите приложение сейчас и после рисования, слой с изображением добавляется и проходит формируемого пути:

![](graphics-animation-walkthrough-images/00-final-app.png "Слой с изображением добавляется и проходит по формируемого пути")

## <a name="summary"></a>Сводка

В этой статье мы останавливается пример, в котором связаны концепции графики и анимации. Во-первых, мы было показано, как использовать двухмерной графики, чтобы нарисовать контур `UIView` в ответ на сенсорный ввод пользователя. Затем мы было показано, как использовать основной анимации, чтобы сделать изображение перемещаться по этому пути.


## <a name="related-links"></a>Связанные ссылки

- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Основы рецепты анимации](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
