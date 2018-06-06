---
title: Общие сведения о упреждающего предложений в Xamarin.iOS
description: В этой статье показано, как использовать упреждающее предложения в Xamarin.iOS приложению engagement дисков, позволяя систему, чтобы заранее предоставить полезные сведения автоматически пользователю.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f736e9dda00546ddef7cf03457813c7e3d10882b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788025"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Общие сведения о упреждающего предложений в Xamarin.iOS

_В этой статье показано, как использовать упреждающее предложения в Xamarin.iOS приложению engagement дисков, позволяя систему, чтобы заранее предоставить полезные сведения автоматически пользователю._

Новые и iOS 10 упреждающего предложения присутствует новостей пользователям способы связаться с приложения Xamarin.iOS заранее присутствует полезные сведения об автоматически для пользователя в соответствующее время.

iOS 10 предоставляет новые способы определяющим взаимодействия в приложение, позволяя систему, чтобы заранее предоставить полезные сведения автоматически пользователю в нужное время. Так же, как iOS 9 позволили добавления в приложение, используя Spotlight, передачи и предложения Siri сложного поиска (см. [новые интерфейсы API поиска](~/ios/platform/search/index.md)), с iOS 10 приложения могут предоставлять функции, которые могут быть представлены пользователю системой изнутри следующие расположения:

- Приложение переключателя
- На экран блокировки
- CarPlay
- Карты
- Siri взаимодействия
- QuickType предложения

Приложение предоставляет эта функциональность в систему с помощью набор технологий, таких как `NSUserActivity`, веб-разметка, Core Spotlight, MapKit, Media Player и UIKit. Кроме того благодаря поддержке упреждающего предложений для приложения, он получает все более глубокую интеграцию Siri бесплатно.

## <a name="location-based-suggestions"></a>Предложения на основе расположения

Опыта работы с iOS 10, `NSUserActivity` класс содержит `MapItem` свойство, разработчик может предоставить сведения о местоположении, который может использоваться в других контекстах. Например, если приложение отображает отзывы о ресторанах, разработчик может настроить `MapItem` свойства расположение ресторана, который используется для просмотра в приложении. Если пользователь переходит к приложению Maps, расположение ресторана доступен автоматически.

Если приложение поддерживает поиска приложения, его можно использовать новый адрес компоненты `CSSearchableItemAttributesSet` класса для указания расположения, которые пользователь может захотеть посетить. Установив `MapItem` свойство, другие свойства являются автоматически заполняться.

В дополнение к параметру `Latitude` и `Longitude` свойствах компонента адреса, рекомендуется указать что приложение `NamedLocation` и `PhoneNumbers` свойства, поэтому Siri может инициировать вызов расположение.

## <a name="web-markup-based-suggestions"></a>Предложения на основе веб-разметка

