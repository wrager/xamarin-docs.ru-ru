---
title: Проверка подлинности пользователей с Azure Active Directory B2C
description: Azure Active Directory B2C — это Облачное решение управления удостоверения для веб-потребительском и приложений для мобильных устройств. В этой статье показано, как с помощью библиотеки проверки подлинности Майкрософт и Azure Active Directory B2C интеграции управления удостоверениями потребителя в мобильного приложения.
ms.prod: xamarin
ms.assetid: B0A5DB65-0585-4A00-B908-22CCC286E6B6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: 627c6773c099c9cf45f871a9bb73a201bf98271a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2018
---
# <a name="authenticating-users-with-azure-active-directory-b2c"></a>Проверка подлинности пользователей с Azure Active Directory B2C

_Azure Active Directory B2C — это Облачное решение управления удостоверения для веб-потребительском и приложений для мобильных устройств. В этой статье показано, как с помощью библиотеки проверки подлинности Майкрософт и Azure Active Directory B2C интеграции управления удостоверениями потребителя в мобильного приложения._

![](~/media/shared/preview.png "Этот API является в настоящее время в предварительной версии")

> [!NOTE]
> [Библиотеки проверки подлинности Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) находится на стадии предварительной версии, но подходит для использования в рабочей среде. Тем не менее может критически важных изменений в API, формат внутреннего кэша и другие механизмы библиотеку, которая может повлиять на приложения.

## <a name="overview"></a>Обзор

Azure Active Directory B2C является служба управления идентификацией для потребительском приложений, который позволяет пользователю войти в приложение с:

- С помощью существующего социальных учетные записи (Microsoft, Google, Facebook, Amazon, LinkedIn).
- Создание новых учетных данных (адрес электронной почты и пароль, или имя пользователя и пароль). Эти учетные данные, называются *локального* учетные записи.

Интеграция со службой управления удостоверение Azure Active Directory B2C в мобильного приложения выполняется следующим образом:

