---
title: Xamarin.Essentials электронной почты
description: Класс электронной почты позволяет приложения, чтобы открыть приложение электронной почты по умолчанию с указанными сведениями, включая темы, текст и получателей (TO, CC, BCC).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3fee30e31dc18665d59f944462959fd3f8166968
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2018
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials электронной почты

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Электронной почты** класс позволяет приложения, чтобы открыть приложение электронной почты по умолчанию с указанными сведениями, включая темы, текст и получателей (TO, CC, BCC).

## <a name="using-email"></a>С помощью электронной почты

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность электронной почты работает путем вызова `ComposeAsync` метод `EmailMessage` , содержащий сведения об электронной почты:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            }
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Sms is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occured
        }
    }
}
```

## <a name="api"></a>API

- [Исходный код электронной почты](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Документация по электронной почте API](xref:Xamarin.Essentials.Email)
