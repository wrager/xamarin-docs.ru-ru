---
title: Блокировка экрана Xamarin.Essentials
description: Класс ScreenLock можно запросить для предотвращения неполадкой находится в спящем режиме, когда приложение выполняется на экране.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7175362dcb7f85746ea85447936d7fe3e2fd026b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
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
