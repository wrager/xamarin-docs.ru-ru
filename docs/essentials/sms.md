---
title: Xamarin.Essentials SMS
description: Класс Sms позволяет приложения, чтобы открыть приложение SMS по умолчанию с использованием заданного сообщения, чтобы отправить получателю.
ms.assetid: 81A757F2-6F2A-458F-B9BE-770ADEBFAB58
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 460aea01381934f1862946c6e17e314560e889f2
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
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
