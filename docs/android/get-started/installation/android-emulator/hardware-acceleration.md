---
title: Аппаратное ускорение эмулятора Android
description: Сведения о подготовке компьютера для максимальной производительности эмулятора Google Android
ms.prod: xamarin
ms.assetid: 915874C3-2F0F-4D83-9C39-ED6B90BB2C8E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/10/2018
ms.openlocfilehash: b5c20eb9f40bb4c4981d6b60b9fd4bc75fd29336
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-hardware-acceleration"></a>Аппаратное ускорение эмулятора Android

Эмулятор Google Android работает слишком медленно без аппаратного ускорения. Вы можете значительно повысить производительность эмулятора Google Android с помощью специальных образов эмулятора для оборудования на базе процессора x86 и одной из двух технологий виртуализации:

1. **Microsoft Hyper-V и платформа гипервизора** &ndash; Hyper-V — это компонент виртуализации в Windows 10, с помощью которого можно запускать виртуализированные компьютерные системы поверх физического узла. Это рекомендованная технология виртуализации для ускорения образов эмулятора Google Android. Дополнительные сведения о Hyper-V см. в [руководстве по Hyper-V в Windows 10](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/).
2. **Hardware Accelerated Execution Manager (HAXM) от Intel** &ndash; Это механизм виртуализации для компьютеров на базе процессоров Intel. Это рекомендованный механизм виртуализации для разработчиков, которые не могут использовать Hyper-V.

Диспетчер пакетов SDK для Android автоматически использует аппаратное ускорение по возможности при запуске образа эмулятора для виртуального устройства на базе **x86** (как описано в разделе [Конфигурация и использование](~/android/deploy-test/debugging/android-sdk-emulator/index.md)).

## <a name="hyper-v-overview"></a>Обзор Hyper-V

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](~/media/shared/preview.png)

> [!NOTE]
> Hyper-V в настоящее время поддерживается в предварительной версии.

Мы настоятельно рекомендуем разработчикам, работающим в Windows 10 (обновление за апрель 2018 года), использовать Microsoft Hyper-V. Средства Visual Studio для Xamarin упрощают тестирование и отладку приложений Xamarin.Android в ситуациях, когда невозможно или неудобно использовать устройство Android.

Чтобы приступить к работе с Hyper-V и эмулятором Google Android:

