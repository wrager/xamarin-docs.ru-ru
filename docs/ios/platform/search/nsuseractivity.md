---
title: Поиск с NSUserActivity в Xamarin.iOS
description: В этом документе описывается индекса NSUserActivity, сделав его доступным для поиска Spotlight и Safari. В этом примере рассматривается реакция на выбор NSUserActivity в результатах поиска.
ms.prod: xamarin
ms.assetid: 0B28B284-C7C9-4C0D-A782-D471FBBC4CAE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 4b053f66e9b6b7715cbe52c4e43d9db32db48f4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788212"
---
# <a name="search-with-nsuseractivity-in-xamarinios"></a>Поиск с NSUserActivity в Xamarin.iOS

`NSUserActivity` была введена в iOS 8 и используется для предоставления данных для обработки.
Он позволяет создавать действия в определенные части приложения, которое затем можно передать из системы на другой экземпляр своего приложения, работающего на устройстве различных операций ввода-вывода. Принимающее устройство, затем можно продолжить действие запускается на устройстве предыдущего, комплектовать справа, где остановились пользователя. Дополнительные сведения об использовании перемещение вручную см. в разделе нашей [Общие сведения о переадресации](~/ios/platform/handoff.md) документации.

Опыта работы с iOS 9, `NSUserActivity` можно индексировать (открытого и закрытого) и поиске с поиску Spotlight и Safari. Пометив `NSUserActivity` как поиск и добавление метаданных индексируемых действия могут быть перечислены в результатах поиска на устройстве iOS.

