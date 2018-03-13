---
title: "Устранение неполадок Android SDK Emulator"
description: "Как определять и устранять проблемы в Android SDK Emulator"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 486df3bbee3f8af511140e2d287f9f95571c7b3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="android-sdk-emulator-troubleshooting"></a>Устранение неполадок Android SDK Emulator

В этой статье вы найдете объяснение наиболее типичных ошибок и предупреждений Android SDK Emulator (и их решения).
 
<a name="perfwarn" />

## <a name="performance-warnings"></a>предупреждения производительности

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Начиная с Visual Studio 2017 версии 15.4, могут появляться предупреждения о производительности при первом развертывании приложения в Android SDK Emulator. Причины появления этих предупреждений описываются ниже.

### <a name="computer-does-not-contain-an-intel-procesor"></a>В компьютере не установлен процессор Intel

![В компьютере не установлен процессор Intel](troubleshooting-images/01-no-intel-processor.png)

Это диалоговое окно появляется, если в компьютере не установлен процессор Intel, который необходим для ускорения в Android SDK Emulator. Если в вашем компьютере не установлен процессор Intel, мы рекомендуем для разработки использовать физическое устройство с Android.

### <a name="hyper-v-is-installed-or-active"></a>Установлен или активен Hyper-V

![Установлен или активен Hyper-V](troubleshooting-images/02-hyper-v-active.png)

Это диалоговое окно появляется, если установлен или активен Hyper-V, и он должен быть отключен. В разделе [Отключение Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) описывается решение этой проблемы. 

### <a name="haxm-is-not-installed"></a>HAXM не установлен

![HAXM не установлен](troubleshooting-images/03-haxm-not-installed.png)

Это сообщение говорит о том, что в компьютере установлен процессор Intel, Hyper-V отключен, но HAXM не установлен.
В разделе [Установка HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) описывается процедура установки HAXM.

### <a name="haxm-process-not-running"></a>Процесс HAXM не запущен

![Процесс HAXM не запущен](troubleshooting-images/04-haxm-process-not-running.png)

Это диалоговое окно появляется, если в компьютере установлен процессор Intel, Hyper-V отключен, HAXM установлен, но процесс HAXM не запущен. Для решения этой проблемы откройте командную строку и выполните следующие команды:

```cmd
sc query intelhaxm
```

Если процесс HAXM запущен, то вы увидите примерно следующий вывод команды:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


Если параметр **СОСТОЯНИЕ** не равен **ВЫПОЛНЯЕТСЯ**, то для решения этой проблемы см. раздел [Использование Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).


### <a name="other-failures"></a>Неизвестная ошибка

![Неизвестная ошибка](troubleshooting-images/05-other-failure.png)

