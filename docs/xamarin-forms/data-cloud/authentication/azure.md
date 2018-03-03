---
title: "Проверка подлинности пользователей с помощью Azure мобильных приложений"
description: "Мобильные приложения Azure использовать различные поставщики внешнего идентификатора для поддержки проверки подлинности и авторизации пользователей приложения, включая Facebook, Google, Microsoft, Twitter и Azure Active Directory. Для ограничения доступа к только те пользователи, можно установить разрешения для таблицы. В этой статье описывается использование мобильных приложений Azure для управления процессом проверки подлинности в приложении Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: D50D6F56-8B19-44E7-81F3-E0E1C6E240
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 823dcdfdaca79045a407b62ec7e75079ee25d72f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="authenticating-users-with-azure-mobile-apps"></a>Проверка подлинности пользователей с помощью Azure мобильных приложений

_Мобильные приложения Azure использовать различные поставщики внешнего идентификатора для поддержки проверки подлинности и авторизации пользователей приложения, включая Facebook, Google, Microsoft, Twitter и Azure Active Directory. Для ограничения доступа к только те пользователи, можно установить разрешения для таблицы. В этой статье описывается использование мобильных приложений Azure для управления процессом проверки подлинности в приложении Xamarin.Forms._

## <a name="overview"></a>Обзор

При необходимости управлять процессом проверки подлинности в приложении Xamarin.Forms мобильных приложений Azure выполняется следующим образом:

