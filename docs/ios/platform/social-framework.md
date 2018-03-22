---
title: "Социальные Framework"
description: "Социальные Framework предоставляет универсальный интерфейс API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также SinaWeibo для пользователей в Китае."
ms.topic: article
ms.prod: xamarin
ms.assetid: A1C28E66-AA20-1C13-23AF-5A8712E6C752
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 7a190014abd3386a3a675d50ce6a89101d0588a7
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="social-framework"></a>Социальные Framework

_Социальные Framework предоставляет универсальный интерфейс API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также SinaWeibo для пользователей в Китае._


С помощью социальных сетей платформы позволяет приложениям взаимодействовать с социальными сетями из единый интерфейс API без необходимости управлять проверкой подлинности. Он включает систему указано представление контроллер для составления сообщения, а также абстракция, разрешает использование API каждого социальной сети по протоколу HTTP.

> [!IMPORTANT]
> Кросс платформенных API для подключения к различным социальных сетей, в разделе [Xamarin.Social](http://components.xamarin.com/view/xamarin.social/) в хранилище компонентов Xamarin.

## <a name="connecting-to-twitter"></a>Подключения к Twitter

### <a name="twitter-account-settings"></a>Параметры учетной записи Twitter

Для подключения к Twitter, используя платформу социальных сетей, учетной записи должна быть настроена в параметрах устройства, как показано ниже:

 [![](social-framework-images/twitter01.png "Параметры учетной записи Twitter")](social-framework-images/twitter01.png#lightbox)

После ввода учетной записи и проверить с помощью Twitter, любое приложение на устройстве, который использует классы социальных Framework для доступа к Twitter будет использовать эту учетную запись.

### <a name="sending-tweets"></a>Отправка Твиты

Платформа социальных включает контроллер с именем `SLComposeViewController` представляет представление для редактирования и отправки твитов предоставленном системой. Следующем снимке экрана показан пример этого представления.

 [![](social-framework-images/twitter02.png "На этом снимке экрана показан пример SLComposeViewController")](social-framework-images/twitter02.png#lightbox)

Для использования `SLComposeViewController` с Twitter, необходимо создать экземпляр контроллера путем вызова `FromService` метод с `SLServiceType.Twitter` как показано ниже:

```csharp
var slComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
```

После `SLComposeViewController` возвращается экземпляр, он может использоваться для предоставления пользовательского интерфейса для учета в Twitter. Тем не менее, прежде всего следует — в этом случае проверка доступности социальной сети Twitter путем вызова `IsAvailable`:

```csharp
if (SLComposeViewController.IsAvailable (SLServiceKind.Twitter)) {
  ...
}
```

 `SLComposeViewController` никогда не отправляет твит напрямую без вмешательства пользователя. Его можно инициализировать следующими способами:

-   `SetInitialText` — Добавляет исходный текст для отображения в твит. 
-  `AddUrl` — Добавляет твит URL-адрес.
-  `AddImage` — Добавляет изображение твит.


После инициализации, вызвав `PresentVIewController` отображает представление, созданное с `SLComposeViewController`. Пользователь может затем при необходимости изменить и отправки твитов или отменить его отправки. В любом случае следует удалить контроллер в `CompletionHandler`, где результат можно также проверить на предмет твит было отправлено отменена, как показано ниже:

```csharp
slComposer.CompletionHandler += (result) => {
  InvokeOnMainThread (() => {
    DismissViewController (true, null);
    resultsTextView.Text = result.ToString ();
  });
};
```

#### <a name="tweet-example"></a>Пример твит

В следующем коде показано использование `SLComposeViewController` для представления представление, используемое для отправки твитов:

```csharp
using System;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _twitterComposer = SLComposeViewController.FromService (SLServiceType.Twitter);
        #endregion

        #region Computed Properties
        public bool isTwitterAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Twitter); }
        }

        public SLComposeViewController TwitterComposer {
            get { return _twitterComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            SendTweet.Enabled = isTwitterAvailable;
        }
        #endregion

        #region Actions
        partial void SendTweet_TouchUpInside (UIButton sender)
        {
            // Set initial message
            TwitterComposer.SetInitialText ("Hello Twitter!");
            TwitterComposer.AddImage (UIImage.FromFile ("Icon.png"));
            TwitterComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (TwitterComposer, true, null);
        }
        #endregion
    }
}
```

### <a name="calling-twitter-api"></a>Вызов Twitter API

Социальные Framework также включает поддержку для выполнения HTTP-запросов к социальным сетям. Он инкапсулирует запроса в `SLRequest` класс, используемый для определенной социальной сети API.

Например следующий код выполняет запрос в Twitter для получения общей шкале (развернув на приведенном выше коде):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _twitterAccount;
#endregion

#region Computed Properties
public ACAccount TwitterAccount {
    get { return _twitterAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    SendTweet.Enabled = isTwitterAvailable;
    RequestTwitterTimeline.Enabled = false;

    // Initialize Twitter Account access 
    var accountStore = new ACAccountStore ();
    var accountType = accountStore.FindAccountType (ACAccountType.Twitter);

    // Request access to Twitter account
    accountStore.RequestAccess (accountType, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestTwitterTimeline.Enabled = true;
            });
        }
    });
}
#endregion

