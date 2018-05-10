---
title: Поддержка UrhoSharp Mac
description: Настройка определенных MAC и функции для UrhoSharp.
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: c7210cbd5586df9018c2779bf0b92aa7c1c56e89
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
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


