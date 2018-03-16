---
title: "Настройка динамической проигрыватель Xamarin"
description: "Изменение и тестирование приложений в режиме реального времени на устройства iOS или Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DDF9203-8826-4B04-93F5-B8D07EDE3873
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/22/2017
ms.openlocfilehash: ddc16dc1faaf623098aad5bca340c15f943223ba
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="xamarin-live-player-setup"></a>Настройка динамической проигрыватель Xamarin

Xamarin Live Player позволяет вносить изменения в режиме реального времени в приложении и воспроизводятся динамическую на устройстве. Код выполняется внутри приложения Xamarin Live Player — не требуется для установки эмуляторов или использовании кабелей для развертывания! В этой статье описывается настройка Xamarin Live Player.

![Функция предварительного просмотра](~/media/shared/preview.png)

## <a name="1-get-the-app"></a>1. Получение приложения

### <a name="xamarin-live-player-for-android"></a>Xamarin динамической Player для Android
Xamarin Live Player доступен для Android в Google Play:

[ ![Доступно на Google Play](install-images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Для устройств Android без Google Play Xamarin Live Player доступна через [HockeyApp](https://aka.ms/xlp-hockeyapp) распространения. Кроме того, предварительной версии сборки для Android можно установить непосредственно из Google Play с включением защиты для [открыть бета-версии программы](https://play.google.com/apps/testing/com.xamarin.live)

### <a name="xamarin-live-player-for-ios"></a>Live Xamarin Player для iOS
Мы рекомендуем пользователям присоединяться к [приложения Xamarin Live Player _iOS предварительного просмотра_ ](https://aka.ms/liveplayeralpha) более быстрый доступ к последним усовершенствованиям через TestFlight.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="2-get-visual-studio-2017-preview-on-windows-or-for-mactabsvsmac"></a>2. Получить 2017 г предварительной версии Visual Studio в Windows (или [для Mac](?tabs=vsmac))

Xamarin Live Player требуются:

- Visual Studio 2017 г [15.4](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/#visualstudio2017) или более поздней версии.
- Компьютер Visual Studio и устройства в той же сети Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. С помощью Xamarin Live Player в первый раз

1. Откройте **2017 г. Visual Studio**.
2. Последовательно выберите пункты **Сервис > Параметры...**  и выберите **Xamarin > другие** вкладки.
3. Деления **включить динамическую проигрыватель Xamarin**:

  ![В окне «Параметры» установите флажок Включить Xamarin Player Live](install-images/vs2017-options.png)

2. Создайте или откройте проект Xamarin (или [пример](~/tools/live-player/samples.md)).
3. Выберите **Live проигрывателя** в списке устройств:

  ![Список устройств включает параметр Xamarin Live Player](install-images/devices-empty-windows.png)

  * Если уже иметь пару устройство, он будет доступен в качестве параметра.
  * В противном случае вам будет предложено пары устройства, при необходимости.
4. Нажмите клавишу **запуска** кнопку или выберите один из следующих вариантов из **запуска** или щелкните правой кнопкой мыши меню:

  - **Запуск без отладки** — можно изменить приложение и см. в разделе происходят изменения на устройстве (приложение перезапускается, как вносятся изменения и сохранить файл).
  - **Запустите отладку** — можно установить точки останова и просматривать значения переменных, но нельзя изменять код.

  Можно также выбрать **средства > Xamarin Live Player > Live запуска текущее представление**, который позволяет изменить приложение и см. в разделе происходят изменения на устройстве. Текущее представление отображается (а не главный экран приложения).

5. Если устройство уже связан и Xamarin Live Player приложение выполняется на устройстве, код будет выполняться прямо сейчас!

  Если устройство не связаны, QR-код будет отображаться с инструкциями о том, как связать устройство:

  ![Пара окно устройства](install-images/manage-empty-windows.png)

  Если не удается связаться с устройства для связывания, может появиться ошибка.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

## <a name="2-get-visual-studio-for-mac-or-for-windowstabsvswin"></a>2. Получить Visual Studio для Mac (или [для Windows](?tabs=vswin))

Xamarin Live Player требуются:

- OS X 10.11 macOS 10.12 или более поздней версии
- Visual Studio для Mac
- Mac и устройствами на одной и той же сети Wi-Fi

## <a name="3-using-xamarin-live-player-for-the-first-time"></a>3. С помощью Xamarin Player динамической в первый раз

1. Откройте **Visual Studio для Mac**.
2. Последовательно выберите пункты **Visual Studio > Параметры...**  и выберите **проекты > Xamarin Player Live (Предварительная версия)** вкладки.
3. Деления **включить динамическую проигрыватель Xamarin**:

  [![В окне «Параметры» установите флажок Включить Xamarin Player Live](install-images/vsmac-options-sml.png)](install-images/vsmac-options.png#lightbox)

2. Создайте или откройте проект Xamarin (или [пример](~/tools/live-player/samples.md)).
3. Выберите **Live проигрывателя** в списке устройств.

  ![Список устройств включает параметр Xamarin Live Player](install-images/devices.png)

  * Если уже иметь пару устройство, он будет доступен в качестве параметра.
  * В противном случае вам будет предложено пары устройства, при необходимости.
  * Выберите **Xamarin Player динамической устройства...**  для управления устройствами, которые вы хотите использовать с Xamarin Live Player.

4. Нажмите клавишу **запуска** кнопку или выберите один из следующих вариантов из **запуска** или щелкните правой кнопкой мыши меню:

  ![Параметры меню выполнения](install-images/run-menu.png)

  - **Запуск без отладки** — можно изменить приложение и см. в разделе происходят изменения на устройстве (приложение перезапускается, как вносятся изменения и сохранить файл).
  - **Запустите отладку** — можно установить точки останова и просматривать значения переменных, но нельзя изменять код.
  - **Запустите текущее представление Live** — можно изменить приложение и см. в разделе происходят изменения на устройстве. Текущее представление отображается (а не главный экран приложения).

5. Если устройство уже связан и Xamarin Live Player приложение выполняется на устройстве, код будет выполняться прямо сейчас!

  Если устройство не объединяется, QR-код будет отображен с инструкциями о том, как связать устройство.

  ![Пара окно устройства](install-images/manage-empty.png)

  Если не удается связаться с устройства для связывания, отображается сообщение об ошибке:

  ![Не удается подключиться к сообщение об ошибке устройства](install-images/error-cannot-connect.png)


-----

Если возникают проблемы, см. раздел [ограничения и устранение неполадок](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Связанные ссылки

- [Ограничения](~/tools/live-player/limitations.md)
- [Устранение неполадок](~/tools/live-player/troubleshooting.md)
- [Образцы динамической проигрыватель Xamarin](~/tools/livehttps://developer.xamarin.com/samples.md)
