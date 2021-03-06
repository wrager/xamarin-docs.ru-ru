---
title: Реализация фрагментов - Пошаговое руководство
description: В этой статье рассматриваются способы фрагменты можно использовать для разработки приложений Xamarin.Android.
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="implementing-fragments---walkthrough"></a>Реализация фрагментов - Пошаговое руководство

_Фрагменты являются самодостаточным, модульные компоненты, которые могут помочь устранить сложности приложений Android, предназначенных для устройств с различными размерами экранов. В этой статье рассматриваются способы создания и использования фрагментов при разработке Xamarin.Android приложений._

## <a name="overview"></a>Обзор

В этом разделе будет рассмотрено создание и использование фрагментов в приложении Xamarin.Android. Это приложение отображает названия несколько играет с верные в списке. Когда пользователь нажимает на заголовке воспроизведения, приложение будет отображать кавычками, воспроизведение в отдельном действии:

[![Приложение, работающее на телефоне с Android в книжной ориентации](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

При повороте телефона альбомная режим, приведет к изменению внешнего вида приложения: список воспроизведения и кавычки будут отображаться в одном действии. При выборе воспроизведения квоты будет отображаться в той же операции:

[![Приложение, работающее на телефоне с Android в альбомной ориентации](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

Наконец Если приложение выполняется на планшете:

[![Приложение, работающее на планшете Android](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

В этом образце приложения можно легко адаптировать для различных форм-факторов и ориентации с минимальными изменениями кода с помощью фрагментов и [альтернативные макеты](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources).

Данные для приложения будет существовать в два массива строк, которые жестко запрограммированы в приложении в виде массивов строка C#. Каждый из массивов будет использоваться как источник данных для одного фрагмента.  Один массив будет содержать имя некоторые играет по верные и другом массиве будет содержать кавычки из этого play. Когда приложение запускается, будет отображаться имена воспроизведения в `ListFragment`. Когда пользователь нажимает на воспроизведение в `ListFragment`, приложение запустится на другое действие, которое отобразит квоты.

Пользовательский интерфейс для приложения будет состоять из двух макеты, один для книжной и по одному для альбомной ориентации. Во время выполнения Android определит, какие макета для загрузки в зависимости от ориентации устройства и предоставит макета в действие для подготовки к просмотру. Вся логика для отвечает на щелчки мышью и отображения данных будет содержащееся во фрагментах. Действия в приложении существует только как контейнеры, содержащие фрагменты.

В этом пошаговом руководстве будет разделить на два руководства по. [Сначала часть](./walkthrough.md) основное внимание уделяется основные части приложения. Будет создан один набор макетов (оптимизированный для книжной ориентации), вместе с двумя фрагменты и два действия:

1. `MainActivity` &nbsp; Это действие при запуске приложения.
1. `TitlesFragment` &nbsp; Этот фрагмент выдаст сообщение список названий воспроизведения, которые были созданы с верные. Будут размещаться на `MainActivity`.
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` Запустится `PlayQuoteActivity` в ответ на выбор воспроизведения в `TitlesFragment`.
1. `PlayQuoteFragment` &nbsp; Этот фрагмент выдаст сообщение предложения из play с верные. Будут размещаться на `PlayQuoteActivity`.

[Второй части примера](./walkthrough-landscape.md) обсудим Добавление альтернативного макета (оптимизированный для альбомной ориентации) обоих фрагментах отображается на экране. Кроме того некоторые небольшие изменения произойдут в код, чтобы приложение будет адаптировать свое поведение число фрагментов, которые одновременно отображается на экране.

## <a name="related-links"></a>Связанные ссылки

- [FragmentsWalkthrough (пример)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [Общие сведения о конструкторе](~/android/user-interface/android-designer/index.md)
- [Реализация фрагментов](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [Пакет поддержки](http://developer.android.com/sdk/compatibility-library.html)
