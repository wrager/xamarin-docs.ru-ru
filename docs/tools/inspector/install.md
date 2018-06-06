---
title: Требования к установке инспектора и
description: В этом документе описывается установка инспектора Xamarin и описание поддерживаемых операционных систем, интегрированных средах разработки и платформах для приложений.
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: 80bf3cb4e8e27355ccf6213dbfd07a17e992961b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793812"
---
# <a name="inspector-installation-and-requirements"></a>Требования к установке инспектора и

## <a name="download-and-installation"></a>Загрузка и установка

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

1. Загрузите и установите [Xamarin книг & окон инспектора для](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
2. [Проверки вашего собственного приложения!](~/tools/inspector/inspect.md)

# <a name="macostabmacos"></a>[macOS](#tab/macos)

1. Загрузите и установите [Xamarin книг & инспектора для Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
2. [Проверки вашего собственного приложения!](~/tools/inspector/inspect.md)

-----

## <a name="requirements"></a>Требования

### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 или выше
- **Windows** — Windows 7 или более поздней (с помощью Internet Explorer 11 или выше и .NET 4.6.1 или более поздней)

### <a name="supported-ides"></a>Поддерживаемые интегрированными средами разработки

- Xamarin Studio 6.2 или выше
- Visual Studio для Mac Preview 4 или выше
- Visual Studio 2015 с Xamarin 4.3.x или выше
- 2017 г. Visual Studio с Xamarin рабочей нагрузки

Проверка работающем приложении доступен для корпоративных клиентов.

<a name="supported-platforms" />

### <a name="supported-app-platforms"></a>Поддерживаемые приложения платформы

|Платформа приложений|Поддержка интегрированной среды разработки|Примечания|
|--- |--- |--- |
|Mac (единой)|Поддерживаются только для Mac|
|операций ввода-вывода (единый)|Поддерживается в Visual Studio и XS|Проверка приложений iOS из Windows требуется одна и та же версия инспектор также будет установлен на узле Mac сборки.|
|Android|Поддерживается в Visual Studio и XS|Должны быть предназначены Android > = 4.0.3, с **fastdev** включена.<br />Необходимо использовать эмуляторы Google, Visual Studio и Xamarin Android. Эмуляторы Android 7 не может разрешить проверки в данный момент.|
|WPF|Поддерживается только в Visual Studio в Windows|

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Регистрации ошибок

Должна быть представлена ошибок непосредственно через Visual Studio:

- **Справка > Отправить отзыв > сообщить о проблеме**

Включите все следующие сведения:

### <a name="platform-version-information"></a>Сведения о версии платформы

Эта информация является жизненно важной.

Visual Studio для Mac

- **Visual Studio > о Visual Studio > Показать подробности > скопировать сведения**
- Вставить в отчет об ошибках

Xamarin Studio

- **Xamarin Studio > о Xamarin Studio > Показать подробности > скопировать сведения**
- Вставить в отчет об ошибках

Visual Studio

- **Справка > о Visual Studio > скопировать сведения**
- Сообщите нам о версии операционной системы и выполняется 32-разрядная или 64-разрядной Windows.

### <a name="log-files"></a>Файлы журнала

Всегда присоедините интегрированной среды разработки и инспектор файлы журналов клиента.

Инспектор клиента

- MAC: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x также включает возможность выбора файла журнала в Finder (macOS) или в проводнике (Windows) непосредственно из главного меню:

- **Справка > отобразить файл журнала**

Visual Studio для Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Содержимое Visual Studio **вывода** область может быть информативной.

### <a name="project-settings"></a>Параметры проекта

Если можно присоединить **.csproj** для проекта, который вы пытаетесь проверить, было бы очень полезны. Это проще, чем охватив отдельных параметров.

Также убедитесь, что вы находитесь в конфигурации отладки.

### <a name="selected-devices"></a>Выбранные устройства

Для Android и iOS крайне важно, что мы знаем, какое устройство при отладке, когда требуется проверить. Нам нужно знать следующее:

- Имя устройства, как показано в интерфейс IDE
- Версия ОС устройства
- Android: Убедитесь, что вы используете x86 эмулятора
- Android: Какие платформы эмулятор используется? Эмулятор Google? Эмулятор Android в Visual Studio? Xamarin Android Player?
- Правильно отлаживаемого приложения выглядеть и работать на устройстве?
- Устройство имеет подключение к сети (проверка через веб-браузер)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Удалить

### <a name="windows"></a>Windows

В зависимости от того, как вы получили книг & инспектора необходимо выполнить две процедуры удаления. Проверьте эти полностью удалить программное обеспечение.

#### <a name="visual-studio-installer"></a>Visual Studio Installer

При наличии 2017 г. для Visual Studio, откройте **установщик Visual Studio**и найдите в **отдельные компоненты** для **книги Xamarin**. Если он установлен, снимите этот флажок и нажмите кнопку «Изменить» для удаления.

#### <a name="system-uninstall"></a>Удаление системы

Если вы установили книг & Инспектор самостоятельно Скачанный установщик, его необходимо установить через **приложений и возможности** страница параметров настройки системы в Windows 10 или с помощью **Установка и удаление программ**на панели управления в предыдущих версиях Windows.

> **Пуск > Параметры > Система > приложений и компонентов**

![](install-images/windows-remove.png "Xamarin книг и инспектора, перечисленные в разделе «Приложения и компоненты»")

**Процедуры для установщик Visual Studio, чтобы убедиться, что книги по-прежнему следует придерживаться & инспектора не получить переустановки без ведома пользователя.**

### <a name="macos"></a>macOS

Начиная с [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/), книг Xamarin & Инспектор могут быть удалены из терминала, выполнив:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

Программа удаления подробно файлов и каталогов, она удаляет и запрашивать подтверждение перед продолжением.

Передайте `-help` аргумент `uninstall` скрипт для более сложных сценариев.

В более старых версиях требуется вручную удалить следующие компоненты:

1. Приложение Workbooks в `"/Applications/Xamarin Workbooks.app"`
2. Приложение Inspector в `"Applications/Xamarin Inspector.app"`
3. Надстройки `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` и `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
4. Inspector и вспомогательные файлы `/Library/Frameworks/Xamarin.Interactive.framework` и `/Library/Frameworks/Xamarin.Inspector.framework`
