---
title: "Привязка собственного платформ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4588a879452b4ee49535ac8882977be2c064a43f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="binding-native-frameworks"></a>Привязка собственного платформ

Иногда собственная Библиотека распространяется в виде [framework](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Цели Sharpie предоставляет удобное средство для привязки правильно определен платформ через `-framework` параметр.

Например, привязка [Adobe Creative SDK Framework](https://creativesdk.adobe.com/downloads.html) для операций ввода-вывода выполняется просто:

<pre>$ <b>sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1</b></pre>

В некоторых случаях будет указать платформу **Info.plist** указывающая, к какой пакет SDK платформы должны компилироваться. Если этой информации и без явного указания `-sdk` передается параметр, Sharpie цель будет построена на основе инфраструктуры **Info.plist** (либо `DTSDKName` ключа или сочетание `DTPlatformName` и `DTPlatformVersion`ключей).

`-framework` Параметр не допускает явного заголовочные файлы для передачи. Общий файл заголовка, выбранное правило, основанное на имени платформы. Если не удается найти общий заголовок, Sharpie цель не пытается привязать платформу и привязки необходимо выполнить вручную, предоставляя файлов заголовка правильный общий для синтаксического анализа, и любые другие аргументы framework для clang (такие как `-F`параметры пути поиска для платформы).

За кулисами указав `-framework` — это краткий. Следующие аргументы привязки идентичны `-framework` сокращенное выше.
Особенное значение имеет `-F .` framework пути поиска для clang (Обратите внимание, места и период, которые являются неотъемлемой частью команды).

<pre>$ <b>sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .</b></pre>

