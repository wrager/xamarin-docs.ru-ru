---
title: Отправка Push-уведомления из Azure мобильных приложений
description: Концентраторы уведомлений Azure предоставляют масштабируемые push-инфраструктуру для отправки мобильные push-уведомления с любого сервера на любую мобильную платформу, устраняя необходимость связываться с разных систем уведомлений платформы серверной части сложности. В этой статье описывается использование концентраторов уведомлений Azure для отправки push-уведомления из мобильных приложений Azure экземпляра приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 28aba0ec33dc88e3e87f51fbdd28d5ec8a72d3c3
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847606"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Отправка Push-уведомления из Azure мобильных приложений

_Концентраторы уведомлений Azure предоставляют масштабируемые push-инфраструктуру для отправки мобильные push-уведомления с любого сервера на любую мобильную платформу, устраняя необходимость связываться с разных систем уведомлений платформы серверной части сложности. В этой статье описывается использование концентраторов уведомлений Azure для отправки push-уведомления из мобильных приложений Azure экземпляра приложения Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure Push с концентратором уведомлений и Xamarin.Forms, [университета Xamarin](https://university.xamarin.com/)**

Push-уведомлений используются для передачи из серверной системе приложения на мобильном устройстве, чтобы увеличить engagement приложения и сведения, такие как сообщения. Уведомления могут отправляться в любое время, даже в том случае, если пользователь не использует активно целевого приложения.

Серверных системах отправки push-уведомлений на мобильных устройствах через систем уведомления платформы (PNS), как показано на следующей схеме:

[![](azure-images/pns.png "Систем уведомлений платформы")](azure-images/pns-large.png#lightbox "систем уведомлений платформы")

Для отправки push-уведомлений, серверной системе обращается в PNS платформой для отправки уведомления в экземпляре приложения клиента. Это значительно увеличивает сложность серверной части при кросс платформенных push-уведомлений не требуются, поскольку серверной части должен использовать каждый платформой PNS API и протокол.

Концентраторы уведомлений Azure исключить сложность, не учитывая деталей разных систем уведомлений платформы, уведомлением кросс платформенных отправляемых с помощью одного вызова API, как показано на следующей схеме:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Для отправки push-уведомлений, серверной системе только контакты Azure концентратор уведомлений, который в свою очередь, взаимодействует с разных систем уведомлений платформы, поэтому Уменьшение сложности серверной части кода, отправляет push-уведомлений.

Мобильные приложения Azure включают встроенную поддержку push-уведомления с использованием концентраторов уведомлений. При отправке извещающего уведомления из экземпляра мобильных приложений Azure в приложение Xamarin.Forms выполняется следующим образом:

1. Xamarin.Forms приложение регистрирует PNS, который возвращает дескриптор.
1. Экземпляр мобильных приложений Azure отправляет уведомление его концентратор уведомлений Azure, задающего дескриптор устройства, для которых будет включен.
1. Концентратор уведомлений Azure отправляет уведомление соответствующие PNS для устройства.
1. Служба PNS отправляет уведомление на устройство.
1. Xamarin.Forms приложение обрабатывает уведомление и отображает его.

Образец приложения показывает приложения списка задач, данные которого хранится в экземпляре мобильных приложений Azure. Каждый раз при добавлении нового элемента к экземпляру мобильных приложений Azure, push-уведомление отправляется в приложение Xamarin.Forms. Следующих снимках экрана показано каждой платформы, отображение полученных push-уведомлений.

[![](azure-images/screenshots.png "Образец приложения, получения Push-уведомление")](azure-images/screenshots-large.png#lightbox "образец приложения, получения Push-уведомление")

Дополнительные сведения о концентраторах уведомлений Azure см. в разделе [концентраторов уведомлений Azure](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) и [Добавление push-уведомлений в приложение Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure и настройка системы уведомления платформы

Интеграция концентратор уведомлений Azure в экземпляр мобильных приложений Azure выполняется следующим образом:

1. Создайте экземпляр мобильных приложений Azure. Дополнительные сведения см. в разделе [использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Настройка концентратора уведомления. Дополнительные сведения см. в разделе [Настройка концентратора уведомления](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Обновите экземпляр мобильные приложения Azure для отправки извещающих уведомлений. Дополнительные сведения см. в разделе [обновление project server для отправки извещающих уведомлений](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Зарегистрируйте каждый PNS.
1. Настройка концентратора уведомления для взаимодействия с каждой PNS.

В следующих разделах приведены инструкции дополнительной настройки для каждой платформы.

### <a name="ios"></a>iOS

Следующие дополнительные действия должны выполняться для использования Apple Push Notification Service (APNS) из концентратора уведомлений Azure:

1. Создание запроса для принудительной сертификата с помощью средства доступа к цепочке ключей подписи сертификата. Дополнительные сведения см. в разделе [создать файл запроса подписи сертификата для сертификата push](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) в центре документации Azure.
1. Регистрация приложения Xamarin.Forms для поддержки уведомлений push в центре разработчиков Apple. Дополнительные сведения см. в разделе [регистрации приложения push-уведомления](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) в центре документации Azure.
1. Создание принудительной уведомления включен профиль подготовки для приложения Xamarin.Forms в центре разработчиков Apple. Дополнительные сведения см. в разделе [создайте подготовительный профиль для приложения](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) в центре документации Azure.
1. Настройка концентратора уведомления для взаимодействия с APNS. Дополнительные сведения см. в разделе [Настройка концентратора уведомления для APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Настройте Xamarin.Forms приложению использовать новый идентификатор приложения и подготовительного профиля. Дополнительные сведения см. в разделе [настройки в проекте iOS в Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) или [настройке проекта для iOS в Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) в центре документации Azure.

### <a name="android"></a>Android

Следующие дополнительные действия должны выполняться для использования Firebase облака обмена сообщениями (FCM) из концентратора уведомлений Azure:

1. Зарегистрировать FCM. Ключ API сервера и идентификатор клиента, автоматически создается и упаковываются в `google-services.json` файл, который загружается. Дополнительные сведения см. в разделе [включить Firebase облака обмена сообщениями (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Настройка концентратора уведомления для взаимодействия с FCM. Дополнительные сведения см. в разделе [Настройка завершить обратно мобильные приложения для отправки запросов push с помощью FCM](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

Для использования Windows Notification Service (WNS) из концентратора уведомлений Azure должен выполнять следующие дополнительные действия:

1. Зарегистрировать службу уведомлений Windows (WNS). Дополнительные сведения см. в разделе [Регистрация приложения Windows для push-уведомлений с WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) в центре документации Azure.
1. Настройка концентратора уведомления для взаимодействия с WNS. Дополнительные сведения см. в разделе [Настройка концентратора уведомления для WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) в центре документации Azure.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Добавление поддержки принудительной уведомлений в приложения Xamarin.Forms

В следующих разделах рассматриваются реализацию, необходимых в каждом проекте конкретную платформу для поддержки push-уведомлений.

### <a name="ios"></a>iOS

Реализация поддержки принудительной уведомлений в приложения iOS выполняется следующим образом:

1. Регистрации с Apple Push Notification Service (APNS) в `AppDelegate.FinishedLaunching` метод. Дополнительные сведения см. в разделе [регистрации в системе уведомлений Apple Push](#ios_register).
1. Реализуйте `AppDelegate.RegisteredForRemoteNotifications` метод для обработки ответа регистрации. Дополнительные сведения см. в разделе [обработки ответа регистрации](#ios_registration_response).
1. Реализуйте `AppDelegate.DidReceiveRemoteNotification` метод для обработки входящих push-уведомлений. Дополнительные сведения см. в разделе [обработки входящих Push-уведомления](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Регистрация с помощью службы Push-уведомлений Apple

Прежде чем приложение iOS может получать push-уведомлений, его необходимо зарегистрировать с Apple Push Notification Service (APNS), который создаст токен уникальный устройства и вернуть его в приложение. Вызывается во фрагментах регистрации `FinishedLaunching` переопределить в `AppDelegate` класса:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

При регистрации приложения iOS APNS его необходимо указать типы push-уведомлений, которые хотите получать. `RegisterUserNotificationSettings` Метод регистрирует типы уведомлений, приложение может получать, с `RegisterForRemoteNotifications` метод регистрации для получения push-уведомления из APNS.

> [!NOTE]
> Если не вызывается `RegisterUserNotificationSettings` метода приведет к push-уведомлений, полученных без вмешательства пользователя приложения.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Обработка ответа регистрации

Запрос на регистрацию APNS происходит в фоновом режиме. При получении ответа iOS будет вызывать `RegisteredForRemoteNotifications` переопределить в `AppDelegate` класса:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Этот метод создает шаблон простой уведомления сообщения как JSON и регистрирует устройство для получения уведомлений шаблона от концентратора уведомлений.

> [!NOTE]
> `FailedToRegisterForRemoteNotifications` Переопределения должен быть реализован для обработки ситуаций, например, отсутствует сетевое подключение. Это важно, поскольку пользователи могут запускать приложения на время выполнения вне сети.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Обработка входящих Push-уведомлений

`DidReceiveRemoteNotification` Переопределить в `AppDelegate` класс используется для обработки входящих push-уведомления, когда приложение работает и вызывается при получении уведомления:

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

`userInfo` Словаре `aps` ключ, значение которого является `alert` словарь, оставшиеся данные уведомления. Этот словарь извлекается, с `string` сообщение уведомления, отображаемых в диалоговом окне.

> [!NOTE]
> Если приложение не выполняется при получении push-уведомлений, приложение будет запускаться, но `DidReceiveRemoteNotification` метода не будет обрабатывать уведомление. Вместо этого получить полезные данные уведомления и реагировать соответствующим образом из `WillFinishLaunching` или `FinishedLaunching` переопределяет.

Дополнительные сведения о APNS см. в разделе [Push-уведомления в iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Реализация поддержки принудительной уведомлений в приложения выполняется следующим образом:

1. Добавить [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet пакета в проект Android, а также задать целевой версии приложения для Android 7.0 или более поздней версии.
1. Добавить `google-services.json` файла, загруженные из консоли Firebase в корне проекта Android и задайте его действие построения **GoogleServicesJson**. Дополнительные сведения см. в разделе [добавить файл JSON служб Google](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Регистрация с сообщениями Firebase облака (FCM) путем объявления получателя в манифесте Android правой кнопкой мыши и в реализации `FirebaseRegistrationService.OnTokenRefresh` метод. Дополнительные сведения см. в разделе [регистрация Firebase Cloud Messaging](#android_register_fcm).
1. Регистрация с концентратором уведомлений Azure в `AzureNotificationHubService.RegisterAsync` метод. Дополнительные сведения см. в разделе [регистрации в концентраторе уведомлений Azure](#android_register_azure).
1. Реализуйте `FirebaseNotificationService.OnMessageReceived` метод для обработки входящих push-уведомлений. Дополнительные сведения см. в разделе [отображение содержимого Push-уведомлений](#android_displaying_notification).

Дополнительные сведения об обмене сообщениями облака Firebase см. в разделе [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) и [удаленного уведомления с Firebase Cloud Messaging](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Регистрация в Firebase облако обмена сообщениями

Прежде чем приложение сможет получить push-уведомлений, его необходимо зарегистрировать FCM, которая создания токена регистрации и вернуться в приложение. Дополнительные сведения о маркерах регистрации см. в разделе [регистрации в FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Это достигается путем:

- Объявление получателя в манифесте Android. Дополнительные сведения см. в разделе [объявление приемника в Android манифеста](#declaring_a_receiver).
- Реализация службы Firebase идентификатор экземпляра. Дополнительные сведения см. в разделе [реализации службы идентификатор экземпляра Firebase](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Объявление получателя в манифесте Android

Изменить **AndroidManifest.xml** и вставьте следующий `<receiver>` элементы в `<application>` элемента:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Этот XML-код выполняет следующие операции:

- Объявляет внутреннюю `FirebaseInstanceIdInternalReceiver` реализацию, которая используется для запуска служб безопасно.
- Объявляет `FirebaseInstanceIdReceiver` реализацию, которая предоставляет уникальный идентификатор для каждого экземпляра приложения. Данный получатель также проверяет подлинность и авторизует действия.

`FirebaseInstanceIdReceiver` Получает `FirebaseInstanceId` и `FirebaseMessaging` событий и доставляет их в класс, производный от `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Реализация службы идентификатор экземпляра Firebase

Регистрация приложения с FCM достигается путем создания класса, производного от класса `FirebaseInstanceIdService` класса. Этот класс отвечает за создание маркеров безопасности, который разрешает доступ к FCM клиентское приложение. В примере приложения `FirebaseRegistrationService` класс является производным от `FirebaseInstanceIdService` класса и показано в следующем примере кода:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

`OnTokenRefresh` Метод вызывается, когда приложение получает токен регистрации из FCM. Этот метод извлекает маркер из `FirebaseInstanceId.Instance.Token` свойство, которое обновляется FCM асинхронно. `OnTokenRefresh` Редко вызывается метод, так как маркер обновляется только в том случае, когда приложение установки или удаления, когда пользователь удаляет данные приложений, когда приложение удаляет идентификатор экземпляра, после или маркера безопасности под угрозой. Кроме того что приложение периодически обновляется в свой маркер, обычно каждые 6 месяцев запросит FCM идентификатор экземпляра службы.

`OnTokenRefresh` Метод также вызывает `SendRegistrationTokenToAzureNotificationHub` метод, который позволяет связать маркер регистрации пользователя с концентратором уведомлений Azure.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Регистрация в концентраторе уведомлений Azure

`AzureNotificationHubService` Класс предоставляет `RegisterAsync` метод, который связывает маркер регистрации пользователя с концентратором уведомлений Azure. В следующем примере кода показан `RegisterAsync` метод, который может быть вызван с `FirebaseRegistrationService` класса при изменении маркер регистрации пользователя:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Этот метод создает шаблон простой уведомления сообщения как JSON и регистры для получения шаблона уведомлений из центра уведомлений с помощью токена регистрации Firebase. Это гарантирует, что все уведомления, отправленные из концентратора уведомлений Azure, будут обрабатываться представленного маркером регистрации устройства.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Отображение содержимого Push-уведомление

Отображение содержимого push-уведомление достигается путем создания класса, производного от класса `FirebaseMessagingService` класса. Этот класс включает переопределяемый `OnMessageReceived` указанный метод, который вызывается, когда приложение получает уведомление от FCM, что приложение выполняется на переднем плане. В примере приложения `FirebaseNotificationService` класс является производным от `FirebaseMessagingService` класса и показано в следующем примере кода:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Когда приложение получает уведомление от FCM, `OnMessageReceived` метод извлекает содержимое сообщения и вызывает `SendNotification` метод. Этот метод преобразует содержимое сообщения в локальный уведомление, которое запускается во время выполнения приложения с уведомлением, отображаются в области уведомлений.

##### <a name="handling-notification-intents"></a>Обработка уведомлений целей

Когда пользователь касается уведомления, любые данные, сопровождающие сообщения уведомления доступны в `Intent` дополнения. Эти данные можно извлечь с помощью следующего кода:

```csharp
if (Intent.Extras != null)
{
  foreach (var key in Intent.Extras.KeySet())
  {
    var value = Intent.Extras.GetString(key);
    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
  }
}
```

Запуск приложений `Intent` возникает, когда пользователь касается его сообщение уведомления, этот код будет входить содержащиеся в ней данные `Intent` в окне вывода.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

Прежде чем универсальной платформы Windows (UWP), приложение может получить push-уведомлений, его необходимо зарегистрировать с помощью Windows Notification Service (WNS), который будет возвращать канал уведомления. Вызываемая регистрации `InitNotificationsAsync` метод `App` класса:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Этот метод возвращает канала push-уведомлений, создает шаблон сообщения уведомления, как JSON и регистрирует устройство для получения уведомлений шаблона от концентратора уведомлений.

`InitNotificationsAsync` Метод вызывается из `OnLaunched` переопределить в `App` класса:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Это гарантирует, что регистрации push-уведомлений создается или обновляется каждый раз при запуске приложения, таким образом обеспечивая канала WNS push не всегда активно.

При получении push-уведомление он будет автоматически отображаться в виде *всплывающих* — немодальное окно, содержащее сообщение.

## <a name="summary"></a>Сводка

В этой статье показано, как использование концентраторов уведомлений Azure для отправки push-уведомления из мобильных приложений Azure экземпляра приложения Xamarin.Forms. Концентраторы уведомлений Azure предоставляют масштабируемые push-инфраструктуру для отправки мобильные push-уведомления с любого сервера на любую мобильную платформу, устраняя необходимость связываться с разных систем уведомлений платформы серверной части сложности.


## <a name="related-links"></a>Связанные ссылки

- [Использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Центры уведомлений Azure](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Добавление push-уведомлений в приложение Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Push-уведомлений в iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Обмен сообщениями Firebase Cloud](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Пакет SDK для Azure мобильного клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
