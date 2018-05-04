---
title: Веб-представление
description: Представляет локальный или сетевой веб-содержимого и документы.
ms.prod: xamarin
ms.assetid: E44F5D0F-DB8E-46C7-8789-114F1652A6C5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: a1cba53223567e353194a4fcd52c8e22fa48ddf0
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="webview"></a>Веб-представление

[WebView](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) — это представление для отображения HTML и веб-содержимого в приложении. В отличие от `OpenUri`, веб-браузер на устройстве, который принимает пользователь `WebView` отображает содержимое HTML в приложении.

Это руководство состоит из следующих разделов:

- **[Содержимое](#Content)**  &ndash; WebView поддерживает различных источников содержимого, включая внедренный HTML, веб-страниц и строк HTML.
- **[Навигации](#Navigation)**  &ndash; WebView включает поддержку для перехода к конкретной странице и вернуться.
- **[События](#Events)**  &ndash; прослушивать и отвечать на действия, предпринятые пользователем в WebView.
- **[Производительность](#Performance)**  &ndash; Дополнительные сведения о характеристиках производительности веб-представление для каждой платформы.
- **[Разрешения](#Permissions)**  &ndash; Узнайте, как устанавливать разрешения, чтобы веб-представление будет работать в приложении.
- **[Макет](#Layout)**  &ndash; WebView имеет некоторые особых требования для его спланированную. Узнайте, как убедитесь, что правильно отображает веб-представление:

![](webview-images/in-app-browser.png "В приложении браузера")

## <a name="content"></a>Content

Веб-представление содержит поддержки для следующих типов содержимого.

- Веб-сайтов HTML и CSS &ndash; WebView имеет полную поддержку для веб-сайтов, написанные с использованием HTML и CSS, включая поддержку JavaScript.
- Документы &ndash; WebView реализована с помощью собственных компонентов на каждой платформе, WebView способен отображать документы, которые могут быть отображены на каждой платформе. Это означает, что PDF-файлов выполняется в iOS и Android.
- Строк HTML &ndash; WebView можно показать строки HTML из памяти.
- Локальные файлы &ndash; WebView может представлять любой из типов содержимого выше внедренных в приложение.

> [!NOTE]
> `WebView` в Windows не поддерживает Silverlight, Flash или элементы управления ActiveX, даже если они поддерживаются в Internet Explorer на этой платформе.

### <a name="websites"></a>Веб-сайты

Для отображения веб-сайта из Интернета, задайте `WebView` [ `Source` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebViewSource/) свойства в строке URL-адрес:

```csharp
var browser = new WebView {
  Source = "http://xamarin.com"
};
```

> [!NOTE]
> URL-адреса должен быть полностью сформирован с протоколом заданным (т. е. он должен быть «http://» или «https://», добавляемый).

#### <a name="ios-and-ats"></a>iOS и ATS

Начиная с версии 9 операций ввода-вывода допускает только приложения для взаимодействия с серверами, реализующие рекомендации безопасности по умолчанию. Значения должны быть заданы в `Info.plist` для обеспечения взаимодействия с незащищенных серверов.

> [!NOTE]
> Если приложению требуется подключение небезопасных веб-сайта, следует всегда введите домен как исключения, используя `NSExceptionDomains` вместо отключения ATS полностью с помощью `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` следует использовать только в чрезвычайных ситуациях аварийного.

Следующий пример демонстрирует для включения конкретного домена (в этом вариантов xamarin.com) для обхода ATS требования:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>xamarin.com</key>
            <dict>
                <key>NSIncludesSubdomains</key>
                <true/>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSTemporaryExceptionMinimumTLSVersion</key>
                <string>TLSv1.1</string>
            </dict>
        </dict>
    </dict>
```

Рекомендуется включать только некоторые домены для обхода ATS, позволяя использовать надежные узлы при пользоваться преимуществами дополнительную безопасность в недоверенных доменах. Ниже продемонстрировано менее надежного способа отключения ATS для приложения:

```xml
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsArbitraryLoads </key>
        <true/>
    </dict>
```

В разделе [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md) Дополнительные сведения об этой новой функции в iOS 9.

### <a name="html-strings"></a>HTML-строки

Если вы хотите предоставлять строка HTML, динамически определены в коде, необходимо создать экземпляр [ `HtmlWebViewSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.HtmlWebViewSource/):

```csharp
var browser = new WebView();
var htmlSource = new HtmlWebViewSource();
htmlSource.Html = @"<html><body>
  <h1>Xamarin.Forms</h1>
  <p>Welcome to WebView.</p>
  </body></html>";
browser.Source = htmlSource;
```

![](webview-images/html-string.png "Отображение HTML-строки WebView")

В приведенном выше коде `@` служит для пометки HTML как строка литерала, то есть все обычные escape-символы игнорируются.

### <a name="local-html-content"></a>Локальное содержимое HTML

Веб-представление может отображать содержимое HTML, CSS и Javascript внедренных в приложение. Пример:

```html
<html>
  <head>
    <title>Xamarin Forms</title>
  </head>
  <body>
    <h1>Xamrin.Forms</h1>
    <p>This is an iOS web page.</p>
    <img src="XamarinLogo.png" />
  </body>
</html>
```

CSS:

```css
html,body {
  margin:0;
  padding:10;
}
body,p,h1 {
  font-family: Chalkduster;
}
```

Обратите внимание, что шрифты, указанные в таблице выше CSS будет необходимо настроить для каждой платформы не каждая платформа имеет те же самые шрифты.

Для отображения локального содержимого с помощью `WebView`, вам потребуется открыть HTML-файл, как и любой другой, а затем загрузить содержимое в виде строки в `Html` свойство `HtmlWebViewSource`. Дополнительные сведения об открытии файлов см. в разделе [работа с файлами](~/xamarin-forms/app-fundamentals/files.md).

Следующих снимках экрана отобразится результат отображение локального содержимого на каждой платформе:

![](webview-images/local-content.png "Отображение локального содержимого веб-представление")

Несмотря на то, что первая страница загрузки, `WebView` не имеет сведений о происхождения HTML. Это проблема при работе со страницами, которые ссылаются на локальные ресурсы. Когда это может произойти примеры при другом, страница делает ссылку локальных страниц использовать отдельный файл JavaScript или странице ссылки на таблицу стилей CSS.  

Чтобы устранить эту проблему, необходимо указать `WebView` расположении файлов в файловой системе. Этого следует настроить `BaseUrl` свойство `HtmlWebViewSource` используемые `WebView`.

Из-за файловой системы на всех операционных систем, необходимо определить этот URL-адрес для каждой платформы. Xamarin.Forms предоставляет `DependencyService` при разрешении зависимостей во время выполнения для каждой платформы.

Чтобы использовать `DependencyService`, сначала Определите интерфейс, который можно реализовать на каждой платформе:

```csharp
public interface IBaseUrl { string Get(); }
```

Обратите внимание, что пока реализация этого интерфейса возлагается на каждой платформе, приложение не запустится. В общий проект, убедитесь в том, не забудьте установить для `BaseUrl` с помощью `DependencyService`:

```csharp
var source = new HtmlWebViewSource();
source.BaseUrl = DependencyService.Get<IBaseUrl>().Get();
```

Затем необходимо предоставить реализации интерфейса для каждой платформы.

#### <a name="ios"></a>iOS

Веб-содержимое на iOS, должны находиться в корневом каталоге проекта или **ресурсов** каталог с действием построения *BundleResource*, как показано ниже:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/ios-vs.png "Локальные файлы в iOS")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](webview-images/ios-xs.png "Локальные файлы в iOS")

-----

`BaseUrl` Должно быть присвоено путь основного пакета:

```csharp
[assembly: Dependency (typeof (BaseUrl_iOS))]
namespace WorkingWithWebview.iOS{
  public class BaseUrl_iOS : IBaseUrl {
    public string Get() {
      return NSBundle.MainBundle.BundlePath;
    }
  }
}
```

#### <a name="android"></a>Android

На Android, поместите HTML, CSS и изображений в папку средств с действием построения *AndroidAsset* как показано ниже:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](webview-images/android-vs.png "Локальные файлы в Android")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](webview-images/android-xs.png "Локальные файлы в Android")

-----

В Android `BaseUrl` должно быть присвоено `"file:///android_asset/"`:

```csharp
[assembly: Dependency (typeof(BaseUrl_Android))]
namespace WorkingWithWebview.Android {
  public class BaseUrl_Android : IBaseUrl {
    public string Get() {
      return "file:///android_asset/";
    }
  }
}
```

В Android файлы в **активы** папки можно получить через контекст текущего Android, который предоставляется `MainActivity.Instance` свойство:

```csharp
var assetManager = MainActivity.Instance.Assets;
using (var streamReader = new StreamReader (assetManager.Open ("local.html"))) {
  var html = streamReader.ReadToEnd ();
}
```

#### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

В проектах универсальной платформы Windows (UWP), поместите HTML, CSS и изображения в корневом каталоге проекта с действием сборки *содержимого*.

`BaseUrl` Должно быть присвоено `"ms-appx-web:///"`:

```csharp
[assembly: Dependency(typeof(BaseUrl))]
namespace WorkingWithWebview.UWP
{
    public class BaseUrl : IBaseUrl
    {
        public string Get()
        {
            return "ms-appx-web:///";
        }
    }
}
```

## <a name="navigation"></a>Навигация

Веб-представления поддерживает навигацию по несколько методов и свойств, которые он предоставляет доступ:

- **GoForward()** &ndash; Если `CanGoForward` имеет значение true, вызов `GoForward` переходит вперед на следующую страницу посещен.
- **GoBack()** &ndash; Если `CanGoBack` имеет значение true, вызвав `GoBack` , произойдет переход Последние посещенные страницы.
- **CanGoBack** &ndash; `true` при наличии страниц для обратного перехода, `false` Если браузер находится в начальный URL-адрес.
- **CanGoForward** &ndash; `true` , если пользователь переходит вперед и можно перемещаться вперед страницу, которая уже была выбрана.

На страницах `WebView` не поддерживает мультисенсорные жесты. Очень важно, чтобы убедиться в том, оптимизированные для мобильных устройств это содержимое и отображается без необходимости масштабирования.

Обычно для приложений, чтобы показать ссылки в `WebView`, а не браузера устройства. В этих случаях полезно разрешить обычный навигации, но при попаданий пользователя при начальной ссылку, приложение следует вернуться в представление обычного приложения.

Используйте встроенные средства навигации методы и свойства, чтобы реализовать этот сценарий.

Начните с создания страницы для представления браузера:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.InAppDemo"
Title="In App Browser">
    <ContentPage.Content>
        <StackLayout>
            <StackLayout Orientation="Horizontal" Padding="10,10">
                <Button Text="Back" HorizontalOptions="StartAndExpand" Clicked="backClicked" />
                <Button Text="Forward" HorizontalOptions="End" Clicked="forwardClicked" />
            </StackLayout>
            <WebView x:Name="Browser" WidthRequest="1000" HeightRequest="1000" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В нашем коде:

```csharp
public partial class InAppDemo : ContentPage
{
  //sets the URL for the browser in the page at creation
    public InAppDemo (string URL)
    {
        InitializeComponent ();
        Browser.Source = URL;
    }


    private void backClicked(object sender, EventArgs e)
    {
    // Check to see if there is anywhere to go back to
        if (Browser.CanGoBack) {
            Browser.GoBack ();
        } else { // If not, leave the view
            Navigation.PopAsync ();
        }
    }

    private void forwardClicked(object sender, EventArgs e)
    {
        if (Browser.CanGoForward) {
            Browser.GoForward ();
        }
    }
}
```

Вот и все!

![](webview-images/in-app-browser.png "Кнопки навигации веб-представление")

## <a name="events"></a>События

WebView вызывает два события, чтобы реагировать на изменения в состоянии:

- **Навигация по** &ndash; событие возникает, когда WebView начинает загрузку новой страницы.
- **Переход** &ndash; событие, возникающее при загрузке страницы и навигации был остановлен.

Если предполагается использование веб-страницы, слишком долго для загрузки, рассмотрите возможность использования этих событий для реализации индикатора состояния. Например, XAML-кода выглядит следующим образом:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="WebViewDemo.LoadingDemo" Title="Loading Demo">
  <ContentPage.Content>
    <StackLayout>
      <Label x:Name="LoadingLabel"
        Text="Loading..."
        HorizontalOptions="Center"
        IsVisible="false" />
      <WebView x:Name="Browser"
      HeightRequest="1000"
      WidthRequest="1000"
      Navigating="webOnNavigating"
      Navigated="webOnEndNavigating" />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

Два обработчика событий:

```csharp
void webOnNavigating (object sender, WebNavigatingEventArgs e)
{
    LoadingLabel.IsVisible = true;
}

void webOnEndNavigating (object sender, WebNavigatedEventArgs e)
{
    LoadingLabel.IsVisible = false;
}
```

Это приводит к следующие выходные данные (загрузка):

![](webview-images/loading-start.png "Пример события навигации по веб-представление")

По завершении загрузки:

![](webview-images/loading-end.png "Веб-представление, к которому необходимо перейти пример события")

## <a name="performance"></a>Производительность

Современного видели каждого из популярных веб-браузеры внедрению технологии, как с аппаратным ускорением подготовки к просмотру и компиляции JavaScript. К сожалению, из-за ограничений безопасности, большая часть этих улучшений не были доступны в equaivalent iOS из `WebView`, `UIWebView`. Xamarin.Forms `WebView` использует `UIWebView`. Если это проблема, необходимо написать пользовательское средство отрисовки, использующий `WKWebView`, которая поддерживает быстрый просмотр. Обратите внимание, что `WKWebView` поддерживается только для iOS 8 и более поздних.

Веб-представление на Android по умолчанию почти так же быстро, как встроенного браузера.

[UWP WebView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/web-view) использует модуль подготовки отчетов Microsoft Edge. Настольных и планшетных устройств увидите такую же производительность, как с помощью браузера Edge сам.

## <a name="permissions"></a>Разрешения

Чтобы `WebView` для работы, необходимо проверить, что разрешения заданы для каждой платформы. Обратите внимание, что на некоторых платформах `WebView` работает в режиме отладки, но не при построении для выпуска. Это, так как некоторые разрешения, как и для доступа к Интернету на Android, устанавливаются по умолчанию в Visual Studio для Mac в режиме отладки.

- **UWP** &ndash; требуется возможность Интернет (клиент и сервер), при отображении содержимого в сети.
- **Android** &ndash; требует `INTERNET` только в том случае, при отображении содержимого из сети. Локальное содержимое не требует специальных разрешений.
- **iOS** &ndash; не требует специальных разрешений.

## <a name="layout"></a>Макет

В отличие от большинства других представлений Xamarin.Forms `WebView` требует `HeightRequest` и `WidthRequest` указаны, если он содержится в StackLayout или RelativeLayout. Если не указать эти свойства `WebView` не будет подготовлен.

В следующих примерах демонстрируется макеты, которые приводят к работе, подготовки к просмотру `WebView`s:

StackLayout с WidthRequest & HeightRequest:

```xaml
<StackLayout>
    <Label Text="test" />
    <WebView Source="http://www.xamarin.com/"
        HeightRequest="1000"
        WidthRequest="1000" />
</StackLayout>
```

RelativeLayout с WidthRequest & HeightRequest:

```xaml
<RelativeLayout>
    <Label Text="test"
        RelativeLayout.XConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=10}"
        RelativeLayout.YConstraint= "{ConstraintExpression
                                      Type=Constant, Constant=20}" />
    <WebView Source="http://www.xamarin.com/"
        RelativeLayout.XConstraint="{ConstraintExpression Type=Constant,
                                     Constant=10}"
        RelativeLayout.YConstraint="{ConstraintExpression Type=Constant,
                                     Constant=50}"
        WidthRequest="1000" HeightRequest="1000" />
</RelativeLayout>
```

AbsoluteLayout *без* WidthRequest & HeightRequest:

```xaml
<AbsoluteLayout>
    <Label Text="test" AbsoluteLayout.LayoutBounds="0,0,100,100" />
    <WebView Source="http://www.xamarin.com/"
      AbsoluteLayout.LayoutBounds="0,150,500,500" />
</AbsoluteLayout>
```

Сетка *без* WidthRequest & HeightRequest. Сетка является одним из несколько макетов, которые не требуют указывать запрошенного значения высоты и ширины.:

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="100" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <Label Text="test" Grid.Row="0" />
    <WebView Source="http://www.xamarin.com/" Grid.Row="1" />
</Grid>
```


## <a name="related-links"></a>Связанные ссылки

- [Работа с веб-представление (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/)
- [Веб-представление (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView)
