---
title: Реализация HybridWebView
description: Элементы управления Xamarin.Forms настраиваемого пользовательского интерфейса должен быть производным от класса представления, который используется для размещения макетов и элементов управления на экране. В этой статье показано, как создать пользовательское средство отрисовки для HybridWebView пользовательского элемента управления, где показано, как улучшить веб-платформой элементы управления для разрешения кода C#, вызываемую из JavaScript.
ms.prod: xamarin
ms.assetid: 58DFFA52-4057-49A8-8682-50A58C7E842C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: af0dbef84d8ceb178fe5c1ac6fc7194c178141dc
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="implementing-a-hybridwebview"></a>Реализация HybridWebView

_Элементы управления Xamarin.Forms настраиваемого пользовательского интерфейса должен быть производным от класса представления, который используется для размещения макетов и элементов управления на экране. В этой статье показано, как создать пользовательское средство отрисовки для HybridWebView пользовательского элемента управления, где показано, как улучшить веб-платформой элементы управления для разрешения кода C#, вызываемую из JavaScript._

Каждый Xamarin.Forms представление имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) визуализируется в Xamarin.Forms приложение в iOS, `ViewRenderer` создается экземпляр класса, который в свою очередь создает собственный `UIView` элемента управления. На платформе Android `ViewRenderer` класс создает экземпляры `View` элемента управления. В универсальной платформы Windows (UWP), `ViewRenderer` класс создает экземпляр собственного `FrameworkElement` элемента управления. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) и соответствующие собственные элементы управления, которые реализуют:

![](hybridwebview-images/view-classes.png "Связь между класс представления и его реализации собственных классов")

