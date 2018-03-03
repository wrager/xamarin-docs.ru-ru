---
title: "UrhoSharp iOS и tvOS поддержки"
description: "iOS и tvOS определенные настройки и компоненты"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: 9cf779b23ed830c07af0100152a44d6c3c4e317b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS и tvOS поддержки

_iOS и tvOS определенные настройки и компоненты_

Пока Urho переносимой библиотеке классов, и обеспечивает такой же API, используемые в нескольких различных платформ для логику игры по-прежнему необходимо инициализировать Urho в драйвере определенной платформы, а в некоторых случаях, необходимо воспользоваться преимуществами конкретных компонентов платформы .

В следующих страницах, предполагается, что `MyGame` является sublcass из `Application` класса.

# <a name="ios-and-tvos"></a>iOS и tvOS

**Поддерживаемые архитектуры:** armv7 arm64, i386

# <a name="creating-a-project"></a>Создание проекта

Создайте проект iOS, а затем добавить данные в каталог ресурсов и убедитесь, что все файлы имеют **BundleResource** как **действие при построении**.

![Установка проектов](ios-images/image-4.png "добавить данные в каталог ресурсов")

# <a name="configuring-and-launching-urho"></a>Настройка и запуск Urho

Добавление с помощью инструкций для `Urho` и `Urho.iOS` пространства имен, а затем добавьте следующий код для инициализации Urho, а также для запуска приложения:

```csharp
new MyGame().Run();
```

Обратите внимание, что поскольку ожидает iOS `FinishedLaunching` для выполнения, следует очередь вызов `Run()` для выполнения после завершения работы метода, это распространенных случаях:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

Важно отключить оптимизацию PNG, так как оптимизатор PNG операций ввода-вывода по умолчанию будет создавать образы, которые не могут правильно в настоящее время использует Urho

# <a name="custom-embedding-of-urho"></a>Внедрение пользовательских Urho

Можно также, что Urho взять на себя экрана всего приложения и ее использование в качестве компонента приложения, можно создать `UrhoSurface` это `UIView` , который можно внедрить в существующем приложении.

Это то, что необходимо сделать:

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

Это будет размещаться классе Urho, таким образом, а затем это можно сделать:

```csharp
new MyGame().Run ();
```

