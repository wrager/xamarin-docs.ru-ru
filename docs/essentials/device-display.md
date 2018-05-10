---
title: Сведения об отображении Xamarin.Essentials устройства
description: Класс DeviceDisplay предоставляет сведения о метриках экрана устройства приложение выполняется на.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 570fb6cf3eadfbbc7e189f875498d3cea0bc1514
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-device-display-information"></a>Сведения об отображении Xamarin.Essentials устройства

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** класс предоставляет сведения о метриках экрана устройства, приложения.

## <a name="using-devicedisplay"></a>С помощью DeviceDisplay

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Метрики экрана

Помимо базовую информацию об устройстве **DeviceDisplay** класс содержит сведения о экрана устройства и ориентацию.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** класс также предоставляет и события, можно подписаться, которое вызывает событие, когда любой метрики изменения экрана:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanaged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChanagedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/DeviceDisplay)
- [Документация по DeviceDisplay API](xref:Xamarin.Essentials.DeviceDisplay)
