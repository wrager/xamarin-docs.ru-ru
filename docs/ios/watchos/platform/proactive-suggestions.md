---
title: "Упреждающее предложения"
description: "В этой статье показано, как использовать упреждающее предложения в приложении watchOS 3 для диска обязательств, позволяя систему, чтобы заранее предоставить полезные сведения автоматически пользователю."
ms.topic: article
ms.prod: xamarin
ms.assetid: 10CC9F16-963C-44F1-8B98-F09FB2310DFF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: f9711cc39662a7e77d926551a0d2b49363d8ec4d
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="proactive-suggestions"></a>Упреждающее предложения

_В этой статье показано, как использовать упреждающее предложения в приложении watchOS 3 для диска обязательств, позволяя систему, чтобы заранее предоставить полезные сведения автоматически пользователю._


Новые и watchOS 3 упреждающего предложения присутствует новостей пользователям способы связаться с приложения Xamarin.iOS заранее присутствует полезные сведения об автоматически для пользователя в соответствующее время.


## <a name="about-proactive-suggestions"></a>О упреждающего предложения

Новые watchOS 3, `NSUserActivity` включает `MapItem` свойство, которое позволяет приложению предоставлять сведения о расположении, который может использоваться в других контекстах. Например, если приложения отображается гостиницы проверок и предоставляет `MapItem` расположение, если пользователь переключается на приложение Maps, расположение гостиницы, просто просматривалась бы быть доступны.

Приложение предоставляет эта функциональность в систему с помощью набор технологий, таких как `NSUserActivity`, MapKit, Media Player и UIKit. Кроме того благодаря поддержке упреждающего предложений для приложения, он получает все более глубокую интеграцию Siri бесплатно.

## <a name="location-based-suggestions"></a>Предложения на основе расположения

Новые watchOS 3, `NSUserActivity` класс содержит `MapItem` свойство, разработчик может предоставить сведения о местоположении, который может использоваться в других контекстах. Например, если приложение отображает отзывы о ресторанах, разработчик может настроить `MapItem` свойства расположение ресторана, который используется для просмотра в приложении. Если пользователь переходит к приложению Maps, расположение ресторана доступен автоматически.

Если приложение поддерживает поиска приложения, его можно использовать новый адрес компоненты `CSSearchableItemAttributesSet` класса для указания расположения, которые пользователь может захотеть посетить. Установив `MapItem` свойство, другие свойства являются автоматически заполняться.

В дополнение к параметру `Latitude` и `Longitude` свойствах компонента адреса, рекомендуется указать что приложение `NamedLocation` и `PhoneNumbers` свойства, поэтому Siri может инициировать вызов расположение.

## <a name="contextual-siri-reminders"></a>Контекстно-зависимое Siri напоминания

Дает пользователю возможность позволяет быстро создать напоминание для доступа к содержимому, они в настоящее время просматривать в приложении впоследствии Siri. Например, если они просматривать ресторане в приложении, они могут вызывать Siri и сказать *«Напомнить об этом при получении home».* Siri, создает оповещение со ссылкой на проверку в приложении.

## <a name="implementing-proactive-suggestions"></a>Реализация упреждающего предложения

Добавление предложений упреждающего поддержки, чтобы приложения Xamarin.iOS обычно так же легко, как реализация несколько интерфейсов API или развернув на несколько интерфейсов API, которые уже должна реализовывать приложения.

Упреждающее предложения работы с приложениями в тремя разными способами:

- **`NSUserActivity`** -Помогает понять, какие сведения пользователь в настоящее время работает с на экране системы.
- **Расположение предложения** — Если приложение предоставляет или потребляет сведения на основе расположения, эти расширения API предложение новые способы разглашать эти сведения для приложений.

И поддерживается в приложении реализация следующих:

- **Контекстно-зависимое напоминания Siri** — в iOS 10, `NSUserActivity` была увеличена, чтобы разрешить использование Siri, чтобы быстро создать напоминание для доступа к содержимому они в настоящее время просматривать в приложении в будущем.
- **Расположение предложения** -улучшает iOS 10 `NSUserActivity` для записи расположения просмотреть внутри приложения и продвинуть их в разных местах системы.
- **Контекстно-зависимое запросов Siri**  -  `NSUserActivity` предоставляет контекст для сведения, представленные внутри приложения для Siri, чтобы пользователь может получить указания или быть вызов месте вызова Siri из в приложении.

Все эти функции имеют одно Общие, то все они используют `NSUserActivity` в той или иной для функционирования форме. 

