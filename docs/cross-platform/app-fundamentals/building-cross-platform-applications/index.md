---
title: Создание кросс-платформы приложений
description: В этом разделе описывается, сводку, а также шесть частей построение приложений с помощью платформы разработки Xamarin — из Поняв, как работает Xamarin к разработке приложений для мобильных устройств, тестирование и развертывание в различных хранилищах приложения.
ms.prod: xamarin
ms.assetid: 442FC40A-84DD-A218-0D15-EAD86594B6D7
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/28/2016
ms.openlocfilehash: b9d167d8874ecfde9094bacc26cff3e7d1b10bab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="sharing-code-options"></a>Параметры совместного использования кода

Существует два варианта совместное использование кода между кросс платформенных мобильных приложений: общие проекты активов и переносимой библиотеки классов. Эти параметры являются [обсуждаемые здесь](~/cross-platform/app-fundamentals/code-sharing.md); Дополнительные сведения о [переносимой библиотеки классов](~/cross-platform/app-fundamentals/pcl.md) и [общие проекты](~/cross-platform/app-fundamentals/shared-projects.md) также доступна.

<a name="Sections" />

## <a name="building-cross-platform-mobile-apps"></a>Создание кросс-платформенные мобильные приложения

 [Обзор набора средств Visual Studio для Unity](~/cross-platform/app-fundamentals/building-cross-platform-applications/overview.md)

 [Часть 1 – основные сведения о платформе Xamarin Mobile](~/cross-platform/app-fundamentals/building-cross-platform-applications/understanding-the-xamarin-mobile-platform.md)

 [Часть 2 — архитектура](~/cross-platform/app-fundamentals/building-cross-platform-applications/architecture.md)

 [Часть 3 – Настройка перекрестной Xamarin платформы решения](~/cross-platform/app-fundamentals/building-cross-platform-applications/setting-up-a-xamarin-cross-platform-solution.md)

 [Часть 4 – Работа с несколькими платформами](~/cross-platform/app-fundamentals/building-cross-platform-applications/platform-divergence-abstraction-divergent-implementation.md)

 [Часть 5 – совместное использование стратегии практические кода](~/cross-platform/app-fundamentals/building-cross-platform-applications/practical-code-sharing-strategies.md)

 [Часть 6. Тестирование и утверждение магазина приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/testing-and-app-store-approvals.md)

 <a name="Cross-Platform_Mobile_Application_Case_Studies" />


## <a name="case-studies"></a>Примеры использования

Принципы, описанные в этом документе, помещаются в рекомендаций в демонстрационном приложении *Tasky*, а также [готовые приложения](https://xamarin.com/prebuilt) как [Xamarin CRM](https://xamarin.com/prebuilt/#xamarincrm).

 <a name="Tasky" />


### <a name="tasky"></a>Tasky

Tasky — это приложение список дел простой для iOS, Android и Windows Phone.
Он демонстрирует основные принципы создания кросс платформенные приложения с помощью Xamarin и использует локальную базу данных SQLite.

 [![Список tasky](images/iphone-list-sml.png)](images/iphone-list.png#lightbox) [ ![tasky списка](images/iphone-list-sml.png)](images/iphone-list.png#lightbox)

Чтение [Tasky практический пример](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md).


## <a name="summary"></a>Сводка

В этом разделе представлены средства разработки приложений Xamarin и описывается создание приложений, предназначенных для нескольких мобильных платформ.

Он охватывает многоуровневой архитектуры структуры кода для повторного использования в нескольких платформ и описание различных программных шаблонов, которые могут использоваться в этой архитектуре.

Примеры предоставляют общие функции приложения (например, файл и сетевые операции) и как они могут строиться кросс платформенных способом.

Наконец он кратко рассматривается проверка и предоставляет ссылки на обучающий материал, помещает эти принципы в действие.



## <a name="related-links"></a>Связанные ссылки

- [Параметры совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md)
- [Практический пример: Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
- [Tasky пример приложения (github)](https://developer.xamarin.com/samples/mobile/TaskyPortable/)
- [Разработка приложений для мобильных устройств Xamarin: Кросс платформенных C# и принципы работы с Xamarin.Forms](http://www.amazon.com/Xamarin-Mobile-Application-Development-Cross-Platform/dp/1484202155/))
- [Разработка мобильных приложений с помощью C#, Грег Shackles (O'Reilly)](http://shop.oreilly.com/product/0636920024002.do)
- [Профессиональные кросс платформенной разработки мобильных приложений на языке C# Олсон Скотт, Джон Хантер, Бен Horgen, Goers Кенни (Wrox)](http://www.wiley.com/WileyCDA/WileyTitle/productCd-1118157702.html)
