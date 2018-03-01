---
title: "Требования к установке и"
ms.topic: article
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: dffea94b309b3c945a37a116668486f404f544b3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="installation-and-requirements"></a>Требования к установке и

<script> var inspectorOnLoad функции () = {var primaryTextBase = «Xamarin книг для;» var secondaryTextBase = «или загрузки для»; var inspectorDownloadUrlMac = «https://dl.xamarin.com/interactive/XamarinInteractive.pkg»; var inspectorDownloadUrlWin = « https://DL.xamarin.com/Interactive/XamarinInteractive.msi»;

  var aPrimary = document.getElementById("inspector-download-primary"); var aSecondary = document.getElementById("inspector-download-secondary");

  var aMac = aPrimary; var aWin = aSecondary; var macTextBase = primaryTextBase; var winTextBase = secondaryTextBase;

  Если (/win/i.test(navigator.platform.toLowerCase())) {aMac = aSecondary; aWin = aPrimary; macTextBase = secondaryTextBase; winTextBase = primaryTextBase;}

  aMac.href = inspectorDownloadUrlMac; aMac.text = macTextBase + «Mac»; aWin.href = inspectorDownloadUrlWin; aWin.text = winTextBase + «Windows»; };

document.addEventListener («DOMContentLoaded», inspectorOnLoad);
</script>

<a name="install" />

## <a name="download-and-install"></a>Загрузка и установка

<ol>
  <li>Проверьте <a href="#Requirements"> требования</a> ниже.</li>
  <li>Загрузите и установите <a href="https://dl.xamarin.com/interactive/XamarinInteractive.pkg" id="inspector-download-primary">Xamarin книги для Mac</a> (<a href="https://dl.xamarin.com/interactive/XamarinInteractive.msi" id="inspector-download-secondary">загрузки для Windows или</a>).
  </li>
  <li>Запуск <a href="~/tools/workbooks/workbook.md"> воспроизведение</a> с книгами или испытать <a href="https://developer.xamarin.com/workbooks/">образцы</a>.
    </li>
</ol>

## <a name="requirements"></a>Требования

#### <a name="supported-operating-systems"></a>Supported Operating Systems

- **Mac** -OS X 10.11 или выше
- **Windows** — Windows 7 или более поздней (с помощью Internet Explorer 11 или выше и .NET 4.6.1 или более поздней)

#### <a name="supported-app-platforms"></a>Поддерживаемые приложения платформы

<table>
<thead>
  <tr>
    <th>Платформа приложений</th>
    <th>Поддержка ОС</th>
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
    <td>Поддерживается на Mac и Windows</td>
    <td>
      <ul>
        <li>Xamarin.iOS 11.0 и Xcode версии 9.0 или более поздней, должны быть установлены на компьютере Mac.</li>
        <li>Необходим узел построения Mac выполнения всех указанных выше, для выполнения операций ввода-вывода книг в Windows и <a href="~/tools/ios-simulator.md">удаленной симулятор iOS</a> установить в Windows.</li>
      </ul>
    </td>
  </tr>
  <tr>
    <td>Android</td>
    <td>Поддерживается на Mac и Windows</td>
    <td>Необходимо использовать эмулятор Google, Visual Studio или Xamarin Android, с виртуальным устройством > = 5.0</td>
  </tr>
  <tr>
    <td>WPF</td>
    <td>Поддерживается только в Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Консоль (.NET Framework)</td>
    <td>Поддерживается на Mac и Windows</td>
    <td/>
  </tr>
  <tr>
    <td>Консоль (.NET Core)</td>
    <td>Поддерживается на Mac и Windows</td>
    <td/>
  </tr>
</tbody>
</table>

## <a name="reporting-bugs"></a>Регистрации ошибок

Проверьте [сообщить о проблемах на GitHub][bugs]и включают все следующие сведения:

### <a name="log-files"></a>Файлы журналов

Всегда Присоедините файлы журналов клиента книги:

- MAC: `~/Library/Logs/Xamarin/Workbooks/Xamarin Workbooks {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Workbooks\logs\Xamarin Workbooks {date}.log`

