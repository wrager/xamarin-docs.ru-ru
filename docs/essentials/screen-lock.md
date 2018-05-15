---
title: Блокировка экрана Xamarin.Essentials
description: Класс ScreenLock можно запросить для предотвращения неполадкой находится в спящем режиме, когда приложение выполняется на экране.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f7e6a3d6933ed1fce7522fdbb8102e5100bd1589
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
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
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [На экране блокировки исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Документация по API блокировка экрана](xref:Xamarin.Essentials.ScreenLock)
