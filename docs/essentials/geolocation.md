---
title: Географическое положение Xamarin.Essentials
description: Класс Geolocation предоставляет API для получения текущей координаты географического положения устройства.
ms.assetid: 8F66092C-13F0-4FEE-8AA5-901D5F79B357
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 399dcd54d574875bcb5e491e87731b817e840e54
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-geocoding"></a>Геокодирования Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Geolocation** класс предоставляет API для получения текущей координаты географического положения устройства.

## <a name="getting-started"></a>Начало работы

Чтобы получить доступ к **Geolocation** требуется функциональность следующие платформы определенные настройки.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Грубое и нормально расположение разрешения являются обязательными и должен быть настроен в проекте Android. Кроме того Если приложение предназначено для Android 5.0 (уровень API 21) или более поздней версии, необходимо объявить, ваше приложение использует аппаратные компоненты в файле манифеста. Это можно сделать одним из следующих способов:

Откройте **AssemblyInfo.cs** файл **свойства** папки и добавьте:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessCoarseLocation)]
[assembly: UsesPermission(Android.Manifest.Permission.AccessFineLocation)]
[assembly: UsesFeature("android.hardware.location", Required = false)]
[assembly: UsesFeature("android.hardware.location.gps", Required = false)]
[assembly: UsesFeature("android.hardware.location.network", Required = false)]
```

ИЛИ обновление манифеста Android.

Откройте **AndroidManifest.xml** файл **свойства** папки и добавьте следующий код внутри блока **манифеста** узла.

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-feature android:name="android.hardware.location" android:required="false" />
<uses-feature android:name="android.hardware.location.gps" android:required="false" />
<uses-feature android:name="android.hardware.location.network" android:required="false" />
```

Или щелкните правой кнопкой мыши проект Anroid и откройте свойства проекта. В разделе **Android манифеста** найти **требуемые разрешения:** области и проверка **ACCESS_COARSE_LOCATION** и **ACCESS_FINE_LOCATION**разрешения. Будет автоматически обновлена **AndroidManifest.xml** файла.

# <a name="iostabios"></a>[iOS](#tab/ios)

Приложения требуется наличие ключей вашей **Info.plist** для NSLocationWhenInUseUsageDescription, чтобы получить доступ к расположению устройства.

Откройте редактор plist-файл и добавьте **конфиденциальность - расположение при в используйте Описание использования** свойство и введите значение для отображения пользователю.

ИЛИ вручную изменить файл и добавьте следующие строки:

```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>This app needs access location when open.</string>
```

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Необходимо задать `Location` разрешение для приложения. Это можно сделать путем открытия **Package.appxmanifest** и выбрав команду **возможности** вкладка и проверка **расположение**.

-----

## <a name="using-geolocation"></a>С помощью географического положения

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Geoloation API также пользователю будет предложено ввести разрешений при необходимости.

Вы можете получить последним известным [расположение](xref:Xamarin.Essentials.Location) устройства путем вызова `GetLastKnownLocationAsync` метод. Это часто выполняется быстрее, выполнив полный запрос, но может быть менее точным.

```csharp
try
{
    var location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
    }
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
    // Unable to get location
}
```

Чтобы запросить текущего устройства [расположение](xref:Xamarin.Essentials.Location) координаты `GetLocationAsync` может использоваться. Рекомендуется для передачи полной `GeolocationRequest` и `CancellationToken` , так как он может занять некоторое время, чтобы получить расположение устройства.

```csharp
try
{
    var request = new GeolocationRequest(GeolocationAccuracy.Medium);
    var location = await Geolocation.GetLocationAsync(request);

    if (location != null)
    {
        Console.WriteLine($"Latitude: {location.Latitude}, Longitude: {location.Longitude}");
    }
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
    // Unable to get location
}
```

## <a name="geolocation-accuracy"></a>Точность определения географического положения

В следующей таблице приведены точность каждой платформы

### <a name="lowest"></a>Наименьшая

| Platform | Расстояние (в метрах) |
| --- | --- |
| Android | 500 |
| iOS | 3000 |
| UWP | 1000 - 5000 |

### <a name="low"></a>Low

| Platform | Расстояние (в метрах) |
| --- | --- |
| Android | 500 |
| iOS | 1000. |
| UWP | 300 - 3000 |

### <a name="medium-default"></a>Средний (по умолчанию)

| Platform | Расстояние (в метрах) |
| --- | --- |
| Android | 100 - 500 |
| iOS | 100 |
| UWP | 30-500 |

### <a name="high"></a>High

| Platform | Расстояние (в метрах) |
| --- | --- |
| Android | 0 - 100 |
| iOS | 10 |
| UWP | < = 10 |

### <a name="best"></a>Лучше всего

| Platform | Расстояние (в метрах) |
| --- | --- |
| Android | 0 - 100 |
| iOS | ~0 |
| UWP | < = 10 |

## <a name="api"></a>API

- [Географическое расположение исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Geolocation)
- [Документация по API географическое положение](xref:Xamarin.Essentials.Geolocation)
