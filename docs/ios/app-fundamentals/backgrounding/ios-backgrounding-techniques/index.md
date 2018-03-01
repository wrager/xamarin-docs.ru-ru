---
title: "Методы Backgrounding iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b623e9a13c7b4fd3854ee6a144d6401720d9ba8c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="ios-backgrounding-techniques"></a>Методы Backgrounding iOS

В следующих разделах мы рассмотрим следующие функции iOS параллельно с существующими параметрами backgrounding:

-  [Нежестких фоновые задачи](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -продлить время работы батареи, запустив фоновые задачи нежестких фрагментами, когда устройство находится в спящем режиме, для других задач по обработке.
-  [Служба фоновой передачи](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - надежно отправлять и загружать файлы, независимо от размера сети состояние или файл.
-  [Фон Fetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -обновление приложения в фоновом режиме интервалы определяемых системой.
-  [Удаленный уведомления](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -push-уведомлений используется для запуска обновления содержимого в фоновом режиме, прежде чем пользователь запускает приложение с возможностью уведомлять пользователя, или автоматическое обновление.
-  **Фоновые обновления пользовательского интерфейса** — Подготовьте пользовательского интерфейса приложения для пользователя и обновление моментального снимка приложения, в фоновом режиме.