---
title: Методы Backgrounding iOS
description: 'В этом документе ссылки на руководства, описывающие различные методы backgrounding в iOS: фоновые задачи, фоновая служба передачи, выборку в фоновом режиме и удаленный уведомления.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783759"
---
# <a name="ios-backgrounding-techniques"></a>Методы Backgrounding iOS

В следующих разделах мы рассмотрим следующие функции iOS параллельно с существующими параметрами backgrounding:

-  [Нежестких фоновые задачи](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -продлить время работы батареи, запустив фоновые задачи нежестких фрагментами, когда устройство находится в спящем режиме, для других задач по обработке.
-  [Служба фоновой передачи](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - надежно отправлять и загружать файлы, независимо от размера сети состояние или файл.
-  [Фон Fetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -обновление приложения в фоновом режиме интервалы определяемых системой.
-  [Удаленный уведомления](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -push-уведомлений используется для запуска обновления содержимого в фоновом режиме, прежде чем пользователь запускает приложение с возможностью уведомлять пользователя, или автоматическое обновление.
-  **Фоновые обновления пользовательского интерфейса** — Подготовьте пользовательского интерфейса приложения для пользователя и обновление моментального снимка приложения, в фоновом режиме.
