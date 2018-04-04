---
title: Как устранить ошибку PathTooLongException?
description: В этой статье описываются способы решения PathTooLongException, которая может возникнуть при построении приложения.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f50ca3e738cb781f9c80e83f58f2e0fa1fa8e113
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>Как устранить ошибку PathTooLongException?

## <a name="cause"></a>Причина

Имена путей созданного проекта Xamarin.Android могут быть довольно длинными.
Например во время построения удается создать путь следующим образом:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

В Windows (где Максимальная длина пути составляет [260 символов](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** может создаваться при построении проекта, если созданный пути превышает максимальную длину. 

## <a name="fix"></a>Исправление

Начиная с Xamarin.Android 8.0 `UseShortFileNames` MSBuild может быть установлено для обхода этой ошибки. Если значение этого свойства `True` (значение по умолчанию — `False`), процесс построения использует более короткие имена пути, чтобы уменьшить вероятность создания **PathTooLongException**.
Например, если `UseShortFileNames` имеет значение `True`, указанном выше пути сокращается путь, который подобен следующему:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

Чтобы задать это свойство, добавьте в проект следующие свойства MSBuild **.csproj** файла:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

Дополнительные сведения о настройке свойств сборки см. в разделе [процессе сборки](~/android/deploy-test/building-apps/build-process.md).
