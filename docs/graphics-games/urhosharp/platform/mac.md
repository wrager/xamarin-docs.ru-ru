---
title: "Поддержка UrhoSharp Mac"
description: "Настройка определенных MAC и компоненты"
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: cdaca45c3132abf3f52d407d4ce59c680248fce7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-mac-support"></a>Поддержка UrhoSharp Mac

_Настройка определенных MAC и компоненты_

Пока Urho переносимой библиотеке классов, и обеспечивает такой же API, используемые в нескольких различных платформ для логику игры по-прежнему необходимо инициализировать Urho в драйвере определенной платформы, а в некоторых случаях, необходимо воспользоваться преимуществами конкретных компонентов платформы .

В следующих страницах, предполагается, что `MyGame` является подклассом `Application` класса.

# <a name="macos-x"></a>MacOS X

**Поддерживаемые архитектуры:** x86/x86-64 для 32-разрядных и 64-разрядная версия.

# <a name="creating-a-project"></a>Создание проекта

Создайте проект консольного, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


