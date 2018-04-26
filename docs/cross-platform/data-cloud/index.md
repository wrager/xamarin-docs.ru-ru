---
title: Microsoft Azure
description: Документация и примеры кода, загружаемые файлы для Azure.
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/09/2017
ms.openlocfilehash: ab449a58cc87699b97a1ade7721a08f771c4f55d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="microsoft-azure"></a>Microsoft Azure

_Документация и примеры кода, загружаемые файлы для Azure._

[ ![](images/evolve-mikej-azure-sml.png "Функции службы приложений Azure легко добавить для приложений Xamarin, включая Облачное хранилище данных и кросс платформенных push-уведомлений")](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Развивать 2016: Разработка подключенной приложения Azure и Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## <a name="connected-services-in-visual-studio-for-mac"></a>Подключенные службы в Visual Studio для Mac

Новый [подключенные службы](connected-services.md) функции Visual Studio для Mac помогает разработчикам быстро и легко добавлять Azure функциональные возможности для мобильного приложения в Интегрированной среде разработки. В настоящее время доступны для тестирования в альфа-канала.


## <a name="azure-app-services"></a>Службы приложений Azure

Что представляет собой коллекцию [документации мобильные приложения Azure](~/cross-platform/data-cloud/mobile-apps.md) , помогает выполнить процесс реализации [клиент Azure Mobile](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
Xamarin также предлагает пакеты NuGet обмена сообщениями Azure для [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) и [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/) Чтобы реализовать извещающие уведомления в разных платформах.

Настройка приложения на [портал служб Azure приложения](https://portal.azure.com/) для доступа к мобильным приложениям, веб-API, хранения и многое другое. Дополнительные сведения о [чем отличаются службы приложений](http://azure.microsoft.com/updates/whats-new-with-azure-app-service/) и посмотрите в [эти видеоматериалы корпорации Майкрософт](http://azure.microsoft.com/campaigns/azure-march-announcement/).

## <a name="active-directory-authentication"></a>Проверка подлинности Active Directory

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md) можно использовать для входа пользователей в приложений Xamarin через [Xamarin.Auth компонента](https://www.nuget.org/packages/Xamarin.Auth/).
Приложения также будут доступны дополнительные службы, такие как Office 365.

## <a name="webapi"></a>WebAPI

Веб-API корпорации Майкрософт предоставляет интерфейс стиле REST, который можно легко использовать в приложениях Xamarin.
Вы можете легко оборотов [веб-сайта Azure](https://trywebsites.azurewebsites.net/) и построения приложения на основе WebAPI для подключения к приложения Xamarin.


###  <a name="introduction-to-web-servicescross-platformdata-cloudweb-servicesindexmd"></a>[Общие сведения о веб-служб](~/cross-platform/data-cloud/web-services/index.md)

В этом учебнике рассказывается, как интегрировать REST, WCF и SOAP веб-технологии службы с помощью приложений Xamarin для мобильных устройств. Проверяет различные реализации службы, оценивает доступные средства и библиотеки для интеграции их и предоставляет образец шаблона для использования службы данных. Наконец он предоставляет общий обзор создания RESTful веб-службы для использования с мобильного приложения Xamarin.

## <a name="samples"></a>Примеры

В дополнение к [образцы из документации](https://github.com/xamarin/mobile-samples/tree/master/Azure), следующие завершения приложения демонстрируют различные функции Azure, встроенные в приложения Xamarin:

- [Спорт](https://github.com/xamarin/Sport) — приложение для отслеживания понятное спортивных лиги, использующий данные хранилища и принудительная отправка уведомлений.
- [Секунд](https://github.com/pierceboggan/Moments) — мгновенных фотографий, совместное использование, хранилище Azure использует для изображений.
- [Xamarin CRM](https://github.com/xamarin/app-crm) — использует веб-API для серверной части.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) -мобильные приложения Azure.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) — образец для [рядов архитектура](https://www.microsoft.com/net/learn/architecture) из книг.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) — Azure + IoT выборки из 2016 сборки.


## <a name="related-links"></a>Связанные ссылки

- [Пример Azure PCL (по @paulbatum) (пример)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Портал Azure](http://azure.microsoft.com/)
- [Мобильный клиент для Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
