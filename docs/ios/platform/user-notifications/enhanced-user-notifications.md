---
title: "Улучшенный пользовательский уведомления"
description: "В этой статье описываются все способы, что пользователям уведомления были улучшены iOS 10 и способ их использования в приложении Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 50553cb1dc5f7ea782c0f13e32f60d7b6ce3e181
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="enhanced-user-notifications"></a>Улучшенный пользовательский уведомления

_В этой статье описываются все способы, что пользователям уведомления были улучшены iOS 10 и способ их использования в приложении Xamarin.iOS._

Новые и iOS 10 уведомление пользователя, платформа дает для доставки и обработки локальных и удаленных уведомлений. С помощью этой платформы, приложения или расширения приложения можно запланировать доставки уведомлений, локальный, указав набор условий, таких как расположение или время дня.

## <a name="about-user-notifications"></a>О уведомления для пользователей

Как уже говорилось выше, для доставки и обработки локальных и удаленных уведомлений позволяет новую платформу уведомление пользователя. С помощью этой платформы, приложения или расширения приложения можно запланировать доставки уведомлений, локальный, указав набор условий, таких как расположение или время дня.

Кроме того, приложения или расширения можно получить (и потенциально изменять) уведомления локальных и удаленных малозаметными устройства iOS.

Новая инфраструктура пользовательского интерфейса для уведомления пользователя позволяет приложения или расширения приложения, чтобы настроить внешний вид уведомления локальных и удаленных, когда они отображаются для пользователя.

Эта платформа предоставляет следующие возможности, приложение может доставить уведомления пользователя:

- **Предупреждения Visual** — где уведомление собирает данные о вниз от верхней части экрана в виде заголовка.
- **Звук и вибрации** -может быть связан с уведомлением.
- **Приложение значок эмблемы** - где значок приложения отображает эмблемы, показывающая, что доступно новое содержимое, например количество непрочитанных сообщений.

Кроме того в зависимости от текущего контекста пользователя, можно различными способами, появляется уведомление:

- Если устройство не будет разблокировано, уведомление будет развертывание из верхней части экрана виде заголовка.
- Если устройство заблокировано, уведомление будет отображаться на экране блокировки пользователя.
- Если пользователь пропустит уведомление, можно открыть центр уведомлений и просматривать доступные, ожидания уведомления, существует.

Приложения Xamarin.iOS имеет два типа, он может отправлять уведомления для пользователей.

- **Локальный уведомления** — они не будут отправлены в приложения, установленные локально на устройстве пользователя.
- **Удаленный уведомления** -отправляются с удаленного сервера и либо отображаются для пользователя или они активируют фоновое обновление содержимого приложения.

### <a name="about-local-notifications"></a>Об уведомлениях о локальном

Локальный уведомлений, которые можно отправить приложение iOS имеют следующие атрибуты и функции:

- Они отправляются приложений, которые являются локальными на устройстве пользователя. 
- Они могут быть настроены для использования времени или расположение на основе триггеров. 
- Приложение планирует уведомление с устройства, и он открывается при соблюдении условия активации.
- Когда пользователь взаимодействует с уведомлением, приложение получит обратный вызов.

Некоторые примеры локального уведомления.

- Оповещения календаря
- Оповещения
- Триггеры учитывать расположение

Дополнительные сведения см. в разделе Apple [локальных и удаленных руководство по программированию уведомления](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) документации.

### <a name="about-remote-notifications"></a>Общие сведения об удаленном уведомлениях

Удаленный уведомлений, которые можно отправить приложение iOS имеют следующие атрибуты и функции:

- Приложение имеет серверный компонент, с которым он взаимодействует.
- Apple Push Notification Service (APNs) используется для передачи доставки удаленного уведомлений на устройство пользователя с серверов на основе облака для разработчиков.
- Когда приложение получает удаленный уведомление отображается для пользователя.
- Когда пользователь взаимодействует с уведомлением, приложение получит обратный вызов.

Некоторые примеры удаленного уведомления.

- Оповещения о новостях
- Спорт обновлений
- Мгновенных сообщений

Существует два типа уведомлений удаленного приложения iOS:

- **С выходом пользователя** -они отображаются для пользователя на устройстве.
- **Автоматического обновления** -они предоставляют механизм для обновления содержимого приложения iOS в фоновом режиме. При получении автоматического обновления приложения могут подключиться к запросу серверов удалить последнюю содержимого вниз.

