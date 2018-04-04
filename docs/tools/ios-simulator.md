---
title: Удаленное взаимодействие iOS Simulator (для Windows)
description: Проверять и запускать отладку приложений iOS в Visual Studio в Windows
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/07/2017
ms.openlocfilehash: bcf0aa2b1677af0b980c6ca48bb29c1cad32e52d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="remoted-ios-simulator-for-windows"></a>Удаленное взаимодействие iOS Simulator (для Windows)

_Проверять и запускать отладку приложений iOS в Visual Studio в Windows_

[![](ios-simulator-images/hero-sml.png "Симулятор iOS под управлением Windows")](ios-simulator-images/hero.png#lightbox)

## <a name="download-and-install"></a>Загрузка и установка

Загрузить [установщика](https://dl.xamarin.com/xamarin-simulator/Xamarin.Simulator.Installer.msi) и установить на компьютере Windows. Visual Studio Tools для Xamarin должна быть установлена.

> [!NOTE]
> С помощью удаленного iOS Simulator на Visual Studio требуется компьютеру Mac, имеющему с установлен компонент Xamarin.

## <a name="getting-started"></a>Начало работы

Чтобы использовать симулятор удаленного iOS:

1. Убедитесь, что Visual Studio подключен к компьютеру Mac хотя бы один раз перед запуском удаленного симулятор iOS.
2. Убедитесь, приложение iOS или tvOS **запускаемый проект** и начните отладку.

Можно отключить симулятор iOS, удаленный из **Сервис > Параметры > Xamarin > Параметры iOS** , снимите флажок для **удаленного симулятор для Windows** показано ниже:

[![](ios-simulator-images/options-sml.png "флажок, чтобы использовать симулятор")](ios-simulator-images/options.png#lightbox)

Затем открывает симуляторе iOS на подключенном компьютере Mac. Установите этот флажок для включения удаленного iOS simulator.

## <a name="features"></a>Функции

Удаленный симулятор iOS предоставляет способ тестирования и отладки приложений iOS на симуляторе полностью из Visual Studio в Windows.

### <a name="simulator-window"></a>Окна имитатора

Панель инструментов окна включает несколько кнопок для взаимодействия с симулятора.

- **Домашняя** — имитирует кнопка «Главная» на устройстве.
- **Блокировка** — блокирует имитатор (вы можете проведите разблокировать).
- **Снимок экрана** — сохраняет снимок экрана симулятора на диске.
- [**Параметры** ](#settings) — настроить расположение и клавиатуры.
- Другие [ **параметры** ](#options) — разнообразные варианты симуляторе доступны такие как поворот, встряхивания или вызова других состояний, в симуляторе. При некоторых параметров, скрываются, они могли быть доступны из значок с многоточием, который отображается на панели инструментов или щелкнуть правой кнопкой мыши в окне.

    [![](ios-simulator-images/maps-app-sml.png "пример сопоставляет симулятор iOS")](ios-simulator-images/maps-app.png#lightbox)


### <a name="settings"></a>Параметры

Значок «производительность» открывает **параметры** окна:

[![](ios-simulator-images/settings-sml.png "Параметры симулятора iOS")](ios-simulator-images/settings.png#lightbox)

Это дает возможность включения аппаратной клавиатуры на симуляторе и выберите расположение указывается на устройстве (включая статического расположения и других скользящего параметры расположения).



### <a name="other-options"></a>Другие параметры

Щелкните правой кнопкой мыши окно симулятора, чтобы просмотреть все параметры, доступные в имитаторе, такие как поворот, активируя жест встряхивания и перезагрузки имитатор:

[![](ios-simulator-images/more-sml.png "Дополнительные параметры для симулятора iOS")](ios-simulator-images/more.png#lightbox)

### <a name="touchscreen-support"></a>Поддержка сенсорного экрана

Большинство современных компьютеры Windows имеют сенсорных экранов и удаленного iOS simulator позволяет touch окно симулятора, чтобы протестировать взаимодействие с пользователем в приложении iOS.

Сюда входят сжатия, считывания и несколько пальцем сенсорный ввод - вещей, которые ранее может быть легко протестировать их работу с физических устройств.

Поддержка пера в Windows, также преобразуется в ввода Apple карандаша в симуляторе.

<!--
<a name="knownissues" />

# Known Issues

 - Apple Watch devices may show in the Visual Studio device list, but are not yet supported.
 - Launching in **Release** mode may also start Apple’s simulator on the networked Mac.
 - Closing the remote iOS Simulator on Windows will not immediately stop debugging in Visual Studio. Stop debugging manually from the menu or the red button.
 - Opening too many different simulators simultaneously will produce unexpected results.
 - Exception of type `Foundation.NSErrorException` may be thrown while launching Simulators. Workaround is to kill csproxy (server process) on the Mac host and re-deploy to the simulator.
 - Performance may be slower when using Xcode 8
-->
