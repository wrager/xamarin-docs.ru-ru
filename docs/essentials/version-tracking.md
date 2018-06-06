---
title: 'Xamarin.Essentials: Отслеживание версий'
description: Класс VersionTracking в Xamarin.Essentials позволяет проверить версию приложения и номерам сборки, а также просматривать дополнительные сведения о таких, как, если он является первым время, когда-либо запустить приложение или текущую версию получить предыдущей сборки сведения и многое другое.
ms.assetid: 670C7E8A-E882-4AC0-97D2-A53D90ADD6A3
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2c092d6767045f0af956c5dab74801077dadb51f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783006"
---
# <a name="xamarinessentials-version-tracking"></a>Xamarin.Essentials: Отслеживание версий

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**VersionTracking** позволяет проверить версию приложения и номерам сборки, а также просматривать дополнительные сведения о таких, как, если он является первым время, когда-либо запустить приложение или получить текущую версию предыдущего сведения о сборке и многое другое.

## <a name="using-version-tracking"></a>Отслеживание версий с помощью

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

При первом использовании **VersionTracking** класса, он будет запущен, отслеживание текущую версию. Необходимо вызвать метод `Track` раннее только в приложении каждый раз, он загружается отслеживаемых сведений о текущей версии:

```csharp
VersionTracking.Track();
```

После первоначального `Track` вызывается можно считать сведения о версии:

```csharp

// First time ever launched application
var firstLaunch = VersionTracking.IsFirstLaunchEver;

// First time launching current version
var firstLaunchCurrent = VersionTracking.IsFirstLaunchForCurrentVersion;

// First time launching current build
var firstLaunchBuild = VersionTracking.IsFirstLaunchForCurrentBuild;

// Current app version (2.0.0)
var currentVersion = VersionTracking.CurrentVersion;

// Current build (2)
var currentBuild = VersionTracking.CurrentBuild;

// Previous app version (1.0.0)
var previousVersion = VersionTracking.PreviousVersion;

// Previous app build (1)
var previousBuild = VersionTracking.PreviousBuild;

// First version of app installed (1.0.0)
var firstVersion = VersionTracking.FirstInstalledVersion;

// First build of app installed (1)
var firstBuild = VersionTracking.FirstInstalledBuild;

// List of versions installed (1.0.0, 2.0.0)
var versionHistory = VersionTracking.VersionHistory;

// List of builds installed (1, 2)
var buildHistory = VersionTracking.BuildHistory;
```

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

Все сведения о версии хранится с использованием [предпочтения](preferences.md) API в Xamarin.Essentials и сохраняется в имени файла **.xamarinessentials [YOUR--ПАКЕТА-идентификатор приложения]**.

После удаления приложения _LocalSettings_и отслеживания сведений для удаления всех версий.

## <a name="api"></a>API

- [Версия отслеживания исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/VersionTracking)
- [Версия API отслеживания документации](xref:Xamarin.Essentials.VersionTracking)
