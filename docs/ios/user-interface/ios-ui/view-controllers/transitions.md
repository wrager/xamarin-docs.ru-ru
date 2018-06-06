---
title: Представление переходов контроллера в Xamarin.iOS
description: В этом документе описывается настройка анимировать переходы между Просмотр контроллеров в Xamarin.iOS приложениях.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 35795002310cd79a1897061fe6e3e41b48b45b4d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790452"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Представление переходов контроллера в Xamarin.iOS

UIKit добавляет поддержку для настройки анимированных перехода, возникающее при представлении Просмотр контроллеров. Эта поддержка входит в состав встроенных контроллеров, а также любые пользовательские контроллеров-наследников непосредственно `UIViewController`. Кроме того `UICollectionViewController` использует преимущества настройки переход контроллера для использования анимации в макетах представления коллекции.

## <a name="custom-transitions"></a>Пользовательские переходов

Анимированный переход между Просмотр контроллеров в iOS 7 можно настраивать полностью. `UIViewController` Теперь включает `TransitioningDelegate` свойство, которое предоставляет класс пользовательского animator в систему, когда происходит переход.

Чтобы использовать пользовательский переход с `PresentViewController`:

1.  Задать `ModalPresentationStyle` для `UIModalPresentationStyle.Custom` на контроллере, должна быть предоставлена.
2.  Реализуйте `UIViewControllerTransitioningDelegate` создание класса animator, который является экземпляром класса `UIViewControllerAnimatedTransitioning` .
3.  Задать `TransitioningDelegate` свойства к экземпляру `UIViewControllerTransitioningDelegate` , также на контроллере, должна быть предоставлена.
4.  Представляет представление-контроллер.


Например, следующий код демонстрирует контроллер представление типа `ControllerTwo` - `UIViewController` подкласс:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Запуск приложения и коснитесь кнопки в результате анимации по умолчанию представления второй контроллер для анимации в нижней части, как показано ниже:

 ![](transitions-images/no-custom-transition.png "Запуск приложения и коснитесь кнопки приводит анимации по умолчанию второй представления контроллеров для анимации в снизу")

Однако задание `ModalPresentationStyle` и `TransitioningDelegate` приводит к пользовательской анимации для перехода:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

`TransitioningDelegate` Отвечает за создание экземпляра `UIViewControllerAnimatedTransitioning` подкласс - вызывается `CustomAnimator` в следующем примере:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning GetAnimationControllerForPresentedController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Если происходит переход, система создает экземпляр `IUIViewControllerContextTransitioning`, которой оно указано в animator методы. `IUIViewControllerContextTransitioning` содержит `ContainerView` где происходит анимации, а также инициирует переход контроллера представление и контроллер представление, в которое осуществляется переход.

`UIViewControllerAnimatedTransitioning` Класс обрабатывает фактическое анимации. Необходимо реализовать два метода:

1.  `TransitionDuration` — Возвращает продолжительность анимации в секундах.
1.  `AnimateTransition` — выполняет фактическое анимации.