1. Создание клиента Azure Active Directory B2C. Дополнительные сведения см. в разделе [создание клиента Azure Active Directory B2C на портале Azure](/azure/active-directory-b2c/active-directory-b2c-get-started/).
1. Зарегистрируйте мобильное приложение с клиентом Azure Active Directory B2C. Процесс регистрации назначает **идентификатор приложения** , однозначно определяющее приложение и **URL-адрес перенаправления** может использоваться для направления ответов обратно в приложение. Дополнительные сведения см. в разделе [Azure Active Directory B2C: зарегистрировать приложение](/azure/active-directory-b2c/active-directory-b2c-app-registration/).
1. Создание политики регистрации и входе в систему. Эта политика будет определять действия, необходимые, потребители проходит через во время регистрации и входа в и также определяет содержимое токенов, приложение будет получать успешной регистрации или входа в систему. Дополнительные сведения см. в разделе [Azure Active Directory B2C: встроенных политик](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).
1. Используйте [библиотеки проверки подлинности Microsoft](https://www.nuget.org/packages/Microsoft.Identity.Client) (MSAL) в мобильное приложение для запуска рабочего процесса проверки подлинности с вашим клиентом Azure Active Directory B2C.

> [!NOTE]
> А также интеграции Azure Active Directory B2C удостоверений в приложения для мобильных устройств, MSAL может также использоваться для интеграции управления удостоверениями Azure Active Directory в приложения для мобильных устройств. Это можно сделать с помощью регистрации мобильного приложения Azure Active Directory в [портала регистрации приложения](https://apps.dev.microsoft.com/). Процесс регистрации назначает **идентификатор приложения** , однозначно определяющий приложения, который должен быть указан при использовании MSAL. Дополнительные сведения см. в разделе [как зарегистрировать приложение с конечной точкой v2.0](/azure/active-directory/develop/active-directory-v2-app-registration/), и [проверки подлинности вашего мобильного приложения с помощью Microsoft библиотеки проверки подлинности](https://blog.xamarin.com/authenticate-mobile-apps-using-microsoft-authentication-library/) в блоге Xamarin.

MSAL использует веб-браузере устройства для проверки подлинности. Это повышает удобство использования приложения, как пользователям нужно только вход после каждого устройства, повышение скорости преобразования входа и авторизации потоки в приложении. Обозреватель устройства также обеспечивает повышенную безопасность. Пользователь не завершит процесс проверки подлинности, элемент управления будет возвращать приложению из вкладку веб-браузера. Это достигается путем регистрации пользовательской схемы URL-адрес для перенаправления URL-адрес, возвращенный процесс проверки подлинности, а затем обнаружение и обработка пользовательский URL-адрес после отправки. Дополнительные сведения о выборе пользовательской схемы URL-адрес см. в разделе [Выбор URI перенаправления собственного приложения](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-native-app-redirect-uri/).

> [!NOTE]
> Механизм для регистрации пользовательской схемы URL-адрес с операционной системой и обработке схему конкретной платформы.

Указывает, каждый запрос, отправленный в клиент Azure Active Directory B2C *политики*. Политики описаны взаимодействий удостоверений потребителей например регистрации или входа в. Например политику регистрации позволяет определить поведение клиента Azure Active Directory B2C можно настроить следующие параметры:

- Типов счетов, потребители могут использовать для входа в приложение.
- Сбор данных от клиента во время регистрации.
- Многофакторная проверка подлинности.
- Содержимое страницы регистрации.
- Токен утверждений, которые мобильное приложение получает выполнения политики.

Клиент Azure Active Directory может содержать несколько политик для различных типов, которые затем могут использоваться в приложении, при необходимости. Кроме того политики может быть использован другими приложениями, что позволяет определять и изменять потребителя идентификаторов интерфейсов без изменения кода. Дополнительные сведения о политиках см. в разделе [Azure Active Directory B2C: встроенных политик](/azure/active-directory-b2c/active-directory-b2c-reference-policies/).

## <a name="setup"></a>Установка

Библиотека NuGet библиотеки аутентификации Microsoft (MSAL) необходимо добавить в проект переносимой библиотеки классов (PCL) и платформы проектов в решении Xamarin.Forms. Следующие разделы содержат дополнительные инструкции по использованию MSAL для взаимодействия с клиентом Azure Active Directory B2C из мобильного приложения.

### <a name="portable-class-library"></a>Переносимая библиотека классов

PCLs, которые используют MSAL нужно будет изменена для использования Profile7. Дополнительные сведения о переносимых библиотеках классов см. в разделе [Введение в переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md).

### <a name="ios"></a>iOS

На iOS, настраиваемые схема URL-адресов, которое было зарегистрировано в Azure Active Directory B2C должны быть зарегистрированы в **Info.plist**, как показано на следующем снимке экрана:

![](azure-ad-b2c-images/customurl-ios.png "Регистрация пользовательская схема URL-адрес в iOS")

По завершении запроса авторизации Azure Active Directory B2C перенаправляет зарегистрированных перенаправление URL-адреса. Поскольку URL-адрес использует пользовательской схемы, это приводит к запуска мобильного приложения iOS, передав в URL-АДРЕСЕ как параметр запуска, где он обрабатывается `OpenUrl` переопределить приложения `AppDelegate` класса, как показано в следующем коде Пример:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        ...
        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(url);
            return true;
        }
    }
}
```

Код в `OpenURL` метод гарантирует, что элемент управления возвращается к MSAL после завершения интерактивной часть рабочего процесса проверки подлинности.

### <a name="android"></a>Android

В Android пользовательские схема URL-адресов, которое было зарегистрировано в Azure Active Directory B2C должны быть зарегистрированы в **AndroidManifest.xml**, путем добавления `<activity>` элемент внутри существующего `<application>` элемента. `<activity>` Элемент указывает `IntentFilter` на `Activity` , обработка схему, а также показано в следующем примере:

```xml
<application ...>
  <activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      <data android:scheme="INSERT_URL_SCHEME_HERE" android:host="auth" />
    </intent-filter>
  </activity>
</application>
```

По завершении запроса авторизации Azure Active Directory B2C перенаправляет зарегистрированных перенаправление URL-адреса. Поскольку URL-адрес использует пользовательской схемы, это приводит к Android запуска мобильного приложения, передача в URL-АДРЕСЕ как параметр запуска, где оно обрабатывается `microsoft.identity.client.BrowserTabActivity`. Обратите внимание, что `data android:scheme` свойство должно быть присвоено нестандартной схемы URL-адрес, который регистрируется с помощью приложения Azure Active Directory B2C.

Кроме того `MainActivity` класса должны вноситься изменения, как показано в следующем примере кода:

```csharp
using Microsoft.Identity.Client;

namespace TodoAzure.Droid
{
    ...
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);
            Microsoft.WindowsAzure.MobileServices.CurrentPlatform.Init();
            LoadApplication(new App());
            App.UiParent = new UIParent(this);
        }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);
            AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode, resultCode, data);
        }
    }
}

