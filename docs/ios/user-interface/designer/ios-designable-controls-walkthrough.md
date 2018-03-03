---
title: "Пошаговое руководство. Использование пользовательских элементов управления с помощью конструктора Xamarin для iOS"
description: "Эта статья содержит пошаговое руководство, в котором показано, как создать пользовательский элемент управления и использовать его в конструкторе Xamarin для операций ввода-вывода. В этом примере показано создание элемента управления в панели элементов конструктора, его можно перетаскивания или перетащить в представление. Кроме того показано, как реализовать элемент управления, отображается правильно во время разработки и среды выполнения, а также создание свойства, которые могут быть установлены во время разработки."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: e78b76a531e9f8ea88adca46fc59b2063fce14cc
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-custom-controls-with-the-xamarin-designer-for-ios"></a>Пошаговое руководство. Использование пользовательских элементов управления с помощью конструктора Xamarin для iOS

_Эта статья содержит пошаговое руководство, в котором показано, как создать пользовательский элемент управления и использовать его в конструкторе Xamarin для операций ввода-вывода. В этом примере показано создание элемента управления в панели элементов конструктора, его можно перетаскивания или перетащить в представление. Кроме того показано, как реализовать элемент управления, отображается правильно во время разработки и среды выполнения, а также создание свойства, которые могут быть установлены во время разработки._

## <a name="requirements"></a>Требования

Конструктор Xamarin для операций ввода-вывода доступна в Visual Studio для Mac и Visual Studio 2015 и 2017 г. в Windows.

В этом руководстве предполагается Знакомство с содержимым, охваченных [Приступая к работе проводит](~/ios/get-started/index.md).

## <a name="walkthrough"></a>Пошаговое руководство

> [!IMPORTANT]
> Начиная с версии Xamarin.Studio 5.5, способ создания пользовательских элементов управления несколько отличается в более ранних версиях. Чтобы создать пользовательский элемент управления, либо `IComponent` (с помощью связанной реализации методов) требуется интерфейс или класс может быть сопровождаться `[DesignTimeVisible(true)]`. Последний способ используется в следующем примере пошагового руководства.


1. Создайте новое решение из **iOS > приложения > одним приложением представление > C#** шаблона, назовите его `ScratchTicket`и продолжайте работу мастера создания проекта:


    [![](ios-designable-controls-walkthrough-images/01new.png "Создание нового решения")](ios-designable-controls-walkthrough-images/01new.png)


1. Создать новый файл пустым классом с именем `ScratchTicketView`:


    [![](ios-designable-controls-walkthrough-images/02new.png "Создайте новый класс ScratchTicketView")](ios-designable-controls-walkthrough-images/02new.png)