1. Регистрация мобильного приложения Azure на сайте поставщика удостоверений и затем задайте учетные данные, сформированное поставщиком в серверной части мобильных приложений. Дополнительные сведения см. в разделе [регистрации приложения для проверки подлинности и настроить приложение службы](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#register-your-app-for-authentication-and-configure-app-services).
1. Определите новую схему URL-адрес для приложения Xamarin.Forms, который позволяет системой проверки подлинности для перенаправления Xamarin.Forms приложению после завершения процесса проверки подлинности. Дополнительные сведения см. в разделе [добавить приложения разрешено внешний URL-адреса перенаправления](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#redirecturl).
1. Ограничьте доступ к серверной части мобильных приложений Azure только прошедшие проверку подлинности клиентов. Дополнительные сведения см. в разделе [ограничить разрешения пользователям, прошедшим проверку](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#restrict-permissions-to-authenticated-users).
1. Вызов проверки подлинности из приложения Xamarin.Forms. Дополнительные сведения см. в разделе [Добавление функции аутентификации в переносимой библиотеки классов](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-portable-class-library), [Добавление функции аутентификации в приложение iOS](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-ios-app), [Добавление функции аутентификации в приложении Android](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-the-android-app)и [ Добавление проверки подлинности для проектов приложений Windows 10](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users#add-authentication-to-windows-10-including-phone-app-projects).

> [!NOTE]
> В iOS 9 и более поздней версии безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения, чтобы предотвратить случайное раскрытие конфиденциальных сведений. Поскольку ATS включена по умолчанию в приложениях, разработанных для iOS 9, все соединения будут применяться ATS требования безопасности. Если соединения не удовлетворяют этим требованиям, произойдет сбой с исключением.
> Если это не позволяет использовать ATS могут быть присоединены из `HTTPS` протокола и безопасную передачу Интернет-ресурсов. Это можно сделать путем обновления приложения **Info.plist** файла. Дополнительные сведения см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

Исторически мобильных приложений используется внедренный веб-представления для выполнения проверки подлинности с поставщиком удостоверений. Это не рекомендуется использовать по следующим причинам:

- Приложения, на котором размещается веб-представление может получить доступ к полной проверки подлинности учетные данные пользователя, не только предоставления авторизации, предназначенного для приложения. Это приводит к нарушению принципа наименьших прав доступа, эти приложения имеют доступ к более мощных учетные данные, чем нужно, потенциально увеличивает уязвимость приложения.
- Ведущее приложение может записать имена пользователей и пароли, автоматически отправлять формы и обход согласие пользователя и скопируйте файлы cookie сеанса и использовать их для выполнения действий, прошедшего проверку подлинности имени пользователя.
- Внедренные веб-представления не разглашаем состояния проверки подлинности веб-браузере устройства, поскольку требует от пользователя вход в систему для каждого запроса авторизации, который считается меньше пользователем или другими приложениями.
- Некоторые конечные точки авторизации выполните действия для обнаружения и блокировки авторизации запросов, поступающих из веб-представления.

Можно использовать для выполнения проверки подлинности, что именно этот подход, выполнены в последнюю версию веб-браузере устройства [пакет SDK Azure Mobile клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/). С помощью обозревателя устройства для проверки подлинности запросов повышает удобство использования приложения, как пользователям нужно только для входа на поставщике удостоверений один раз на устройство, повышение скорости преобразования потоков входа и авторизации в приложении. Обозреватель устройства также обеспечивает повышенную безопасность, как приложения, могут проверять и изменять содержимое в веб-представление, но не содержимое, отображаемое в браузере.

## <a name="using-an-azure-mobile-apps-instance"></a>С помощью экземпляра Azure мобильных приложений

[Пакет SDK Azure Mobile клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) предоставляет `MobileServiceClient` класс, который используется приложением Xamarin.Forms для доступа к экземпляру мобильных приложений Azure.

Образец приложения использует Google в качестве поставщика удостоверений, в котором пользователи с учетными записями Google для входа в приложение Xamarin.Forms. Google используется в качестве поставщика удостоверений в этой статье, подход применяется одинаково для других поставщиков удостоверений.

<a name="logging-in" />

### <a name="logging-in-users"></a>Вход пользователей

Экран входа в образце приложения показано на следующих снимках экрана:

![](azure-images/login.png "Страница входа")

При использовании Google как поставщик удостоверений ряд других поставщиков удостоверений можно использовать, включая Facebook, Microsoft, Twitter и Azure Active Directory.

В следующем примере кода показан способ вызова процесса входа в систему:

```csharp
async void OnLoginButtonClicked(object sender, EventArgs e)
{
  ...
  if (App.Authenticator != null)
  {
    authenticated = await App.Authenticator.AuthenticateAsync();
  }
  ...
}
```

`App.Authenticator` Свойство `IAuthenticate` экземпляра, который задается параметром каждого проекта под конкретную платформу. `IAuthenticate` Интерфейс задает `AuthenticateAsync` операции, должны предоставляться каждой платформы проекта. Следовательно, вызов `App.Authenticator.AuthenticateAsync` выполняет метод `IAuthenticate.AuthenticateAsync` метод в проекте платформы.

Все платформы `IAuthenticate.AuthenticateAsync` вызов методов `MobileServiceClient.LoginAsync` метод для отображения имени входа интерфейса и кэш данных. В следующем примере кода показан `LoginAsync` метод для платформы iOS:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    UIApplication.SharedApplication.KeyWindow.RootViewController,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

В следующем примере кода показан `LoginAsync` метод для платформы Android:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    this,
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

В следующем примере кода показан `LoginAsync` метод для универсальной платформы Windows:

```csharp
public async Task<bool> AuthenticateAsync()
{
  ...
  // The authentication provider could also be Facebook, Twitter, or Microsoft
  user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(
    MobileServiceAuthenticationProvider.Google,
    Constants.URLScheme);
  ...
}
```

На всех платформах `MobileServiceAuthenticationProvider` перечисление используется для указания поставщика удостоверений, который будет использоваться в процессе проверки подлинности. Когда `MobileServiceClient.LoginAsync` вызывается метод, мобильные приложения Azure инициирует процесс проверки подлинности, отобразив страницу входа выбранного поставщика, а также путем создания после успешного входа в систему с поставщиком удостоверений маркер проверки подлинности. `MobileServiceClient.LoginAsync` Возвращает `MobileServiceUser` экземпляр, который будет храниться в `MobileServiceClient.CurrentUser` свойство. Это свойство предоставляет `UserId` и `MobileServiceAuthenticationToken` свойства. Они представляют, прошедшего проверку подлинности пользователя и маркер проверки подлинности для пользователя. Токен проверки подлинности будут включены во всех запросах к экземпляру мобильных приложений Azure, позволяя приложению Xamarin.Forms выполнять действия на экземпляре Azure мобильного приложения, требующие разрешения пользователя, прошедшего проверку подлинности.

<a name="logging-out" />

### <a name="logging-out-users"></a>Выход пользователей

В следующем примере кода показано, как вызывается процесс выхода.

```csharp
async void OnLogoutButtonClicked(object sender, EventArgs e)
{
  bool loggedOut = false;

  if (App.Authenticator != null)
  {
    loggedOut = await App.Authenticator.LogoutAsync ();
  }
  ...
}
```

`App.Authenticator` Свойство `IAuthenticate` экземпляр, который задается для каждого platformproject. `IAuthenticate` Интерфейс задает `LogoutAsync` операции, должны предоставляться каждой платформы проекта. Следовательно, вызов `App.Authenticator.LogoutAsync` выполняет метод `IAuthenticate.LogoutAsync` метод в проекте платформы.

Все платформы `IAuthenticate.LogoutAsync` вызов методов `MobileServiceClient.LogoutAsync` метод, чтобы отменить проверку подлинности пользователя вошедшего в систему с поставщиком удостоверений. В следующем примере кода показан `LogoutAsync` метод для платформы iOS:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  foreach (var cookie in NSHttpCookieStorage.SharedStorage.Cookies)
  {
    NSHttpCookieStorage.SharedStorage.DeleteCookie (cookie);
  }
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

В следующем примере кода показан `LogoutAsync` метод для платформы Android:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  CookieManager.Instance.RemoveAllCookie();
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

В следующем примере кода показан `LogoutAsync` метод для универсальной платформы Windows:

```csharp
public async Task<bool> LogoutAsync()
{
  ...
  await TodoItemManager.DefaultManager.CurrentClient.LogoutAsync();
  ...
}
```

Когда `IAuthenticate.LogoutAsync` вызывается метод, удаляются все файлы cookie, заданные поставщиком удостоверений, перед `MobileServiceClient.LogoutAsync` метод вызывается, чтобы отменить проверку подлинности пользователя вошедшего в систему с поставщиком удостоверений.

## <a name="summary"></a>Сводка

В этой статье описывается использование мобильных приложений Azure для управления процессом проверки подлинности в приложении Xamarin.Forms. Мобильные приложения Azure использовать различные поставщики внешнего идентификатора для поддержки проверки подлинности и авторизации пользователей приложения, включая Facebook, Google, Microsoft, Twitter и Azure Active Directory. `MobileServiceClient` Класс используется для Xamarin.Forms приложения для управления доступом к экземпляру мобильных приложений Azure.


## <a name="related-links"></a>Связанные ссылки

- [TodoAzureAuth (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuth/)
- [Использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Добавление проверки подлинности в приложение Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-users/)
- [Пакет SDK для Azure мобильного клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
