---
title: Фонарика Xamarin.Essentials
description: Класс фонарика имеет возможность включить или отключить камеру устройства флэш-памяти, чтобы он стал фонарик.
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3d867d742314f2677eeab35bbc354f696c99bf75
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-flashlight"></a>Фонарика Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Фонарика** класс имеет возможность включить или отключить камеру устройства флэш-памяти, чтобы он стал фонарик.

## <a name="getting-started"></a>Начало работы

Чтобы получить доступ к **фонарика** требуется функциональность следующие платформы определенные настройки.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Разрешения на фонарика и камеры являются обязательными и должен быть настроен в проекте Android. Это можно сделать одним из следующих способов:

Откройте **AssemblyInfo.cs** файл **свойства** папки и добавьте:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

ИЛИ обновление манифеста Android.

Откройте **AndroidManifest.xml** файл **свойства** папки и добавьте следующий код внутри блока **манифеста** узла.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Или щелкните правой кнопкой мыши проект Anroid и откройте свойства проекта. В разделе **Android манифеста** найти **требуемые разрешения:** области и проверка **ФОНАРИКА** и **КАМЕРЫ** разрешения. Будет автоматически обновлена **AndroidManifest.xml** файла.

Добавив эти разрешения [Google Play автоматически будет отфильтровывать устройств](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) без определенного оборудования. Можно обойти это, добавив следующий код в файл AssemblyInfo.cs в проекте Android:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# <a name="iostabios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-flashlight"></a>С помощью фонарика

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Фонарика можно включать и отключать через `TurnOnAsync` и `TurnOffAsync` методов:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

### <a name="androidtabandroid-specifics"></a>[Android](#tab/android-specifics)

Класс фонарика был optmized на основе операционной системы устройства.

#### <a name="api-level-23-and-higher"></a>API уровня 23 и выше

На новые уровни API [Torch режим](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) позволяет включить или отключить единицы устройство флэш-памяти.

#### <a name="api-level-22-and-lower"></a>Уровень API 22 и ниже

Текстуры поверхности камеры создается, чтобы включить или отключить `FlashMode` камеры устройства. 

### <a name="iostabios-specifics"></a>[iOS](#tab/ios-specifics)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) используется для включения и отключения Torch и режим флэш-памяти устройства.

### <a name="uwptabuwp-specifics"></a>[UWP](#tab/uwp-specifics)

[Лампочка](https://docs.microsoft.com/en-us/uwp/api/windows.devices.lights.lamp) служит для обнаружения первого индикатора на задней панели устройства, чтобы включить или отключить.

-----

## <a name="api"></a>API

- [Фонарика исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Flashlight)
- [Документация фонарика API](xref:Xamarin.Essentials.Flashlight)