1. Добавьте следующий код для `ScratchTicketView` класса:

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;
    
    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;
    
            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }
    
            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }
    
            public ScratchTicketView()
            {
                Initialize();
            }
    
            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }
    
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }
    
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);
    
                var touch = touches.AnyObject as UITouch;
    
                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }
    
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }
    
            public override void Draw(CGRect rect)
            {
                base.Draw(rect);
    
                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);
    
                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();
    
                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }
    
                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);        
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```


1. Добавить `FillTexture.png`, `FillTexture2.png` и `Monkey.png` файлов (доступных [из GitHub](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)) для **ресурсов** папки.
    
1. Дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе:

    
    [![](ios-designable-controls-walkthrough-images/03new.png "Конструктор iOS")](ios-designable-controls-walkthrough-images/03new.png)



1. Перетаскивание **представление изображения** из **элементов** на представление в раскадровку.

    
    [![](ios-designable-controls-walkthrough-images/04new.png "Добавить представление изображения в макет")](ios-designable-controls-walkthrough-images/04new.png)


1. Выберите **представление изображения** и изменить его **изображения** свойства `Monkey.png`.

    
    [![](ios-designable-controls-walkthrough-images/05new.png "Свойства образа изображение представления Monkey.png")](ios-designable-controls-walkthrough-images/05new.png)

    
1. Как мы используем классы размер нам нужно будет ограничивать это представление изображения. Щелкните изображение дважды, чтобы перевести в режим ограничения. Давайте ограничить его центр, нажав кнопку закрепления center дескриптор и его выравнивание по вертикали и горизонтали:
    
    
    [![](ios-designable-controls-walkthrough-images/06new.png "Выравнивание по центру изображения")](ios-designable-controls-walkthrough-images/06new.png)

    
1. Чтобы ограничить значения высоты и ширины, щелкните закрепление размер маркеров (маркеры костей форме) и выберите ширины и высоты соответственно:

    
    [![](ios-designable-controls-walkthrough-images/07new.png "Добавление ограничений")](ios-designable-controls-walkthrough-images/07new.png)


1. Обновление кадра, в зависимости от ограничений, нажав кнопку "Обновить" на панели инструментов:


    [![](ios-designable-controls-walkthrough-images/08new.png "Ограничения инструментов")](ios-designable-controls-walkthrough-images/08new.png)


1. Затем выполните построение проекта, чтобы **Scratch представление билет** будет отображаться в разделе **настраиваемых компонентов** на панели инструментов:

    
    [![](ios-designable-controls-walkthrough-images/09new.png "Компоненты пользовательских элементов")](ios-designable-controls-walkthrough-images/09new.png)


1. Перетаскивание **Scratch представление билет** , чтобы он отображался над изображением monkey. Измените перетащите маркеры, чтобы представление билет Scratch охватывает monkey полностью, как показано ниже:

    
    [![](ios-designable-controls-walkthrough-images/10new.png "Представление временных файлов билет через представление изображения")](ios-designable-controls-walkthrough-images/10new.png)


1. Ограничить представление билет Scratch представление изображения, нарисовав ограничивающий прямоугольник, чтобы выбрать оба представления. Выберите параметры, чтобы ограничить его ширину, высоту, Center и среднего и обновление рамки, в зависимости от ограничений, как показано ниже:
 
    
    [![](ios-designable-controls-walkthrough-images/11new.png "Центрирование и добавление ограничений")](ios-designable-controls-walkthrough-images/11new.png)


1. Запустите приложение и «scratch off» изображение для отображения полей monkey.


 [ ![](ios-designable-controls-walkthrough-images/10-app.png "Запустите образец приложения")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="adding-design-time-properties"></a>Добавление свойств во время разработки

Конструктор также включает поддержку во время разработки для пользовательских элементов управления свойство числового типа, перечисления, строки, bool, CGSize, UIColor и UIImage. Чтобы продемонстрировать, добавим свойство `ScratchTicketView` для задания изображения, которое является «верхушку Выкл.»

Добавьте следующий код в `ScratchTicketView` класса для свойства:

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image 
{
    get { return image; }
    set { 
            image = value;
            SetNeedsDisplay ();
        }
}
```

Мы также можно добавить проверку, значение null для `Draw` метода, следующим образом:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);
            
        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);       
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

Включая `ExportAttribute` и `BrowsableAttribute` со значением аргумента `true` результатов в свойстве, будет отображаться в конструкторе **свойство** панель. Изменение свойства в другое изображение, входящий в состав проекта, такие как `FillTexture2.png`, приводит к обновление элемента управления во время разработки, как показано ниже:

 [ ![](ios-designable-controls-walkthrough-images/11-customproperty.png "Изменение свойства времени разработки")](ios-designable-controls-walkthrough-images/10-app.png)

## <a name="summary"></a>Сводка

В этой статье мы рассмотрения как создать настраиваемый элемент управления, а также использовать в приложения iOS с помощью конструктора операций ввода-вывода. Мы узнали, как создание и построение элемента управления, чтобы сделать его доступным для приложения в конструкторе **элементов**. Кроме того мы рассмотрели способы реализации элемента управления, таким образом, что он правильно отображает во время разработки и среды выполнения, а также способ предоставления свойств пользовательского элемента управления в конструкторе.



## <a name="related-links"></a>Связанные ссылки

- [ScratchTicket (пример)](https://developer.xamarin.com/samples/monotouch/ScratchTicket/)
- [требуемые изображения (пример)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
