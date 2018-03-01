---
title: "Реальный пример, с помощью CocoaPods"
ms.topic: article
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ae92b491e6186371f1fc1ead835f918a94f18f86
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="real-world-example-using-cocoapods"></a>Реальный пример, с помощью CocoaPods


**В этом примере используется [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).**

Появился в версии 3.0 поддерживает привязки CocoaPods Sharpie цель и даже имеется команда переднего плана (`sharpie pod`) для загрузки, Настройка и сборка CocoaPods очень легко. Вы должны [faimilarize самостоятельно с CocoaPods](https://cocoapods.org) перед использованием этой функции в целом.

`sharpie pod` Команда имеет один глобальный параметр и два подкоманд:

```csharp
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Подкоманда также содержит некоторые полезные справки:

```csharp
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Несколько имен CocoaPod и subspec имена могут быть предоставлены для `init`.

<pre>$ <b>sharpie pod init ios AFNetworking</b>
<span class="terminal-green">**</span> Setting up CocoaPods master repo ...
   (this may take a while the first time)
<span class="terminal-green">**</span> Searching for requested CocoaPods ...
<span class="terminal-green">**</span> Working directory:
<span class="terminal-green">**</span>   - Writing Podfile ...
<span class="terminal-green">**</span>   - Installing CocoaPods ...
<span class="terminal-green">**</span>     (running `<span class="terminal-blue">pod install --no-integrate --no-repo-update</span>`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
<span class="terminal-green">**</span> 🍻 Success! You can now use other `<span class="terminal-green">sharpie pod</span>`  commands.</pre>

После настройки вашей CocoaPod теперь можно создать привязки.

<pre>$ <b>sharpie pod bind</b></pre>

Это приведет к проекту CocoaPod Xcode, выполняется построение и затем вычисляются и анализировать Sharpie цель. Большой объем выходных данных консоли будет создан, но следует вызвать определение привязки в конце:

<pre><em>(... lots of build output ...)</em>

<span class="terminal-blue">Parsing 19 header files...</span>

<span class="terminal-magenta">Binding...</span>
  <span class="terminal-magenta">[write]</span> ApiDefinitions.cs
  <span class="terminal-magenta">[write]</span> StructsAndEnums.cs

<span class="terminal-green">Done.</span></pre>

