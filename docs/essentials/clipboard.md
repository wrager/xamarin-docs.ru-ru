---
title: Буфер обмена Xamarin.Essentials
description: Класс Clipboard позволяет скопировать и вставить текст в буфер обмена между приложениями.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 218f90746f130f02c47040a9191b1001fa80c203
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
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

- [Буфер обмена исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Clipboard)
- [Документация по API буфера обмена](xref:Xamarin.Essentials.Clipboard)