```

`OnCreate` Метод изменяется путем назначения `UIParent` экземпляр `App.UiParent` свойство. Это гарантирует, что поток проверки подлинности происходит в контексте текущего действия.

Код в `OnActivityResult` метод гарантирует, что элемент управления возвращается к MSAL после завершения интерактивной часть рабочего процесса проверки подлинности.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

На универсальной платформе Windows требуется использование MSAL без дополнительной настройки.

## <a name="initialization"></a>Инициализация

Библиотека проверки подлинности Microsoft использует члены `PublicClientApplication` класса для запуска рабочего процесса проверки подлинности. Образец приложения объявляются и инициализируются `public` свойства этого типа с именем `ADB2CClient`в `AuthenticationProvider` класса. В следующем примере кода показано, как это свойство инициализируется.

```csharp
ADB2CClient = new PublicClientApplication(Constants.ClientID, Constants.Authority);
```

При регистрации мобильных приложений с клиентом Azure Active Directory B2C, процесс регистрации, назначенный **идентификатор приложения**. Этот идентификатор должен быть указан в `PublicClientApplication` конструктор, вместе с `Authority` константу, которая состоит из базового URL-адреса и Azure Active Directory B2C политики для выполнения.

## <a name="signing-in"></a>Вход

Экран входа в образце приложения показано на следующих снимках экрана:

![](azure-ad-b2c-images/login.png "Страница входа")

Вход с поставщиками удостоверений из социальных сетей, или с учетной записью, разрешены. Хотя Майкрософт, Google и Facebook, как показано выше, используются как поставщики удостоверений из социальных сетей, можно также использоваться другими поставщиками удостоверений.

В следующем примере кода показан способ вызова процесс входа:

```csharp
using Microsoft.Identity.Client;

public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult = await ADB2CClient.AcquireTokenAsync(
        Constants.Scopes,
        GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
        App.UiParent);
    ...
}

```

`AcquireTokenAsync` Метод запускает веб-браузере устройства и отображает параметры проверки подлинности, определенные в политике Azure Active Directory B2C, который указан в политике для ссылок на `Constants.Authority` константой. Эта политика определяет действия, необходимые, потребители проходит через во время регистрации и входа в систему и утверждений, приложение получит в успешной регистрации или входа в.

Результат `AcquireTokenAsync` вызов метода `AuthenticationResult` экземпляра. Если проверка подлинности прошла успешно, `AuthenticationResult` экземпляр будет содержать токен удостоверения, в которой будут кэширован локально. Если проверка подлинности завершается неудачно, `AuthenticationResult` экземпляр будет содержать данные о том, почему не удалось выполнить проверку подлинности.

В примере приложения, если проверка подлинности прошла успешно `TodoList` переходе на страницу.

## <a name="silent-re-authentication"></a>Автоматическая повторная проверка подлинности

Когда `LoginPage` в образце приложения на экране попытка получить маркер пользователя без отображения пользовательского интерфейса аутентификации. Это достигается с помощью `AcquireTokenSilentAsync` метода, как показано в следующем примере кода:

```csharp
public async Task<bool> LoginAsync(bool useSilent = false)
{
    ...
    AuthenticationResult authenticationResult;

    if (useSilent)
    {
        authenticationResult = await ADB2CClient.AcquireTokenSilentAsync(
            Constants.Scopes,
            GetUserByPolicy(ADB2CClient.Users, Constants.PolicySignUpSignIn),
            Constants.Authority,
            false);
    }
    ...
}
```

`AcquireTokenSilentAsync` Метод пытается получить маркер пользователя из кэша, без взаимодействия с пользователем для входа в систему. Это обрабатывает сценарий, где маркер подходящий могут уже присутствовать в кэше из предыдущих сеансов. Если выполнена успешно, попытки получения токена `TodoList` переходе на страницу. Если попытка получить маркер отсутствует, ничего не происходит, и пользователь будет иметь возможность запустить новый рабочий процесс проверки подлинности.

## <a name="signing-out"></a>Выход

В следующем примере кода показано, как вызывается процесс выхода.

```csharp
public async Task<bool> LogoutAsync()
{
    ...
    foreach (var user in ADB2CClient.Users)
    {
        ADB2CClient.Remove(user);
    }
    ...
}
```

Это удалит все токены проверки подлинности из локального кэша.

## <a name="summary"></a>Сводка

В этой статье показано, как с помощью библиотеки проверки подлинности Microsoft (MSAL) и Azure Active Directory B2C интеграции управления удостоверениями потребителя в мобильного приложения. Azure Active Directory B2C — это Облачное решение управления удостоверения для веб-потребительском и приложений для мобильных устройств.


## <a name="related-links"></a>Связанные ссылки

- [AzureADB2CAuth (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureADB2CAuth/)
- [Azure Active Directory B2C](/azure/active-directory-b2c/)
- [Библиотека проверки подлинности Майкрософт](https://www.nuget.org/packages/Microsoft.Identity.Client)
