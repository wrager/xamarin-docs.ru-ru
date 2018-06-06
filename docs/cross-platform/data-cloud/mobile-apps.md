---
title: Мобильные приложения Microsoft Azure
description: Документ содержит ссылки на руководства, описывающие, как создать приложение Xamarin, подключенный к Azure. В нем описывается работа с Xamarin компонента Azure, пользователи и push-уведомлений.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: baa687bfb3b2e8306e70e83b6a6ee54595110860
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780634"
---
# <a name="microsoft-azure-mobile-apps"></a>Мобильные приложения Microsoft Azure

_Образцы кода и кода загружаемые файлы для документации портала Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started  http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data   http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push   http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs  http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data    http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


Эти ссылки используются для доступны в документации по Xamarin [мобильных приложений Azure](https://docs.microsoft.com/azure/app-service-mobile/) веб-сайта.
Добавление функциональности Azure приложение Xamarin, загрузив [клиент Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Работа с компонентом Xamarin Azure

Общие документации [работы с Xamarin клиентской библиотеки (компонент)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) для выполнения различных задач с помощью мобильных приложений Azure. Эта страница содержит большое количество фрагменты кода, без подробные описания и примеры, доступных для каждого из перечисленных ниже статей пошагового руководства.

## <a name="getting-started"></a>Начало работы

Также приводятся пошаговые инструкции для получения первого приложения Xamarin Azure запуска.
Она охватывает Создание нового мобильного приложения Azure на портале Загрузка и запуск предварительно настроенного приложения.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Начало работы с пользователями

Приведены подробные инструкции по настройке и кодирование экран входа с помощью мобильных служб Azure. Поддерживаемых поставщиков проверки подлинности включают Microsoft, Facebook, Google и Twitter.

-  [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Авторизация пользователей в сценарии

Пример кода для серверных системах Javascript

-  [TODO.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Начало работы с принудительной отправкой

Выполните инструкции для настройки push-уведомлений Apple и Google веб-узлов, а затем отправить push-уведомление из мобильных служб Azure на устройство.

-  [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
-  [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)


## <a name="get-started-with-notification-hubs"></a>Начало работы с концентраторами уведомлений

Завершите настройку push-уведомлений на сайте Apple и Google, настройки концентратор уведомлений Azure и затем создать push-уведомлений на устройства.

-  [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
-  [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)



## <a name="related-links"></a>Связанные ссылки

- [Приступая к работе (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure мобильного клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Путь обучение мобильные приложения Azure](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
