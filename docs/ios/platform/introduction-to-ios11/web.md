---
title: Веб-изменения в iOS 11
description: В этом документе рассматриваются изменения, внесенные в WebKit и платформы служб Safari на iOS 11. Он описывает, как работать с стиля в SFSafariViewController обновлений и новых возможностях WKWebView.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: f5876a9d201950ebac45e8b1f786b0e97452a7f1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787452"
---
# <a name="web-changes-in-ios-11"></a>Веб-изменения в iOS 11

iOS 11 предлагает новый вариант браузера Safari — Safari 11.0 — включая изменения WebKit и SafariServices. В этом руководстве рассматриваются эти изменения.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` впервые появился в iOS 9 в качестве альтернативы для отображения веб-содержимого или проверка подлинности пользователей из приложения. Дополнительные сведения о его возможности можно найти в [веб-представления](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) руководства.

iOS 11 был представлен изменений стиля к представлению контроллеру Safari, предоставление пользователям более удобное взаимодействие между приложением и веб-сайте. Например удаление адресной строки теперь дает представление-контроллер Safari вида в приложении браузера, а не мини-браузера. Можно также настроить цветовую схему, в соответствии с цветовой схемы приложения, задав `preferredBarTintColor` и `PreferredControlTintColor` свойства:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

В следующем фрагменте кода обрабатывается полосы в фиолетовым и белым цветом, как показано на следующем рисунке:

![К просмотру в белый и фиолетовым полосы SFSafariViewController](web-images/image1.png)

Также можно изменить, установив кнопку Закрыть, представленных в Safari View Controller `DismissButtonStyle` значение `Done`, `Close`, или `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Отклонить изменения текста кнопки](web-images/image2.png)

Это значение может быть изменено во время `SFSafariViewController` представлены.


В зависимости от содержимого, отображаются внутри контроллер представление Safari может потребоваться убедитесь, что строки меню не свернуть прокрутке пользователем. Этот параметр доступен, задав новый `BarCollapsedEnabled` свойства `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Штрих-свертывания отключена](web-images/image3.png)

Apple внесла обновлений к конфиденциальности в контроллере представление Safari iOS 11. Теперь просматривать данные, такие как файлы cookie и локальное хранилище существуют только на основе каждого приложения, а не для всех экземпляров Safari представление-контроллер. В этом случае закрытый в приложении просмотра активности пользователей.

Дополнительные функции например перетаскивание поддержку URL-адресов и поддержка `window.open()` также были добавлены к `SFSafariViewController` в iOS 11. Можно найти дополнительные сведения об этих новых функциях в [SFSafariViewController документации компании Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` впервые появился в составе WebKit в iOS 8 как средство отображения веб-содержимого для пользователя. Гораздо более возможность настройки, чем `SFSafariViewController`, что позволяет создавать собственные навигации и пользовательский интерфейс.

Добавлены три основные улучшения для Apple `WKWebView` с iOS 11: 

- Возможность управления файлами cookie
- Фильтрация содержимого
- Загрузка настраиваемого ресурса. 

Файл cookie управления осуществляется с помощью нового [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) класс, который позволяет добавлять и удалять файлы cookie, чтобы получить все файлы cookie, сохраненные в WKWebView и для наблюдения за хранилище для изменения файла cookie.

Содержимого фильтрации позволяет управлять типа содержимого, которое будут видеть пользователей, позволяя убедитесь, что его безопасный семейства понятного имени и, при необходимости, доступно только в выбранной группе пользователей. Это реализуется с помощью нового [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) класса, указав пары триггеров и действий в формате JSON. Дополнительные сведения об этих триггеров и действий, которые можно найти в компании Apple [содержимого правила блокировки](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) руководства.

iOS 11 теперь позволяет настраивать `WKWebView` с настраиваемого ресурса загрузки для веб-содержимого. Это реализуется с помощью `IWKUrlSchemeHandler` интерфейс, который позволяет обрабатывать URL-схем, не являются собственными для веб-пакета. Этот интерфейс содержит метод start и stop, который должен быть реализован:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

После реализации обработчик используется для задания `SetUrlSchemeHandler` свойство `WKWebViewConfiguration`. Затем нужно Загрузите URL-адрес объекта, использующего пользовательской схемы:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

