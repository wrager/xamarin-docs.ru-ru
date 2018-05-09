---
title: Xamarin.Essentials телефон
description: Класс PhoneDialer позволяет приложения, чтобы открыть веб-ссылку в браузере предпочтительный оптимизированного системы или внешнем браузере.
ms.assetid: E7457942-4D7B-4195-A2FF-417919B9537F
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 70e43d58eab562f032b59edf7095ca2614af8082
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
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
