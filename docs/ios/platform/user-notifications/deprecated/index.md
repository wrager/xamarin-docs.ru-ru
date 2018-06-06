---
title: Устаревшие уведомления технологии Xamarin.iOS
description: В этом документе описываются технологии уведомления операций ввода-вывода, которые стали нерекомендуемыми в пользу framework уведомления для пользователей, представленные в iOS 10.
ms.prod: xamarin
ms.assetid: 20C4F6E5-56DF-4A85-BBF0-E38C88586307
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/07/2016
ms.openlocfilehash: 4becc5e296fb6e2496d9ffd863f7137419480262
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788557"
---
# <a name="deprecated-notification-technologies-in-xamarinios"></a>Устаревшие уведомления технологии Xamarin.iOS

В этом разделе показано, как реализовать локальный и push-уведомления в Xamarin.iOS. Он будет приведено описание различных элементов пользовательского интерфейса iOS уведомления о и рассматриваются API-Интерфейс связанные с созданием и отображает соответствующее уведомление.

> [!IMPORTANT]
> Сведения в этом разделе, относятся к iOS 9 и предыдущий, она была оставлена здесь для поддержки более старых версий iOS. Для iOS, 10 и более поздней версии, см. в разделе [руководство по Framework уведомления пользователя](~/ios/platform/user-notifications/index.md) за поддержку локальных и удаленных уведомлений на устройства iOS.

## <a name="sections"></a>Разделы

<a name="Local Notifications In iOS" />

##  <a name="local-notifications-in-ioslocal-notifications-in-iosmd"></a>[Локальный уведомления в iOS](local-notifications-in-ios.md)

В этом разделе описывается, как реализовать локальный уведомления в Xamarin.iOS. Он будет приведено описание различных элементов пользовательского интерфейса iOS уведомления о и рассматриваются API-Интерфейс связанные с созданием и отображает соответствующее уведомление.

<a name="Local Notifications Walkthrough" />

##  <a name="walkthrough---using-local-notifications-in-xamarinioslocal-notifications-in-ios-walkthroughmd"></a>[Пошаговое руководство. Использование локальных уведомлений в Xamarin.iOS](local-notifications-in-ios-walkthrough.md)

В этом разделе мы рассмотрим способы использования локального уведомлений в приложения Xamarin.iOS. Он описывается основные принципы создания и публикации на уведомление, которое появится оповещение при получении приложением.

<a name="Remote Notifications In iOS" />

##  <a name="remote-notifications-in-iosremote-notifications-in-iosmd"></a>[Удаленный уведомления в iOS](remote-notifications-in-ios.md)

В этом разделе мы рассмотрим push-уведомлений в iOS. В ней представлен Apple Push-уведомления шлюза Service (APNS) и роль, которую он играет в публикации уведомлений в приложения iOS. Он объясняется, как создать сертификаты безопасности, которые необходимо включить push-уведомления и обсуждения. Finally в этом разделе рассматриваются некоторые из задач обслуживания, которые серверы приложений должны быть выполнены для отслеживания клиенты мобильных устройств.

## <a name="related-links"></a>Связанные ссылки

- [Уведомления (пример)](https://developer.xamarin.com/samples/monotouch/Notifications/)
