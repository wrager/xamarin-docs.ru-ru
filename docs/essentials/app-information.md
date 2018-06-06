---
title: 'Xamarin.Essentials: Сведения о приложении'
description: Этот документ описывает класс AppInfo в Xamarin.Essentials, который предоставляет сведения о приложении. Например он предоставляет имя приложения и версии.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 25765fbbcbd2475bfcbc72ca5c4feefa8ef19d08
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782109"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Сведения о приложении

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**AppInfo** класс предоставляет сведения о приложении.

## <a name="using-appinfo"></a>С помощью AppInfo

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Через API-Интерфейс предоставляются следующие сведения:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="api"></a>API

- [AppInfo исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Документация по AppInfo API](xref:Xamarin.Essentials.AppInfo)