Процесс подготовки отчета можно использовать для реализации настроек платформой путем создания пользовательского средства визуализации для [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_HybridWebView) `HybridWebView` пользовательского элемента управления.
1. [Использовать](#Consuming_the_HybridWebView) `HybridWebView`из Xamarin.Forms.
1. [Создание](#Creating_the_Custom_Renderer_on_each_Platform) пользовательское средство отрисовки для `HybridWebView` на каждой платформе.

Каждый элемент теперь обсуждаются в свою очередь для реализации `HybridWebView` подготовки отчетов, который расширяет возможности элементов управления веб платформы для разрешения кода C#, вызываемую из JavaScript. `HybridWebView` Экземпляр будет использоваться для отображения HTML-страницы, пользователю предлагается ввести их имена. Затем, когда пользователь нажимает кнопку HTML, будет вызывать функцию JavaScript на языке C# `Action` , отображается всплывающее окно, содержащее имя пользователей.

Дополнительные сведения о процессе для вызова C# из кода JavaScript см. в разделе [вызов C# в коде JavaScript](#Invoking_C_from_JavaScript). Дополнительные сведения о HTML-страницы в разделе [Создание веб-страницы](#Creating_the_Web_Page).

<a name="Creating_the_HybridWebView" />

## <a name="creating-the-hybridwebview"></a>Создание HybridWebView

`HybridWebView` Пользовательский элемент управления можно создать путем создания подкласса [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класса, как показано в следующем примере кода:

```csharp
public class HybridWebView : View
{
  Action<string> action;
  public static readonly BindableProperty UriProperty = BindableProperty.Create (
    propertyName: "Uri",
    returnType: typeof(string),
    declaringType: typeof(HybridWebView),
    defaultValue: default(string));

  public string Uri {
    get { return (string)GetValue (UriProperty); }
    set { SetValue (UriProperty, value); }
  }

  public void RegisterAction (Action<string> callback)
  {
    action = callback;
  }

  public void Cleanup ()
  {
    action = null;
  }

  public void InvokeAction (string data)
  {
    if (action == null || data == null) {
      return;
    }
    action.Invoke (data);
  }
}
```

`HybridWebView` Пользовательского элемента управления в проекте библиотеки .NET Standard и определяет следующий API для элемента управления:

- Объект `Uri` свойство, которое указывает адрес веб-страницы для загрузки.
- Объект `RegisterAction` метод, который регистрирует `Action` с элементом управления. Зарегистрированных действий будет вызываться из JavaScript, содержащиеся в файле HTML для ссылок на `Uri` свойство.
- Объект `CleanUp` метод, который удаляет ссылку на зарегистрированный `Action`.
- `InvokeAction` Метода, который вызывает зарегистрированную `Action`. Этот метод будет вызываться из пользовательского средства отрисовки в каждом проекте конкретную платформу.

<a name="Consuming_the_HybridWebView" />

## <a name="consuming-the-hybridwebview"></a>Использование HybridWebView

`HybridWebView` Пользовательский элемент управления может ссылаться на языке XAML в проекте библиотеки .NET Standard объявление пространства имен для его расположения и с помощью префикса пространства имен на пользовательский элемент управления. В следующем примере кода показан способ `HybridWebView` пользовательский элемент управления может использоваться XAML-страницы:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             x:Class="CustomRenderer.HybridWebViewPage"
             Padding="0,20,0,0">
    <ContentPage.Content>
        <local:HybridWebView x:Name="hybridWebView" Uri="index.html"
          HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
    </ContentPage.Content>
</ContentPage>
```

`local` Префикс пространства имен можно присвоить любое имя. Тем не менее `clr-namespace` и `assembly` значения должны соответствовать сведений пользовательского элемента управления. После объявления пространства имен, префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показан способ `HybridWebView` пользовательский элемент управления может использоваться на странице C#:

```csharp
public class HybridWebViewPageCS : ContentPage
{
  public HybridWebViewPageCS ()
  {
    var hybridWebView = new HybridWebView {
      Uri = "index.html",
      HorizontalOptions = LayoutOptions.FillAndExpand,
      VerticalOptions = LayoutOptions.FillAndExpand
    };
    ...
    Padding = new Thickness (0, 20, 0, 0);
    Content = hybridWebView;
  }
}
```

`HybridWebView` Экземпляр будет использоваться для отображения собственного веб-элемент управления на каждой платформе. Он имеет `Uri` в HTML-файле, хранимых в каждом проекте специфический для платформы, который будет отображаться с собственного веб-элемент управления является свойство. Отображаемого HTML-кода пользователю предлагается ввести их имени, с помощью функции JavaScript, вызов на языке C# `Action` в ответ на нажатие кнопки HTML.

`HybridWebViewPage` Регистрирует действие можно было вызывать из JavaScript, как показано в следующем примере кода:

```csharp
public partial class HybridWebViewPage : ContentPage
{
  public HybridWebViewPage ()
  {
    ...
    hybridWebView.RegisterAction (data => DisplayAlert ("Alert", "Hello " + data, "OK"));
  }
}
```

Это действие вызывает [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) метод для отображения модального всплывающего окна, которая представляет имя, введенное на странице HTML, отображаемый элементом `HybridWebView` экземпляра.

Возможность добавления пользовательского средства отрисовки в каждый проект приложения для улучшения специфический для платформы веб-элементов управления, позволяя кода C# можно было вызывать из JavaScript.

<a nane="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского средства визуализации для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `ViewRenderer<T1,T2>` класс, который отображает пользовательский элемент управления. Первый аргумент типа должен быть пользовательского элемента управления, в данном случае является модуль подготовки отчетов `HybridWebView`. Второй аргумент типа должен быть собственный элемент управления, который реализует пользовательское представление.
1. Переопределить `OnElementChanged` метода, который отображает пользовательскую логику управления и записи для ее настройки. Этот метод вызывается, когда создается соответствующий Xamarin.Forms пользовательский элемент управления.
1. Добавить `ExportRenderer` атрибут класс пользовательского средства отрисовки, чтобы указать, что он будет использоваться для подготовки к просмотру пользовательский элемент управления с помощью Xamarin.Forms. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms является необязательным для предоставления пользовательского средства отрисовки в каждом проекте платформы. Если пользовательское средство отрисовки не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для базового класса элемента управления. Однако пользовательские модули подготовки отчетов, необходимы в каждом проекте платформы при подготовке к просмотру [представление](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) элемента.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](hybridwebview-images/solution-structure.png "Обязанности проекта HybridWebView пользовательского модуля подготовки отчетов")

`HybridWebView` Пользовательский элемент управления отрисовывается классами платформой модуля подготовки отчетов, которые являются производными от `ViewRenderer` класса для каждой платформы. Это приведет к каждой `HybridWebView` пользовательский элемент управления готовится к просмотру с платформой веб-элементов, как показано на следующем снимке экрана:

![](hybridwebview-images/screenshots.png "HybridWebView на каждой платформе")

`ViewRenderer` Предоставляемых классами `OnElementChanged` метод, который вызывается при создании пользовательского элемента управления Xamarin.Forms для подготовки к просмотру соответствующий собственный элемент управления. Этот метод принимает `ElementChangedEventArgs` параметра, который содержит `OldElement` и `NewElement` свойства. Эти свойства представляют собой элемент Xamarin.Forms, модуль подготовки отчетов *было* присоединен и элемент Xamarin.Forms, модуль подготовки отчетов *—* присоединенного, соответственно. В примере приложения `OldElement` свойство будет `null` и `NewElement` свойство будет содержать ссылку на `HybridWebView` экземпляра.

Переопределенная версия `OnElementChanged` метод в каждом классе платформой модуля подготовки отчетов — это место для выполнения при создании экземпляра web собственного элемента управления и настройки. `SetNativeControl` Метод следует использовать для создания собственного веб-элемент управления, и этот метод также присвоит ссылкой на элемент управления для `Control` свойства. Кроме того, можно получить ссылку на элемент управления Xamarin.Forms, который готовится к просмотру с помощью `Element` свойство.

В некоторых случаях `OnElementChanged` метод может вызываться несколько раз. Таким образом чтобы предотвратить утечку памяти, необходимо соблюдать осторожность при создании нового собственного элемента управления. В следующем примере кода показан подход, используемый при создании собственного элемента управления в пользовательском отрисовщике:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Собственный элемент управления должен создаваться только один раз, когда свойству `Control` задано значение `null`. Элемент управления следует настраивать и подписывать на обработчики событий только при присоединении пользовательского отрисовщика к новому элементу Xamarin.Forms. Аналогично, от всех обработчиков событий, на которые были созданы подписки, следует отписываться только при изменении элемента с подключенным отрисовщиком. Этот подход поможет для создания высокопроизводительных пользовательское средство отрисовки, который не страдает от утечек памяти.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя типа пользовательского элемента управления Xamarin.Forms готовится к просмотру и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматриваются структуру веб-странице загрузки каждой собственного веб-элемент управления, процесс для вызова C# в коде JavaScript и реализацию этого в каждом классе платформой пользовательское средство отрисовки.

<a name="Creating_the_Web_Page" />

### <a name="creating-the-web-page"></a>Создание веб-страницы

В следующем примере кода показана веб-страница, которое будет отображаться для `HybridWebView` пользовательского элемента управления:

```html
<html>
<body>
<script src="http://code.jquery.com/jquery-1.11.0.min.js"></script>
<h1>HybridWebView Test</h1>
<br/>
Enter name: <input type="text" id="name">
<br/>
<br/>
<button type="button" onclick="javascript:invokeCSCode($('#name').val());">Invoke C# Code</button>
<br/>
<p id="result">Result:</p>
<script type="text/javascript">
function log(str)
{
    $('#result').text($('#result').text() + " " + str);
}

function invokeCSCode(data) {
    try {
        log("Sending Data:" + data);
        invokeCSharpAction(data);
    }
    catch (err){
        log(err);
    }
}
</script>
</body>
</html>
```

Веб-страница позволяет пользователю вводить их имени в `input` элемент и предоставляет `button` элемент, который будет вызывать код C# при нажатии. Для этого выполняется следующим образом:

- Когда пользователь нажимает на `button` элемент, `invokeCSCode` функцию JavaScript вызывается со значением `input` элемент передается функции.
- `invokeCSCode` Вызовов функций `log` функции для отображения данных, он отправляет в C# `Action`. Затем он вызывает `invokeCSharpAction` метод, вызываемый в C# `Action`, передача параметров, полученных от `input` элемента.

`invokeCSharpAction` Функцию JavaScript не определен на веб-странице и будут вноситься в него каждого пользовательского средства отрисовки.

<a name="Invoking_C_from_JavaScript" />

### <a name="invoking-c-from-javascript"></a>Вызов C# из JavaScript

На каждой платформе идентично процесс для вызова C# в коде JavaScript.

- Пользовательское средство отрисовки создает собственный веб-элемент управления и загружает HTML-файл, указанный параметром `HybridWebView.Uri` свойство.
- После загрузки веб-страницы пользовательского модуля подготовки отчетов внедряет `invokeCSharpAction` функции JavaScript в веб-страницу.
- Когда пользователь вводит свои имя и нажимает кнопку на HTML- `button` элемент, `invokeCSCode` вызывается функция, которая в свою очередь вызывает `invokeCSharpAction` функции.
- `invokeCSharpAction` Функция вызывает метод пользовательского средства визуализации, в свою очередь вызывает `HybridWebView.InvokeAction` метод.
- `HybridWebView.InvokeAction` Метод вызывает зарегистрированную `Action`.

В следующих разделах будет рассматриваются реализации этого процесса для каждой платформы.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

В следующем примере кода показано пользовательское средство отрисовки для платформы iOS.

```csharp
[assembly: ExportRenderer (typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.iOS
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, WKWebView>, IWKScriptMessageHandler
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.webkit.messageHandlers.invokeAction.postMessage(data);}";
        WKUserContentController userController;

        protected override void OnElementChanged (ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                userController = new WKUserContentController ();
                var script = new WKUserScript (new NSString (JavaScriptFunction), WKUserScriptInjectionTime.AtDocumentEnd, false);
                userController.AddUserScript (script);
                userController.AddScriptMessageHandler (this, "invokeAction");

                var config = new WKWebViewConfiguration { UserContentController = userController };
                var webView = new WKWebView (Frame, config);
                SetNativeControl (webView);
            }
            if (e.OldElement != null) {
                userController.RemoveAllUserScripts ();
                userController.RemoveScriptMessageHandler ("invokeAction");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup ();
            }
            if (e.NewElement != null) {
                string fileName = Path.Combine (NSBundle.MainBundle.BundlePath, string.Format ("Content/{0}", Element.Uri));
                Control.LoadRequest (new NSUrlRequest (new NSUrl (fileName, false)));
            }
        }

        public void DidReceiveScriptMessage (WKUserContentController userContentController, WKScriptMessage message)
        {
            Element.InvokeAction (message.Body.ToString ());
        }
    }
}
```

`HybridWebViewRenderer` Класс загружает веб-страницы, указанной в `HybridWebView.Uri` свойство в собственный [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) управления и `invokeCSharpAction` функция JavaScript, которая вставляется в веб-странице. Когда пользователь вводит свои имя и нажимает HTML `button` элемент, `invokeCSharpAction` функции JavaScript, которая выполняется, с `DidReceiveScriptMessage` метод, вызываемый после получения сообщения из веб-страницы. В свою очередь, вызывает этот метод `HybridWebView.InvokeAction` метод, который будет вызывать зарегистрированных действий для отображения всплывающего окна.

Это достигается следующим образом:

- При условии, что `Control` свойство `null`, выполняются следующие операции:
  - Объект [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) создается экземпляр, что позволяет Выдача сообщений и внедрение скриптов пользователя в веб-страницу.
  - Объект [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) создается экземпляр для вставки `invokeCSharpAction` функции JavaScript в веб-страницу после загрузки веб-страницы.
  - [ `WKUserContentController.AddScript` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddUserScript/p/WebKit.WKUserScript/) Добавляет метод [ `WKUserScript` ](https://developer.xamarin.com/api/type/WebKit.WKUserScript/) экземпляра к контроллеру содержимого.
  - [ `WKUserContentController.AddScriptMessageHandler` ](https://developer.xamarin.com/api/member/WebKit.WKUserContentController.AddScriptMessageHandler/p/WebKit.IWKScriptMessageHandler/System.String/) Метод добавляет обработчик сообщений скрипта с именем `invokeAction` для [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) экземпляр, который вызовет функцию JavaScript `window.webkit.messageHandlers.invokeAction.postMessage(data)` определены во всех кадров на всех веб-представления, которые будут использовать `WKUserContentController` экземпляра.
  - Объект [ `WKWebViewConfiguration` ](https://developer.xamarin.com/api/type/WebKit.WKWebViewConfiguration/) , создается экземпляр с [ `WKUserContentController` ](https://developer.xamarin.com/api/type/WebKit.WKUserContentController/) экземпляр как содержимого контроллера.
  - Объект [ `WKWebView` ](https://developer.xamarin.com/api/type/WebKit.WKWebView/) создать экземпляр элемента управления и `SetNativeControl` метод вызывается попытку назначить ссылку для `WKWebView` управления `Control` свойство.
- При условии, что пользовательское средство отрисовки присоединен к новый элемент Xamarin.Forms:
  - [ `WKWebView.LoadRequest` ](https://developer.xamarin.com/api/member/WebKit.WKWebView.LoadRequest/p/Foundation.NSUrlRequest/) Метод загружает HTML-файл, который задается параметром `HybridWebView.Uri` свойство. Код указывает, что файл хранится в `Content` папку проекта. Если отображается веб-страницы, `invokeCSharpAction` функцию JavaScript будут вноситься в веб-странице.
- При присоединении элемента модуля подготовки отчетов для изменения:
  - Ресурсы освобождаются.

> [!NOTE]
> `WKWebView` Класса поддерживается только в iOS 8 и более поздних версий.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

В следующем примере кода показано пользовательское средство отрисовки для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.Droid
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Android.Webkit.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){jsBridge.invokeAction(data);}";
        Context _context;

        public HybridWebViewRenderer(Context context) : base(context)
        {
            _context = context;
        }

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                var webView = new Android.Webkit.WebView(_context);
                webView.Settings.JavaScriptEnabled = true;
                SetNativeControl(webView);
            }
            if (e.OldElement != null)
            {
                Control.RemoveJavascriptInterface("jsBridge");
                var hybridWebView = e.OldElement as HybridWebView;
                hybridWebView.Cleanup();
            }
            if (e.NewElement != null)
            {
                Control.AddJavascriptInterface(new JSBridge(this), "jsBridge");
                Control.LoadUrl(string.Format("file:///android_asset/Content/{0}", Element.Uri));
                InjectJS(JavaScriptFunction);
            }
        }

        void InjectJS(string script)
        {
            if (Control != null)
            {
                Control.LoadUrl(string.Format("javascript: {0}", script));
            }
        }
    }
}
```

`HybridWebViewRenderer` Класс загружает веб-страницы, указанной в `HybridWebView.Uri` свойство в собственный [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) управления и `invokeCSharpAction` функция JavaScript, которая вставляется в веб-странице загрузки веб-страницы с `InjectJS` метод. Когда пользователь вводит свои имя и нажимает HTML `button` элемент, `invokeCSharpAction` выполнении функции JavaScript. Это достигается следующим образом:

- При условии, что `Control` свойство `null`, выполняются следующие операции:
  - Собственный [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) создается экземпляр и включить JavaScript в элементе управления.
  - `SetNativeControl` Метод вызывается попытку назначить ссылку собственному [ `WebView` ](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) управления `Control` свойство.
- При условии, что пользовательское средство отрисовки присоединен к новый элемент Xamarin.Forms:
  - [ `WebView.AddJavascriptInterface` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.AddJavascriptInterface/p/Java.Lang.Object/System.String/) Вводит новый метод `JSBridge` экземпляра в главного фрейма контекста WebView JavaScript, присвоив ему имя `jsBridge`. Это позволяет методам в `JSBridge` класс должен быть получен из JavaScript.
  - [ `WebView.LoadUrl` ](https://developer.xamarin.com/api/member/Android.Webkit.WebView.LoadUrl/p/System.String/) Метод загружает HTML-файл, который задается параметром `HybridWebView.Uri` свойство. Код указывает, что файл хранится в `Content` папку проекта.
  - `InjectJS` Метод вызывается для вставки `invokeCSharpAction` функции JavaScript в веб-страницу.
- При присоединении элемента модуля подготовки отчетов для изменения:
  - Ресурсы освобождаются.

Когда `invokeCSharpAction` выполняется функция JavaScript, которая, в свою очередь вызывает `JSBridge.InvokeAction` метода, как показано в следующем примере кода:

```csharp
public class JSBridge : Java.Lang.Object
{
  readonly WeakReference<HybridWebViewRenderer> hybridWebViewRenderer;

  public JSBridge (HybridWebViewRenderer hybridRenderer)
  {
    hybridWebViewRenderer = new WeakReference <HybridWebViewRenderer> (hybridRenderer);
  }

  [JavascriptInterface]
  [Export ("invokeAction")]
  public void InvokeAction (string data)
  {
    HybridWebViewRenderer hybridRenderer;

    if (hybridWebViewRenderer != null && hybridWebViewRenderer.TryGetTarget (out hybridRenderer)) {
      hybridRenderer.Element.InvokeAction (data);
    }
  }
}
```

Класс должен быть производным от `Java.Lang.Object`, и методы, предоставляемые в код JavaScript должен быть снабжен атрибутом `[JavascriptInterface]` и `[Export]` атрибуты. Таким образом, когда `invokeCSharpAction` функции JavaScript, которая вставляется в веб-страницы и выполняется, он вызывает `JSBridge.InvokeAction` метод из-за которой декорированных `[JavascriptInterface]` и `[Export("invokeAction")]` атрибуты. В свою очередь `InvokeAction` вызывает метод `HybridWebView.InvokeAction` метода, который будет вызванного зарегистрированных действий для отображения всплывающего окна.

> [!NOTE]
> Проекты, использующие `[Export]` атрибут должен включать ссылку на `Mono.Android.Export`, или приведет к ошибке компилятора.

Обратите внимание, что `JSBridge` класс поддерживает `WeakReference` для `HybridWebViewRenderer` класса. Это позволяет избежать создания циклическую ссылку между двумя классами. Дополнительные сведения см. [слабые ссылки](https://msdn.microsoft.com/library/ms404247(v=vs.110).aspx) на сайте MSDN.

> [!IMPORTANT]
> Android Oreo и убедитесь, что задает манифеста Android **Android целевой версии** для **автоматического**. В противном случае выполнение этого кода приведет к ошибка сообщение «invokeCSharpAction не определена».

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского средства визуализации на UWP

В следующем примере кода показано пользовательское средство отрисовки для UWP.

```csharp
[assembly: ExportRenderer(typeof(HybridWebView), typeof(HybridWebViewRenderer))]
namespace CustomRenderer.UWP
{
    public class HybridWebViewRenderer : ViewRenderer<HybridWebView, Windows.UI.Xaml.Controls.WebView>
    {
        const string JavaScriptFunction = "function invokeCSharpAction(data){window.external.notify(data);}";

        protected override void OnElementChanged(ElementChangedEventArgs<HybridWebView> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                SetNativeControl(new Windows.UI.Xaml.Controls.WebView());
            }
            if (e.OldElement != null)
            {
                Control.NavigationCompleted -= OnWebViewNavigationCompleted;
                Control.ScriptNotify -= OnWebViewScriptNotify;
            }
            if (e.NewElement != null)
            {
                Control.NavigationCompleted += OnWebViewNavigationCompleted;
                Control.ScriptNotify += OnWebViewScriptNotify;
                Control.Source = new Uri(string.Format("ms-appx-web:///Content//{0}", Element.Uri));
            }
        }

        async void OnWebViewNavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
        {
            if (args.IsSuccess)
            {
                // Inject JS script
                await Control.InvokeScriptAsync("eval", new[] { JavaScriptFunction });
            }
        }

        void OnWebViewScriptNotify(object sender, NotifyEventArgs e)
        {
            Element.InvokeAction(e.Value);
        }
    }
}
```

`HybridWebViewRenderer` Класс загружает веб-страницы, указанной в `HybridWebView.Uri` свойство в собственный `WebView` управления и `invokeCSharpAction` функции JavaScript, которая вставляется в веб-странице загрузки веб-страницы с `WebView.InvokeScriptAsync` метод. Когда пользователь вводит свои имя и нажимает HTML `button` элемент, `invokeCSharpAction` функции JavaScript, которая выполняется, с `OnWebViewScriptNotify` метод, вызываемый после получения уведомления с веб-страницы. В свою очередь, вызывает этот метод `HybridWebView.InvokeAction` метод, который будет вызывать зарегистрированных действий для отображения всплывающего окна.

Это достигается следующим образом:

- При условии, что `Control` свойство `null`, выполняются следующие операции:
  - `SetNativeControl` Метод вызывается для создания экземпляров новых собственный `WebView` управления и назначить ему ссылку на него для `Control` свойства.
- При условии, что пользовательское средство отрисовки присоединен к новый элемент Xamarin.Forms:
  - Обработчики событий для `NavigationCompleted` и `ScriptNotify` зарегистрированных событий. `NavigationCompleted` Событие возникает при любом собственный `WebView` завершения загрузки содержимого текущего элемента управления или если ошибка навигации. `ScriptNotify` Событие возникает, когда содержимое в машинном коде `WebView` управления использует JavaScript для передачи строки для приложения. Веб-страницы активируется `ScriptNotify` события путем вызова `window.external.notify` во время передачи `string` параметра.
  - `WebView.Source` Свойству URI HTML-файл, который задается параметром `HybridWebView.Uri` свойство. В коде предполагается, что файл хранится в `Content` папку проекта. Если отображается веб-страницы, `NavigationCompleted` событие будет срабатывать и `OnWebViewNavigationCompleted` метод будет вызван. `invokeCSharpAction` Функции JavaScript, которая затем внедряются в веб-странице с `WebView.InvokeScriptAsync` метод, при условии, что Навигация выполнена успешно.
- При присоединении элемента модуля подготовки отчетов для изменения:
  - События, отменена.

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создать пользовательское средство отрисовки для `HybridWebView` пользовательский элемент управления, в котором показано, как для расширения элементов управления веб платформы для разрешения кода C#, вызываемую из JavaScript.


## <a name="related-links"></a>Связанные ссылки

- [CustomRendererHybridWebView (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/hybridwebview/)
- [C# вызова из кода JavaScript](https://developer.xamarin.com/recipes/android/controls/webview/call_csharp_from_javascript/)
