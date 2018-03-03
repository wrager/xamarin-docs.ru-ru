---
title: "Служба уведомлений"
description: "В этом руководстве рассматриваются как Android службы может использовать локальный уведомления для отправки сведений пользователю."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6C06FDE7-6385-40EF-AC7C-8EFB54E29F45
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5309b46b67a79225611aafb546b35e73d891b38
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="service-notifications"></a>Служба уведомлений

_В этом руководстве рассматриваются как Android службы может использовать локальный уведомления для отправки сведений пользователю._


## <a name="service-notifications-overview"></a>Общие сведения о службе уведомлений

Службы с помощью уведомлений приложение для отображения сведений для пользователя, даже если приложение Android не находится на переднем плане. Это возможно для уведомлений для выполнения действий пользователя, например, отображение действия из приложения. В следующем образце кода показано, как служба может отправлять уведомления для пользователя:

```csharp
[Service]
public class MyService: Service 
{
    // A notification requires an id that is unique to the application.
    const int NOTIFICATION_ID = 9000;
    
    public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
    {
        // Code omitted for clarity - here is where the service would do something.
    
        // Work has finished, now dispatch anotification to let the user know.
        Notification.Builder notificationBuilder = new Notification.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_notification_small_icon)
            .SetContentTitle(Resources.GetString(Resource.String.notification_content_title))
            .SetContentText(Resources.GetString(Resource.String.notification_content_text));
        
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.Notify(NOTIFICATION_ID, notificationBuilder.Build());
    }
}
```

Этот снимок экрана с примером может служить уведомление, которое отображается:

[![Значок уведомления отображается в строке состояния](service-notifications-images/01-notification-sml.png)](service-notifications-images/01-notification.png)

При слайды пользователя вниз на экран уведомление сверху, отображается полный уведомления:

![Notication, отображаемых в области уведомлений](service-notifications-images/02-fullnotification.png)


## <a name="updating-a-notification"></a>Уведомление об обновлении

Чтобы обновить уведомление, служба будет повторно опубликовать уведомление, используя тот же идентификатор уведомления. Android будет отображать и обновлять уведомления в строке состояния, при необходимости.

```csharp 
void UpdateNotification(string content)
{
   var notification = GetNotification(content, pendingIntent);

   NotificationManager notificationManager = (NotificationManager)GetSystemService(Context.NotificationService);
   notificationManager.Notify(NOTIFICATION_ID, notification);
}

Notification GetNotification(string content, PendingIntent intent)
{
   return new Notification.Builder(this)
           .SetContentTitle(tag)
           .SetContentText(content)
           .SetSmallIcon(Resource.Drawable.NotifyLg)
           .SetLargeIcon(BitmapFactory.DecodeResource(Resources, Resource.Drawable.Icon))
           .SetContentIntent(intent).Build();
}
```

Дополнительные сведения об уведомлениях о доступна в [локального уведомления](~/android/app-fundamentals/notifications/local-notifications.md) раздел [уведомлений Android](~/android/app-fundamentals/notifications/index.md) руководства.


## <a name="related-links"></a>Связанные ссылки

- [Локальный уведомления в Android](~/android/app-fundamentals/notifications/local-notifications.md)