1.4.x также включает возможность выбора файла журнала в Finder (macOS) или в проводнике (Windows) непосредственно из главного меню:

- **Файл журнала для показа → справки**

#### <a name="log-paths-for-workbooks-13-and-earlier"></a>Пути журнала для книг 1.3 и более ранних версий:

- MAC: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

### <a name="platform-version-information"></a>Сведения о версии платформы

Он очень полезно знать сведения об операционной системе и установленные продукты Xamarin.

В главном меню в книгах:

* **Сведений о версии копирования →**

#### <a name="instructions-for-workbooks-13-and-earlier"></a>Инструкции для книг 1.3 и более ранних версий:

Visual Studio для Mac

- **Visual Studio о Visual Studio → отображаются сведения → Копировать сведения →**
- Вставить в отчет об ошибках

Visual Studio

- **Справка → о Visual Studio → Копировать сведения**
- Сообщите нам о версии операционной системы и выполняется 32-разрядная или 64-разрядной Windows.

### <a name="samples"></a>Примеры

Если можно присоединить или связать `.workbooks` файла возникают затруднения, которые могут помочь устранить ошибку быстрее.

### <a name="devices"></a>Устройства

Если возникают затруднения при подключении устройства iOS или Android книги, а проверка уже [нашу страницу отзывов по устранению неполадок](~/tools/workbooks/troubleshooting/index.md), нам необходимо знать:

- Имя устройства, который вы пытаетесь подключиться к
- Версия ОС устройства
- Android: Убедитесь, что вы используете x86 эмулятора
- Android: Какие платформы эмулятор используется? Эмулятор Google?
  Эмулятор Android в Visual Studio? Xamarin Android Player?
- операций ввода-вывода в Windows: какая версия эмулятора IOS удаленного Xamarin установки (проверьте `Add/Remove Programs` в `Control Panel`)?
- операций ввода-вывода в Windows: также укажите сведения о версии платформы для построения узла Mac
- Устройство имеет подключение к сети (проверка через веб-браузер)?

[bugs]: https://github.com/Microsoft/workbooks/issues/new

## <a name="uninstall"></a>Удалить

### <a name="windows"></a>Windows

В зависимости от того, как вы получили книг & инспектора необходимо выполнить две процедуры удаления. Проверьте эти полностью удалить программное обеспечение.

#### <a name="visual-studio-installer"></a>Visual Studio Installer

При наличии 2017 г. для Visual Studio, откройте **установщик Visual Studio**и найдите в **отдельные компоненты** для **книги Xamarin**. Если он установлен, снимите этот флажок и нажмите кнопку **изменить** для удаления.

#### <a name="system-uninstall"></a>Удаление системы

Если вы установили книг & Инспектор самостоятельно Скачанный установщик, его необходимо установить через **приложений и возможности** страница параметров настройки системы в Windows 10 или с помощью **Установка и удаление программ**на панели управления в предыдущих версиях Windows.

> **Запуск → Настройка → системы → приложений и компонентов**

![](install-images/windows-remove.png "Xamarin книг и инспектора, перечисленные в &quot;приложения &amp; функции&quot;")

**Процедуры для установщик Visual Studio, чтобы убедиться, что книги по-прежнему следует придерживаться & инспектора не получить переустановки без ведома пользователя.**

<a name="uninstall-macos" />

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

## <a name="downgrading"></a>Понижение уровня

Идентификатор пакета для `/Applications/Xamarin Workbooks.app` изменилось с `com.xamarin.Inspector` для `com.xamarin.Workbooks` 1,4 выпуска для упрощения будущих разделение установщиков книг Xamarin & инспектора.

Из-за ошибки в более старых программ установки не удается понизить выпуски 1.4 или более поздней версии с помощью 1.3.2 или более старых программ установки.

Понизить 1.4 или более нового до 1.3.2 или старых:

1. [Вручную удалите книг & инспектора](#macOS)
2. Запустите 1.3.2 или более ранних версий `.pkg` установщика