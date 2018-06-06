---
title: 'Xamarin.Essentials: Сведения об устройстве'
description: Этот документ описывает класс DeviceInfo в Xamarin.Essentials, который предоставляет сведения об устройстве, что приложение работает.
ms.assetid: A1AC5373-926A-4FB6-8D7D-4B87EB8EB522
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: b7246afca19607ef2f70288d4643696f4ac35d52
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782402"
---
# <a name="xamarinessentials-device-information"></a>Xamarin.Essentials: Сведения об устройстве

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**DeviceInfo** класс предоставляет сведения об устройстве, приложение выполняется.

## <a name="using-deviceinfo"></a>С помощью DeviceInfo

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Через API-Интерфейс предоставляются следующие сведения:

```csharp
// Device Model (SMG-950U)
var device = DeviceInfo.Model;

// Manufacturer (Samsung)
var manufacturer = DeviceInfo.Manufacturer;

// Device Name (Motz's iPhone)
var deviceName = DeviceInfo.Name;

// Operating System Version Number (7.0)
var version = DeviceInfo.VersionString;

// Platform (Android)
var platform = DeviceInfo.Platform;

// Idiom (Phone)
var idiom = DeviceInfo.Idiom;

// Device Type (Physical)
var deviceType = DeviceInfo.DeviceType;
```

## <a name="platformsxrefxamarinessentialsdeviceinfoplatforms"></a>[Платформы](xref:Xamarin.Essentials.DeviceInfo.Platforms)

`DeviceInfo.Platform` Сопоставляет строковой константы, который сопоставляется операционной системы. Можно проверить значения с `Platforms` класса:

- **DeviceInfo.Platforms.iOS** — iOS
- **DeviceInfo.Platforms.Android** — Android
- **DeviceInfo.Platforms.UWP** — UWP
- **DeviceInfo.Platforms.Unsupported** — не поддерживается

## <a name="idiomsxrefxamarinessentialsdeviceinfoidioms"></a>[Стили](xref:Xamarin.Essentials.DeviceInfo.Idioms)

`DeviceInfo.Idiom` соотносится строковую константу, соответствующий типу устройства приложение выполняется на. Можно проверить значения с `Idioms` класса:

- **DeviceInfo.Idioms.Phone** — телефон
- **DeviceInfo.Idioms.Tablet** — планшета
- **DeviceInfo.Idioms.Desktop** — рабочего стола
- **DeviceInfo.Idioms.TV** — ТВ
- **DeviceInfo.Idioms.Unsupported** — не поддерживается

## <a name="device-type"></a>Тип устройства

`DeviceInfo.DeviceType` Сопоставляет перечисление, чтобы определить, является ли приложение задать задержку на физический или виртуальный устройства. Виртуальное устройство находится в симуляторе или эмуляторе.

## <a name="api"></a>API

- [DeviceInfo исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceInfo)
- [Документация по DeviceInfo API](xref:Xamarin.Essentials.DeviceInfo)
