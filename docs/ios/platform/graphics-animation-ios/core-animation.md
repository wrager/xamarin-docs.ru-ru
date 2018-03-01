---
title: "Основной анимации"
description: "В этой статье рассматриваются framework Core анимации, показывающий, как высокая производительность, плавности анимации в UIKit, а также позволяет использовать их непосредственно в управления анимации более низкого уровня."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 80fe298f3dd24aac7f84213aee96499dd369d16d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="core-animation"></a>Основной анимации

_В этой статье рассматриваются framework Core анимации, показывающий, как высокая производительность, плавности анимации в UIKit, а также позволяет использовать их непосредственно в управления анимации более низкого уровня._

включает iOS [ *основной анимации* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) для обеспечения поддержки анимации представления в приложении.
Все вести smooth анимации в iOS, таких как прокрутка таблиц и проведение пальцем по экрану между различными представлениями выполнять также вызвано тем, что они используют основной анимации внутренним образом.

Платформы анимации основных компонентов и Core графики можно использовать вместе для создания привлекательных, анимированной двумерной графики. Фактически основной анимации даже преобразовывать двумерной графики в трехмерном пространстве Создание прекрасных возможности взаимодействия. Тем не менее для создания true трехмерной графики, потребовалось бы использование как OpenGL ES, или для игр включите этот интерфейс API, например MonoGame, несмотря на то, что 3D выходит за рамки данной статьи.

## <a name="core-animation"></a>Основной анимации

операций ввода-вывода использует framework Core анимации для создания эффекты анимации, такие как переход между представлениями, скользящий меню и прокрутка эффекты лишь некоторые из них. Для работы с анимации двумя способами.

