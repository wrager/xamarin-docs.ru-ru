---
title: 'Xamarin.Essentials: по электронной почте'
description: Класс электронной почты в Xamarin.Essentials позволяет приложения, чтобы открыть приложение электронной почты по умолчанию с указанными сведениями, включая темы, текст и получателей (TO, CC, BCC).
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: aea2f429126180ae3d98bc665bed5574f416ea53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782441"
---
# <a name="xamarinessentials-email"></a>Xamarin.Essentials: по электронной почте

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
            };
            
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
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