Это диалоговое окно появляется, если в компьютере установлен процессор Intel, Hyper-V отключен, HAXM установлен, процесс HAXM запущен, но эмулятор не удалось запустить по неизвестной причине.
Сведения о решении этой проблемы см. в разделе [Использование Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="disabling-performance-warnings"></a>Отключение предупреждений о производительности

При необходимости предупреждения о производительности можно отключить. В Visual Studio нажмите **Инструменты > Параметры > Xamarin > Параметры Android** и отключите параметр **Предупреждать, если ускорение виртуальных устройств Android не поддерживается (HAXM)**:

[![Отключение предупреждений о проблемах с ускорением виртуальных устройств Android](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Начиная с Visual Studio для Mac версии 7.2 (сборка 559), могут появляться предупреждения о производительности при первом развертывании приложения в Android SDK Emulator. Причины появления этих предупреждений описываются ниже.

### <a name="haxm-is-not-installed"></a>HAXM не установлен

![HAXM не установлен](troubleshooting-images/03-haxm-not-installed.png)

Это сообщение говорит о том, что HAXM не установлен.
В разделе [Установка HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm) описывается процедура установки HAXM.

### <a name="haxm-process-not-running"></a>Процесс HAXM не запущен

![Процесс HAXM не запущен](troubleshooting-images/04-haxm-process-not-running.png)

Это сообщение появляется, если процесс HAXM не запущен. Подробные сведения о решении этой проблемы см. в разделе [Использование Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### <a name="other-failures"></a>Неизвестная ошибка

![Неизвестная ошибка](troubleshooting-images/05-other-failure.png)

Это сообщение появляется, если эмулятор не запускается по неизвестной причине. Сведения о решении этой проблемы см. в разделе [Использование Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

-----


## <a name="solutions-to-common-problems"></a>Решение типичных проблем

Многие типичные проблемы с Android SDK Emulator можно решить путем изменения конфигурации вашего компьютера или установки дополнительного программного обеспечения. В следующих разделах описываются такие проблемы и приводится их решение.


### <a name="deployment-issues"></a>Проблемы развертывания

Если отображается ошибка о сбое при установке APK на ваш компьютер или при запуске Android Debug Bridge (**adb**), убедитесь, что Android SDK может подключиться к эмулятору. Для этого выполните следующее:

1. Запустите эмулятор из **Диспетчера виртуальных устройств Android (AVD)** (выберите ваше виртуальное устройство и зажмите **Запустить**).

2. Откройте командную строку и перейдите в папку, в которой установлен **adb**. Например, в Windows это может быть путь **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**.

3. Введите следующую команду:

   ```shell
   adb devices
   ```

4. Если эмулятор доступен из Android SDK, то он отобразится в списке подключенных устройств. Пример:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. Если эмулятор не появился в этом списке, запустите **Диспетчер пакетов SDK для Android**, примените все обновления и запустите эмулятор еще раз.



### <a name="haxm-issues"></a>Проблемы с HAXM

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Если Android SDK Emulator не запускается, то обычно это вызвано проблемами с HAXM. Проблемы с HAXM часто вызваны конфликтом с другими технологиями виртуализации, неправильной конфигурацией или устаревшим драйвером HAXM.

<a name="virt-conflicts" />

#### <a name="haxm-virtualization-conflicts"></a>Конфликты HAXM с другими технологиями виртуализации

HAXM может конфликтовать с другими технологиями, использующими виртуализацию, такими как Hyper-V, Windows Device Guard и некоторые антивирусы:

- **Hyper-V** &ndash; если на вашем компьютере с Windows включен Hyper-V, то см. раздел [Отключение Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv).

- **Device Guard** &ndash; Device Guard и Credential Guard могут препятствовать отключению Hyper-V на компьютерах с Windows. Порядок отключения Device Guard и Credential Guard см. в разделе [Отключение Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Антивирусное ПО** &ndash; если на вашем компьютере запущенно антивирусное ПО, использующее аппаратную виртуализацию (например, Avast), отключите или удалите его, перезагрузите компьютер и снова запустите Android SDK Emulator.


#### <a name="incorrect-bios-settings"></a>Неправильные настройки BIOS

Если вы используете HAXM на компьютере с Windows, HAXM не заработает, пока технология виртуализации (Intel VT-x) не будет включена в BIOS. Если VT-x отключен, то при попытке запуска Android SDK Emulator вы получите следующую ошибку:

**Компьютер удовлетворяет требованиям для запуска HAXM, но технология виртуализации Intel (VT-x) отключена.**

Для исправления этой ошибки перезагрузите компьютер в BIOS, включите VT-x и SLAT (трансляция адресов второго уровня) и перезагрузите компьютер обратно в Windows.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Если Android SDK Emulator не запускается, то обычно это вызвано проблемами с HAXM. Проблемы с HAXM часто вызваны конфликтом с другими технологиями виртуализации, неправильной конфигурацией или устаревшим драйвером HAXM. Переустановите драйвер HAXM при помощи процедуры, описанной в разделе [Установка HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----
