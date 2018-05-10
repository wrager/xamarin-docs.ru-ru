---
title: Xamarin.Essentials телефон
description: Класс PhoneDialer позволяет приложения, чтобы открыть веб-ссылку в браузере предпочтительный оптимизированного системы или внешнем браузере.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 6bea1b26e43459be12272f9dd2ecc6fafba2b8b4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-phone-dialer"></a>Xamarin.Essentials телефон

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**PhoneDialer** класс позволяет приложения, чтобы открыть веб-ссылку в браузере предпочтительный оптимизированного системы или внешнем браузере.

## <a name="using-phone-dialer"></a>С помощью телефона

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность телефон работает путем вызова `Open` метод с номером телефона, чтобы открыть программу "," с. Когда `Open` запрашивается API будет автоматически пытаться форматировать число, основанное на код страны, если указано.

```csharp
public class PhoneDialerTest
{
    public async Task PlacePhoneCall(string number)
    {
        try
        {
            PhoneDialer.Open(number);
        }
        catch (ArgumentNullException anEx)
        {
            // Number was null or white space
        }
        catch (FeatureNotSupportedException ex)
        {
            // Phone Dialer is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [Телефон исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/PhoneDialer)
- [Документация по API набора номера телефона](xref:Xamarin.Essentials.PhoneDialer)