Например, следующий класс реализует `UIViewControllerAnimatedTransitioning` для анимации кадра контроллера представления:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
        {
            var inView = transitionContext.ContainerView;
            var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
            var toView = toVC.View;

            inView.AddSubview (toView);

            var frame = toView.Frame;
            toView.Frame = CGRect.Empty;

            UIView.Animate (TransitionDuration (transitionContext), () => {
                toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
            }, () => {
                transitionContext.CompleteTransition (true);
            });
        }
}
```

Теперь при касании кнопки, анимация реализации в `UIViewControllerAnimatedTransitioning` класс используется:

 ![](transitions-images/custom-transition.png "Примером масштаба фактически под управлением")

## <a name="collection-view-transitions"></a>Переходы представление коллекции

Представления коллекций имеют встроенную поддержку для создания анимации:

-  **Контроллеры навигации** — анимировать переход между двумя `UICollectionViewController` экземпляры можно дополнительно обработать автоматически при `UINavigationController` управляет ими.
-  **Переход макета** — новый `UICollectionViewTransitionLayout` класс позволяет интерактивный переход между макетами.


### <a name="navigation-controller-transitions"></a>Переходы контроллера навигации

При использовании в контроллере навигации `UICollectionViewController` включает поддержку анимировать переходы между контроллерами. Эта поддержка встроена и требуется только несколько простых шагов для реализации:

1.  Задать `UseLayoutToLayoutNavigationTransitions` для `false` на `UICollectionViewController` .
1.  Добавить экземпляр `UICollectionViewController` корень стека контроллера навигации.
1.  Создайте вторую `UICollectionViewController` и задайте его `UseLayoutToLayoutNavigtionTransitions` свойства `true` .
1.  Принудительная второй `UICollectionViewController` стек навигации контроллера.


Следующий код добавляет `UICollectionViewController` подкласс с именем `ImagesCollectionViewController` корень стека контроллер навигации с `UseLayoutToLayoutNavigationTransitions` свойство `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Если выбран элемент, второй экземпляр `ImagesController` создается только в этот раз с помощью класса другой макет. Для этого контроллера `UseLayoutToLayoutNavigtionTransitions` равно `true`, как показано ниже:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
        circleLayout = new CircleLayout (Monkeys.Instance.Count){
                ItemSize = new CGSize (100, 100)
            };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

`UseLayoutToLayoutNavigationTransitions` Свойство должно быть задано до добавления контроллера в стеке навигации. С этим набором свойств обычный горизонтальной перехода со скользящим заменяется анимированных переход между макетами двух контроллеров как показано ниже:

![](transitions-images/nav2.png "Анимированный переход между макетами двух контроллеров")

### <a name="transition-layout"></a>Макет перехода

Помимо поддержки перехода макета в навигации контроллеров, вызывается новый макет `UICollectionViewTransitionLayout` теперь доступен. Этот класс макет позволяет интерактивного управления в процессе перехода макета, позволяя `TransitionProgress` для задания из кода. `UICollectionViewTransitionLayout` отличается от - и не является заменой - `SetCollectionViewLayout` метод iOS 6, которая вызвала переход анимированных макета. Этот метод не было указано встроенную поддержку для управления хода выполнения анимированных перехода.

 `UICollectionViewTransitionLayout` Например, позволяет распознаватель жестов настроена для перехода между макетами в ответ на взаимодействие с пользователем, управляя исходного макета, а также предполагаемого макета переход управления.

Шаги по реализации интерактивный перехода в с помощью распознавателя `UICollectionViewTransitionLayout` таковы:

1.  Создайте средство распознавания жестов.
1.  Вызовите `StartInteractiveTransition` метод `UICollectionView` , целевой макета и обработчик завершения.
1.  Задать `TransitionProgress` свойство `UICollectionViewTransitionLayout` экземпляр, возвращаемый из `StartInteractiveTransition` метод.
1.  К недействительности структуры.
1.  Вызовите `FinishInteractiveTransition` метод `UICollectionView` для завершения перехода или `CancelInteractiveTransition` метод отменить его.  `FinishInteractiveTransition` вызывает анимации для завершения перехода к макету целевой, тогда как `CancelInteractiveTransition` приводит к анимации, возвращая первоначальный вид.
1.  Завершал перехода в обработчике завершения `StartInteractiveTransition` метод.
1.  Добавьте средство распознавания жестов в представлении коллекции.


Следующий код реализует интерактивного макета перехода в распознаватель жестов жестом сжатия:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Как пользователь pinches представление коллекции `TransitionProgress` задать относительно масштаба жестом сжатия. В этой реализации Если пользователь завершает сжатие до перехода на 50% завершено, то переход отменяется. В противном случае завершения перехода.




## <a name="related-links"></a>Связанные ссылки

- [Введение в iOS 7 (пример)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Обзор пользовательского интерфейса iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Фоновая обработка](~/ios/app-fundamentals/backgrounding/index.md)
