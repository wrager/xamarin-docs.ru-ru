---
title: 'Xamarin.Essentials: батареи'
description: Этот документ описывает класс батареи в Xamarin.Essentials, который позволяет проверить сведения батареи устройства и отслеживайте изменения.
ms.assetid: 47EB26D8-8C62-477B-A13C-6977F74E6E43
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 35764b4c2270359a7c010e1186f882e236e17fd5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782090"
---
# <a name="xamarinessentials-battery"></a>Xamarin.Essentials: батареи

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Батареи** позволяет проверить сведения батареи устройства и отслеживайте изменения.

## <a name="getting-started"></a>Начало работы

Чтобы получить доступ к **батареи** требуется функциональность следующие платформы определенные настройки.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`Battery` Разрешение является обязательным и должен быть настроен в проекте Android. Это можно сделать одним из следующих способов:

Откройте **AssemblyInfo.cs** файл **свойства** папки и добавьте:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Battery)]
```

ИЛИ обновление манифеста Android.

Откройте **AndroidManifest.xml** файл **свойства** папки и добавьте следующий код внутри блока **манифеста** узла.

```xml
<uses-permission android:name="android.permission.BATTERY" />
```

Или щелкните правой кнопкой мыши проект Anroid и откройте свойства проекта. В разделе **Android манифеста** найти **требуемые разрешения:** области и проверка **батареи** разрешение. Будет автоматически обновлена **AndroidManifest.xml** файла.

# <a name="iostabios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-battery"></a>Используя батареи

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Проверьте сведения о текущем батареи.

```csharp
var level = Battery.ChargeLevel; // returns 0.0 to 1.0 or -1.0 if unable to determine.

var state = Battery.State;

switch (state)
{
    case BatteryState.Charging:
        // Currently charging
        break;
    case BatteryState.Full:
        // Battery is full
        break;
    case BatteryState.Discharging:
    case BatteryState.NotCharging:
        // Currently discharging battery or not being charged
        break;
    case BatteryState.NotPresent:
        // Battery doesn't exist in device (desktop computer)
    case BatteryState.Unknown:
        // Unable to detect battery state
        break;
}

var source = Battery.PowerSource;

switch (source)
{
    case BatteryPowerSource.Battery:
        // Being powered by the battery
        break;
    case BatteryPowerSource.Ac:
        // Being powered by A/C unit
        break;
    case BatteryPowerSource.Usb:
        // Being powered by USB cable
        break;
    case BatteryPowerSource.Wireless:
        // Powered via wireless charging
        break;
    case BatteryPowerSource.Unknown:
        // Unable to detect power source
        break;
}
```

Событие происходит при каждом изменении любого свойства батареи:

```csharp
public class BatteryTest
{
    public BatteryTest()
    {
        // Register for battery changes, be sure to unsubscribe when needed
        Battery.BatteryChanged += Battery_BatteryChanged;
    }

    void Battery_BatteryChanged(BatteryChangedEventArgs   e)
    {
        var level = e.ChargeLevel;
        var state = e.State;
        var source = e.PowerSource;
        Console.WriteLine($"Reading: Level: {level}, State: {state}, Source: {source}");
    }
}
```

## <a name="platform-differences"></a>Различия между платформами

| Platform | Различие |
| --- | --- |
| iOS | Устройство должно использовать для тестирования интерфейсов API. |
| iOS | Будет возвращать только переменного тока или батареи для PowerSource. |
| UWP | Будет возвращать только переменного тока или батареи для PowerSource. |

## <a name="api"></a>API

- [Батарея исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Battery)
- [Документация по API батареи](xref:Xamarin.Essentials.Battery)