iOS 9 добавлена возможность включения разметки структурированных данных в веб-сайт, который дополняет содержимое, которое отображается в результатах поиска Spotlight и Safari (см. [поиска с веб-разметка](~/ios/platform/search/web-markup.md)). iOS 10 дополнительную возможность включать разметки на основе расположения (такие как [PostalAddress](http://schema.org/PostalAddress) в соответствии с определением [Schema.org](http://schema.org/)) для дальнейшего улучшения взаимодействия с пользователем. Например если пользователи представления расположение веб-сайта, система может предложить местоположения при открытии карты.

## <a name="text-based-suggestions"></a>Предложения на основе текста

UIKit была расширена в iOS 10, чтобы включить [TextContentType](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) свойство [UITextInputTraits](https://developer.apple.com/reference/uikit/uitextinputtraits) класса для указания содержимого при этом семантическое значение в текстовое поле. С этими данными системы можно обычно автоматически выберите тип сочетания, улучшения Автозамена предложений и заранее интегрировать данные из других приложений и веб-сайтов.

Например, если пользователь вводит текст в текстовое поле помечен `UITextContentType.FullStreetAddress`, система может предложить автоматического заполнения поля с учетом расположения пользователя недавно Просмотр.

## <a name="media-based-suggestions"></a>Предложения на основе мультимедиа

Если приложение воспроизводит носителя с помощью [MPPlayableContentManager](https://developer.xamarin.com/api/type/MediaPlayer.MPPlayableContentManager/) API iOS 10 позволяет пользователям просматривать альбома и воспроизводить мультимедиа через приложение на экране блокировки.

## <a name="contextual-siri-reminders"></a>Контекстно-зависимое Siri напоминания

Дает пользователю возможность позволяет быстро создать напоминание для доступа к содержимому, они в настоящее время просматривать в приложении впоследствии Siri. Например, если они просматривать ресторане в приложении, они могут вызывать Siri и сказать *«Напомнить об этом при получении home».* Siri, создает оповещение со ссылкой на проверку в приложении.

## <a name="contact-based-suggestions"></a>Предложения на основе контакта

Позволяет приложению контактов (и контактные сведения), могут появляться в **обратитесь к** приложения на устройстве iOS, а также все контакты пользователей, хранящихся в системе.

## <a name="ride-sharing-based-suggestions"></a>Воспользуйтесь преимуществами управления доступом на основе предложения

Если используется приложение для управления доступом игнорировать [MKDirectionsRequest](https://developer.xamarin.com/api/type/MapKit.MKDirectionsRequest/) API, iOS 10 будет представлять их как значение в переключателе приложения во время, когда пользователь, вероятно, будет игнорировать. Приложение также должен быть зарегистрирован как приложение управления доступом расстояния, указав `MKDirectionsModeRideShare` для [MKDirectionsApplicationSupportedModes](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) ключа в его `Info.plist` файла.

Если приложение поддерживает только расстояния для управления доступом, с подсказкой системы сразу начинаете работать с *«Get расстояния для...»*, если поддерживаются другие типы маршрутизации направление (например, Walking или велосипед), система будет использовать *«Получить инструкциям по...»*

> [!IMPORTANT]
> [MKMapItem](https://developer.xamarin.com/api/type/MapKit.MKMapItem/) объект, получающий приложения не содержит сведений, широты и долготы и потребует геокодирования.

## <a name="implementing-proactive-suggestions"></a>Реализация упреждающего предложения

Добавление предложений упреждающего поддержки, чтобы приложения Xamarin.iOS обычно так же легко, как реализация несколько интерфейсов API или развернув на несколько интерфейсов API, которые уже должна реализовывать приложения.

Упреждающее предложения работы с приложениями в тремя разными способами:

- **`NSUserActivity` и Schema.org**  -  `NSUserActivity` помогает понять, какие сведения пользователь в настоящее время работает с на экране системы. Schema.org добавляет аналогичные возможности на веб-страницы.
- **Расположение предложения** — Если приложение предоставляет или потребляет сведения на основе расположения, эти расширения API предложение новые способы разглашать эти сведения для приложений.
- **Предложения приложение носителя** - системы можно повысить уровень приложения и его содержимое носителя на основе контекста взаимодействия пользователя с помощью устройства iOS.

И поддерживается в приложении реализация следующих:

- **Перемещение вручную**  -  `NSUserActivity` была добавлена в iOS 8 для поддержки передачи, который разработчик может начать действие на одном устройстве, а затем продолжается на другой (в разделе [Общие сведения о переадресации](~/ios/platform/handoff.md)).
- **Поиска Spotlight** -iOS 9 добавлена возможность распространения содержимого приложения изнутри с помощью результаты поиска Spotlight `NSUserActivity` (см. [поиска Core Spotlight](~/ios/platform/search/corespotlight.md)).
- **Контекстно-зависимое напоминания Siri** — в iOS 10, `NSUserActivity` была увеличена, чтобы разрешить использование Siri, чтобы быстро создать напоминание для доступа к содержимому пользователь в настоящее время просматривать в приложении, в дальнейшем.
- **Расположение предложения** -улучшает iOS 10 `NSUserActivity` для записи расположения просмотреть внутри приложения и продвинуть их в разных местах системы.
- **Контекстно-зависимое запросов Siri**  -  `NSUserActivity` предоставляет контекст для сведения, представленные внутри приложения для Siri, чтобы пользователь может получить указания или быть вызов месте вызова Siri из в приложении.
- **Контакт взаимодействия** — новые возможности iOS 10, `NSUserActivity` позволяет приложениям связи преобразуется из карточку контакта (в приложении «контакты») как связи альтернативный метод.

Все эти функции имеют одно Общие, то все они используют `NSUserActivity` в той или иной для функционирования форме. 

## <a name="nsuseractivity"></a>NSUserActivity

Как упоминалось выше, `NSUserActivity` помогает понять, какие сведения пользователь в настоящее время работает с на экране системы. `NSUserActivity` механизм для записи действий пользователей при переходе через приложение кэширует состояние недоступно. Например просмотрев приложения ресторан:

[![](proactive-suggestions-images/activity02.png "Состояние недоступно: NSUserActivity механизм кэширования")](proactive-suggestions-images/activity02.png#lightbox)

С помощью следующих действий:

1. Так как пользователь работает с приложением, `NSUserActivity` создается для повторного создания состояние приложения в более поздней версии.
2. Если пользователь ищет ресторан, следует та же последовательность создания действий.
3. И еще раз, когда пользователь просматривает результат. В этом случае последний пользователь просматривает расположение и в iOS 10 система учитывает более основные понятия (например, расположение или связи взаимодействия).

Внимательно ознакомьтесь последнего экрана:

[![](proactive-suggestions-images/activity03.png "NSUserActivity подробные сведения")](proactive-suggestions-images/activity03.png#lightbox)

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

Разработчик должен иметь это же идентификатор типа действия (`com.xamarin.platform`) как действие, созданной ранее. Приложение использует данные, хранящиеся в `NSUserActivity` для восстановления состояния пользователя места остановки.

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
2. Как пользователь перемещается из приложения с помощью переключателя приложения многозадачной, система автоматически отобразит предложение (в нижней части экрана) для получения указаний ресторан, с помощью своих любимых навигации приложения.
3. Если пользователь переключается на приложение сообщений и начинает ввод *«Давайте соединяются под»*, клавиатура QuickType будет автоматически предлагать вставки в адресе ресторана.
4. Если пользователь переходит к приложению Maps, адрес ресторана автоматически предлагается в качестве назначения.
5. Это работает даже для сторонних приложений (которые поддерживают `NSUserActivity`), поэтому пользователь может переключить приложение для управления доступом игнорировать и ресторана адрес автоматически предлагается в качестве назначения существует также.
6. Он также предоставляет контекст для Siri, поэтому пользователь может вызвать Siri в приложении ресторан и попросите *«Получить направлениях...»* и Siri предоставит инструкции в ресторан, при просмотре.

Все выше функции имеет единственное общих, все они указывают, где предложение изначально поступает из. В случае приведенном выше примере это приложение проверки вымышленной ресторана.

Чтобы включить эту функцию для приложения через несколько небольших изменений и дополнений для существующих инфраструктур были расширены iOS 10:

- `NSUserActivity` содержит дополнительные поля для отслеживания сведения о расположении, которые можно просматривать внутри приложения.
- Для записи расположения были внесены несколько дополнений MapKit и CoreSpotlight.
- Расположение функциональные будет добавлен Siri, карты, клавиатура, многозадачной и других приложений в системе.

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

Затем на основе Описание расположения требуется для экземпляров на основе текста (например, клавиатура QuickType):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Широта и долгота являются обязательными, но убедитесь, что пользователь перенаправляется точное расположение приложения Желательное их, отправки, поэтому должно быть включено:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Установив номера телефонов, приложение может получить доступ к Siri, пользователь может вызвать Siri из приложения, о том, что-то вроде *«Вызвать это место»*:

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Наконец приложение может указывать, подходит ли экземпляр для навигации и телефонные звонки.

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Реализация взаимодействия контакта

Новые в iOS 10 взаимодействия приложений тесно интегрированы в приложении «контакты» из карточки контакта. Для приложений, которые реализованы контактов взаимодействия пользователь может добавлять сведения о приложении конкретным сотрудникам из их контактов. И если, например, они нажмите кнопку "быстрое действие" в верхней части карточки для отправки сообщения, вложенного приложения будет указан как параметр для отправки сообщений.

Если выбран третьей стороной приложения, его можно сохраненных и представлен в виде стандартного способа для сообщения данному пользователю в следующий раз, пользователю нужно обратиться к нему.

Взаимодействия контакта, реализованных в приложения с использованием `NSUserActivity` и новую платформу целей, представленные в iOS 10. Дополнительные сведения о работе с целей см. в разделе нашей [основные понятия SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) и [SiriKit реализации](~/ios/platform/sirikit/implementing-sirikit.md) руководства.

#### <a name="donating-interactions"></a>Благотворительность взаимодействия

Рассмотрим, как предоставить поток взаимодействия для приложения:

[![](proactive-suggestions-images/activity04.png "Обзор благотворительность взаимодействия")](proactive-suggestions-images/activity04.png#lightbox)

Приложение создает `INInteraction` , содержащий **намерение** (`INIntent`), **участников** и **метаданные**. **Намерение** представляет действие пользователя, такие как видео вызова или отправка текстового сообщения. **Участников** включить пользователи, получение связи. **Метаданные** определяет дополнительную информацию, например успешно отправлять сообщения, и т. д.

Разработчик никогда не непосредственно создает экземпляр `INIntent` или `INIntentResponse`, они будут использовать один из указанных дочерних классов (на основе задачи Выполнение приложения от имени пользователя), которые наследуются от этих родительских классов. Например `INSendMessageIntent` и `INSendMessageIntentResponse` для отправки текстового сообщения. 

Когда будет заполнена взаимодействие, вызовите `DonateInteraction` метод для информирования системы, доступные для использования взаимодействия.

Когда пользователь взаимодействует с приложением, из карты контакта, взаимодействие возвращает вместе с `NSUserActivity`, который затем используется для запуска приложения:

[![](proactive-suggestions-images/activity05.png "Взаимодействие возвращает вместе с NSUserActivity, который используется для запуска приложения")](proactive-suggestions-images/activity05.png#lightbox)

Рассмотрим приведенный ниже способа отправки сообщения:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
    public class DonateInteraction
    {
        #region Constructors
        public DonateInteraction ()
        {
        }
        #endregion

        #region Public Methods
        public void SendMessageIntent (string text, INPerson from, INPerson[] to)
        {

            // Create App Activity
            var activity = new NSUserActivity ("com.xamarin.message");

            // Define details
            var info = new NSMutableDictionary ();
            info.Add (new NSString ("message"), new NSString (text));

            // Populate Activity
            activity.Title = "Sent MonkeyChat Message";
            activity.UserInfo = info;

            // Enable capabilities
            activity.EligibleForSearch = true;
            activity.EligibleForHandoff = true;
            activity.EligibleForPublicIndexing = true;

            // Inform system of Activity
            activity.BecomeCurrent ();

            // Create message Intent
            var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

            // Create Intent Reaction
            var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

            // Create interaction
            var interaction = new INInteraction (intent, response);

            // Donate interaction to the system
            interaction.DonateInteraction ((err) => {
                // Handle donation error
                ...
            });
        }
        #endregion
    }
}
```

Этот код, подробно касается, он создает и заполняет экземпляр класса `NSUserActivity` (как показано в [Создание действия](#Creating-an-Activity) выше). Затем он создает экземпляр `INSendMessageIntent` (который наследуется от `INIntent`) и заполняет сведения отправляемого сообщения:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

`INSendMessageIntentResponse` Создается и передается `NSUserActivity` созданной выше:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` Построен с целью отправки сообщений (`INSendMessageIntent`) и ответа (`INSendMessageIntentResponse`) только что создан:

```csharp
var interaction = new INInteraction (intent, response);
```

Наконец система не уведомления взаимодействия:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
    // Handle donation error
    ...
});
```

Обработчик завершения передается в которых приложение может реагировать на пожертвование успешно или сбоем.

### <a name="activities-best-practices"></a>Рекомендации по обеспечению действий

При работе с действиями, Apple предлагает следующие рекомендации:

- Используйте `NeedsSave` обновлений отложенной полезных данных.
- Убедитесь, чтобы сохранить строгую ссылку на текущую активность.
- Передача только небольшой полезных данных, включающие достаточно информации для восстановления состояния.
- Убедитесь, что идентификаторы типа действия уникальное и описательное с помощью нотации обратного DNS для указания их. 

## <a name="schemaorg"></a>Schema.org

Как показано выше, `NSUserActivity` помогает понять, какие сведения пользователь в настоящее время работает с на экране системы. Schema.org добавляет аналогичные возможности на веб-страницы.

Schema.org можно предоставить те же типы взаимодействия на основе расположения на веб-сайт. Apple предназначены новые предложения расположение работать точно так же при просмотре в Safari, как в собственном приложении.

Поддержка некоторых Schema.org.

- Он предоставляет стандартом словарный состав языка разметки открытого веб-узла.
- Это работает, включая структурированных метаданных на веб-страницах.
- Существуют более 500 схемы, представляющее основные понятия доступны.
- Благодаря использованию его на веб-сайт, разработчик может получить некоторые преимущества использования `NSUserActivity` в собственном приложении.

Схемы будут представлены в виде дерева, как и структура, где определенного типы, такие как *ресторан*, наследовать из более универсальных типов, таких как *деловую*. Дополнительные сведения см. в разделе [Schema.org](http://schema.org).

Например, если веб-странице входят следующие данные:

```xml
<script type="application/ld+json>
{
    "@context":"http://schema.org",
    "@type":"Restaurant",
    "telephone":"(415) 781-1111",
    "url":"https://www.yanksing.com",
    "address":{
        "@type":"PostalAddress",
        "streetAddress":"101 Spear St",
        "addressLocality":"San Francisco",
        "postalCode":"94105",
        "addressRegion":"CA"
    },
    "aggregateRating":{
        "@type":"AggregateRating",
        "ratingValue":"3.5",
        "reviewCount":"2022"
    }
}
</script>
```

Если пользователь посетил эту страницу в Safari и затем переключается на другое приложение, сведения о расположении на странице будет захвачен и будет предложена в качестве подсказки расположение в других частях системы (как показано в приведенном выше действий).

Safari извлечет все содержимое веб-страницы, отвечающий на любой из следующих свойств схемы:

- **PostalAddress**
- **GeoCoordinates**
- Свойство телефона.

Дополнительные сведения см. в разделе нашей [поиска с веб-разметка](~/ios/platform/search/web-markup.md) руководства.

## <a name="consuming-location-suggestions"></a>Использование предложения расположение

В следующем разделе мы рассмотрим использование предложения расположение, взяты из других частей в системе (например, приложение Maps) или других сторонних приложений.

Существует два основных способа, приложение может использовать расположение предложения:

- С клавиатуры QuickType.
- Напрямую, использует сведения о расположении в приложениях для маршрутизации.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Расположение предложения и QuickType клавиатуры

Если приложение имеет дело с адресами в текстовых форматах, приложение может воспользоваться преимуществами расположение предложений через пользовательский Интерфейс QuickType. iOS 10 расширяет QuickType пользовательского интерфейса с помощью следующих средств:

- Приложение можно добавить указания о семантической цель для текстовых полей в пользовательском Интерфейсе.
- Приложение можно получить упреждающего предложения в приложении.
- Приложения могут использовать преимущества улучшенные Автозамена.

Новый `TextContentType` свойство, поле текстовых элементов управления в iOS 10 позволяет разработчику определить семантической цель для значения, которое пользователь должен ввести в данном поле. Пример:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Будет сообщаете системе, что приложение ожидает от пользователя ввести полный почтовый адрес в заданного поля. Это позволит клавиатуры QuickType автоматически предоставлять предложения расположение на клавиатуре, если пользователь вводит значение в этом поле.

Ниже приведены некоторые наиболее распространенные типы, доступные разработчику в `UITextContentType` статического класса:

* `Name`
* `GivenName`
* `FamilyName`
* `Location`
* `FullStreetAddress`
* `AddressCityAndState`
* `TelephoneNumber`
* `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Маршрутизации приложений и расположения предложения

В этом разделе будут рассмотрены в потреблении предложения расположение непосредственно из приложения маршрутизации. Для маршрутизации приложения и добавить эту функциональность, разработчик будет использовать существующий `MKDirectionsRequest` framework следующим образом:

- Для повышения уровня приложения в многозадачной.
- Для регистрации приложения в качестве маршрутизации приложения.
- Для обработки, запустив приложение с MapKit `MKDirectionsRequest` объекта.
- Чтобы предоставить возможность узнать, как предложить пользователю приложения в соответствующие моменты iOS на основе участия пользователя.

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

Новое в iOS 10, приложение может быть отправлено адрес, который не имеет координаты geo, к тому, что разработчику необходимо закодировать адрес:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
    // Handle the display of the address
    
});

```

## <a name="media-app-suggestions"></a>Предложения приложение мультимедиа

Если приложение обрабатывает любой тип носителя как приложение подкастов или потоковой передаче мультимедийного содержимого, например, аудио или видео с iOS 10, он может использовать преимущества предложения мультимедиа в системе.

Для приложений, обрабатывающих мультимедиа iOS поддерживает следующее поведение:

- iOS способствует повышению уровня приложения, которые пользователь, вероятно, для использования на основе их предыдущего поведения.
- Рекомендации, относящиеся к приложению будут представлены в Spotlight и представление сегодня.
- Если приложение соответствует одному из следующих триггеров, он может повысить экрана подсказки блокировки:
    - После подключения наушники или устройство Bluetooth установит соединение.
    - После получения в автомобиле.
    - После поступающих дома или работы. 

Включая простого вызова API в iOS 10, разработчик может создать более привлекательные блокировки экрана взаимодействие для пользователей приложения носителя. С помощью `MPPlayableContentManager` класса для управления воспроизведение мультимедиа, полный носитель элементов управления (например, представленный приложения музыка) появляется на экране блокировки для приложения.


```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
    public class PlayableContentDelegate : MPPlayableContentDelegate
    {
        #region Constructors
        public PlayableContentDelegate ()
        {
        }
        #endregion

        #region Override methods
        public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
        {
            // Access the media item to play
            var item = LoadMediaItem (indexPath);

            // Populate the lock screen
            PopulateNowPlayingItem (item);

            // Prep item to be played
            var status = PreparePlayback (item);

            // Call completion handler
            completionHandler (null);
        }
        #endregion

        #region Public Methods
        public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
        {
            var item = new MPMediaItem ();

            // Load item from media store
            ...

            return item;
        }

        public void PopulateNowPlayingItem (MPMediaItem item)
        {
            // Get Info Center and album art
            var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
            var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

            // Populate Info Center
            infoCenter.NowPlaying.Title = item.Title;
            infoCenter.NowPlaying.Artist = item.Artist;
            infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
            infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
            infoCenter.NowPlaying.Artwork = albumArt;
        }

        public bool PreparePlayback (MPMediaItem item)
        {
            // Prepare media item for playback
            ...

            // Return results
            return true;
        }
        #endregion
    }
}
```

## <a name="summary"></a>Сводка

В этой статье покрытия упреждающего предложения и показано, как разработчик может использовать их для трафика приложения Xamarin.iOS диска. Он охваченных шаг, чтобы реализовать упреждающего предложения и представлены рекомендации по использованию.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Руководство по программированию SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
