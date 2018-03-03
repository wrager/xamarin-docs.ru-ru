---
title: "Мобильные приложения Microsoft Azure"
description: "Образцы кода и кода загружаемые файлы для документации портала Azure."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: fb3c26b7d090ca42328c61192c794dec1544d1d3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-azure-mobile-apps"></a>Мобильные приложения Microsoft Azure

_Образцы кода и кода загружаемые файлы для документации портала Azure._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
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


Эти ссылки используются для доступны в документации по Xamarin [мобильных приложений Azure](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) веб-сайта.
Добавление функциональных возможностей Azure Xamarin приложения сложнее, чем загрузка [клиент Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Работа с компонентом Xamarin Azure

Общие документации [работы с Xamarin клиентской библиотеки (компонент)](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) для выполнения различных задач с помощью мобильных приложений Azure. Эта страница содержит большое количество фрагменты кода, без подробные описания и примеры, доступных для каждого из перечисленных ниже статей пошагового руководства.

## <a name="getting-started"></a>Начало работы

Также приводятся пошаговые инструкции для получения первого приложения Xamarin Azure запуска.
Она охватывает Создание нового мобильного приложения Azure на портале Загрузка и запуск предварительно настроенного приложения.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## <a name="validate-modify-and-augment-data-in-scripts"></a>Проверить, изменения и дополнения данных в скриптах

Демонстрирует способы добавления скриптов на стороне сервера к таблицам данных мобильных служб Azure для реализации проверки на стороне сервера и другие функциональные возможности.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## <a name="add-paging-to-data"></a>Добавление данных разбиения на страницы

Краткий пример постраничного просмотра больших наборов данных с помощью Skip() и Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## <a name="get-started-with-users"></a>Начало работы с пользователями

Приведены подробные инструкции по настройке и кодирование экран входа с помощью мобильных служб Azure. Поддерживаемых поставщиков проверки подлинности включают Microsoft, Facebook, Google и Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## <a name="authorize-users-in-scripts"></a>Авторизация пользователей в сценарии

Пример кода для серверных системах Javascript

-  [TODO.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## <a name="get-started-with-push"></a>Начало работы с принудительной отправкой

Выполните инструкции для настройки push-уведомлений Apple и Google веб-узлов, а затем отправить push-уведомление из мобильных служб Azure на устройство.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## <a name="get-started-with-notification-hubs"></a>Начало работы с концентраторами уведомлений

Завершите настройку push-уведомлений на сайте Apple и Google, настройки концентратор уведомлений Azure и затем создать push-уведомлений на устройства.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## <a name="related-links"></a>Связанные ссылки

- [Приступая к работе (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Пример Azure PCL (по @paulbatum) (пример)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure мобильного клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Путь обучение мобильные приложения Azure](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