#region Actions
partial void RequestTwitterTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
    var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = TwitterAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Рассмотрим этот код, подробно. Во-первых он получает доступ к учетной записи хранилища и возвращает тип учетной записи Twitter:

```csharp
var accountStore = new ACAccountStore ();
var accountType = accountStore.FindAccountType (ACAccountType.Twitter);
```

Далее он запрашивает у пользователя, если ваше приложение может получить доступ к своей учетной записи Twitter и, если разрешен доступ учетной записи загружается в память и обновить пользовательский Интерфейс:

```csharp
// Request access to Twitter account
accountStore.RequestAccess (accountType, (granted, error) => {
    // Allowed by user?
    if (granted) {
        // Get account
        _twitterAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
        InvokeOnMainThread (() => {
            // Update UI
            RequestTwitterTimeline.Enabled = true;
        });
    }
});
```

При запросе данных временной шкалы (нажатием кнопки в пользовательском Интерфейсе), приложение сначала формы запроса на доступ к данным из Twitter:

```csharp
// Initialize request
var parameters = new NSDictionary ();
var url = new NSUrl("https://api.twitter.com/1.1/statuses/user_timeline.json?count=10");
var request = SLRequest.Create (SLServiceKind.Twitter, SLRequestMethod.Get, url, parameters);
```
В этом примере ограничивает возвращаемые результаты для последние десять записей, включая `?count=10` в URL-АДРЕСЕ. Наконец, он присоединяет запрос к учетной записи Twitter (который был загружен выше) и выполняет вызов Twitter для выборки данных:

```csharp
// Request data
request.Account = TwitterAccount;
request.PerformRequest ((data, response, error) => {
    // Was there an error?
    if (error == null) {
        // Was the request successful?
        if (response.StatusCode == 200) {
            // Yes, display it
            InvokeOnMainThread (() => {
                Results.Text = data.ToString ();
            });
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", response.StatusCode);
            });
        }
    } else {
        // No, display error
        InvokeOnMainThread (() => {
            Results.Text = string.Format ("Error: {0}", error);
        });
    }
});
```

Если данные были успешно загружены, будет отображаться необработанные данные JSON (как показано в примере выходных данных ниже):

