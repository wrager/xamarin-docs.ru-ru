---
title: 'Xamarin.Essentials: подключение'
description: Класс подключения в Xamarin.Essentials позволяет отслеживать изменения в условиях сетевого устройства, проверьте текущее доступа к сети и каким образом он в настоящее время подключен.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 54c165e15e725caaecb1573b74cfe295170db141
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782870"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: подключение

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Подключения** класса позволяет отслеживать изменения условий сети устройства, проверьте текущее доступа к сети и каким образом он в настоящее время подключен.

## <a name="getting-started"></a>Начало работы

Чтобы получить доступ к **подключения** требуется функциональность следующие платформы определенные настройки.

# <a name="androidtabandroid"></a>[Android](#tab/android)

`AccessNetworkState` Разрешение является обязательным и должен быть настроен в проекте Android. Это можно сделать одним из следующих способов:

Откройте **AssemblyInfo.cs** файл **свойства** папки и добавьте:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

ИЛИ обновление манифеста Android.

Откройте **AndroidManifest.xml** файл **свойства** папки и добавьте следующий код внутри блока **манифеста** узла.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

Или щелкните правой кнопкой мыши проект Anroid и откройте свойства проекта. В разделе **Android манифеста** найти **требуемые разрешения:** области и проверка **состояние доступа к сети** разрешение. Будет автоматически обновлена **AndroidManifest.xml** файла.

# <a name="iostabios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-connectivity"></a>С помощью подключения к

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Проверка текущей сетевой доступ:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Доступ к сети](xref:Xamarin.Essentials.NetworkAccess) подразделяется на следующие категории:

* **Интернет** — локальную и доступом в Интернет.
* **ConstrainedInternet** — ограниченный доступ к Интернету. Указывает кучную подключения к порталу, предоставляется локальный доступ к веб-портал, когда требуется доступ к Интернету, что определенные учетные данные предоставляются через портал.
* **Локальный** — локальный доступ только по сети.
* **Нет** — подключение не доступно.
* **Неизвестный** — не удается определить подключение к Интернету.

Можно проверить, какой тип [профиля подключения](xref:Xamarin.Essentials.ConnectionProfile) устройство активно использует:

```csharp
var profiles = Connectivity.Profiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Каждый раз, когда профиль подключения или сетевой доступ к изменения можно получить событие при запуске:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(ConnectivityChangedEventArgs  e)
    {
        var access = e.NetworkAccess;
        var profiles = e.Profiles;
    }
}
```

## <a name="limitations"></a>Ограничения

Обратите внимание, что очень важно, `Internet` сообщается с помощью `NetworkAccess` , но не доступен полный доступ к Интернету. Из-за работы подключения для каждой платформы, оно гарантирует только подключение доступно. Для экземпляра устройство может быть подключено к сети Wi-Fi, но маршрутизатор отключен от Интернета. В данном экземпляре, может сообщаться Internet, но недоступен активное соединение.

## <a name="api"></a>API

* [Подключение исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Документация по API-интерфейса](xref:Xamarin.Essentials.Connectivity)
