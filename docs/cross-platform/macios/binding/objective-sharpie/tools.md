---
title: Средства и команды
description: Обзор средства, включенные в Sharpie цель и аргументы командной строки для их использования.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
ms.openlocfilehash: 0e333ce7c336d13c8b55326a5d51a64092885dfd
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="tools--commands"></a>Средства и команды

_Обзор средства, включенные в Sharpie цель и аргументы командной строки для их использования._

<style type="text/css"> синий .terminal {цвет: rgb(10,96,254);} .terminal зеленый {цвет: rgb(12,156,26);} .terminal magenta {цвет: rgb(152,12,103);} </style>


После успешной цель Sharpie [установлен](~/cross-platform/macios/binding/objective-sharpie/get-started.md)откройте терминал и ознакомиться с <em>команды</em> Sharpie цель должен предлагать:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Цели Sharpie предоставляет следующие средства:

|Средство|Описание|
|--- |--- |
|**Xcode**|Предоставляет сведения о текущей установке Xcode и версии iOS и Mac пакетов SDK, которые доступны. Мы используем эти сведения позже при мы создаем нашей привязок.|
|**POD**|Выполняет поиск, настраивает, устанавливает (в локальном каталоге) и привязывает Objective-C [CocoaPod](https://cocoapods.org/) библиотеки из характеристик главного репозитория. Это средство оценивает установленных CocoaPod автоматически вычислить правильные входные данные для передачи `bind` средство ниже. Новые возможности 3.0!|
|**bind**|Анализирует файлы заголовков (`*.h`) в библиотеке Objective-C в исходный [ApiDefinition.cs и StructsAndEnums.cs](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) файлов.|
|**update**|Проверяет наличие новых версий Sharpie цель и загружает и запускает установщик, если он доступен.|
|**Проверьте документы**|Содержит подробные сведения о `[Verify]` атрибуты.|
|**Документы**|Переход к этому документу, в веб-браузере по умолчанию.|

Чтобы получить справку по определенного инструмента Sharpie цель, введите имя средства и `-help` параметр. Например `sharpie xcode -help` возвращает следующие результаты:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Перед началом процесса привязки, нам нужно получить сведения о нашей текущей пакетов SDK, введя следующую команду в окне терминала `sharpie xcode -sdks`. Выходные данные могут отличаться в зависимости от того, какие версии Xcode установки. Цели Sharpie ищет пакеты SDK, установлена в любом `Xcode*.app` под `/Applications` каталога:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

Из примера выше, мы видим, что у нас есть `iphoneos9.1` пакет SDK установлен на компьютере и имеет `arm64` поддержка архитектуры. Мы будет использовать это значение для всех образцов этого раздела. С этими данными мы готовы выполнить синтаксический анализ файлов заголовка библиотеки Objective-C в исходный `ApiDefinition.cs` и `StructsAndEnums.cs` для привязки проекта.

