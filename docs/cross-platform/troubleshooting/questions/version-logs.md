---
title: Где найти Мои сведения о версии и журналы?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 9234ee0553b2cc9376c0e4e39ffc0700deaacda1
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Где найти Мои сведения о версии и журналы?

## <a name="outline"></a>Контур

- [Сведения о версии](#version-information)
    - Сведения о версии Windows
    - Сведения о версии Mac
    - Пакет SDK для Android, средства платформы построения средств
- [Журналы интегрированной среды разработки и установки](#ide-and-installer-logs)
    - [Журналы Windows](#windows-logs)
        - Xamarin Studio
        - Xamarin для Visual Studio
        - Xamarin универсального установщика
        - Отдельные `.msi` установщики, подробных журналов
        - Запуск Visual Studio, подробных журналов
    - [Журналы Mac](#mac-logs)
        - Создавайте узла
    - Visual Studio для Mac
        - Xamarin Studio
        - Программа установки Xamarin
- [Выходные данные построения подробных сведений](#verbose-build-output-logs)
- [Отладочный журналы для Xamarin.Android и Xamarin.iOS приложений.](#debug-logs-for-xamarin-apps)
    - Android `adb` logcat журналы
    - симулятор iOS входе (Mac)
    - устройство iOS входе (Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Сведения о версии

Обычно резервное оптимально подходит для отправки всех данных из **сведения о копировании** кнопки. В противном случае мы часто необходимо для получения дополнительных сведений. Например версий операционной системы, Xcode версии Android API уровней, и версия .NET можно все важные при помощи в устранении проблемы.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Сведения о версии Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Справка > о > Показать подробности > сведения о копировании [Кнопка]**

#### <a name="visual-studio"></a>Visual Studio

**Справка > о Microsoft Visual Studio > Копировать сведения [Кнопка]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Сведения о версии Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Visual Studio > о Visual Studio > Показать подробности > сведения о копировании [Кнопка]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Пакет SDK для Android, средства платформы построения средств

Откройте диспетчер Android SDK и снимки экрана верхней **средства** раздела.

![](https://kb.xamarin.com/customer/portal/attachments/337323 "Снимок экрана диспетчер Android SDK > папки "Tools"")

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Сервис > Откройте диспетчер пакета SDK для Android**

#### <a name="visual-studio"></a>Visual Studio

Выберите значок панели инструментов диспетчера пакета SDK:

![](https://kb.xamarin.com/customer/portal/attachments/420238 "Запустите диспетчер Android SDK Manager значок панели инструментов в Visual Studio")

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Журналы интегрированной среды разработки и установки

Для каждого местоположения журнала убедитесь, что присоединение и архивировать папку весь журнал целиком.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Журналы Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Набор средств Visual Studio для Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" /> Visual Studio 2017 г.

[Как получить журналы установки Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" /> «Универсальный» установщик Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Это журналы из `XamarinInstaller.exe` установщика.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Отдельные `.msi` установщики, подробных журналов

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Ссылка: [параметры командной строки](http://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Запуск Visual Studio, подробных журналов

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Ссылка:  [ /log (devenv.exe)](http://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Журналы Mac

Можно выбрать **Go > перейдите в папку** меню элемента в системе поиска, затем скопируйте и вставьте любой из этих путей в диалоговом окне.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio для Mac

`~/Library/Logs/VisualStudio/7.0` (это число может изменяться в зависимости от версии, которую вы используете)

Эту папку можно также открыть через «Help -> открыть каталог журнала».

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (это число может изменяться в зависимости от версии, которую вы используете)

Эту папку можно также открыть через «Help -> открыть каталог журнала».

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />«Универсальный» установщик Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Это журналы из `XamarinInstaller.dmg` установщика.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Узел сборок Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Выходные данные построения подробных сведений

1.  Включить [выходных данных диагностики MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2.  Для приложений iOS, следует также включить **mtouch подробного вывода** путем добавления `-v -v -v -v` под **свойства проекта > сборка iOS > Общие [tab] > Дополнительные параметры > mtouch дополнительных аргументов**.

    **Visual Studio (Windows)**

    [![](https://kb.xamarin.com/customer/portal/attachments/337319 "Введите в поле ввода дополнительных mtouch аргументы четырьмя тире v")](https://kb.xamarin.com/customer/portal/attachments/337319)

    **Visual Studio для Mac**

    [![](https://kb.xamarin.com/customer/portal/attachments/337316 "Введите в поле ввода дополнительных mtouch аргументы четырьмя тире v")](https://kb.xamarin.com/customer/portal/attachments/337316)

3.  Выполните очистку и перестройте проект.

4.  Скопируйте и вставьте выходные данные построения из интегрированной среды разработки в текстовый файл.
     - Visual Studio (Windows): **представление > выходной > Показать выходные данные от: сборка**
     - Visual Studio для Mac: **представление > дополняет > ошибки > выходные данные построения [tab]**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Отладочный журналы для Xamarin.Android и Xamarin.iOS приложений.

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Представление > дополняет > выходных данных приложения**

![](https://kb.xamarin.com/customer/portal/attachments/337290 "Значок планшета выходные данные приложения в Visual Studio для Mac")


(Обратите внимание, что этот пункт меню будут отображаться только в том случае, после запуска приложения.)

### <a name="visual-studio"></a>Visual Studio

**Представление > выходной > Показать выходные данные от: отладка**

![](https://kb.xamarin.com/customer/portal/attachments/337292 "Панель вывода отображается параметр отладки в Visual Studio")

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Android [ `adb` ](http://developer.android.com/tools/help/adb.html) logcat журналы

После выполнения команды `adb` команды, подключения Назад **android_logcat.txt** файл с рабочего стола. Эти инструкции предполагают, что у вас есть только одно устройство.

См. также [Android журнал отладки](~/android/deploy-test/debugging/android-debug-log.md) страницы.

#### <a name="visual-studio"></a>Visual Studio

1.  **Сервис > Android > запустите Android Adb командной строки**
2.  Очистка журнала: `adb logcat -c`
3.  Воспроизвести проблему.
4.  Выходные данные журнала: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1.  **Сервис > Откройте командную строку пакета SDK для Android**
2.  Очистка журнала: `adb logcat -c`
3.  Воспроизвести проблему.
4.  Выходные данные журнала: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />симулятор iOS входе (Mac)

*   Чтобы открыть системный журнал, выберите **Отладка > откройте системный журнал...**  в приложении симулятор iOS.

    ![](https://kb.xamarin.com/customer/portal/attachments/382617 "Отображение откройте системный журнал параметр меню «Отладка»")

*   Чтобы просмотреть отчеты о сбоях в симуляторе, откройте Console.app и перейдите к `~/Library/Logs > DiagnosticReports`.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />устройство iOS входе (Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Представление > дополняет > журнала устройства iOS**

#### <a name="xcode"></a>Xcode 

**Окна > устройства > ${DeviceName}**

Отчеты о сбоях можно найти в разделе **Просмотр журналов устройства** кнопки. В системном журнале устройство отображается в нижней части окна стрелку раскрытия <img alt="Disclosure arrow" src="https://kb.xamarin.com/customer/portal/attachments/382618" style="width: 15px; height: 12px;" />.

#### <a name="xcode-5"></a>Xcode 5

**Окна > Организатор > устройства [tab] > ${DeviceName}**
