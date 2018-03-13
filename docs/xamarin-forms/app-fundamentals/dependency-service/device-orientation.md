---
title: "Проверка ориентации устройства"
description: "Использовать помощью DependencyService для ориентации устройства доступ из общего кода"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: d23f29fbfb51473ff5f89f27c0bfd621cfffbce0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="checking-device-orientation"></a>Проверка ориентации устройства

В этой статье помогут вам использовать [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) Проверьте ориентацию устройства из общего кода с использованием собственных API для каждой платформы. Это пошаговое руководство основано на существующий `DeviceOrientation` подключаемого модуля по Ali Özgür. В разделе [в репозитории GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) для получения дополнительной информации.

- **[Создание интерфейса](#Creating_the_Interface)**  &ndash; понять, как интерфейс создается в общем коде.
- **[Реализация iOS](#iOS_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинный код для iOS.
- **[Реализация Android](#Android_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для Android.
- **[Реализация Windows](#WindowsImplementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для Windows Phone и универсальной платформы Windows (UWP).
- **[Реализация в общем коде](#Implementing_in_Shared_Code)**  &ndash; использование `DependencyService` вызывать собственную реализацию из общего кода.

Приложения с помощью `DependencyService` будет иметь следующую структуру:

![](device-orientation-images/orientation-diagram.png "Структура приложений помощью DependencyService")

> [!NOTE]
> Можно определить, является ли устройство в книжной и альбомной ориентации в общий код, как показано в [устройства Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). Метод, описанный в этой статье использует собственные функции для получения дополнительных сведений о ориентации, является ли устройство сверху вниз.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Создание интерфейса

Сначала необходимо Создайте интерфейс в общий код, который выражает функциональные возможности, которые планируется реализовать. Например интерфейс содержит один метод:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Процесс разработки для этого интерфейса в общий код позволит приложению доступ к API-интерфейсы ориентации устройства на каждой платформе Xamarin.Forms.

> [!NOTE]
> Классы, реализующие интерфейс должен иметь конструктор для работы с `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Реализация iOS

Интерфейс должен быть реализован в каждом проекте специфический для платформы приложений. Обратите внимание, что класс имеет конструктор, чтобы `DependencyService` можно создавать новые экземпляры:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Наконец, добавьте это `[assembly]` атрибута выше класса (и за пределами любого пространства имен, которые были определены), включая все необходимые `using` инструкции:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Этот атрибут класс регистрируется как реализация `IDeviceOrientation` интерфейс, который означает, что `DependencyService.Get<IDeviceOrientation>` может использоваться в общий код, чтобы создать его экземпляр.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Реализация Android

В следующем коде реализуется `IDeviceOrientation` на Android:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Добавьте это `[assembly]` атрибута выше класса (и за пределами любого пространства имен, которые были определены), включая все необходимые `using` инструкции:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Этот атрибут класс регистрируется как реализация `IDeviceOrientaiton` интерфейс, который означает, что `DependencyService.Get<IDeviceOrientation>` может использоваться в общий код можно создать его экземпляр.

<a name="WindowsImplementation" />

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone и реализации платформы универсальных приложений Windows

В следующем коде реализуется `IDeviceOrientation` интерфейса на Windows Phone и универсальной платформе Windows:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Добавить `[assembly]` атрибута выше класса (и за пределами любого пространства имен, которые были определены), включая все необходимые `using` инструкции:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Этот атрибут класс регистрируется как реализация `DeviceOrientationImplementation` интерфейс, который означает, что `DependencyService.Get<IDeviceOrientation>` может использоваться в общий код можно создать его экземпляр.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Реализация в общий код

Теперь мы можно писать и тестировать общий код, который обращается к `IDeviceOrientation` интерфейса. Эта простая страница содержит кнопки, которая обновляет свой собственный текст, в зависимости от ориентации устройства. Она использует `DependencyService` для получения экземпляра `IDeviceOrientation` интерфейс &ndash; во время выполнения этот экземпляр будет реализации платформой, имеет полный доступ к собственного пакета SDK:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Запуск этого приложения в iOS, Android или платформ Windows и нажав кнопку приведет текст кнопки, обновление с ориентации устройства.

![](device-orientation-images/orientation.png "Образец ориентации устройства")


## <a name="related-links"></a>Связанные ссылки

- [С помощью с помощью DependencyService (пример)](https://developer.xamarin.com/samples/UsingDependencyService)
- [Помощью DependencyService (пример)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Примеры Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
