---
title: "Запрос проверки приложения"
description: "В этой статье описан метод RequestReview, Apple iOS 10 и его применение в Xamarin.iOS добавлены."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 469a63a990b1adb108284cfb88ee54e05218a8a9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="request-app-review"></a>Запрос проверки приложения

_В этой статье описан метод RequestReview, Apple iOS 10 и его применение в Xamarin.iOS добавлены._

Опыта работы с iOS 10.3 `RequestReview()` метод позволяет приложению iOS запрашивать у пользователя, чтобы оценить и проверьте его. При вызове этого метода в приложении доставки, которое пользователь установил в магазине приложений iOS 10 будет обработки всего оценок и процесс анализа для разработчиков. Так как этот процесс регулируется политикой магазина, оповещение может или могут не отображаться.

![](request-app-review-images/review01.png "Пример оповещения запрос на проверку")

## <a name="requesting-a-rating-or-review"></a>Запрашивает оценку или проверки

Хотя `RequestReview()` статический метод `SKStoreReviewController` класса может быть вызван в любой точке, где это имеет смысл во взаимодействие с пользователем, процесс проверки управляло и обрабатываются политикой магазина. В результате этот метод может или не может отображать оповещение и никогда не должен вызываться в ответ на действия пользователя, например, коснувшись кнопки.

Например приложение может запросить проверку после ее запуска заданное число раз или игра может запросить проверку проигрыватель по завершении уровень.

Для запросов на проверку сразу же после завершения работы приложения Xamarin.iOS запуск, внесите следующие изменения для `AppDelegate.cs` файла:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }
        
        ...
        
    }
}
```

> [!NOTE]
> **Примечание:** вызов `RequestReview()` перевыборки разработки приложения всегда отображает оценку и просматривать диалоговое окно, чтобы его можно протестировать. Это не относится к приложениям, которые были переданы через TestFlight, где будут игнорироваться вызова метода.

Когда `RequestReview()` метод вызывается в приложении доставки, которое пользователь установил в магазине приложений, операций ввода-вывода 10 будет обрабатывать весь процесс оценки и проверки для разработчика. Еще раз так как этот процесс регулируется политикой магазина, оповещение может или могут не отображаться.

## <a name="linking-to-an-app-store-product-page"></a>Связывание на страницу продукта App Store 

Дополнение к новым `RequestReview` , разработчик может по-прежнему предоставить метод прямую ссылку на страницу продукта приложения в магазине приложений в приложении. Путем добавления `action=write-review` в конец URL-адрес страницы продукта страница открывается которой пользователь может написать отзыв приложения автоматически. 

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован метод RequestReview, Apple iOS 10 и его применение в Xamarin.iOS добавлены.



## <a name="related-links"></a>Связанные ссылки

- [Образец iOSTenThree](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
