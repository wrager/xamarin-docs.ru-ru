---
title: Веб-представления в Xamarin.iOS
description: Этот документ описывает различные способы приложения Xamarin.iOS можно отобразить веб-содержимого. Он описывает UIWebView, WKWebView, SFSafariViewController, Safari и безопасность транспорта для приложения.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f720eae68415ab9efe021e53c9da4875209cd221
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790500"
---
# <a name="web-views-in-xamarinios"></a>Веб-представления в Xamarin.iOS

В течение времени существования iOS Apple был выпущен целый ряд способов для разработки приложений для включения веб-функций представления в своих приложениях. Большинство пользователей использовать встроенный веб-браузера Safari на устройстве iOS и ожидать, функциональные возможности веб-представление из других приложений согласуется с данного интерфейса. Ожидают, что те же жесты для работы, производительность совпадать на линейку и функциональные возможности.

В этой статье мы рассмотрим каждый из трех веб-представления, предоставляемые Apple: `UIWebView`, `WKWebview`, и `SFSafariViewController`их сходства и различия и описание их использования. 

iOS 11 появились новые изменения как `WKWebView` и `SFSafariViewController`. Дополнительные сведения об этом см. в разделе [веб-изменения в руководстве iOS 11](~/ios/platform/introduction-to-ios11/web.md) руководства.

## <a name="uiwebview"></a>UIWebView

`UIWebView` — Apple устаревший способ предоставления веб-содержимого в приложении. Было выпущено в iOS 2.0 и рекомендуется к использованию начиная с 8.0.

Если вы планируете поддерживать iOS версии более ранней, чем 8.0, необходимо использовать `UIWebView`. Из-за того, что `UIWebView` меньше оптимизирован для производительности чем альтернативы, рекомендуется проверять версию iOS пользователя. Если он 8.0 или более поздней версии, с помощью одного из вариантов объясняются ниже создаст улучшить взаимодействие с пользователем.
 
Чтобы добавить UIWebView Xamarin.iOS приложения, используйте следующий код:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Получается следующий веб-представление.

[![](uiwebview-images/webview.png "Эффект ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Дополнительные сведения об использовании `UIWebView`, см. следующие рецепты:


- [Загрузить веб-страницу](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Загрузить локального содержимого](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Загрузить веб-документы](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` впервые появился в iOS 8 позволяя разработчиками приложений для реализации веб-интерфейсом, похожим Safari мобильных страниц. Причиной этого является, в частности, тот факт, `WKWebView` использует обработчик Nitro Javascript, тот же механизм, используемый мобильных Safari. `WKWebView` следует всегда использовать UIWebView были недоступны из-за отказа [повышенную производительность](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), встроенных в понятных жестов и простотой взаимодействие между веб-страницы и приложения.
  
`WKWebView` могут добавляться в приложение в виде почти такую же UIWebView, однако разработчик имеют больший контроль над пользовательского интерфейса и взаимодействия с Пользователем и функциональные возможности. Создание и отображение объекта view web будет отобразить запрошенную страницу, однако можно выбрать способ представления представления, как пользователь может перемещаться и как пользователь выходит из представления.  

В следующем примере кода можно использовать для запуска `WKWebView` в приложении Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Получается следующий веб-представление.

[![](uiwebview-images/wkwebview.png "Пример веб-представление без ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Важно обратить внимание, что `WKWebView` находится в пространстве имен WebKit, их необходимо добавить это с помощью директивы в начало класса.

`WKWebView` также может использоваться в приложениях, Xamarin.Mac и поэтому может потребоваться рассмотреть возможность его использования при создании кроссплатформенного приложения Mac и iOS.

[Обрабатывать предупреждения JavaScript](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) рецепт также содержит сведения об использовании WKWebView с Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` Последний способ предоставления веб-содержимого в приложении и в iOS 9 и более поздних версий. В отличие от `UIWebView` или `WKWebView`, `SFSafariViewController` — это представление-контроллер и поэтому не может использоваться с другими представлениями. Должно представлять `SFSafariViewController` как контроллер новое представление, так же должны выводиться любой контроллер представление.
 
 `SFSafariViewController` Это по сути «мини-safari», могут быть внедрены в веб-приложения. Как WKWebView он использует тот же механизм Nitro Javascript, но также предоставляет широкий набор дополнительных возможностей Safari, таких как Автозаполнение, чтения и возможность совместного использования файлов cookie и данные с мобильных устройств Safari. Взаимодействие между пользователем и `SFSafariViewController` недоступен для приложения. Приложение не будет доступа к Safari компонентов по умолчанию.
 
Кроме того, по умолчанию реализует **сделать** кнопки, позволяя пользователям легко вернуться в приложение и пересылать и снова кнопки навигации, что позволяет пользователю перемещаться по стеку веб-страниц. Кроме того он также предоставляет пользователю адресной строки позволяя душевное спокойствие, что они ожидаемый веб-страницы. Поле адреса не позволяет пользователю изменить URL-адрес. 

Эти реализации изменить нельзя, поэтому `SFSafariViewController` идеально подходит для использования в качестве браузера по умолчанию, если приложение хочет предоставить веб-страницы без настройки.

В следующем примере кода можно использовать для запуска `SFSafariViewController` в приложении Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Получается следующий веб-представление.

[![](uiwebview-images/sfsafariviewcontroller.png "Пример веб-представление с SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Можно также открыть мобильного приложения Safari из приложения, используя приведенный ниже код:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Получается следующий веб-представление.

[![](uiwebview-images/safari.png "Веб-страницы, представленных в Safari")](uiwebview-images/safari.png#lightbox)

Переход пользователей приложения в Safari обычно всегда следует избегать. Большинство пользователей не ожидает перехода за пределами приложения, поэтому при покидании приложения пользователи могут никогда не возвращается, по существу выполняется прерывание обязательств.

Усовершенствования iOS 9 позволить пользователю легко вернуться к приложения с помощью кнопки "Назад", предоставленный в левом верхнем углу страницы Safari.

## <a name="app-transport-security"></a>Безопасность транспорта приложения

Безопасность транспорта для приложения, или *ATS* впервые появился в iOS 9, чтобы убедиться, что всем каналам связи соответствуют для защиты соединения советы и рекомендации по Apple.

Дополнительные сведения на AT, включая способы его реализации в приложении, см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md) руководства.

## <a name="related-links"></a>Связанные ссылки

- [WebViews (пример)](https://developer.xamarin.com/samples/monotouch/WebView/)
