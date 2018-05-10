---
title: Xamarin.Essentials защищенного хранилища
description: Класс SecureStorage помогает обеспечить безопасное хранение пар «ключ значение».
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
ms.openlocfilehash: 24d1e29ba0203aaafc3e21533478f6c505cc09b3
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-secure-storage"></a>Xamarin.Essentials защищенного хранилища

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**SecureStorage** класс помогает обеспечить безопасное хранение пар «ключ значение».

## <a name="using-secure-storage"></a>С помощью защищенного хранилища

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Чтобы сохранить значение для данного _ключ_ в защищенного хранилища:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

Чтобы получить значение из защищенного хранилища.

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

[Android ключей](https://developer.android.com/training/articles/keystore.html) используется для сохранения ключа шифрования для шифрования значение перед сохранением в [общих настроек](https://developer.android.com/training/data-storage/shared-preferences.html) с именем файла из **.xamarinessentials [YOUR--ПАКЕТА-идентификатор приложения]** .  Ключ, используемый в файле Общие настройки является _хэш MD5_ ключа, переданные в `SecureStorage` API-интерфейса.

## <a name="api-level-23-and-higher"></a>API уровня 23 и выше

На новые уровни API **AES** ключа можно было получить из хранилища ключей Android и использовать с **AES, GCM и NoPadding** шифр, чтобы зашифровать значение, прежде чем оно сохранено в файле Общие настройки.

## <a name="api-level-22-and-lower"></a>Уровень API 22 и ниже

На более старые уровни API Android хранилище ключей поддерживает только хранение **RSA** ключи, которые используются с **RSA, ECB/PKCS1Padding** шифрования для шифрования **AES** ключа (случайным образом создаются во время выполнения) и хранится в файле Общие настройки в разделе _SecureStorageKey_, если он уже не был создан.

Все зашифрованные значения будут удалены, когда приложение удаляется с устройства.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Цепочки ключей](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) используется для безопасного хранения значений на устройствах iOS.  `SecRecord` Используется для хранения значения имеет `Service` значение **.xamarinessentials [YOUR--ПАКЕТА-идентификатор приложения]**.

В некоторых случаях данные цепочки ключей синхронизируется с iCloud и после удаления приложение может не удаляются безопасные значения iCloud и другие устройства пользователя.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) используется для значения encryped безопасно на устройствах UWP.

Encryped значения хранятся в `ApplicationData.Current.LocalSettings`, внутри контейнера с именем **.xamarinessentials [идентификатор ВАШЕГО приложения]**.

После удаления приложения _LocalSettings_и все зашифрованные значения также удаляются.

-----

## <a name="limitations"></a>Ограничения

Этот API предназначен для хранения небольших объемов текста.  Производительность может снизиться при попытке использовать для хранения больших объемов текста.

## <a name="api"></a>API

- [SecureStorage исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [Документация по SecureStorage API](xref:Xamarin.Essentials.SecureStorage)
