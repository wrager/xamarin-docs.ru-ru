---
title: Xamarin.Essentials Акселерометра
description: Класс акселерометр позволяет наблюдать за датчика акселерометр устройства, указывающий ускорение устройства в трех двухмерном пространстве.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 33364b5df8edd3a5cc745d0131067bd9f3940d69
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials Акселерометра

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Акселерометр** позволяет отслеживать датчика акселерометр устройства, указывающий ускорение устройства в трех двухмерном пространстве.

## <a name="using-accelerometer"></a>С помощью Акселерометра

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность акселерометр работает путем вызова `Start` и `Stop` методы для ожидания изменения для ускорения. Все изменения отправляются обратно через `ReadingChanged` событий. Ниже приведен пример использования:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
    }

    public void ToggleAcceleromter()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

Уведомить показания акселерометр в ж. G — единица тяготения принудительное равно выдерживать по полю сила притяжения Земли (9.81 м/с ^ 2).

Система координат определяется относительно экрана телефона в его по умолчанию. Оси не меняются местами, при изменении ориентации экрана устройства.

Горизонтальная ось X и точки справа оси Y вертикально обращена вверх и указывает ось Z к внешней лицевой экрана. В этой системе координат за экрана иметь отрицательные значения Z.

Примеры

* Когда устройство находится плоской на таблицы и помещается его слева направо, ускорение значения x будет положительным.

* Если устройство находится плоской на таблицы, имеет значение ускорение + 1,00 G или (+ 9.81 м/с ^ 2), ускорение устройства, которые соответствуют (0 МБ в секунду ^ 2) минус силы тяжести (-9.81 м/с ^ 2) и нормализованный как ж.

* Когда устройство находится плоской на таблицы и помещается над sky с ускорением м с ^ 2, ускорение значение равно 9.81 +, которому сопоставлен ускорение устройства (+ m/s ^ 2) минус силы тяжести (-9.81 м/с ^ 2) и нормализованного в ж. 

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Скорость датчика](xref:Xamarin.Essentials.SensorSpeed)

- **Самый быстрый** — получить данные датчика максимально быстро (не гарантирует возврат в потоке пользовательского интерфейса).
- **Игра** — интенсивность подходит для игры (не гарантирует возврат в потоке пользовательского интерфейса).
- **Обычный** — курс по умолчанию, подходит для изменения ориентации экрана.
- **Пользовательский интерфейс** — интенсивность подходит для общего пользовательского интерфейса.

## <a name="api"></a>API

- [Акселерометр исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Acceleromter)
- [Документация по API акселерометра](xref:Xamarin.Essentials.Accelerometer)
