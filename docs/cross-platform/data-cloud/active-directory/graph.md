---
title: Доступ к API Graph
description: В этом документе описывается добавление проверки подлинности Azure Active Directory для мобильных приложений, созданных с помощью Xamarin.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: c43dfa79831f22e55490b27c3c360602ae717627
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781134"
---
# <a name="accessing-the-graph-api"></a>Доступ к API Graph

Выполните следующие действия, чтобы использовать API Graph из приложения Xamarin.

1. [Регистрация в Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на *windowsazure.com* портала, нажмите
2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Шаг 3. Добавление проверки подлинности Active Directory для приложения

В приложении, добавьте ссылку на **Azure библиотеку аутентификации Active Directory (ADAL в Azure)** с помощью диспетчера пакетов NuGet в Visual Studio или Visual Studio для Mac.
Необходимо выбрать **Показать пакеты предварительного выпуска** также можно включить этот пакет по-прежнему в предварительной версии.

> [!IMPORTANT]
> Примечание: Azure ADAL 3.0 в данный момент Предварительный просмотр и могут существовать критическим изменениям до выпуска окончательной версии. 


![](graph-images/06.-adal-nuget-package.jpg "Добавьте ссылку на Azure библиотеку аутентификации Active Directory (ADAL в Azure)")

В приложении теперь необходимо добавить следующие переменные уровня класса, которые требуются для потока проверки подлинности.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Следует обратить внимание вот `commonAuthority`. При проверки подлинности конечной точки `common`, приложение становится **мультитенантной**, означающее любой пользователь с свои учетные данные Active Directory можно использовать имя входа. После проверки подлинности этот пользователь будет работать в контексте собственной службы Active Directory — т. е. они будут видеть сведения, относящиеся к его Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Напишите метод для получения токена доступа

Приведенный ниже код (в Android) будут запуск проверки подлинности и после завершения присваивания результата в `authResult`. IOS и Windows Phone реализации немного отличаться: второй параметр (`Activity`) отличается в iOS и отсутствует на Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

В приведенном выше коде `AuthenticationContext` отвечает за проверку подлинности на commonAuthority. Он имеет `AcquireTokenAsync` метод, который будет принимать параметры в качестве ресурса, который должен быть доступен, в этом случае `graphResourceUri`, `clientId`, и `returnUri`. Приложение вернется к `returnUri` после завершения проверки подлинности. Этот код будет оставаться одинаковым для всех платформ, однако последний параметр `AuthorizationParameters`, будут отличаться на разных платформах и отвечает за управление потока проверки подлинности.

В случае Android или iOS, мы передаем `this` параметр `AuthorizationParameters(this)` для совместного использования контекста, в то время как в Windows будет передано без какого-либо параметра как новый `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Маркер продолжения для Android

После завершения проверки подлинности поток должен вернуться в приложение. В случае Android обрабатывается следующий код, который следует добавить **MainActivity.cs**:


```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);

    
}
```

### <a name="handle-continuation-for-windows-phone"></a>Маркер продолжения для Windows Phone

Windows Phone измените `OnActivated` метод в **App.xaml.cs** -файл кода:

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Теперь при запуске приложения, вы увидите диалоговое окно проверки подлинности.
После успешной проверки подлинности он будет запрашивать разрешения для доступа к ресурсам (в нашем случае Graph API):

![](graph-images/08.-authentication-flow.jpg "После успешной проверки подлинности будет запрашивать разрешения для доступа к ресурсам в нашем случае Graph API")

Если проверка подлинности будет успешной и вы задали приложению доступ к ресурсам, вы должны получить `AccessToken` и `RefreshToken` поле со списком в `authResult`. Эти токены необходимы для дальнейшего вызовов API и для авторизации с помощью Azure Active Directory в фоновом.

![](graph-images/07.-access-token-for-authentication.jpg "Эти токены необходимы дополнительные вызовы API, а также для авторизации с помощью Azure Active Directory в фоновом")

Например приведенный ниже код позволяет получить список пользователей из Active Directory. Можно заменить URL-адрес Web API веб-API, защищенного Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```

