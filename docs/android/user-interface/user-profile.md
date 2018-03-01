---
title: "Профиль пользователя"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1C58E12B-4634-4691-BF59-D5A3F6B0E6F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2017
ms.openlocfilehash: 53ac30abea05095583fcac5ddc315f93ce7024f2
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="user-profile"></a>Профиль пользователя

Android поддерживала перечислением контактов с `ContactsContract` поставщика с момента API уровня 5. Например, в список контактов, достаточно использовать `ContactContracts.Contacts` класса, как показано в следующем коде:

```csharp
var uri = ContactsContract.Contacts.ContentUri;
           
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.Id,
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);
           
if (cursor.MoveToFirst ()) {
    do {
        Console.WriteLine ("Contact ID: {0}, Contact Name: {1}",
            cursor.GetString (cursor.GetColumnIndex (projection [0])),
            cursor.GetString (cursor.GetColumnIndex (projection [1])));
                   
    } while (cursor.MoveToNext());
}
```

Android 4 (API уровня 14) новый `ContactsContact.Profile` класс доступен через поставщик ContactsContract. `ContactsContact.Profile` Предоставляет доступ к личный профиль владельца устройства, включая контактные данные, такие как владельца устройства имя и номер телефона.

<a name="Required_Permissions" />

## <a name="required-permissions"></a>Необходимые разрешения

Для чтения и записи контактные данные, приложения должны запрашивать `Read_Contacts` и `Write_Contacts` разрешения, соответственно. Кроме того, для чтения и редактирования профиля пользователя, приложения должны запрашивать `Read_Profile` и `Write_Profile` разрешения.

<a name="Updating_Profile_Data" />

## <a name="updating-profile-data"></a>Обновление данных профиля

После задания этих разрешений приложение может использовать обычные методы Android для взаимодействия с данными профиля пользователя. Например, чтобы обновить отображаемое имя профиля, вызовем `ContentResolver.Update` с `Uri` извлечь с помощью `ContactsContract.Profile.ContentRawContactsUri` свойства, как показано ниже:

```csharp
var values = new ContentValues ();
          
values.Put (ContactsContract.Contacts.InterfaceConsts.DisplayName,
    "John Doe");
           
ContentResolver.Update (ContactsContract.Profile.ContentRawContactsUri,
    values, null, null);
```

<a name="Reading_Profile_Data" />

## <a name="reading-profile-data"></a>Чтение данных профиля

Выполнение запроса для `ContactsContact.Profile.ContentUri` операций чтения обратно данные профиля. Например следующий код будет считывать отображаемое имя профиля пользователя.

```csharp
string[] projection = {
    ContactsContract.Contacts.InterfaceConsts.DisplayName };
           
var cursor = ManagedQuery (uri, projection, null, null, null);

if (cursor.MoveToFirst ()) {
    Console.WriteLine(
        cursor.GetString (cursor.GetColumnIndex (projection [0])));
}
```

<a name="Navigating_to_the_People_App" />

## <a name="navigating-to-the-people-app"></a>Переход к приложению людей

Наконец, чтобы перейти в конфигурацию в новое приложение людей, входящий в состав Android 4, просто создайте с целью `ActionView` действия и `ContactsContract.Profile.ContentUri`и передать его в `StartActivity` метод следующим образом:

```csharp
var intent = new Intent (Intent.ActionView,
    ContactsContract.Profile.ContentUri);           
StartActivity (intent);
```

При выполнении приведенный выше код, как показано на следующем снимке экрана пользователей приложения будет загружать профиль пользователя:

[![Отображение профиля пользователя John Doe приложения экрана людей](user-profile-images/15-people-app.png)](user-profile-images/15-people-app.png)

Работать с профилем пользователя теперь похож на взаимодействие с другими данными в Android и обеспечивает дополнительный уровень персонализации устройства.



## <a name="related-links"></a>Связанные ссылки

- [ContactsProviderDemo (пример)](https://developer.xamarin.com/samples/monodroid/ContactsProviderDemo/)
- [Знакомство с приложением Сандвичевы мороженого](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 платформы](http://developer.android.com/sdk/android-4.0.html)