Дополнительные сведения см. в разделе Apple [локальных и удаленных руководство по программированию уведомления](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) документации.

### <a name="about-the-existing-notifications-api"></a>Об уведомлениях существующих API

До iOS 10, следует использовать приложение iOS `UIApplication` зарегистрировать уведомление в систему и для планирования, как уведомления о должна активироваться (либо по времени или расположения).

Существует несколько проблем, разработчик может возникнуть при работе с существующие API-Интерфейс уведомлений:

- Обнаружены различные обратные вызовы, необходимый для локального или удаленного уведомления, что может привести к дублированию кода.
- Приложение ограниченные управления уведомления после запланированная с системой.
- Обнаружены различные уровни поддержки для всех существующих платформ Apple.

### <a name="about-the-new-user-notification-framework"></a>О новой платформы уведомление пользователя

С iOS 10 Apple внес новую платформу уведомление пользователя, который заменяет существующий `UIApplication` метод указанных выше.

Уведомление пользователя framework предоставляет следующие возможности.

- Знакомые API, который включает функционального соответствия с предыдущих методов, упрощая код переносится из существующей инфраструктуры.
- Включает расширенный набор параметров содержимого, позволяющий более подробная уведомлений, отправляемых пользователю.
- Локальный и удаленный уведомления могут обрабатываться одним кодом и обратные вызовы.
- Упрощает процесс обработки обратных вызовов, отправляемых к приложению, когда пользователь взаимодействует с уведомлением.
- Улучшенное управление ожидающие и доставленных уведомлений, включая возможность удалить или обновить уведомления.
- Добавляет возможность выполнять в приложении представления уведомлений.
- Добавляет возможность запланировать и обрабатывать уведомления из внутри расширения приложения.
- Добавляет новую точку расширения для уведомлений о себе. 

Новая инфраструктура уведомление пользователя предоставляет единый API-Интерфейс уведомлений в нескольких платформ, Apple поддерживает включая: 

- **iOS** -полная поддержка для управления и планирования уведомления.
- **tvOS** -добавляет возможность эмблемы значки приложений для локальных и удаленных уведомления.
- **watchOS** — добавляет возможность перенаправлять уведомления из устройства iOS парной их Apple Watch и предоставляет приложениям Контрольное значение делать локального уведомления непосредственно на часах.