-  [Через UIKit](#Using_UIKit_Animation), включает представление основано анимации как анимировать переходы между контроллерами.
-   [Через анимацию Core](#Using_Core_Animation), какие слои напрямую, что позволяет более точно управлять.

## <a name="using-uikit-animation"></a>С помощью UIKit анимации

UIKit предоставляет несколько функций, которые упрощают добавление анимации для приложения. Несмотря на то, что внутренним образом использует основной анимации, он абстрагирует его чтобы работать только с помощью представлений и контроллеров.

В этом разделе рассматриваются функции UIKit анимации в том числе:

-  Переходы между контроллерами
-  Переходы между представлениями
-  Просмотр свойств анимации


### <a name="view-controller-transitions"></a>Представление Переходы контроллера

 `UIViewController` предоставляет встроенную поддержку для переноса данных между контроллерами представления через `PresentViewController` метод. При использовании `PresentViewController`, переход на второй контроллер также могут быть анимированы.

Например, рассмотрим приложение с двумя контроллерами, в которых касания кнопки в первом контроллере вызывает `PresentViewController` для отображения второй контроллер. Чтобы контролировать, какие анимации перехода, используемая для отображения второй контроллер, задать его [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) свойства, как показано ниже:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

В этом случае `PartialCurl` используется анимация, несмотря на то, что некоторые другие, доступны, включая:

-  `CoverVertical` — Слайды вверх в нижней части экрана
-  `CrossDissolve` — Исчезание старое представление & отчетливее нового представления
-  `FlipHorizontal` -Отражение горизонтальной справа налево. На увольнения перехода зеркальное отражение слева направо.


Для анимации перехода, передайте `true` как второй аргумент `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

На следующем снимке экрана показано, как выглядит перехода для `PartialCurl` случай:

 ![](core-animation-images/06-view-transitions.png "На этом снимке экрана показано PartialCurl перехода")

### <a name="view-transitions"></a>Представление переходов

В дополнение к переходы между контроллерами UIKit поддерживает анимации переходы между представлениями для замены одного представления для другого.

Например, допустим, имеется контроллер с `UIImageView`, где коснувшись изображения должен отображать секунды `UIImageView`. Для анимации изображение Обзор от представления переход к представлению второй образ достаточно вызвать метод `UIView.Transition`, передавая ему `toView` и `fromView` как показано ниже:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` также принимает `duration` параметр, который определяет продолжительность анимации запуска, а также [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) Чтобы задать такие параметры, такие как анимация и функции для реалистичной анимации. Кроме того можно указать обработчик завершения, который будет вызван после завершения анимации.

Снимок экрана ниже показано анимированных переход между изображение представления, если `TransitionFlipFromTop` используется:

 ![](core-animation-images/07-animated-transition.png "На этом снимке экрана показано анимированных перехода между представлениями изображения при использовании TransitionFlipFromTop")

### <a name="view-property-animations"></a>Анимация свойств представления

Поддерживает UIKit анимации на различных свойств `UIView` класса бесплатно, включая:

-  Frame
-  Границы
-  Центр
-  Коэффициент альфа
-  Transform
-  Цвет


Эти анимации происходит неявно, указав изменения свойств в `NSAction` делегат, переданный статический `UIView.Animate` метод. Например, следующий код выполняет анимацию центральной точки `UIImageView`:

```csharp
pt = imgView.Center;

UIView.Animate (
    duration: 2, 
    delay: 0, 
    options: UIViewAnimationOptions.CurveEaseInOut | 
        UIViewAnimationOptions.Autoreverse,
    animation: () => {
        imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
            - imgView.Frame.Width / 2, pt.Y);},
    completion: () => {
        imgView.Center = pt; }
);
```

Результатом изображения анимации и обратно в верхней части экрана, как показано ниже:

 ![](core-animation-images/08-animate-center.png "Изображение анимации и обратно в верхней части экрана на выходе")

Как и в `Transition` метод `Animate` позволяет длительность задать, а также функции для реалистичной анимации. В этом примере также используется `UIViewAnimationOptions.Autoreverse` параметр, который задает анимации для анимации от значения, в котором начальный. Однако код также задает `Center` обратно в исходное значение в обработчик завершения. Во время анимации интерполяция значения свойств со временем, фактического значения модели свойства всегда является конечное значение, которое было задано. В этом примере значение — точка рядом с правой стороны суперпредставления. Без параметра `Center` до начальной точки, который является где которого анимация завершается из-за `Autoreverse` устанавливается, изображение будет вернутся к справа от оператора после завершения анимации, как показано ниже:

 ![](core-animation-images/09-animation-complete.png "Без установки центра начальной точки, изображение будет вернутся к справа от оператора после завершения анимации")

## <a name="using-core-animation"></a>С помощью основных анимации

 `UIView` анимация разрешить много возможностей и следует по возможности использовать из-за Простота реализации. Как упоминалось ранее, UIView анимации используют платформу Core анимации. Тем не менее, нельзя сделать некоторые вещи с `UIView` анимации, такие как анимация дополнительные свойства, которые невозможно анимировать с представлением или интерполяция нелинейного пути. В таких случаях, где требуется более точное управление Core анимацию можно использовать непосредственно, а также.

### <a name="layers"></a>Слои

При работе с основной анимации, анимация происходит через *слои*, которые имеют тип `CALayer`. Слой аналогичен представления, поскольку уровень иерархии, намного как просмотр иерархии. На самом деле слои резервное представления с представлением, добавляя поддержку взаимодействия с пользователем. Можно получить доступ к слоя любого представления с помощью представления `Layer` свойство. На самом деле контекст используется в `Draw` метод `UIView` фактически создается из уровня. На внутреннем уровне слоя резервное `UIView` имеет своего делегата, задайте режим, который является то, что вызывает `Draw`. Поэтому при рисовании для `UIView`, фактически представляют собой графические ее слой.

Слой анимации может быть явными и неявными. Неявные анимации декларативной. Объявить уровень свойств, которые следует изменить, и анимации просто работает. С другой стороны явную анимации создаются через класс анимации, который добавляется к слою. Явные анимации позволяют управлять добавлением способ создания анимации. Следующие разделы углубимся в явных и неявных анимации в более подробно.

### <a name="implicit-animations"></a>Неявные анимации

Для анимации свойства слоя, установив неявное анимации. `UIView` анимация создавать Неявные анимации. Тем не менее можно создать неявный анимации непосредственно для слоя, а также.

Например, следующий код задает уровень `Contents` из образа, задает ширину границы и цвета, и добавляет слой как уровне слоя представления:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    layer = new CALayer ();
    layer.Bounds = new CGRect (0, 0, 50, 50);
    layer.Position = new CGPoint (50, 50);
    layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
    layer.ContentsGravity = CALayer.GravityResize;
    layer.BorderWidth = 1.5f;
    layer.BorderColor = UIColor.Green.CGColor;

    View.Layer.AddSublayer (layer);
}
```

Добавление неявное анимации для слоя, просто перенести изменения свойств в `CATransaction`. Это позволяет анимации свойств, которые не подлежит анимации с анимацией представления, такие как `BorderWidth` и `BorderColor` как показано ниже:

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    CATransaction.Begin ();
    CATransaction.AnimationDuration = 10;
    layer.Position = new CGPoint (50, 400);
    layer.BorderWidth = 5.0f;
    layer.BorderColor = UIColor.Red.CGColor;
    CATransaction.Commit ();
}
```

Этот код также анимирует слоя `Position`, — расположение точки привязки слоя измеряется от левого верхнего угла superlayer координат. Точки привязки слоя нормализованный точка в системе координат слоя.

На следующем рисунке показана точка положение и привязки:

 ![](core-animation-images/10-postion-anchorpt.png "На этом рисунке показано точки положение и привязки")

При запуске данного примера `Position`, `BorderWidth` и `BorderColor` анимации, как показано на следующем снимке экрана:

 ![](core-animation-images/11-implicit-animation.png "При запуске данного примера положение, BorderWidth и BorderColor анимировать как показано")

### <a name="explicit-animations"></a>Явные анимации

Помимо неявного анимации Core анимация включает широкий набор классов, наследующих от `CAAnimation` , которые позволяют инкапсулировать анимации, которые затем добавляются явным образом в слой. Они позволяют детальный контроль над анимации, такие как изменение начальное значение анимации, группирование анимации и указание опорные кадры разрешены нелинейного пути.

Ниже приведен пример использования явное анимации `CAKeyframeAnimation` для слоя, приведенными выше (в разделе неявное анимации):

```csharp
public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);
    
    // get the initial value to start the animation from
    CGPoint fromPt = layer.Position;
    
    /* set the position to coincide with the final animation value
    to prevent it from snapping back to the starting position
    after the animation completes*/
    layer.Position = new CGPoint (200, 300);
    
    // create a path for the animation to follow
    CGPath path = new CGPath ();
    path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
    
    // create a keyframe animation for the position using the path
    CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
    animPosition.Path = path;
    animPosition.Duration = 2;
    
    // add the animation to the layer.
    /* the "position" key is used to overwrite the implicit animation created
    when the layer positino is set above*/
    layer.AddAnimation (animPosition, "position");
}
```

Этот код изменяет `Position` слоя, создав путь, который затем используется для определения анимации ключевого кадра. Обратите внимание, что слой `Position` равно окончательное значение `Position` из анимации. Без этого слоя будут неожиданно вернется его `Position` перед анимации так, как только изменяется значение презентации и не фактического значения модели анимации. Установив значение модели в последнее значение из анимации, слой сохранятся в конце анимации.

Следующих снимках экрана показано слой, содержащий изображение анимации по указанному пути.

 ![](core-animation-images/12-explicit-animation.png "На этом снимке экрана показано слой, содержащий изображение анимации по указанному пути")
 
## <a name="summary"></a>Сводка

В этой статье мы рассмотрели возможности анимации через *основной анимации* платформ. Мы рассмотрели основные анимации, отображение и как его лежащих в основе анимации в UIKit, и как он может использоваться непосредственно для управления анимации более низкого уровня.

## <a name="related-links"></a>Связанные ссылки

- [Пример основных анимации](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Основные графики](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Графики и анимации Пошаговое руководство](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Основной анимации](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
