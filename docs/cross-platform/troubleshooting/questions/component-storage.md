---
title: "Где хранятся компоненты на моем компьютере?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 9549363d7aeb5ff7a5cfa79eb38443aaa80b7019
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Где хранятся компоненты на моем компьютере?

При установке компонента Xamarin в проекте приложения, он помещается в двух местах:

1. В папке компоненты на корневом уровне папкой решения. При удалении компонента из всех проектов в решении, он будет удаляются из этой папки, а также.

2. Копии хранятся также в следующих расположениях:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - MAC: `~/Library/Caches/Xamarin/Components`

Поэтому чтобы полностью удалить компонент из системы, удалите из проектов и решений и выше папки кэша.
