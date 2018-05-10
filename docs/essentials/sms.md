---
title: Xamarin.Essentials SMS
description: Класс Sms позволяет приложения, чтобы открыть приложение SMS по умолчанию с использованием заданного сообщения, чтобы отправить получателю.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5baeee03626ba659ac7e5c06be40039476a67e08
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-sms"></a>Xamarin.Essentials SMS

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Sms** класс позволяет приложения, чтобы открыть приложение SMS по умолчанию с использованием заданного сообщения, чтобы отправить получателю.

## <a name="using-sms"></a>С помощью Sms

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Функциональные возможности SMS работает путем вызова `ComposeAsync` метод `SmsMessage` , содержащий получатель сообщения и текст сообщения, оба из которых являются необязательными.

```csharp
public class SmsTest
{
    public async Task SendSms(string messageText, string recipient)
    {
        try
        {
            var message = new SmsMessage(messageText, recipient);
            await Sms.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException ex)
        {
            // Sms is not supported on this device.
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

## <a name="api"></a>API

- [SMS исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Sms)
- [Документация по SMS API](xref:Xamarin.Essentials.Sms)
