---
title: "Требования к установке и"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
ms.openlocfilehash: a587935e35882ed1dc68817fbbe1ae3e91200f29
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Требования к установке и

<script> var inspectorOnLoad функции () = {var primaryTextBase = «Книг Xamarin & инспектора для»; var secondaryTextBase = «или загрузки для»; var inspectorDownloadUrlMac = «https://dl.xamarin.com/interactive/XamarinInteractive.pkg»; var inspectorDownloadUrlWin = «https://dl.xamarin.com/interactive/XamarinInteractive.msi»;

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Если (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + «Mac»; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + «Windows»; };

document.addEventListener («DOMContentLoaded», inspectorOnLoad);
</script>

## <a name="download-and-installation"></a>Загрузка и установка

<ol>
  <li>Загрузите и установите <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin книг & инспектора для Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">загрузки для Windows или</a>).
  </li>
  <li><a href="~/tools/inspector/inspect.md"> Проверки вашего собственного приложения!</a>
    </li>
</ol>

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

<table>
<thead>
  <tr>
    <th>Платформа приложений</th>
    <th>Поддержка интегрированной среды разработки</th>
    <th>Примечания</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Mac (единой)</td>
    <td>Поддерживаются только для Mac</td>
    <td/>
  </tr>
  <tr>
    <td>операций ввода-вывода (единый)</td>
    <td>Поддерживается в Visual Studio и XS</td>
    <td>Проверка приложений iOS из Windows требуется одна и та же версия инспектор также будет установлен на узле Mac сборки.</td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Поддерживается в Visual Studio и XS</td>
    <td>
      <ul>
        <li>Должны быть предназначены Android > = 4.0.3</li>
        <li>Должен быть fastdev включен</li>
        <li>Необходимо использовать эмуляторы Google, Visual Studio и Xamarin Android. Эмуляторы Android 7 не может разрешить проверки в данный момент.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Поддерживается только в Visual Studio в Windows</td>
    <td/>
  </tr>
</tbody>
</table>

<a name="reporting-bugs" />

## <a name="reporting-bugs"></a>Регистрации ошибок

Должна быть представлена ошибок непосредственно через Visual Studio:

- **Справка → отправить отзыв → сообщить о проблеме**

Включите все следующие сведения:

### <a name="platform-version-information"></a>Сведения о версии платформы

Эта информация является жизненно важной.

Visual Studio для Mac

- **Visual Studio о Visual Studio → отображаются сведения → Копировать сведения →**
- Вставить в отчет об ошибках

Xamarin Studio

- **Xamarin Studio → о Xamarin Studio → Показать подробные сведения копии →**
- Вставить в отчет об ошибках

Visual Studio

- **Справка > о Visual Studio > скопировать сведения**
- Сообщите нам о версии операционной системы и выполняется 32-разрядная или 64-разрядной Windows.

### <a name="log-files"></a>Файлы журналов

Всегда присоедините интегрированной среды разработки и инспектор файлы журналов клиента.

Инспектор клиента

- MAC: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x также включает возможность выбора файла журнала в Finder (macOS) или в проводнике (Windows) непосредственно из главного меню:

- **Файл журнала для показа → справки**

Visual Studio для Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- Содержимое Visual Studio `Output` область может быть информативной.

### <a name="project-settings"></a>Параметры проекта

Если можно присоединить `.csproj` для проекта, который вы пытаетесь проверить, было бы очень полезны. Это проще, чем охватив отдельных параметров.

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

При наличии Visual Studio 2017 г., откройте «Установщик Visual Studio» и «Отдельных компонентов» искать «Xamarin книг». Если он установлен, снимите этот флажок и нажмите кнопку «Изменить» для удаления.

#### <a name="system-uninstall"></a>Удаление системы

Если вы установили книг & Инспектор самостоятельно Скачанный установщик, его необходимо установить через **приложений и возможности** страница параметров настройки системы в Windows 10 или с помощью **Установка и удаление программ**на панели управления в предыдущих версиях Windows.

> **Запуск → Настройка → системы → приложений и компонентов**

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
2. Надстройки `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` и `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
3. Inspector и вспомогательные файлы `/Library/Frameworks/Xamarin.Interactive.framework` и `/Library/Frameworks/Xamarin.Inspector.framework`

