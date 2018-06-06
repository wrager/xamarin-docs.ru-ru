---
title: watchOS возможности платформы
description: Документ содержит ссылки на различные руководства, описывающие возможности watchOS платформы, например Apple Pay, уведомления, сложностей, упреждающее предложения, тренировки приложений и многое другое.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 8ad4dc52c3bca0f54adb64bb97acaa23aeb1e590
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791297"
---
# <a name="watchos-platform-features"></a>watchOS возможности платформы

_Apple Watch функции, характерные для включения в watchOS приложений._

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Усовершенствования оплаты Apple](~/ios/watchos/platform/apple-pay.md)

В watchOS 3 PassKit framework была расширена для поддержки для безопасности, в приложении платежи (физические товары и службы) для приложений, выполняющихся в Apple Watch.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Фоновые задачи](~/ios/watchos/platform/background-tasks.md)

watchOS 3 представлены несколько фоновые задачи, которые приложение может использовать для обновления информации проверку содержимого перед его открытием нужны пользователю.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Введение в watchOS 4](introduction-to-watchos4.md)

Новые возможности в watchOS 4.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Введение в watchOS 3](introduction-to-watchos3/index.md)

В этой статье описаны новые и измененные API-интерфейса в watchOS 3, что и для разработчиков Xamarin.

##  <a name="notificationsnotificationsmd"></a>[Уведомления](notifications.md)

Дополнительные сведения о пользовательских уведомлять обработки в приложении Контрольное значение.

##  <a name="complicationscomplicationsmd"></a>[Усложнения](complications.md)

Добавление поддержки усложнения для отображения свежих данных на Циферблат часов.


## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Упреждающие предложения](~/ios/watchos/platform/proactive-suggestions.md)

watchOS 3 позволяет приложению заранее предоставлять информацию для пользователя в пределах заданного контекстов. Для поддержки этой возможности [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) теперь включает `MapItem` свойство, которое позволяет приложения, которые предоставляют сведения о расположении для дальнейшего использования другими приложениями.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Быстрые методы взаимодействия](~/ios/watchos/platform/quick-interaction-techniques.md)

Обеспечивает взаимодействие с пользователем быстрого необходимы для создания привлекательных приложений Apple Watch и сложностей. Новые для watchOS 3 Apple добавлена поддержка распознавателей жестов, доступ к цифровой Главная и новые методы уведомления пользователя и навигации. Это, а также добавлена поддержка SceneKit и SpriteKit, разработчик мог легко создавать информативные, взгляда интерфейсы, которые являются быстрый и отвечать на запросы.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Усовершенствования приложения для тренировок](~/ios/watchos/platform/workout-apps.md)

Новые watchOS 3 тренировки связанных приложения имеют возможность работать в фоновом режиме на Apple Watch. Чтобы включить эту функцию (и получить доступ к данным HealthKit), необходимо включить приложение `WKBackgroundModes` ключа в `Info.plist` файл со значением `workout-processing`.
