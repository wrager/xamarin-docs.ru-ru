---
title: 'Xamarin.Essentials: буфер обмена'
description: Этот документ описывает класс буфера обмена в Xamarin.Essentials, который позволяет скопировать и вставить текст в буфер обмена между приложениями.
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 41b15b480fa23bd49667b68e904043e4f1a95732
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782363"
---
# <a name="xamarinessentials-clipboard"></a>Xamarin.Essentials: буфер обмена

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
Clipboard.SetText("Hello World");
```

Прочитать текст из **буфер обмена**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## <a name="api"></a>API

- [Буфер обмена исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Документация по API буфера обмена](xref:Xamarin.Essentials.Clipboard)
