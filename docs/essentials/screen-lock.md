---
title: Блокировка экрана Xamarin.Essentials
description: Класс ScreenLock можно запросить для предотвращения неполадкой находится в спящем режиме, когда приложение выполняется на экране.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 66702e9f8f5e6ad07f8f7c96f739edf1b2e66617
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-screen-lock"></a>Блокировка экрана Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**ScreenLock** класса можно запросить для предотвращения неполадкой находится в спящем режиме, когда приложение выполняется на экране.

## <a name="using-screenlock"></a>С помощью ScreenLock

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность блокировки экрана, работает путем вызова `RequestActive` и `RequestRelease` методы также запрашивать отключения экрана.

```csharp
public class ScreenLockTest
{
    public void ToggleScreenLock()
    {
        if (ScreenLock.IsMonitoring)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease;
    }
}
```

## <a name="api"></a>API

- [На экране блокировки исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/ScreenLock)
- [Документация по API блокировка экрана](xref:Xamarin.Essentials.ScreenLock)