## <a name="nsuseractivity"></a>NSUserActivity

Как упоминалось выше, `NSUserActivity` помогает понять, какие сведения пользователь в настоящее время работает с на экране системы. `NSUserActivity` механизм для записи действий пользователей при переходе через приложение кэширует состояние недоступно. Например просмотрев ресторан приложения:

[![](proactive-suggestions-images/activity02.png "Приложение ресторана")](proactive-suggestions-images/activity02.png#lightbox)

С помощью следующих действий:

1. Так как пользователь работает с приложением, `NSUserActivity` создается для повторного создания состояние приложения в более поздней версии.
2. Если пользователь ищет ресторан, следует та же последовательность создания действий.
3. И еще раз, когда пользователь просматривает результат. В этом случае последний пользователь просматривает расположение и в iOS 10 система учитывает более основные понятия (например, расположение или связи взаимодействия).

Внимательно ознакомьтесь последнего экрана:

[![](proactive-suggestions-images/activity03.png "Полезные данные NSUserActivity")](proactive-suggestions-images/activity03.png#lightbox)

Здесь Создание приложения `NSUserActivity` и заполнена сведениями для повторного создания состояние позже. Приложение также включены некоторые метаданные, такие как имя и адрес расположения. С этим действием создан приложение позволяет iOS знать, что он представляет текущее состояние пользователя.

Затем приложение решает, если действие выполняется объявление беспроводную для обработки, сохраняется как временное значение предложения расположение или добавлен к индексу Spotlight на устройстве для отображения в результатах поиска.

Дополнительные сведения о переадресации и Spotlight поиска см. в разделе нашей [Общие сведения о переадресации](~/ios/platform/handoff.md) и [iOS 9 новых API поиска](~/ios/platform/search/index.md) руководства.

### <a name="creating-an-activity"></a>Создание действия

Прежде чем создавать действия, идентификатор типа действия будет необходимо создать для его идентификации. Идентификатор типа действия — это короткая строка, добавляемый `NSUserActivityTypes` массив приложения `Info.plist` файл, используемый для идентификации данного типа действий пользователя. В массиве для каждого действия, которая поддерживает приложения и предоставляет поиска приложения будет существовать одна запись. См. наш [создание идентификаторов ссылку на тип действия](~/ios/platform/search/nsuseractivity.md) для получения дополнительных сведений.

Рассмотрим пример действия:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Новое действие создается с помощью идентификатора типа действия. Далее некоторые метаданные, определяющие действие создается, чтобы впоследствии можно было восстановить это состояние. Затем действие задано понятное имя и присоединяется к сведения о пользователе. Наконец некоторые возможности включены и действия отправляется в системе.

Приведенный выше код можно улучшить для включения метаданных, предоставляющий контекст для действия вносит следующие изменения:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Если разработчик веб-сайт, который может отображать те же сведения, что и приложение, приложение может содержать URL-адрес и содержимого могут отображаться на других устройствах, которые не установлено приложение (с помощью передачи):

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Восстановление действия

Ответ на пользователя, нажав на результат поиска (`NSUserActivity`) приложение, изменить **AppDelegate.cs** файла и Переопределите `ContinueUserActivity` метод. Пример:

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

Это обеспечивает тот же идентификатор типа действия (`com.xamarin.platform`) как действие, созданной ранее. Приложение использует данные, хранящиеся в `NSUserActivity` для восстановления состояния пользователя места остановки.

### <a name="benefits-of-creating-an-activity"></a>Преимущества создания действия

Минимальным объемом кода, представленного выше приложения теперь могут использовать три новых возможностей iOS 10:

- **Handoff**
- **Поиску Spotlight**
- **Контекстно-зависимое Siri напоминания**

Следующий раздел будет взгляните на включение две новые возможности iOS 10:

- **Расположение предложения**
- **Контекстно-зависимое Siri запросов**

### <a name="location-based-suggestions"></a>Предложения на основе расположения 

Рассмотрим в качестве примера выше приложение поиска ресторана. Если он реализован `NSUserActivity` и правильности заполнения все метаданные и атрибуты, пользователь сможет делать следующее:

1. Найти Ресторан в приложении, они будут соответствовать друг на.
4. Если пользователь переходит к приложению Maps, адрес ресторана автоматически предлагается в качестве назначения.
5. Это работает даже для сторонних приложений (которые поддерживают `NSUserActivity`), поэтому пользователь может переключить приложение для управления доступом игнорировать и ресторана адрес автоматически предлагается в качестве назначения существует также.
6. Он также предоставляет контекст для Siri, поэтому пользователь может вызвать Siri в приложении ресторан и попросите *«Получить направлениях...»* и Siri предоставит инструкции в ресторан, при просмотре.

Все выше функции имеет единственное общих, все они указывают, где предложение изначально поступает из. В случае приведенном выше примере это приложение проверки вымышленной ресторана.

Чтобы включить эту функцию для приложения через несколько небольших изменений и дополнений для существующих инфраструктур были расширены watchOS 3:

- `NSUserActivity` содержит дополнительные поля для отслеживания сведения о расположении, которые можно просматривать внутри приложения.
- Для записи расположения были внесены несколько дополнений MapKit и CoreSpotlight.
- Расположение функциональные будет добавлен Siri, карты, многозадачной и других приложений в системе.

Чтобы реализовать рекомендации на основе расположения, запустите с тем же кодом действия, представленные выше:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Если приложение использует MapKit, весьма простым добавлением текущей карты `MKMapItem` для действия:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Если приложение не использует MapKit, можно внедрить приложения поиска и укажите следующие атрибуты нового расположения:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Рассмотрим приведенный выше код подробно. Во-первых в каждом экземпляре требуется имя расположения:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Выберите текст, на основе описания в необходимый для текста на основе экземпляров (например, клавиатура QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Широта и долгота являются обязательными, но убедитесь, что пользователь направляется в приложение желая направлять их на точное расположение:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Установив номера телефонов, приложение может получить доступ к Siri, пользователь может вызвать Siri из приложения, о том, что-то вроде * «Вызвать это место»:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Наконец приложение может указывать, подходит ли экземпляр для навигации и телефонные звонки.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```
## <a name="activities-best-practices"></a>Рекомендации по обеспечению действий

При работе с действиями, Apple предлагает следующие рекомендации:

- Используйте `NeedsSave` обновлений отложенной полезных данных.
- Убедитесь, чтобы сохранить строгую ссылку на текущую активность.
- Передача только небольшой полезных данных, включающие достаточно информации для восстановления состояния.
- Убедитесь, что идентификаторы типа действия уникальное и описательное с помощью нотации обратного DNS для указания их. 

## <a name="consuming-location-suggestions"></a>Использование предложения расположение

В следующем разделе мы рассмотрим использование предложения расположение, взяты из других частей в системе (например, приложение Maps) или других сторонних приложений.

## <a name="routing-apps-and-locations-suggestions"></a>Маршрутизации приложений и расположения предложения

В этом разделе будут рассмотрены в потреблении предложения расположение непосредственно из приложения маршрутизации. Для маршрутизации приложения и добавить эту функциональность, разработчик будет использовать существующий `MKDirectionsRequest` framework следующим образом:

- Для повышения уровня приложения в многозадачной.
- Для регистрации приложения в качестве маршрутизации приложения.
- Для обработки, запустив приложение с MapKit `MKDirectionsRequest` объекта.
- Предоставление watchOS возможности узнать, как предложить приложения с учетом вовлечение пользователей.

Когда приложение запускается с параметром MapKit `MKDirectionsRequest` объекта, он автоматически запустить предоставления направления пользователей к указанному местоположению или предоставить пользовательский Интерфейс, который позволяет пользователю начать извлечение направлениях. Пример:


```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
    {
        ...
        
        public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
        {
            if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
                var request = new MKDirectionsRequest (url);
                var coordinate = request.Destination?.Placemark.Location?.Coordinate;
                var address = request.Destination.Placemark.AddressDictionary;
                if (coordinate.IsValid()) {
                    var geocoder = new CLGeocoder ();
                    geocoder.GeocodeAddress (address, (place, err) => {
                        // Handle the display of the address

                    });
                }
            }

            return true;
        }
    }       
}
```

Рассмотрим этот код подробно. Проверяется, является ли запрос допустимое назначение:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Если он является, то он создает `MKDirectionsRequest` по URL-адресу:

```csharp
var request = new MKDirectionsRequest(url);
```

Новое в watchOS 3, приложение может быть отправлено адрес, который не имеет координаты geo, к тому, что разработчику необходимо закодировать адрес:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="summary"></a>Сводка

В этой статье покрытия упреждающего предложения и показано, как разработчик может использовать их для диска трафика приложения Xamarin.iOS для watchOS. Он охваченных шаг, чтобы реализовать упреждающего предложения и представлены рекомендации по использованию.


## <a name="related-links"></a>Связанные ссылки

- [Примеры watchOS](https://developer.xamarin.com/samples/watchos/all/)
- [Руководство по программированию SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
