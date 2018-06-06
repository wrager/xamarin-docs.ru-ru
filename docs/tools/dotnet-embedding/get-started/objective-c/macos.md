---
title: Приступая к работе с macOS
description: В этом документе описывается как приступить к использованию с macOS внедрения .NET. Обсуждаются требования и представляет пример приложения, чтобы продемонстрировать, как привязать управляемую сборку, и использовать созданные выходные данные в проекте Xcode.
ms.prod: xamarin
ms.assetid: AE51F523-74F4-4EC0-B531-30B71C4D36DF
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 38049eae5e420e5f3610341c2682fa92d2ac426e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793693"
---
# <a name="getting-started-with-macos"></a>Приступая к работе с macOS

## <a name="what-you-will-need"></a>Необходимо будет

* Следуйте инструкциям в разделе [Приступая к работе с Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) руководства.

## <a name="hello-world"></a>Здравствуй, мир

Сначала нужно создать простой hello world-пример на языке C#.

### <a name="create-c-sample"></a>Создайте пример на C#

Откройте Visual Studio для Mac, создайте новый проект библиотеки классов для Mac с именем **hello из c#** и сохраните файл в **~/Projects/hello-from-csharp**.

Замените код в **MyClass.cs** файла с помощью следующего фрагмента:

```csharp
using AppKit;
public class MyNSView : NSTextView
{
    public MyNSView ()
    {
        Value = "Hello from C#";
    }
}
```

Выполните построение проекта. Результирующую сборку будет сохранена как **~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll**.

### <a name="bind-the-managed-assembly"></a>Привязать управляемой сборки

Когда управляемая сборка, привяжите его путем вызова внедрения .NET.

Как описано в [установки](~/tools/dotnet-embedding/get-started/install/install.md) руководстве, это можно сделать в качестве шага после сборки в проекте с пользовательской целевой объект MSBuild, или вручную:

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=macOS-modern --abi=x86_64 --outdir=output -c --debug
```

Платформа будет помещен в **~/Projects/hello-from-csharp/output/hello-from-csharp.framework**.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Использовать созданные выходные данные в проекте Xcode

Откройте Xcode и создайте новое приложение Cocoa. Назовите его **hello из c#** и выберите **Objective-C** языка.

Откройте **~/Projects/hello-from-csharp/output** каталог в системе поиска выберите **hello из csharp.framework**, перетащите его к проекту Xcode и поместите его непосредственно над **hello из c#**  папки в проекте.

![Перетаскивание framework](macos-images/hello-from-csharp-mac-drag-drop-framework.png)

Убедитесь, что **копирование элементов при необходимости** проверяется в появившемся окне и нажмите кнопку **Готово**.

![Копирование элементов при необходимости](macos-images/hello-from-csharp-mac-copy-items-if-needed.png)

Выберите **hello из c#** проекта и перейдите к **hello из c#** целевого объекта **Общие** вкладки. В **внедренные двоичный** добавьте **hello из csharp.framework**.

![Внедренные двоичных файлов](macos-images/hello-from-csharp-mac-embedded-binaries.png)

Откройте **ViewController.m**и замените содержимое с:

```objc
#import "ViewController.h"

#include "hello-from-csharp/hello-from-csharp.h"

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    MyNSView *view = [[MyNSView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}

@end
```

Наконец, запустите проект Xcode и будут отображаться примерно следующим образом:

![Hello из примера для C# в симуляторе](macos-images/hello-from-csharp-mac.png)

Пример более полным и улучшение [доступен здесь](https://github.com/mono/Embeddinator-4000/tree/objc/samples/mac/weather).
