---
title: Поиск с веб-разметка
description: Добавление результатов поиска веб сервера, которые можно связать приложения.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: bc3446419ef0e469f7184d60fe8876cd2e5da520
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="search-with-web-markup"></a>Поиск с веб-разметка

Для приложений, которые предоставляют доступ к своему содержимому через веб-сайта (не только в приложении), веб-содержимого может быть помечена специальные ссылки, которые будут просматриваться Apple и предоставляют глубокое связывание приложение на устройства iOS 9.

Если ваше приложение iOS уже поддерживает мобильные глубокое связывание и веб-сайте представлены прямые ссылки на содержимое в приложения, Apple _Applebot_ программа-обходчик будет индексировать это содержимое и автоматическое добавление индексов облака:

[![](web-markup-images/webmarkup01.png "Обзор облачных индекса")](web-markup-images/webmarkup01.png#lightbox)

Apple обнаружатся эти результаты в результатах поиска Spotlight и Safari поиска.
Если пользователь нажимает на одну из этих результатов (и у них есть приложение установлено), то они будут выполнены для содержимого в приложении:

[![](web-markup-images/webmarkup02.png "Deep, связывание с веб-сайта в результатах поиска")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Включение индексирования содержимого Web

Существует четыре действия, позволяющие включить содержимое приложения доступным для поиска веб-разметка.

1. Убедитесь, что Apple можно обнаруживать и индекса путем определения ее в качестве веб-сайта приложения **поддержки** или **маркетинга** веб-сайта в iTunes Connect.
2. Убедитесь, что ваше приложение веб-сайт содержит разметки, необходимые для реализации мобильных глубокое связывание. См. следующие разделы содержат дополнительные сведения.
3. Включите прямая ссылка на обработку в приложении iOS.
4. Добавление разметки для структурированных данных, отображаются в веб-сайтом приложения для предоставления подробный и привлечение результат конечному пользователю. Хотя этот шаг не является обязательным, рекомендуется компанией Apple.

Эти шаги подробно будут рассмотрены ниже.

## <a name="make-your-apps-website-discoverable"></a>Сделать доступной для обнаружения приложения веб-сайта

Чтобы найти приложения веб-сайте Apple проще всего использовать в качестве либо **поддержки** или **маркетинга** веб-сайта, при отправке ваше приложение в Apple с помощью iTunes Connect.

## <a name="using-smart-app-banners"></a>С помощью смарт-приложения ленты

Укажите смарт-заголовок приложения на веб-сайте для представления очистить ссылку в веб-приложения. Если приложение еще не установлена, Safari автоматически предложит пользователю установить приложение. В противном случае можно коснуться использование **представление** ссылка для запуска приложения на веб-сайте. Например для создания смарт-заголовок приложения, можно использовать следующий код:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Дополнительные сведения см. в разделе Apple [повышение уровня приложений с помощью интеллектуальный плакаты приложения](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) документации.

## <a name="using-universal-links"></a>С помощью универсальной ссылки

Новые для iOS 9 универсальной ссылки рекомендуется использовать интеллектуальный плакаты приложения или существующие пользовательские схемы URL-адрес, указав следующее:

- **Уникальный** — же URL-адрес не может занять более одного веб-сайта.
- **Защита** — подписанный сертификат является обязательным для связанных веб-сайт, обеспечивающий веб-сайта принадлежащее вам надлежащим образом для приложения.
- **Гибкий** — конечный пользователь может управлять запускает ли URL-адрес веб-сайта или приложения.
- **Универсальные** — же URL-адрес может использоваться для определения содержимого веб-сайта и приложения.

## <a name="using-twitter-cards"></a>Использование карт Twitter

Чтобы обеспечить прямые ссылки на содержимое своего приложения с использованием Twitter карты. Пример:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Дополнительные сведения см. в разделе в Twitter [Twitter протокола карты](http://dev.twitter.com/cards/mobile) документации.

## <a name="using-facebook-app-links"></a>С помощью связей приложения Facebook

Чтобы обеспечить прямые ссылки на содержимое своего приложения с помощью ссылки приложения Facebook. Пример:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Дополнительные сведения см. в разделе Facebook [ссылки приложения](http://applinks.org) документации.

## <a name="opening-deep-links"></a>Открытие прямые ссылки

Необходимо добавить поддержку для открытия и отображение прямые ссылки в приложении Xamarin.iOS. Изменить **AppDelegate.cs** файла и Переопределите `OpenURL` метод для обработки пользовательского формата URL-адрес. Пример:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

    // Handling a URL in the form http://company.com/appname/?123
    try {
        var components = new NSUrlComponents(url,true);
        var path = components.Path;
        var query = components.Query;

        // Is this a known format?
        if (path == "/appname") {
            // Display the view controller for the content
            // specified in query (123)
            return ContentViewController.LoadContent(query);
        }
    } catch {
        // Ignore issue for now
    }

    return false;
}
```

В приведенном выше коде мы ищем URL-адрес, содержащий `/appname` и передав значение `query` (`123` в этом примере) к контроллеру пользовательское представление в нашем приложении для отображения запрошенного содержимого.

## <a name="providing-rich-results-with-structured-data"></a>Предоставляя результаты Rich со структурированными данными

Включая разметку структурированных данных можно предоставить широкие возможности поиска для конечного пользователя, которые выходят за пределы просто название и описание. Включать изображения, данные конкретного приложения (такие как оценки) и действий для результатов с помощью разметки структурированных данных.

Rich результаты более приятной и могут помочь повысить оценкой в облаке на основе индекса поиска обманом дополнительные пользователям взаимодействовать с ними.

Один из вариантов для предоставления разметки структурированных данных — с помощью Open Graph. Пример:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Дополнительные сведения см. в разделе [Open Graph](http://ogp.me) веб-сайта.

Другой распространенный формат для структурированных данных разметки — формат Микроданные schema.org. Пример:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Те же данные может быть представлен в формате JSON-LD schema.org элемента.

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Ниже показан пример метаданные с веб-сайта, предоставляя широкие возможности поиска результаты конечному пользователю:

[![](web-markup-images/deeplink01.png "Полнофункциональный поиск результатов посредством разметки структурированных данных")](web-markup-images/deeplink01.png#lightbox)

Apple в настоящее время поддерживает следующие типы схем из schema.org:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Предложения
 - Организации
 - PriceRange
 - Верный путь
 - SearchAction

Дополнительные сведения об этих типах схемы см. в разделе [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Предоставление действий с структурированных данных

Конкретные типы структурированных данных позволит результат поиска, чтобы быть пригодных к использованию конечным пользователем. В настоящее время поддерживаются следующие действия:

 - Набора номера телефона.
 - Получение направление карты по указанному адресу.
 - Воспроизведение аудио-или видео.

Например определение действий, чтобы набрать номер телефона может выглядеть следующим образом:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Когда этот результат поиска предоставляется для конечного пользователя, в результате будет отображаться значок небольших телефона. Если пользователь касается значка, будет вызван числу, заданному.

Следующий код HTML добавить действия для воспроизведения звукового файла в результатах поиска:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Наконец следующий код HTML добавить действие для получения направлениях из результатов поиска:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Дополнительные сведения см. в разделе Apple [сайта разработчика приложения поиска](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Руководство по программированию для поиска приложения](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
