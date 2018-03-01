---
title: "Демонстрация жизненного цикла приложения"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 56310bb538d9abf850c40ebfb0b0bf551fbb104c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="application-lifecycle-demo"></a>Демонстрация жизненного цикла приложения

В этом разделе мы будем исследовать приложения, демонстрирующий четыре состояния приложения и роль `AppDelegate` методы уведомления получить изменения состояния приложения. Приложение будет печататься обновления на консоль, каждый раз, когда приложение изменяет состояние:

 [ ![](application-lifecycle-demo-images/image3.png "Пример приложения")](application-lifecycle-demo-images/image3.png)

 [ ![](application-lifecycle-demo-images/image4.png "Приложение будет печататься обновления на консоль, каждый раз, когда приложение изменяет состояние")](application-lifecycle-demo-images/image4.png)

## <a name="walkthrough"></a>Пошаговое руководство


  1. Откройте _жизненного цикла_ проекта в _LifecycleDemo_ решения.
  1. Откройте `AppDelegate` класса. Обратите внимание, что мы добавили ведения журнала для методов жизненного цикла, чтобы сообщить нам, когда приложение изменяет состояние.

            ```chsarp
                public override void OnActivated(UIApplication application)
                {
                    Console.WriteLine("OnActivated called, App is active.");
                }
                public override void WillEnterForeground(UIApplication application)
                {
                    Console.WriteLine("App will enter foreground");
                }
                public override void OnResignActivation(UIApplication application)
                {
                    Console.WriteLine("OnResignActivation called, App moving to inactive state.");
                }
                public override void DidEnterBackground(UIApplication application)
                {
                    Console.WriteLine("App entering background state.");
                }
                // not guaranteed that this will run
                public override void WillTerminate(UIApplication application)
                {
                    Console.WriteLine("App is terminating.");
                }
            ```

  1. Запустите приложение в симуляторе или на устройстве. `OnActivated` будет вызван при запуске приложения. Это приложение сейчас в _Active_ состояния.
  1. Нажмите кнопку "Главная" на симуляторе или устройства, чтобы перевести приложение в фоновом режиме. `OnResignActivation` и `DidEnterBackground` будет вызываться как переходов приложения из `Active` для `Inactive` в `Backgrounded` состояние. Так как мы не указаны наше приложение любой код, выполняемый в фоновом режиме, приложение считается _Suspended_ в памяти.
  1. Перейдите обратно в приложение, чтобы вернуть на переднем плане. `WillEnterForeground` и `OnActivated` оба вызывается:

        ![](application-lifecycle-demo-images/image4.png "Изменения состояния, выводимого на консоль")

    Учтите, что мы следующую строку кода к нашей представлению контроллеру уведомление, что приложение ввел переднего плана в фоновом режиме:

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. Нажмите клавишу **Главная** кнопку, чтобы приложение переведено в фоновом режиме. Затем дважды щелкните **Главная** кнопку, чтобы подключить приложения переключателя:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "Переключателя приложения")
  
1. Найдите приложение в переключателе приложения и проведите вверх, чтобы удалить его.
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "Проведите вверх для удаления выполняемого приложения") 
    
операций ввода-вывода будет завершить приложение. Имейте в виду, что `WillTerminate` не вызван, так как мы прекращение выполняется это приложение, _Suspended_ в фоновом режиме.

Теперь, когда мы понимаем, переходы и состояния приложения iOS, мы ознакомитесь с различных функций, доступных для backgrounding в iOS.



## <a name="related-links"></a>Связанные ссылки

- [LifecycleDemo(Part2) (пример)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