[![](social-framework-images/twitter03.png "Пример отображения необработанных данных JSON")](social-framework-images/twitter03.png#lightbox)

В реальном приложении результаты JSON затем может быть проанализировано как обычный и результаты, которые отображаются для пользователя. В разделе [введение веб-служб](~/cross-platform/data-cloud/web-services/index.md) сведения о том, как выполнить синтаксический анализ JSON.

## <a name="connecting-to-facebook"></a>Подключение к Facebook

### <a name="facebook-account-settings"></a>Параметры учетной записи Facebook

Подключение к Facebook с платформой социальных очень похож на процесс, используемый для Twitter, показанном выше. Необходимо настроить учетную запись Facebook в настройках устройства, как показано ниже:

[![](social-framework-images/facebook01.png "Параметры учетной записи Facebook")](social-framework-images/facebook01.png#lightbox)

После настройки любого приложения на устройстве, который использует платформу социальных сетей будет использовать эту учетную запись для подключения к Facebook.

### <a name="posting-to-facebook"></a>Учет в Facebook

Как платформа социальных сетей — универсальный API, предназначенных для доступа к нескольким социальных сетей, код остается практически одинаковых вне зависимости от используемой социальной сети.

Например `SLComposeViewController` может использоваться в точности как показано в примере Twitter, показанного выше, переключение только различные параметры, относящиеся к Facebook и параметры. Пример:

```csharp
using System;
using Foundation;
using Social;
using UIKit;

namespace SocialFrameworkDemo
{
    public partial class ViewController : UIViewController
    {
        #region Private Variables
        private SLComposeViewController _facebookComposer = SLComposeViewController.FromService (SLServiceType.Facebook);
        #endregion

        #region Computed Properties
        public bool isFacebookAvailable {
            get { return SLComposeViewController.IsAvailable (SLServiceKind.Facebook); }
        }

        public SLComposeViewController FacebookComposer {
            get { return _facebookComposer; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update UI based on state
            PostToFacebook.Enabled = isFacebookAvailable;
        }
        #endregion

        #region Actions
        partial void PostToFacebook_TouchUpInside (UIButton sender)
        {
            // Set initial message
            FacebookComposer.SetInitialText ("Hello Facebook!");
            FacebookComposer.AddImage (UIImage.FromFile ("Icon.png"));
            FacebookComposer.CompletionHandler += (result) => {
                InvokeOnMainThread (() => {
                    DismissViewController (true, null);
                    Console.WriteLine ("Results: {0}", result);
                });
            };

            // Display controller
            PresentViewController (FacebookComposer, true, null);
        }
        #endregion
    }
}
```

При использовании с Facebook, `SLComposeViewController` отображает представление, которое выглядит почти идентичен Twitter, показывающая **Facebook** как заголовок в этом случае:

[![](social-framework-images/facebook02.png "Отображение SLComposeViewController")](social-framework-images/facebook02.png#lightbox)

### <a name="calling-facebook-graph-api"></a>Вызов API Facebook Graph

Аналогично Twitter в примере, социальных Framework `SLRequest` объект можно использовать с Facebook graph API. Например следующий код возвращает сведения из API graph об учетной записи Xamarin (развернув на приведенном выше коде):

```csharp
using Accounts;
...

#region Private Variables
private ACAccount _facebookAccount;
#endregion

#region Computed Properties
public ACAccount FacebookAccount {
    get { return _facebookAccount; }
}
#endregion

#region Override Methods
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update UI based on state
    PostToFacebook.Enabled = isFacebookAvailable;
    RequestFacebookTimeline.Enabled = false;

    // Initialize Facebook Account access 
    var accountStore = new ACAccountStore ();
    var options = new AccountStoreOptions ();
    var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
    accountType = accountStore.FindAccountType (ACAccountType.Facebook);

    // Request access to Facebook account
    accountStore.RequestAccess (accountType, options, (granted, error) => {
        // Allowed by user?
        if (granted) {
            // Get account
            _facebookAccount = accountStore.Accounts [accountStore.Accounts.Length - 1];
            InvokeOnMainThread (() => {
                // Update UI
                RequestFacebookTimeline.Enabled = true;
            });
        }
    });

}
#endregion

#region Actions
partial void RequestFacebookTimeline_TouchUpInside (UIButton sender)
{
    // Initialize request
    var parameters = new NSDictionary ();
    var url = new NSUrl ("https://graph.facebook.com/283148898401104");
    var request = SLRequest.Create (SLServiceKind.Facebook, SLRequestMethod.Get, url, parameters);

    // Request data
    request.Account = FacebookAccount;
    request.PerformRequest ((data, response, error) => {
        // Was there an error?
        if (error == null) {
            // Was the request successful?
            if (response.StatusCode == 200) {
                // Yes, display it
                InvokeOnMainThread (() => {
                    Results.Text = data.ToString ();
                });
            } else {
                // No, display error
                InvokeOnMainThread (() => {
                    Results.Text = string.Format ("Error: {0}", response.StatusCode);
                });
            }
        } else {
            // No, display error
            InvokeOnMainThread (() => {
                Results.Text = string.Format ("Error: {0}", error);
            });
        }
    });
}
#endregion
```

Единственная разница между этим кодом и версии Twitter, представленные выше, является необходимость Facebook получить разработчика или конкретных идентификатор приложения (который можно создать с портала разработчиков Facebook) которого необходимо задать в качестве альтернативы, при выполнении запроса:

```csharp
var options = new AccountStoreOptions ();
var options.FacebookAppId = ""; // Enter your specific Facebook App ID here
...

// Request access to Facebook account
accountStore.RequestAccess (accountType, options, (granted, error) => {
    ...
});
```

Не удалось задать этот параметр (или недопустимый ключ с помощью) приведет к ошибку или без возврата данных.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать платформу социальных сетей для взаимодействия с Twitter и Facebook. Показано, где можно настроить учетные записи для каждого социальной сети в настройках устройства. Кроме того, обсуждается способ использования `SLComposeViewController` для предоставления унифицированного представления для учета социальным сетям. Кроме того, он проверяет `SLRequest` класс, используемый для вызова API каждого социальных сетей.


## <a name="related-links"></a>Связанные ссылки

- [SocialFrameworkDemo (пример)](https://developer.xamarin.com/samples/SocialFrameworkDemo/)
- [Введение в веб-службы](~/cross-platform/data-cloud/web-services/index.md)