[![](nsuseractivity-images/apphistory01.png "Обзор журнала приложений")](nsuseractivity-images/apphistory01.png#lightbox)

Если пользователь выбирает результат поиска, которому принадлежит действие из приложения, приложение будет запущено и описаны действия `NSUserActivity` будет перезапущена и для пользователя.

Следующие свойства `NSUserActivity` используются для поддержки поиска приложения:

 - `EligibleForHandoff` — Если `true`, это действие можно использовать в операции передачи.
 - `EligibleForSearch` — Если `true`, это действие будет добавлен к индексу на устройстве и представляются в результатах поиска.
 - `EligibleForPublicIndexing` — Если `true`, это действие будет добавлен индекс на основе облака Apple и пользователям (с помощью поиска), которые еще не установили приложение на устройстве iOS. В разделе [открытый индексирования поиска](#Public-Search-Indexing) ниже, в разделе для получения дополнительных сведений.
 - `Title` — С заголовком ваши действия и отображается в результатах поиска. Пользователи могут выполнять поиск текст заголовка, сам.
 - `Keywords` — Представляет собой массив строк, используемых для описания действие, индексировать и сделать доступным для поиска конечным пользователем.
 - `ContentAttributeSet` — `CSSearchableItemAttributeSet` Дополнительные действия подробно описаны и предоставляют более информативного содержимого в результатах поиска.
 - `ExpirationDate` — Если требуется, чтобы действие будет отображаться только для данной даты, чтобы обеспечить здесь дату.
 - `WebpageURL` — Если действия можно просматривать в Интернете или если приложение поддерживает прямые ссылки Safari, можно задать ссылку на веб-странице.

## <a name="nsuseractivity-quickstart"></a>Краткое руководство NSUserActivity

Следуйте этим инструкциям, чтобы реализовать для поиска `NSUserActivity` в приложении:

- [Создание типа идентификаторы действий](#creatingtypeid)
- [Создание действия](#createactivity)
- [Отвечать на действия](#respondactivity)
- [Индексирование открытый поиска](#indexing)

Некоторые [дополнительные преимущества](#benefits) с помощью `NSUserActivity` чтобы сделать содержимое доступным для поиска.

<a name="creatingtypeid" />

## <a name="creating-activity-type-identifiers"></a>Создание типа идентификаторы действий

Перед созданием действия поиска, необходимо будет создать _идентификатор типа действия_ для его идентификации. Идентификатор типа действия — это короткая строка, добавляемый `NSUserActivityTypes` массив приложения **Info.plist** файл, используемый для идентификации данного типа действий пользователя. В массиве для каждого действия, которая поддерживает приложения и предоставляет поиска приложения будет существовать одна запись. 

Apple предлагает, использовать обратное DNS-нотацию для идентификатора типа действия для предотвращения конфликтов. Например: `com.company-name.appname.activity` для конкретного приложения на основе действий или `com.company-name.activity` для действий, которые могут выполняться в нескольких приложениях.

Идентификатор действия используется при создании `NSUserActivity` экземпляр для определения типа действия. После возобновления действия пользователя, коснувшись результат поиска в результате тип действия (а также идентификатор команды приложения) определяет, какое приложение для запуска, чтобы продолжить действия.

Чтобы создать необходимые идентификаторы типа действий для поддержки данной возможности, измените **Info.plist** файла и переключитесь в **источника** представления. Добавить `NSUserActivityTypes` раздел и создавать идентификаторы в следующем формате:

[![](nsuseractivity-images/type01.png "Ключ NSUserActivityTypes и необходимые идентификаторы в редакторе plist")](nsuseractivity-images/type01.png#lightbox)

В приведенном выше примере мы создали один новый идентификатор типа действия для действия "Поиск" (`com.xamarin.platform`). При создании собственных приложений, замените содержимое `NSUserActivityTypes` массива с идентификаторами тип действия, относящиеся к действия приложения поддерживает.

<a name="createactivity" />

## <a name="creating-an-activity"></a>Создание действия

Ниже приведен пример создания действия для поиска размещенных индекса на устройстве:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.BecomeCurrent();
```

Нам удалось Добавление дополнительных сведений, задав `ContentAttributeSet` свойство нашей `NSUserActivity` следующим образом:

[![](nsuseractivity-images/apphistory02.png "Обзор поиска подробностей сложения")](nsuseractivity-images/apphistory02.png#lightbox)

С помощью `ContentAttributeSet` можно создать результаты широкие возможности поиска, склонить конечным пользователям взаимодействовать с ними.

<a name="respondactivity" />

## <a name="responding-to-an-activity"></a>Отвечать на действия

Ответ на пользователя, нажав на результат поиска (`NSUserActivity`) нашего приложения изменить **AppDelegate.cs** файла и Переопределите `ContinueUserActivity` метод. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Обратите внимание, что это же переопределяющий метод, используемый для ответа на запросы перемещение вручную. Теперь при нажатии на ссылку из нашего приложения в результатах поиска Spotlight нашего приложения будет выведен на передний план (или запуска, если она еще не работает) и будет отображаться содержимое, навигации или компонента, представленного этой ссылки:

[![](nsuseractivity-images/apphistory03.png "Восстановите предыдущее состояние из поиска")](nsuseractivity-images/apphistory03.png#lightbox)

<a name="indexing" />

## <a name="public-search-indexing"></a>Индексирование открытый поиска

Как было показано выше, упрощает позволяют осуществлять поиск содержимого приложения и функции, которые пользователь уже обнаружен и используется на устройстве iOS, заданного с помощью iOS 9. При использовании открытого индексирования iOS 9 предоставляет способ для пользователей, еще не обнаружены содержимого или средств еще (или, даже еще не установили приложение) для получения этих результатов в их поиске слишком.

Открытые индексирования работает следующим образом:

1. При создании действия для приложения можно отметить его как public.
2. Открытые действия отправляются в Apple и индексировать в облаке.
3. Дополнительные пользователи взаимодействовать с приложением на устройстве и использовать определенные возможности или содержимого, представленный этим действием, возрастает в рангом.
4. Популярные открытый результаты будут доступны другим пользователям, даже если они не имеют установленное приложение.
5. Эти открытые результаты будут отображаться в поиску Spotlight и Safari (если действие содержит URL-адрес).

Мы занять закрытый поиска действия, созданным ранее и развернуть его как public:

```csharp
// Create App Search Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Add App Search ability
activity.EligibleForSearch = true;
activity.EligibleForPublicIndexing = true;
activity.BecomeCurrent();
```

Только потому, что действие задало для открытого индексирования, задав `EligibleForPublicIndexing = true`, это не значит, что он автоматически добавляется к индексу общедоступном облаке компании Apple. Сначала должны быть выполнены следующие условия:

1. Он должен отображаться в результатах поиска и выбрать несколькими пользователями. Результаты остаются личными пока не выполнены engagement пороговое значение действия.
2. Подготовка приложений запрещает любые пользовательские данные индексированного и общедоступным.

<a name="benefits" />

## <a name="additional-benefits"></a>Дополнительные преимущества

Принимая поиска приложения через `NSUserActivity` в приложение, вы получаете следующие возможности:

- **Перемещение вручную** — поскольку поиска приложения предоставление содержимого, навигации и/или функции с использованием того же механизма как перемещение вручную (`NSUserActivity`), вы можете легко предоставить пользователям приложения для запуска действия на одном устройстве и продолжается на другой.
- **Предложения Siri** -вместе с стандартные рекомендации, обычно делает предложения Siri, actives из приложения можно предложенные автоматически.
- **Смарт-напоминания Siri** -пользователи смогут запрашивать Siri Напомните им о действиях в приложении.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Руководство по программированию для поиска приложения](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
- [Запись блога & ку](https://blog.xamarin.com/improve-discoverability-with-search-in-ios-9/)
