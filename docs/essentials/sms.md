---
title: Xamarin.Essentials SMS
description: Класс Sms позволяет приложения, чтобы открыть приложение SMS по умолчанию с использованием заданного сообщения, чтобы отправить получателю.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 1f466e01d9b1e48849e60a364cf1d49d9cbdcb37
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
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

- [SMS исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Sms)
- [Документация по SMS API](xref:Xamarin.Essentials.Sms)
