---
title: Платформы Objective c.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8c48a14e5d58f95bf93363e0b354fcf3afd94b44
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="objective-c-platforms"></a>Платформы Objective c.

Внедрение .NET можно ориентироваться на различные платформы при создании кода Objective-C:

* macOS
* iOS
* tvOS
* watchOS [еще не реализован]

Выбрана платформа, передав `--platform=<platform>` аргумент командной строки для внедрения .NET.

При построении для iOS, tvOS watchOS платформах и внедрение .NET всегда будет создаваться платформу, которая содержит Xamarin.iOS, поскольку Xamarin.iOS содержит много среды выполнения поддерживает код, который необходим для этих платформ.

Однако при построении macOS платформы, можно выбрать, следует ли созданный framework внедрять Xamarin.Mac или нет. Можно отменить внедрение Xamarin.Mac Если привязанной сборки ссылается Xamarin.Mac.dll (прямо или косвенно), и этот параметр выбран, передав `--platform=macOS` средство внедрения .NET.

Если привязанной сборки содержит ссылку на Xamarin.Mac.dll, необходимо внедрить Xamarin.Mac и дополнительно embeddinator необходимо знать, какие требуемая версия .NET framework для использования.

Существует три возможных целевые платформы Xamarin.Mac: `modern` (прежнее название — `mobile`), `full` и `system` (разница между каждым описан в Xamarin.Mac [требуемой версии .NET framework] [ 1] документации), и каждый выбирается путем передачи `--platform=macOS-modern`, `--platform=macOS-full` или `--platform=macOS-system` в средство внедрения .NET.

[1]: ~/mac/platform/target-framework.md
