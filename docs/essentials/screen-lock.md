---
title: 'Xamarin.Essentials: Блокировка экрана'
description: Этот документ описывает класс ScreenLock в Xamarin.Essentials, который можно запросить для предотвращения неполадкой находится в спящем режиме, когда приложение выполняется на экране.
ms.assetid: 6B67C114-315E-4199-AA72-3F90E85A4909
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3c8110b7abc86fe1d12485579f134997718540e6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782912"
---
# <a name="xamarinessentials-screen-lock"></a>Xamarin.Essentials: Блокировка экрана

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
        if (!ScreenLock.IsActive)
            ScreenLock.RequestActive();
        else
            ScreenLock.RequestRelease();
    }
}
```

## <a name="api"></a>API

- [На экране блокировки исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/ScreenLock)
- [Документация по API блокировка экрана](xref:Xamarin.Essentials.ScreenLock)
