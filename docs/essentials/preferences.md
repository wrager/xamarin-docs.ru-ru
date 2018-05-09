---
title: Параметры Xamarin.Essentials
description: Класс установки сохраняет параметры приложения в хранилище ключей и значений.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 3886432f51125508349fd7815ac374cb0339c254
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-preferences"></a>Параметры Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Предпочтения** класс помогает для хранения параметров приложения в хранилище ключей и значений.

## <a name="using-secure-storage"></a>С помощью защищенного хранилища

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Чтобы сохранить значение для данного _ключ_ в установках:

```csharp
Preferences.Set("my_key", "my_value");
```

Для извлечения значения из настроек или значение по умолчанию, если не задать:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Чтобы удалить _ключ_ из настроек:

```csharp
Preferences.Remove("my_key");
```

Чтобы удалить все настройки:

```csharp
Preferences.Clear();
```

Помимо этих методов, каждый из которых принимает необязательный в `sharedName` можно использовать, чтобы создать дополнительные контейнеры предпочтениями. Чтение ниже особенностей реализации платформы.

## <a name="supported-data-types"></a>Поддерживаемые типы данных

Следующие типы данных поддерживаются в **предпочтения**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

Все данные хранятся в [общих настроек](https://developer.android.com/training/data-storage/shared-preferences.html). Если не `sharedName` указан, используются параметры по умолчанию общие, в противном случае имя используется для получения **закрытый** общих параметров с указанным именем.

# <a name="iostabios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) используется для хранения значения на устройствах iOS. Если не `sharedName` указан `StandardUserDefaults` , используемый, в противном случае имя используется для создания нового `NSUserDefaults` с указанным именем, используемый для `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) используется для хранения значений на устройстве. Если не `sharedName` указан `LocalSettings` , используется, в противном случае имя используется для создания нового контейнера внутри `LocalSettings`.

--------------

## <a name="limitations"></a>Ограничения

При хранении строки, этот интерфейс API предназначен для хранения небольших объемов текста.  Производительность может быть недостаточно высокие, при попытке использовать для хранения больших объемов текста.

## <a name="api"></a>API

- [Параметры исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Preferences)
- [Документация по API установки](xref:Xamarin.Essentials.Preferences)
