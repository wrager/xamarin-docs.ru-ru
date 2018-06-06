---
title: Backgrounding в Xamarin.iOS
description: Фоновая обработка или backgrounding — это процесс, позволяющий приложениям выполнять задачи в фоновом режиме, пока другое приложение выполняется на переднем плане. Данное руководство служит вводные сведения о фоновой обработки в iOS.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b22f3ef3276129f7f46c23cc1d06666f151f5ac4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783546"
---
# <a name="backgrounding-in-xamarinios"></a>Backgrounding в Xamarin.iOS

_Фоновая обработка или backgrounding — это процесс, позволяющий приложениям выполнять задачи в фоновом режиме, пока другое приложение выполняется на переднем плане. Данное руководство служит вводные сведения о фоновой обработки в iOS._

Backgrounding в мобильных приложениях существенно отличается от традиционных концепцию многозадачной на рабочем столе. Настольных компьютеров имеют множество ресурсов, доступных для приложения, включая площадь экрана, эффективность и памяти. Приложения, может запустить side-by-side и остаются высокопроизводительных и готовым к использованию. На мобильном устройстве ресурсы гораздо более ограничены. Трудно Показать больше одного приложения на небольшом экране и выполнения нескольких приложений на полной скорости бы расходования аккумулятора. Backgrounding является постоянным компромисс между предоставления ресурсов для выполнения фоновых задач, которые необходимы для выполнения также приложениям и обеспечении реагирования foregrounded приложения и устройства. IOS и Android включают правила для backgrounding, но они обрабатывают очень разными способами.

В iOS backgrounding распознается как состояние приложения и приложения перемещаются в и из него состояние фона в зависимости от поведения приложения и пользователя. операций ввода-вывода также предлагает несколько возможностей для подключения приложения выполняются в фоновом режиме, включая запрос ОС для времени для завершения важные задачи, работающие как тип известном приложении необходимости фона, а обновление содержимого приложения в интервалы.

В этом руководстве и сопутствующие руководства мы собираемся сведения о способах выполнения задач приложения в фоновом режиме. Мы рассматриваются основные понятия и рекомендации и выполните шаг по созданию приложения реального мира, которое получает расположение обновления в фоновом режиме.

## <a name="contents"></a>Описание

1.  [Общие сведения о фоновой обработке в iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [Демонстрация жизненного цикла приложения](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [Методы фоновой обработки в iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [Пошаговые руководства. Фоновый режим в iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [Руководство по фоновой обработке в iOS](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>Сводка

В этом руководстве мы представили различные способы выполнения фоновой обработки в iOS. Мы охваченных iOS состояний приложения и просмотреть роль backgrounding играет в iOS жизненного цикла приложения. Кроме того мы узнали, как нам удалось зарегистрировать отдельных задач или всего приложений для работы в фоновом режиме в iOS. Наконец мы закрепляется наше объяснение backgrounding в iOS с помощью построения приложений, которые выполняют обновления в фоновом режиме.



## <a name="related-links"></a>Связанные ссылки

- [Backgrounding на Android](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (пример)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [Расположение (пример)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Простой фоновой передачи (пример)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS фонового выполнения](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
