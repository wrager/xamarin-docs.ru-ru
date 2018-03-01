---
title: "Начало работы с iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: D5453695-69C9-44BC-B226-5B86950956E2
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: c1b21e36d314b79402702f29428f2c337ddc1069
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-ios"></a>Начало работы с iOS


## <a name="requirements"></a>Требования

Помимо требований к из наших [Приступая к работе с Objective-C](~/tools/dotnet-embedding/get-started/objective-c/index.md) руководства, необходимо:

* [Xamarin.iOS 10.11 +](https://jenkins.mono-project.com/view/Xamarin.MaciOS/job/xamarin-macios-builds-master/) из наших _master_ ветви.

## <a name="hello-world"></a>Здравствуй, мир

Первый создадим простой hello world-пример на языке C#.

### <a name="create-c-sample"></a>Создайте пример на C#

Откройте Visual Studio для Mac, создайте новый проект библиотеки классов iOS, назовите его `hello-from-csharp`и сохраните файл в `~/Projects/hello-from-csharp`.

Замените код в `MyClass.cs` файла с помощью следующего фрагмента:

```csharp
using UIKit;
public class MyUIView : UITextView
{
    public MyUIView ()
    {
        Text = "Hello from C#";
    }
}
```

Выполните построение проекта, результирующую сборку будет сохранена как `~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll`.

### <a name="bind-the-managed-assembly"></a>Привязать управляемой сборки

Выполните embeddinator для создания собственного платформы для управляемой сборки.

```shell
cd ~/Projects/hello-from-csharp
objcgen ~/Projects/hello-from-csharp/hello-from-csharp/bin/Debug/hello-from-csharp.dll --target=framework --platform=iOS --outdir=output -c --debug
```

Платформа будет помещен в `~/Projects/hello-from-csharp/output/hello-from-csharp.framework`.

### <a name="use-the-generated-output-in-an-xcode-project"></a>Использовать созданные выходные данные в проекте Xcode

Откройте в Xcode и создать новый iOS одним приложением представление, присвойте ей имя `hello-from-csharp` и выберите **Objective-C** языка.

Откройте `~/Projects/hello-from-csharp/output` каталог в системе поиска выберите `hello-from-csharp.framework`, перетащите его к проекту Xcode и поместите его непосредственно над `hello-from-csharp` папку в проекте.

! [Перетаскивание framework] Images/Hello-from-CSharp-IOS-Drag-DROP-Framework.PNG)

Убедитесь, что `Copy items if needed` проверяется в появившемся окне и нажмите кнопку `Finish`.

![Копирование элементов при необходимости](ios-images/hello-from-csharp-ios-copy-items-if-needed.png)

Выберите `hello-from-csharp` проекта и перейдите к `hello-from-csharp` целевого объекта **вкладка "Общие"**. В **внедренные двоичный** добавьте `hello-from-csharp.framework`.

![Внедренные двоичных файлов](ios-images/hello-from-csharp-ios-embedded-binaries.png)

Откройте ViewController.m и замените содержимое с:

```objective-c
#import "ViewController.h"
#include "hello-from-csharp/hello-from-csharp.h"

@interface ViewController ()
@end

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];

    MyUIView *view = [[MyUIView alloc] init];
    view.frame = CGRectMake(0, 200, 200, 200);
    [self.view addSubview: view];
}
@end
```

Наконец, запустите проект Xcode и будут отображаться примерно следующим образом:

![Hello из примера для C# в симуляторе](ios-images/hello-from-csharp-ios.png)
