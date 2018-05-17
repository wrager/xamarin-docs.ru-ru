---
title: Запуск эмулятора Google Android
description: Отладка приложения с помощью эмулятора Google Android
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 0e290b24c0d7a98b1abaf647fe76e56867042645
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="running-the-google-android-emulator"></a>Запуск эмулятора Google Android

Из этого руководства вы узнаете, как запускать виртуальное устройство в эмуляторе Google Android для отладки и тестирования вашего приложения.

## <a name="using-a-pre-configured-virtual-device"></a>Использование предварительно настроенного виртуального устройства

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В состав Visual Studio входят предварительно настроенные виртуальные устройства, которые отображаются в раскрывающемся меню устройства. Например, на следующем снимке экрана Visual Studio 2017 доступно несколько предварительно настроенных виртуальных устройств:

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[![Виртуальные устройства](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png#lightbox)

Как правило, для тестирования и отладки приложения для телефона следует выбрать виртуальное устройство **VisualStudio\_android-23\_x86\_phone**. Если одно из этих предварительно настроенных виртуальных устройств соответствует вашим требованиям (т. е. соответствует целевому уровню API приложения), перейдите к разделу [Запуск эмулятора](#launching), чтобы приступить к запуску приложения в эмуляторе. (Если вы еще не знакомы с уровнями API Android, см. сведения в статье [Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).)

Если в проекте Xamarin.Android используется уровень целевой платформы, не совместимый с доступными виртуальными устройствами, непригодные для использования виртуальные устройства будут отображаться в раскрывающемся меню под пунктом **Неподдерживаемые устройства**. Например, для следующего проекта целевой платформой является **Android Nougat 7.1 (API 25)**, которая не совместима с предоставляемыми по умолчанию виртуальными устройствами **Android 6.0**:

[![Несовместимое виртуальное устройство](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png#lightbox)

Чтобы изменить минимальную версию Android в соответствии с уровнем API доступных виртуальных устройств, щелкните **Изменить минимальную целевую версию Android**. Кроме того, с помощью **диспетчера эмуляторов Android** можно создать виртуальные устройства, которые поддерживают целевой уровень API, как описано далее в разделе [Настройка виртуальных устройств](#virtualdevice). Перед настройкой виртуальных устройств для нового уровня API сначала необходимо установить соответствующие образы системы для этого уровня API &ndash;, процедура описана в следующем разделе.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

В состав Visual Studio для Mac входят предварительно настроенные виртуальные устройства, которые отображаются в раскрывающемся меню устройства. Например, на следующем снимке экрана Visual Studio 2017 доступны два предварительно настроенных виртуальных устройства:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[![Виртуальные устройства](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png#lightbox)

Как правило, для тестирования и отладки приложения для телефона следует выбрать виртуальное устройство **Android\_Accelerated\_x86**. Если это предварительно настроенное виртуальное устройство соответствует вашим требованиям (т. е. соответствует целевому уровню API приложения), перейдите к разделу [Запуск эмулятора](#launching), чтобы приступить к запуску приложения в эмуляторе. (Если вы еще не знакомы с уровнями API Android, см. сведения в статье [Основные сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Создание настраиваемых виртуальных устройств

Чтобы создать настраиваемые виртуальные устройства, необходимо использовать диспетчер устройств Android Xamarin или устаревший диспетчер эмуляторов Google, который входит в состав пакета SDK для Android. Дополнительные сведения о создании и настройке виртуальных устройств см. в статье [Диспетчер устройств Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Если вы предпочитаете использовать устаревший диспетчер эмуляторов Google, см. сведения в статье [Диспетчер эмуляторов Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md).

Обратите внимание, что при разработке для Android 8.0 Oreo необходимо использовать диспетчер устройств Android Xamarin.

<a name="launching" />

## <a name="launching-the-emulator"></a>Запуск эмулятора

В верхней части интегрированной среды разработки находится раскрывающееся меню для выбора режима **Отладка** или **Выпуск**. При выборе режима **Отладки** отладчик подключается в процессу приложения, выполняемого в эмуляторе. 

После выбора виртуального устройства в раскрывающемся меню устройств выберите режим **Отладка** или **Выпуск**, а затем нажмите кнопку **Воспроизведение**, чтобы запустить приложение:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Режимы "Отладка" и "Выпуск", кнопка "Воспроизведение"](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Режимы "Отладка" и "Выпуск", кнопка "Воспроизведение"](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png#lightbox)

-----

После запуска эмулятора Android Xamarin.Android развернет приложение в эмуляторе. Эмулятор выполняет приложение с настроенным образом виртуального устройства. Ниже приведен пример снимка экрана эмулятора Google Android (в эмуляторе выполняется пустое приложение **MyApp**):

![Эмулятор, выполняющий пустое приложение](running-the-emulator-images/emulator-running.png)

Эмулятор можно оставить в рабочем режиме. Необязательно завершать его работу и перезапускать при каждом запуске приложения. При первом запуске приложения Xamarin.Android в эмуляторе устанавливается общая среда выполнения Xamarin.Android для целевого уровня API, после чего устанавливается приложение. Установка среды выполнения может занять несколько минут. Установка среды выполнения происходит только при первом развертывании приложения Xamarin.Android в эмуляторе &ndash;, последующие развертывания выполняются быстрее, поскольку в эмулятор копируется только приложение.

Дополнительные сведения об использовании эмулятора Google Android см. в следующих разделах для разработчика Android:

-   [Навигация по экрану](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Выполнение основных задач в эмуляторе](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Работа с расширенными элементами управления, параметрами и справкой](https://developer.android.com/studio/run/emulator.html#extended)

