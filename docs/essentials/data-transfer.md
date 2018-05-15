---
title: Передача данных Xamarin.Essentials
description: Класс DataTransfer позволяет приложениям совместно использовать данные, такие как текст и веб-ссылки на другие приложения на устройстве.
ms.assetid: B7B01D55-0129-4C87-B515-89F8F4E94665
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b03ec1330aff1210350adf2600c63d7d84bc1125
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-data-transfer"></a>Передача данных Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**DataTransfer** класса позволяет приложениям совместно использовать данные, такие как текст и веб-ссылки на другие приложения на устройстве.

## <a name="using-data-transfer"></a>Передача данных с помощью

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Функции передачи данных работает путем вызова `RequestAsync` метод полезных данных запроса, включающий сведения о совместно использовать с другими приложениями. Текст и Uri можно комбинировать и обработки каждой платформы фильтрации на основе содержимого.

```csharp

public class DataTransferTest
{
    public async Task ShareText(string text)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Text = text,
                Title = "Share Text"
            });
    }

    public async Task ShareUri(string uri)
    {
        await DataTransfer.RequestAsync(new ShareTextRequest
            {
                Uri = uri,
                Title = "Share Web Link"
            });
    }
}
```

Пользовательский интерфейс для совместного использования на внешнее приложение, которое появляется при запросе:

![Передача данных](data-transfer-images/data-transfer.png)

## <a name="platform-differences"></a>Различия между платформами

| Platform | Различие |
| --- | --- |
| Android | Свойство «Тема» используется для нужного тема сообщения. |
| iOS | Тема не используется. |
| iOS | Заголовок не используется. |
| UWP | Заголовок будет по умолчанию для имени приложения Если не установлено. |
| UWP | Тема не используется. |

## <a name="api"></a>API

- [Исходный код для передачи данных](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DataTransfer)
- [Документация по API передачи данных](xref:Xamarin.Essentials.DataTransfer)
