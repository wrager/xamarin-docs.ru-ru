---
title: Буфер обмена Xamarin.Essentials
description: Класс Clipboard позволяет скопировать и вставить текст в буфер обмена между приложениями.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 67a0218325918b57e5ed2618b57d52d3fe3ee820
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-clipboard"></a>Буфер обмена Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Буфер** класс позволяет скопировать и вставить текст в буфер обмена между приложениями.

## <a name="using-clipboard"></a>Использование буфера обмена

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Чтобы проверить, если **буфер обмена** используется в данный момент, которую можно вставить текст:

```csharp
var hasText = Clipboard.HasText;
```

Чтобы задать текст **буфер обмена**:

```csharp
ClipBoard.SetText("Hello World");
```

Прочитать текст из **буфер обмена**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Буфер обмена исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Документация по API буфера обмена](xref:Xamarin.Essentials.Clipboard)