1. **Обновите Windows 10 до версии за апрель 2018 года (сборка 1803)** &ndash; Чтобы проверить версию Windows, в Кортане нажмите на панель поиска и введите **Сведения**. В результатах поиска выберите **Сведения о компьютере**. Прокрутите диалоговое окно **Сведения** до раздела **Спецификации Windows**. **Версия** должна быть не ранее 1803:

    [![Спецификации Windows](hardware-acceleration-images/win/12-about-windows.w10-sml.png)](hardware-acceleration-images/win/12-about-windows.w10.png#lightbox)

2. **Включите Hyper-V и платформу гипервизора Windows** &ndash;На панели поиска в Кортане введите **Включение или отключение компонентов Windows**.
   Прокрутите диалоговое окно **Компоненты Windows** и убедитесь, что **Платформа гипервизора Windows** включена.

    [![Hyper-V и платформа гипервизора Windows включены](hardware-acceleration-images/win/13-windows-features.w10-sml.png)](hardware-acceleration-images/win/13-windows-features.w10.png#lightbox)

    Возможно, потребуется перезагрузить Windows после включения Hyper-V и платформы гипервизора Windows.

3. **Установите [Visual Studio 15.8, предварительная версия 1](https://aka.ms/hyperv-emulator-dl)** &ndash; В этой версии Visual Studio поддерживается интегрированная среда разработки для запуска эмулятора Google Android с поддержкой Hyper-V.

4. **Установите пакет эмулятора Google Android 27.2.7 или более поздней версии** &ndash; Для установки пакета перейдите в раздел **Сервис > Android > Диспетчер пакетов SDK для Android** в Visual Studio. Откройте вкладку **Инструменты** и убедитесь, что установлена версия эмулятора Android не ранее 27.2.7.

    [![Диалоговое окно "Пакеты SDK и инструменты для Android"](hardware-acceleration-images/win/14-sdk-manager.w158-sml.png)](hardware-acceleration-images/win/14-sdk-manager.w158.png#lightbox)

5. Если версия эмулятора Android ранее 27.3.1, выполните дополнительный шаг, описанный в разделе **Известные проблемы** (далее).


### <a name="known-issues"></a>Известные проблемы

-   В версиях эмулятора от 27.2.7 до 27.3.1 применяется следующий обходной путь для использования Hyper-V:
    1.  В папке **C:\\Пользователи\\_имя пользователя_\\.android** создайте файл **advancedFeatures.ini**, если он еще не существует.
    2.  Добавьте следующую строку в файл **advancedFeatures.ini**:
        ```
        WindowsHypervisorPlatform = on
        ```

-   Производительность может снизиться на некоторых процессорах Intel и AMD.

-   Загрузка и развертывание приложения Android может занимать слишком много времени.

-   Ошибка доступа к MMIO может периодически мешать загрузке эмулятора Android. Перезапустите эмулятор для решения этой проблемы.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Hyper-V поддерживается только в Windows 10. Дополнительные сведения см. в разделе [Требования к Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v#check-requirements).

-----

## <a name="haxm-overview"></a>Обзор решения HAXM

HAXM представляет собой аппаратную подсистему виртуализации (гипервизор), которая использует технологию виртуализации Intel (VT) для ускорения эмуляции приложения Android на хост-компьютере. В сочетании с образами эмулятора Android x86, предоставляемыми компанией Intel, а также официальным диспетчером пакетов SDK для Android решение HAXM позволяет повысить производительность эмуляции Android в системах, поддерживающих технологию виртуализации. 

Если вы осуществляете разработку на компьютере с ЦП Intel с поддержкой технологии виртуализации, применение HAXM позволит значительно ускорить работу эмулятора Google Android (если вы не уверены, поддерживает ли ваш ЦП технологию виртуализации, ознакомьтесь с разделом [Как определить, поддерживает ли ваш процессор технологию виртуализации Intel](https://www.intel.com/content/www/us/en/support/processors/000005486.html)).

> [!NOTE]
> Вы не можете запускать эмулятор на базе ускоренной виртуальной машины внутри другой виртуальной машины, например ВМ под управлением VirtualBox, VMWare или Docker. Эмулятор Google Android следует запускать [непосредственно на системном оборудовании](https://developer.android.com/studio/run/emulator-acceleration.html#extensions).

Прежде чем использовать эмулятор Google Android в первый раз, рекомендуется проверить наличие установленного решения HAXM и его доступность для эмулятора Google Android.

### <a name="verifying-haxm-installation"></a>Проверка установки HAXM

Сведения о доступности решения HAXM приводятся в окне **Запуск эмулятора Android**, которое отображается во время запуска эмулятора. Чтобы запустить эмулятор Google Android, выполните следующие действия:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите диспетчер эмуляторов Android, выбрав **Сервис > Android > Диспетчер эмуляторов Android**:

    [![Расположение элементов меню диспетчера эмуляторов Android](hardware-acceleration-images/win/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/win/01-avd-manager-menu-item.png#lightbox)

2. Если отображается аналогичное приведенному ниже диалоговое окно **Предупреждение о производительности**, значит, на вашем компьютере решение HAXM еще не установлено или настроено неправильно:

    ![Диалоговое окно "Предупреждение о производительности" с информацией об отсутствии готового решения HAXM](hardware-acceleration-images/win/11-perf-warn.png)

   Если выводится аналогичное показанному диалоговое окно **Предупреждение о производительности**, ознакомьтесь с разделом [Предупреждения о производительности](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn), чтобы выявить возможную проблему и устранить ее причину.

3. Выберите образ **x86** (например, **VisualStudio\_android-23\_x86\_phone**), нажмите кнопку **Пуск** и щелкните **Запустить**:

    ![Запуск эмулятора Google Android с использованием образа виртуального устройства по умолчанию](hardware-acceleration-images/win/02-start-default-avd.png)

4. Во время запуска эмулятора следите за содержимым диалогового окна **Запуск эмулятора Android**. Если решение HAXM установлено, появится сообщение **Решение HAXM установлено, и эмулятор работает в быстром режиме виртуализации**, как показано на следующем снимке экрана:

    ![Диалоговое окно "Запуск эмулятора Android" при наличии доступного решения HAXM](hardware-acceleration-images/win/03-haxm-detected.png)

   Если это сообщение не отображается, возможно, решение HAXM не установлено. Например, о такой ситуации может свидетельствовать сообщение, показанное на следующем снимке экрана:

    ![На этом компьютере решение HAXM недоступно](hardware-acceleration-images/win/04-haxm-error.png)

   Если решение HAXM на вашем компьютере недоступно, установите его, выполнив действия, приведенные в следующем разделе.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите диспетчер эмуляторов Android, выбрав **Сервис > Google > Диспетчер эмуляторов**:

    [![Расположение элементов меню диспетчера эмуляторов Android](hardware-acceleration-images/mac/01-avd-manager-menu-item-sml.png)](hardware-acceleration-images/mac/01-avd-manager-menu-item.png#lightbox)

2. Если отображается аналогичное приведенному ниже диалоговое окно **Предупреждение о производительности**, значит, на вашем компьютере решение HAXM еще не установлено или настроено неправильно:

    ![Диалоговое окно "Предупреждение о производительности" с информацией об отсутствии готового решения HAXM](hardware-acceleration-images/mac/04-avd-warning.png)

   Если выводится аналогичное показанному диалоговое окно **Предупреждение о производительности**, ознакомьтесь с разделом [Предупреждения о производительности](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#perfwarn), чтобы выявить возможную проблему и устранить ее причину.

3. Выберите образы **x86** (например, **Android\_Accelerated\_x86**), нажмите кнопку **Пуск** и щелкните **Запустить**:

    [![Запуск эмулятора Google Android с использованием образа виртуального устройства по умолчанию](hardware-acceleration-images/mac/02-start-default-avd-sml.png)](hardware-acceleration-images/mac/02-start-default-avd.png#lightbox)

3. Во время запуска эмулятора следите за содержимым диалогового окна **Запуск эмулятора Android**. Если решение HAXM установлено, появится сообщение **Решение HAXM установлено, и эмулятор работает в быстром режиме виртуализации**, как показано на следующем снимке экрана:

    ![Диалоговое окно "Запуск эмулятора Android" при наличии доступного решения HAXM](hardware-acceleration-images/mac/03-haxm-detected.png)

   Если на вашем компьютере решение HAXM недоступно (например, если отображается сообщение об ошибке вида _Проверьте правильность установки и работоспособность решения Intel HAXM_), установите его, выполнив действия, приведенные в следующем разделе.


-----

<a name="install-haxm" />

### <a name="installing-haxm"></a>Установка HAXM

Если эмулятор не запускается, возможно, решение HAXM придется установить вручную. Пакеты установки HAXM для ОС Windows и macOS можно найти на странице [Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager). Чтобы вручную скачать и установить решение HAXM, выполните следующие действия:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Скачайте с веб-сайта Intel последнюю версию установщика [подсистемы виртуализации HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) для ОС Windows. Скачивая установщик HAXM с веб-сайта Intel, вы гарантированно получаете последнюю версию этой программы.

   Также можно скачать установщик HAXM с помощью диспетчера пакетов SDK (в диспетчере пакетов SDK выберите **Сервис > Дополнения > Ускоритель эмулятора Intel x86 (установщик HAXM)**). Как правило, SDK для Android скачивает установщик HAXM в следующий каталог:

   **C:\\Program Files (x86)\\Android\\android-sdk\\extras\\intel\\Hardware\_Accelerated\_Execution\_Manager**

   Обратите внимание, что диспетчер пакетов SDK не устанавливает решение HAXM, а просто скачивает его установщик в указанный каталог. Запускать программу установки по-прежнему необходимо вручную.

2. Чтобы начать работу с установщиком HAXM, запустите файл **intelhaxm-android.exe**. Примите значения по умолчанию, предлагаемые в диалоговых окнах установщика:

   ![Окно программы установки Intel Hardware Accelerated Execution Manager](hardware-acceleration-images/win/05-haxm-installer.png)

## <a name="hardware-acceleration-and-amd-cpus"></a>Аппаратное ускорение и процессоры AMD

Так как эмулятор Android от Google сейчас поддерживает аппаратное ускорение AMD [только на Linux](https://developer.android.com/studio/run/emulator-acceleration.html#dependencies), аппаратное ускорение недоступно для компьютеров на платформе AMD под управлением Windows.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Скачайте с веб-сайта Intel последнюю версию установщика [подсистемы виртуализации HAXM](https://software.intel.com/en-us/android/articles/intel-hardware-accelerated-execution-manager/) для ОС macOS.

2. Запустите установщик HAXM. Примите значения по умолчанию, предлагаемые в диалоговых окнах установщика:

   [![Окно программы установки Intel Hardware Accelerated Execution Manager](hardware-acceleration-images/mac/05-haxm-installer-sml.png)](hardware-acceleration-images/win/05-haxm-installer.png#lightbox)

-----


## <a name="related-links"></a>Связанные ссылки

* [Запуск приложений в эмуляторе Android](https://developer.android.com/studio/run/emulator)