Дополнительные сведения см. в разделе Apple [ссылке платформы UserNotifications](https://developer.apple.com/reference/usernotifications) и [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) документации.

## <a name="preparing-for-notification-delivery"></a>Подготовка для доставки уведомлений

Перед iOS приложение может отправлять уведомления для пользователя, приложения должны быть зарегистрированы в системе, и поскольку уведомление прерывания для пользователя, приложения должны явно запрашивать разрешение перед их отправкой.

Существует три разных уровня уведомления запросов, которые пользователь может утвердить для приложения.

- Отображает баннер.
- Звук оповещения.
- Эмблемы значка приложения.

Кроме того эти утверждения уровни необходимо запросил и задать для локальных и удаленных уведомлений.

Разрешения уведомления должен запрашиваться как только приложение запускается, добавив следующий код в `FinishedLaunching` метод `AppDelegate` и уведомления нужный тип параметра (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Кроме того, пользователь всегда может изменить права доступа уведомления для приложения в любое время с помощью **параметры** приложения на устройстве. Приложения должна проверять для разрешения запрошенного уведомления пользователей перед представлением уведомления, используя следующий код:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Настройка среды удаленного уведомления

Новое в iOS 10, разработчику требуется сообщить операционной системы, в какой среде Push-уведомлений работают в качестве разработки или производства. Необходимо предоставить эти сведения могут привести приложение к отклонению при отправке iTune App Store уведомление следующего вида:

> Отсутствует правах уведомлений Push - приложение включает в себя API-Интерфейс для службы Push-уведомлений Apple, но `aps-environment` правах отсутствует подпись приложения.

Чтобы предоставить требуемые права, выполните следующее:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Entitlements.plist` файла в **Pad решения** чтобы открыть его для редактирования.
2. Переключитесь в **источника** представления: 

    [![](enhanced-user-notifications-images/setup01.png "В представлении источника")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Нажмите кнопку  **+**  кнопку, чтобы добавить новый раздел.
4. Введите `aps-environment` для **свойство**, оставьте **тип** как `String` и введите либо `development` или `production` для **значение**: 

    [![](enhanced-user-notifications-images/setup02.png "Свойства среды aps")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Сохраните изменения в файле.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните `Entitlements.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
3. Нажмите кнопку  **+**  кнопку, чтобы добавить новый раздел.
4. Введите `aps-environment` для **свойство**, оставьте **тип** как `String` и введите либо `development` или `production` для **значение**: 

    [![](enhanced-user-notifications-images/setup02w.png "Свойства среды aps")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Сохраните изменения в файле.

-----

### <a name="registering-for-remote-notifications"></a>Регистрация для удаленного получения уведомлений

Если приложение будет отправлять и получать удаленный уведомления, его нужно будет сделать _токена регистрации_ с помощью существующего `UIApplication` API. Регистрация требуется устройства имеют доступ к соединению динамической сети APN, создающие необходимые маркера, которое будет отправляться в приложение. Приложению необходимо переадресовывать этот маркер для разработчика приложения на стороне сервера для регистрации для удаленного получения уведомлений:

[![](enhanced-user-notifications-images/token01.png "Обзор токена регистрации")](enhanced-user-notifications-images/token01.png#lightbox)

Используйте следующий код для инициализации требуется регистрации:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Маркер, который отправляется для разработчика приложения на стороне сервера необходимо быть включены как часть полезные данные уведомления, get отправленных сервером для APNs при отправке удаленного уведомления:

[![](enhanced-user-notifications-images/token02.png "Токен, включенным в состав полезные данные уведомления")](enhanced-user-notifications-images/token02.png#lightbox)

Токен выступает в качестве ключа, который объединяет уведомления и приложение, используемое для открытия или ответ на уведомление.

Дополнительные сведения см. в разделе Apple [локальных и удаленных руководство по программированию уведомления](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) документации.

## <a name="notification-delivery"></a>Доставка уведомления

С помощью приложения полностью зарегистрирован и запрашиваемых необходимые разрешения и предоставлены пользователем, приложение готов для отправки и получения уведомлений. 

### <a name="providing-notification-content"></a>Предоставление содержимого уведомлений

Новые для iOS 10 одновременно содержать все уведомления **заголовок** и **подзаголовок** , всегда будет отображаться с **текст** содержимого уведомления. Также новые возможности, возможности добавлять **вложения носителя** содержимого уведомления.

Для создания содержимого локального уведомления, используйте следующий код:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Процесс сходен удаленного уведомлений:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Планирование уведомлений при отправке

С помощью содержимого уведомления, созданные приложению необходимо запланировать уведомление будет отображаются для пользователя, задав *триггер*. iOS 10 предоставляет четыре различных типа триггера.

- **Push-уведомлений** — используется исключительно с удаленной уведомления и вызывается при отправке уведомления APNs пакета в приложение, работающее на устройстве.
- **Интервал времени** -позволяет локальной уведомления для планирования из времени с сейчас и конечным некоторые будущем начало интервала. Например `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`.
- **Дата календаря** -позволяет локальной уведомления для планирования в определенную дату и время.
- **На основе расположения** -позволяет локальной уведомления, чтобы планировать, когда устройство iOS является ввода или оставить в определенном географическом регионе или в данной близость к любой маяки Bluetooth.

При готовности локального уведомления приложению необходимо вызывать `Add` метод `UNUserNotificationCenter` объекта, чтобы запланировать его вид для пользователя. Для удаленного уведомлений серверных приложение отправляет полезные данные уведомления APNs, который затем отправляет пакет на устройство пользователя.

Объединяя все фрагменты, образец локальное уведомление может выглядеть так:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Обработка уведомлений приложение переднего плана

Новые для iOS 10, приложение можно обрабатывать уведомления по-разному при находится на переднем плане и запускается уведомление. Предоставляя `UNUserNotificationCenterDelegate` и реализация `UserNotificationCenter` метода, приложение может взять на себя ответственность за отображение уведомления. Пример:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Этот код просто выписке содержимое `UNNotification` выходных данных приложения и запросом в системе, для отображения стандартных предупреждений для уведомления. 

Если приложение было уведомление сам отображается, когда он был на переднем плане и не использует параметры по умолчанию, передать `None` в обработчик завершения. Пример

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

Этот код в месте, а затем откройте `AppDelegate.cs` для редактирования и измените `FinishedLaunching` метод для поиска следующим образом:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Этот код присоединения пользовательского `UNUserNotificationCenterDelegate` выше текущему `UNUserNotificationCenter` , приложение может обрабатывать уведомления, когда он активен и на переднем плане.

## <a name="notification-management"></a>Управление уведомлениями об

Новые для iOS 10 уведомлениями предоставляет доступ к ожидающие и доставленных уведомлений и добавляет возможность удалить, обновить или преобразовать эти уведомления.

Является важной частью управление уведомлениями об _идентификатор запроса_ , назначенный для уведомления после ее создания и запланированные с системой. Для удаленного уведомлений, этот адрес назначается через новое `apps-collapse-id` в заголовке запроса HTTP.

Идентификатор запроса используется для выбора уведомления, приложения, которые будут управлять уведомления.

### <a name="removing-notifications"></a>Удаление уведомления

Чтобы удалить ожидающие уведомления из системы, используйте следующий код:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

Чтобы удалить уведомление о уже доставленных, используйте следующий код:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Обновление существующего уведомлений

Для обновления существующего уведомлений, создайте новое уведомление с нужными параметрами, изменен (например, триггер времени) и добавить его в систему с тем же идентификатором запроса как уведомление, которое должно быть изменено. Пример


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Для уже доставленных уведомлений существующего уведомлений будет получить обновления и повышены до верхней части списка на экранах домашней и блокировки и в центре уведомлений, если оно уже прочитано пользователем.

## <a name="working-with-notification-actions"></a>Работа с действия уведомления

В iOS 10 уведомления, доставляемые пользователю не являются статичными и предоставляют несколько способов, которые пользователь может взаимодействовать с ними (через встроенные для пользовательских действий).

Существует три типа действия, которые приложение iOS может отвечать на:

- **Действие по умолчанию** -это происходит, когда пользователь касается уведомления, откройте приложение и отображения сведений данного уведомления.
- **Настраиваемые действия** -они были добавлены в iOS 8 и обеспечивают быстрый способ пользователю выполнять пользовательскую задачу непосредственно из уведомления без необходимости запуска приложения. Они могут быть представлены как список кнопок и настраиваемые заголовки или поля ввода текста которая может выполняться в фоновом режиме (когда приложение получает небольшое количество времени для выполнения запроса) или переднего плана (где запускается приложение на переднем плане fu lfill запрос). Настраиваемые действия доступны в iOS и watchOS.
- **Отклонить действие** -это действие, отправляется приложения, когда пользователь закрывает данного уведомления.

### <a name="creating-custom-actions"></a>Создание настраиваемых действий

Чтобы создать и зарегистрировать пользовательское действие в системе, используйте следующий код:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

При создании нового `UNNotificationAction`, ей назначается уникальный идентификатор и название, которое будет отображаться на кнопке. По умолчанию, будет создано действие как действие фона, однако параметры могут передаваться для настройки поведения действия (например при установке его действие переднего плана).

Все созданные действия должны быть связаны с категорией. При создании нового `UNNotificationCategory`, ей назначается уникальный идентификатор, список действий может выполняться список идентификаторов намерение предоставить дополнительные сведения о конечной цели действия в категории и некоторые параметры для управления поведением категории.

Наконец, все категории регистрируются с помощью системы `SetNotificationCategories` метод.

### <a name="presenting-custom-actions"></a>Представления пользовательских действий

После создания и зарегистрирован в системе набор пользовательских действий и категории могут быть представлены с локальной или удаленной уведомления.

Удаленный уведомления значение `category` в удаленных полезные данные уведомления, соответствующую одной из категорий, созданной ранее. Пример:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Локальный уведомления, установите `CategoryIdentifier` свойство `UNMutableNotificationContent` объекта. Пример:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Еще раз этот идентификатор должен соответствовать одной из категорий, созданные выше.

### <a name="handling-dismiss-actions"></a>Обработка Отклонить действия

Как уже говорилось выше, действие отклонить могут отправляться в приложение, когда пользователь закрывает уведомление. Так как это не стандартного действия, параметр необходимо задать при создании категории. Пример:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Обработка ответов действия

Когда пользователь взаимодействует с настраиваемые действия и категории, которые были созданы выше, приложение, необходимо для выполнения требуемой задачи. Это делается путем указания `UNUserNotificationCenterDelegate` и реализация `UserNotificationCenter` метода. Пример:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Переданный в `UNNotificationResponse` класс имеет `ActionIdentifier` свойство, которое может быть действием по умолчанию или отменить действие. Используйте `response.Notification.Request.Identifier` для проверки на наличие каких-либо пользовательских действий.

`UserText` Свойство содержит значение любой ввод пользователем текста. `Notification` Свойство содержит исходный уведомление, которое включает в себя запрос с триггером и содержимое уведомления. Приложение может решить, если она была локального или удаленного уведомления на основе типа триггера.

## <a name="working-with-service-extensions"></a>Работа с расширениями Service

При работе с удаленной уведомления _расширения службы_ позволяют включить шифрование конца в конец внутри полезные данные уведомления. Расширения службы являются пользовательский интерфейс расширением (доступный в iOS 10), выполняются в фоновом режиме, с основным назначением расширения или замены видимого содержимого уведомления, прежде чем он будет предоставлен пользователю. 

[![](enhanced-user-notifications-images/extension01.png "Обзор расширения службы")](enhanced-user-notifications-images/extension01.png#lightbox)

Расширения службы предназначены для быстрого запуска и получают только небольшое время выполнения в системе. В случае, если расширение службы не удается завершить задачу, в течение указанного периода времени, будет вызван метод резервной стратегии. В случае этот резервный механизм содержимого исходного уведомление будет отображаться для пользователя.

Некоторые возможные применения расширения службы включают:

- Предоставление шифрования конца в конец содержимого удаленного уведомления.
- Добавление вложений в удаленном уведомления для обогащения их.

### <a name="implementing-a-service-extension"></a>Реализация модуля службы

Чтобы реализовать расширение службы в приложении Xamarin.iOS, выполните следующее:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Откройте решение для приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **Pad решения** и выберите **добавить** > **Добавление нового проекта**.
3. Выберите **iOS** > **расширения** > **расширения службы уведомлений** и нажмите кнопку **Далее** кнопки: 

    [![](enhanced-user-notifications-images/extension02.png "Выберите расширения службы уведомлений")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **Далее** кнопки: 

    [![](enhanced-user-notifications-images/extension03.png "Введите имя для модуля")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Настройка **имя проекта** и/или **имя решения** , если требуется и нажмите кнопку **создать** кнопки: 

    [![](enhanced-user-notifications-images/extension04.png "Измените имя проекта и/или имя решения")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Откройте решение для приложения в Visual Studio.
2. Щелкните правой кнопкой мыши имя решения в **обозревателе решений** и выберите **добавить** > **Добавление нового проекта**.
3. Выберите **iOS** > **расширения** > **расширения службы уведомлений**: 

    [![](enhanced-user-notifications-images/extension01w.png "Выберите расширения службы уведомлений")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **ОК** кнопки.

-----

> [!IMPORTANT]
> Идентификатор пакета для расширения службы должен соответствовать идентификатор пакета основное приложение с `.appnameserviceextension` в конце. Например, если основное приложение имеет идентификатор пакета `com.xamarin.monkeynotify`, расширение службы должен иметь идентификатор пакета `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Это должно задаваться автоматически в том случае, если расширение добавляется в решение. 

Имеется один основной класс в расширение службы уведомлений, будет необходимо изменить, чтобы обеспечить требуемую функциональность. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

Первый метод `DidReceiveNotificationRequest`, будет передан идентификатор уведомления, а также уведомления содержимого через `request` объекта. Переданный в `contentHandler` будет необходимо вызвать, чтобы пользователь получает уведомление.

Второй метод `TimeWillExpire`, будет вызываться только до времени является израсходовано расширение службы для обработки запроса. Если не удается вызвать метод расширения службы `contentHandler` исходного содержимого в течение указанного периода времени, будет отображаться для пользователя.

### <a name="triggering-a-service-extension"></a>Запуск расширения службы

С расширением службы создания и доставки с приложением ее можно включить, изменив удаленного полезные данные уведомления, отправленные на устройство. Пример:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Новый `mutable-content` ключ определяет расширение службы будет обязательно запускать для обновления содержимого удаленного уведомления. `encrypted-content` Ключ содержит зашифрованные данные, которые может расшифровать расширение службы перед предоставлением пользователю.

Рассмотрим следующий пример расширения службы:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Этот код расшифровывает зашифрованное содержимое из `encrypted-content` ключа, создает новый `UNMutableNotificationContent`, задает `Body` свойство для расшифрованного содержимого и использует `contentHandler` для пользователь получает уведомление.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован любым способом, что пользователям уведомления были улучшены по iOS 10. Оно представлено в новую платформу уведомления пользователя и способ его использования в приложении Xamarin.iOS или расширения приложения.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Справочник по UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Руководство по программированию на локальных и удаленных уведомления](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
