---
title: "Усовершенствования приложения поиска"
description: "В этой статье рассматриваются усовершенствования Apple, внесенных в iOS 10 и способы их реализации в Xamarin.iOS для поиска приложения."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 95f7ad5069abfe4dff82659c0fbc79eef2125e15
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="app-search-enhancements"></a>Усовершенствования приложения поиска

_В этой статье рассматриваются усовершенствования Apple, внесенных в iOS 10 и способы их реализации в Xamarin.iOS для поиска приложения._

В iOS 10 Apple представляет собой ряд дополнительных возможностей для поиска приложения, например сложного связывания Crowdsourced, поиск в приложении, продолжение поиска и визуализации результаты проверки. В этой статье мы рассмотрим применение этих функций в приложения Xamarin.iOS.

## <a name="about-app-search-enhancements"></a>Об улучшениях приложения поиска

Основные отличительные черты в iOS 10 предоставляет ряд дополнительных возможностей для поиска приложения такие как:

- **Популярности прямая ссылка Crowdsourced (с разностной конфиденциальности)** -предоставляет способ распространения содержимого прямая ссылка на приложение в результатах поиска.
- **Поиск в приложении** -использовать новое `CSSearchQuery` класса для предоставления возможности поиска Spotlight в приложении аналогично работе приложения почта, сообщения и заметки.
- **Продолжение поиска** — позволяет пользователю начать поиск в Spotlight или Safari, а затем откройте приложение и продолжения поиска.
- **Представление результатов проверки** -Apple [средством проверки API приложения поиска](https://search.developer.apple.com/appsearch-validation-tool) теперь отображается визуальное представление разметки веб-сайт и сложного связывания при preforming тестов.
- **Сообщение образа приложения для управления доступом** -позволяет популярных в приложении образы, предоставленные для совместного использования в сообщениях (через расширение приложения сообщение) появляются в поиске Spotlight.

Следующих разделах будут рассмотрены в следующих разделах более подробно.

## <a name="crowdsourced-deep-link-popularity"></a>Прямая ссылка Crowdsourced популярности

iOS 10 предоставляет механизм для подсчета частоты, часто используемых сложного ссылок в приложение, следуют пользователя и использует эти сведения для улучшения рейтинг приложения содержимое элемента в результатах поиска, обеспечивая защиту удостоверение пользователя с помощью  *Разностное конфиденциальности*.

Для приложений, использующих `NSUserActivity` объектов для предоставления прямая ссылка URL-адреса и иметь `EligibleForPublicIndexing` свойство `true`, iOS 10 отправляет подмножество *разностной хэши конфиденциальности* на серверах компании Apple. Эта информация затем используется для повышения уровня популярных содержимое в приложении, в результатах поиска.

Дополнительные сведения о реализации сложного связывания в приложении Xamarin.iOS, см. в разделе нашей [поиска с NSUserActivity](~/ios/platform/search/nsuseractivity.md) документации.

## <a name="in-app-searching"></a>Поиск в приложении

Путем реализации нового [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) класс, приложение может предоставлять поиска Spotlight и сопоставления технологии правило для поиска содержимого внутри себя, без участия пользователя оставить приложения (аналогично тому, как почта, сообщения и примечания приложение Работа).

Как правило, приложения, поддерживающие `CSSearchQuery` не требуется поддерживать собственные, зависит от ваших индекс. 

## <a name="search-continuation"></a>Продолжение поиска

В iOS 9 Apple реализовала API поиска (например Core Spotlight `NSUserActivity` и веб-разметка) для предоставления сложного оформлением содержимого в приложения, чтобы предоставить пользователям возможность поиска для этого содержимого, с помощью интерфейсов поиска Spotlight и Safari. См. наш [новые интерфейсы API поиска](~/ios/platform/search/index.md) документации для получения дополнительных сведений.

В iOS 10 Apple построен на эту функцию, чтобы пользователи могли начать поиск в Spotlight или Safari, а затем продолжить поиск при открытии приложения. 

Для реализации этой возможности, измените приложения `Info.plist` файл, добавьте `CoreSpotlightContinuation` ключа типа **логическое** и присвойте ему значение `YES`:

[[ide name="xs]]

[ ![](app-search-enhancements-images/search01.png "Редактирование CoreSpotlightContinuation в файле Info.plist")](app-search-enhancements-images/search01.png)

[[/ide]]

[[ide name="vs]]

[ ![](app-search-enhancements-images/searchw01.png "Редактирование CoreSpotlightContinuation в файле Info.plist")](app-search-enhancements-images/search01.png)

[[/ide]]

Ответ на пользователя, продолжение результат поиска (`NSUserActivity`), изменить `AppDelegate.cs` файла и переопределить `ContinueUserActivity` метод. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Этот код осуществляет поиск тип действия для продолжения запроса (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), затем считывает текущий запрос пользователя из `NSUserActivity` класса пользовательского словаря info (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Здесь приложение необходимо предпринимать действия для продолжения поиска пользователя.

Дополнительные сведения о работе с производит поиск в приложения Xamarin.iOS см. в разделе нашей [поиска Core Spotlight](~/ios/platform/search/corespotlight.md) документации.

## <a name="visualization-of-validation-results"></a>Представление результатов проверки

Apple [средством проверки API поиска приложения](https://search.developer.apple.com/appsearch-validation-tool) теперь отображается визуальное представление разметки и сложного связывания веб-сайта (включая разметки, таких как определенные в [Schema.org](http://schema.org/)) при preforming тестов.

С помощью средства проверки, разработчик может видеть сведения, что программа-обходчик Applebot имеет индексированные для сайта, такие как название, описание, URL-адрес и любых других поддерживаемых элементов.

Дополнительные сведения о работе с веб-разметка, см. в разделе нашей [проведите поиск с веб-разметка](~/ios/platform/search/web-markup.md) документации.

## <a name="message-app-image-sharing"></a>Изображение сообщения приложения для управления доступом

Если расширение приложения сообщение предоставляет изображения для совместного использования в сообщениях, дающим пользователю для поиска Spotlight популярных рисунков в приложении эти сообщения, не покидая приложение можно настроить расширение.

Чтобы включить эту функцию, выполните следующие действия.

1. Создайте расширение сообщений приложения.
2. Добавить `com.apple.developer.associated-domains` правах приложения и включают список веб-доменах, на которых размещены образы, расширение сообщения приложения для управления доступом. Для каждого домена, укажите `spotlight-image-search` службы.
3. Добавить `apple-app-site-association` файл на веб-сайт, на котором размещается изображений. Этот файл содержит словарь для `spotlight-image-search` службы и включает идентификатор приложения, — это префикс идентификатора группы или идентификатор приложения, следуют идентификатор пакета. Файл может содержать до 500 пути и шаблонов, которые будут проиндексирован Spotlight и включены в поиск популярных изображения. Дополнительные сведения см. в разделе Apple [Создание и отправка файла сопоставлений](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) документации.
4. Разрешить Applebot обхода веб-сайтов. См. в разделе Apple [Applebot о](https://support.apple.com/en-us/HT204683) документации.

См. наш [интеграция приложения сообщений](~/ios/platform/message-app-integration/index.md) документации для получения дополнительных сведений.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован усовершенствования Apple, внесенных в iOS 10 и способы их реализации в Xamarin.iOS для поиска приложения.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
