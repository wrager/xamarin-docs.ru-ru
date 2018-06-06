---
title: Поиск с Spotlight ядра в Xamarin.iOS
description: В этом документе описывается использование основных Spotlight в приложении Xamarin.iOS для предоставления ссылок на содержимое в приложении. Он описывается создание, восстановление, обновление и удаление элементов с возможностью поиска.
ms.prod: xamarin
ms.assetid: 1374914C-0F63-41BF-BD97-EBCEE86E57B1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: a8bc3aaa43d7830b0a3baa0768d495458b1ecfad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788043"
---
# <a name="search-with-core-spotlight-in-xamarinios"></a>Поиск с Spotlight ядра в Xamarin.iOS

Обзор основных компонентов — это новая платформа предоставляет API базы данных, чтобы добавить, изменить или удалить ссылки на содержимое в приложении IOS 9. Элементы, которые были добавлены с помощью основных Spotlight будут доступны в поиску Spotlight на устройстве iOS.

Пример типы содержимого, которые могут быть индексированы с помощью Core Spotlight, просмотрите сообщения, почта, календарь и примечания Apple приложений. В настоящее время, то все они используют Core Spotlight для получения результатов поиска.

## <a name="creating-an-item"></a>Создание элемента

Ниже приведен пример создания элемента и его с помощью основных Spotlight индексирования:

```csharp
using CoreSpotlight;
...

// Create attributes to describe an item
var attributes = new CSSearchableItemAttributeSet();
attributes.Title = "App Center Test";
attributes.ContentDescription = "Automatically test your app on 1,000 devices in the cloud.";

// Create item
var item = new CSSearchableItem ("1", "products", attributes);

// Index item
CSSearchableIndex.DefaultSearchableIndex.Index (new CSSearchableItem[]{ item }, (error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Эта информация будет выглядеть таким образом следующие действия в результате поиска:

[![](corespotlight-images/corespotlight01.png "Обзор результатов поиска Spotlight Core")](corespotlight-images/corespotlight01.png#lightbox)

## <a name="restoring-an-item"></a>При восстановлении файла

Когда пользователь нажимает на элемент, добавленный к результатам поиска через Spotlight основных компонентов для приложения, `AppDelegate` метод `ContinueUserActivity` вызывается (этот метод также используется для `NSUserActivity`). Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application,
   NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchableItem.ActionType.ToString ()) {
            // Display content for searchable item...
        }
        break;
    }

    return true;
}
```

Обратите внимание, что это время мы проверку наличия действия `ActivityType` из `CSSearchableItem.ActionType`.

## <a name="updating-an-item"></a>Обновление элемента

Возможны ситуации, при мы создали с Spotlight основной элемент индекса должны быть изменены, такие как изменение в заголовок или эскиз является обязательным. Чтобы внести это изменение, мы используем тот же метод, которое использовалось в первоначальном создании индекса.
Мы создаем новый `CSSearchableItem` используя тот же идентификатор, которое использовалось для создания элемента и присоединить новый `CSSearchableItemAttributeSet` содержащий измененных атрибутов:

[![](corespotlight-images/corespotlight02.png "Обновление элемента Обзор")](corespotlight-images/corespotlight02.png#lightbox)

При записи этого элемента для поиска индекса, существующий элемент обновляется новыми данными.

## <a name="deleting-an-item"></a>Удаление элемента

Core Spotlight предоставляет несколько способов для удаления индекса элемента, если он больше не требуется.

Во-первых можно удалить элемент по его идентификатору, например:

```csharp
// Delete Items by ID
CSSearchableIndex.DefaultSearchableIndex.Delete(new string[]{"1","16"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

После этого можно удалить группу элементов индекса по имени домена. Пример:

```csharp
// Delete by Domain Name
CSSearchableIndex.DefaultSearchableIndex.DeleteWithDomain(new string[]{"domain-name"},(error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```

Наконец можно удалить все элементы индекса с помощью следующего кода:

```csharp
// Delete all index items
CSSearchableIndex.DefaultSearchableIndex.DeleteAll((error) => {
    // Successful?
    if (error !=null) {
        Console.WriteLine(error.LocalizedDescription);
    }
});
```
## <a name="additional-core-spotlight-features"></a>Дополнительные основные возможности Spotlight

Обзор основных компонентов имеет следующие возможности, которые помогают обеспечить индекс точную и актуальную:

- **Пакетное обновление поддержки** — Если ваше приложение должно создать или изменить большое количество индексов в то же время, всего пакета может быть послана `Index` метод `CSSearchableIndex` класс в одном вызове.
- **Ответ на изменения индекса** — при использовании `CSSearchableIndexDelegate` приложение может реагировать на изменения и уведомления из индекса для поиска.
- **Применение защиты данных** — с помощью классов защиты данных, можно реализовать безопасности для элементов, которые позволяют с возможностью поиска индекса с помощью Core Spotlight.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Руководство по программированию для поиска приложения](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
