---
title: "Как включить Intellisense в файлах Android .axml?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: c68ec5a6d369e8673c8764e01943e513489775b0
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-enable-intellisense-in-android-axml-files"></a>Как включить Intellisense в файлах Android .axml?

Android Intellisense в Visual Studio требуется два файла схемы XML, чтобы работать соответствующим образом. Чтобы найти эти файлы, перейдите в папку:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

* (Ранее: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Внутри вы найдете два файла XSD-файл.

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio сохраняет все XML-схем в соответствующей папке:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

Можно переместить эти два файла XSD-файл к указанному месту или просто добавить эти схемы в Visual Studio. Затем можно «добавить» эти схемы в Visual Studio через **XML > схемы > Добавить** диалогового окна.






