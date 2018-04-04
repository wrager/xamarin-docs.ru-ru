---
title: Где хранятся компоненты на моем компьютере?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: f0dad6e6219d373eaa9f8410aea7d96c81eceb6b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="where-are-the-components-stored-on-my-machine"></a>Где хранятся компоненты на моем компьютере?

При установке компонента Xamarin в проекте приложения, он помещается в двух местах:

1. В папке компоненты на корневом уровне папкой решения. При удалении компонента из всех проектов в решении, он будет удаляются из этой папки, а также.

2. Копии хранятся также в следующих расположениях:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - MAC: `~/Library/Caches/Xamarin/Components`

Поэтому чтобы полностью удалить компонент из системы, удалите из проектов и решений и выше папки кэша.
