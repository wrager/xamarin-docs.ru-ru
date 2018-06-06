---
title: Поддержка UrhoSharp Mac
description: В этом документе рассматривается поддержка macOS для UrhoSharp. Он описывает, как создать проект и ссылки на пример кода.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: aae7b09231ae0e8f88bb9435f50fadd2ff822c1a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783347"
---
# <a name="urhosharp-mac-support"></a>Поддержка UrhoSharp Mac

_Настройка определенных MAC и компоненты_

Пока Urho переносимой библиотеке классов, и обеспечивает такой же API, используемые в нескольких различных платформ для логику игры по-прежнему необходимо инициализировать Urho в драйвере определенной платформы, а в некоторых случаях, необходимо воспользоваться преимуществами конкретных компонентов платформы .

В следующих страницах, предполагается, что `MyGame` является подклассом `Application` класса.

## <a name="macos"></a>macOS

**Поддерживаемые архитектуры:** x86/x86-64 для 32-разрядных и 64-разрядная версия.

## <a name="creating-a-project"></a>Создание проекта

Создайте проект консольного, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


