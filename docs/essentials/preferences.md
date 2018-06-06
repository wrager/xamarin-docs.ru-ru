---
title: 'Xamarin.Essentials: параметры'
description: Этот документ описывает класс предпочтения в Xamarin.Essentials, который сохраняет параметры приложения в хранилище ключей и значений. В этом примере рассматривается использование классов и типов данных, которые могут быть сохранены.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e453c04a953e60be2508670723d175bde3dc7c42
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782853"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: параметры

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

- [Параметры исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Документация по API установки](xref:Xamarin.Essentials.Preferences)
