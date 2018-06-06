---
title: Покупки в Xamarin.iOS в приложении
description: В этом документе описывается продавать цифровых продуктов и служб с помощью API-интерфейсы StoreKit. Ссылки на руководства, посвященные конфигурации, использовать продукты, невоспроизводимых продуктов, транзакции, подписки и многое другое.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8a41ed44a331c91a333b95c1d62136244a6945dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787345"
---
# <a name="in-app-purchasing-in-xamarinios"></a>Покупки в Xamarin.iOS в приложении

приложения iOS можно продать цифровых продуктов или служб с помощью StoreKit — набор интерфейсов API, предоставляемые iOS, взаимодействовать с серверами Apple для проведения финансовых операций с пользователем через их Apple ID. API-интерфейсы StoreKit интересует в первую очередь получение сведений о продукта и проведение транзакций — нет ни один компонент пользовательского интерфейса. Приложения, реализующие приобретения в приложении необходимо построить пользовательский интерфейс и отслеживания приобретенных элементов с пользовательским кодом для предоставления необходимых продуктов или служб для пользователя.

Обеспечивает функциональные возможности Покупка из приложения требуются несколько этапов:

-  **Настройка приложения** — профиля подготовки приложения должна быть установлена правильно.
-  **Создание продуктов** -описания продуктов и цен необходимо создать на портале iTunes Connect.
-  **Реализация StoreKit** — StoreKit API должен быть реализован в соответствии с типами, продаваемых продуктов.
-  **Построение пользовательского интерфейса и продуктов, сами** — продукты должен быть реализован, включая механизмы для отслеживания каждого покупки и резервное копирование и восстановление их при необходимости.
-  **Мониторинг продаж и получение средства** — используйте сведения, предоставляемые iTunes Connect для отслеживания доходами тенденции продаж.

В этом документе объясняется, как для выполнения этих действий, чтобы обеспечить покупки из приложений с помощью Xamarin.iOS.

## <a name="requirements"></a>Требования

Для поддержки приобретения в приложении необходимо использовать Xamarin.iOS 5.0 или более поздней версии с Xcode 7 и более поздних версий.

## <a name="contents"></a>Описание

 * [Основные сведения о покупке из приложения и ее конфигурация](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Общие сведения о StoreKit и извлечение сведений о продукции](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Приобретение готовых к использованию продуктов](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Приобретение неготовых к использованию продуктов](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Транзакции и проверки](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Подписки и отчетность](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Сводка

В этой статье есть представляет концепцию покупки из приложений, описанные способы настройки приложения в его преимущества и представлены примеры использования Xamarin.iOS. Оно было рассказано о:

-  **Портал Провизионирования iOS** — функциональные возможности приобретения правила включения в приложении.
-  **iTunes Connect** — Настройка продуктов для продажи в вашем приложении.
-  **Комплект средств для хранения** — описание классов, используемых для выполнения сборки функций Покупка из приложения.
-  **Кодирование приложения по приобретению** — примеры встроить в приложения Xamarin.iOS Покупка из приложения.
-  **Reporting** — Общие сведения о статистические данные, доступные через iTunes Connect.


## <a name="related-links"></a>Связанные ссылки

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [В руководство по программированию на покупку приложения](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [Руководство по iTunes Connect для разработчиков](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Сохранить ссылку на пакет Framework](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Покупка из приложения идентификаторов продуктов, вопросы и ответы](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Покупка из приложения техническое Примечание](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Время отправки первого приложения магазина](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [Центр ресурсов приложения магазина](https://developer.apple.com/appstore/index.html)
- [Советы по отправке в App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Руководство по оценке приложений App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Управление приложений](https://developer.apple.com/appstore/resources/managing/index.html)
