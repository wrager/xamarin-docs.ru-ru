---
title: Xamarin.Forms в Xamarin Native проектов
description: В этой статье описывается использование производных ContentPage страниц, которые напрямую добавляются в собственных проектах Xamarin и перемещаться между ними.
ms.prod: xamarin
ms.assetid: f343fc21-dfb1-4364-a332-9da6705d36bc
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/11/2018
ms.openlocfilehash: ca62b9fec3223e8da62d8e4cc6e1f69a58f335a0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243279"
---
# <a name="xamarinforms-in-xamarin-native-projects"></a>Xamarin.Forms в Xamarin Native проектов

_Собственный формы позволяют производным Xamarin.Forms ContentPage страниц для использования собственных проектов Xamarin.iOS, Xamarin.Android и универсальной платформы Windows (UWP). Собственные проекты могут потреблять производный ContentPage страниц, которые напрямую добавляются в проект или из библиотеки .NET Standard, .NET стандартной библиотеки или общий проект. В этой статье описывается использование производных ContentPage страниц, которые напрямую добавляются в собственных проектах и перемещаться между ними._

Как правило, приложение Xamarin.Forms включает одну или несколько страниц, которые являются производными от [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), и эти страницы являются общими для всех платформ .NET Standard проекта библиотеки или общего проекта. Однако собственный форм позволяет `ContentPage`-производный страницы для добавления в машинном коде приложения Xamarin.iOS, Xamarin.Android и UWP. По сравнению с необходимости использовать собственный проект `ContentPage`-производном .NET Standard проекта библиотеки или общий проект, преимущество страницы непосредственно к собственным проектам вполне, страниц могут быть расширены с помощью собственного представления. С именем собственного представления можно затем при помощи XAML с `x:Name` и ссылаться на нее из кода. Дополнительные сведения о собственном представлениях см. в разделе [собственного представления](~/xamarin-forms/platform/native-views/index.md).

Процесс получения Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производном в собственный проект можно следующим образом:

1. Добавьте пакет Xamarin.Forms NuGet в собственный проект.
1. Добавить [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производного страницы, а также любые зависимости, в собственный проект.
1. Вызовите метод `Forms.Init`.
1. Создать экземпляр [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производным страницы и преобразовать его в соответствующий собственный тип, используя один из следующих методов расширения: `CreateViewController` для операций ввода-вывода, `CreateFragment` или `CreateSupportFragment` для Android, или `CreateFrameworkElement` для UWP.
1. Перейдите в представление собственного типа [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы с помощью собственного навигации API.

Xamarin.Forms должен быть инициализирован путем вызова `Forms.Init` метод перед собственного проекта можно создать [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы. Выбор места для этого в основном зависит от времени наиболее удобно в потоке приложения — может быть выполнена при запуске приложения, или непосредственно перед `ContentPage`-создании производной страницы. В этой статье и сопутствующие примеры приложений `Forms.Init` метод вызывается при запуске приложения.

> [!NOTE]
> **NativeForms** решение образца приложения не содержит все проекты Xamarin.Forms. Вместо этого он состоит из проекта Xamarin.iOS, Xamarin.Android проект и проект UWP. Каждый проект имеет собственный проект, использующий собственные формы для использования [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страниц. Тем не менее, нет причин, почему не удалось использовать собственные проекты `ContentPage`-страницы на основе .NET Standard проекта библиотеки или общий проект.

При использовании собственного форм, таких как компоненты Xamarin.Forms [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/), [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/)и механизм привязки данных, все по-прежнему работали.

## <a name="ios"></a>iOS

На iOS `FinishedLaunching` переопределить в `AppDelegate` класс обычно является местом для выполнения приложения при запуске задач, связанных с. Он вызывается после запуска приложения и обычно переопределяется, чтобы настроить главное окно и просмотреть контроллера. В следующем примере кода показан `AppDelegate` класса в примере приложения:

```csharp
[Register("AppDelegate")]
public class AppDelegate : UIApplicationDelegate
{
    public static AppDelegate Instance;

    UIWindow _window;
    UINavigationController _navigation;

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        Forms.Init();

        Instance = this;
        _window = new UIWindow(UIScreen.MainScreen.Bounds);

        UINavigationBar.Appearance.SetTitleTextAttributes(new UITextAttributes
        {
            TextColor = UIColor.Black
        });

        var mainPage = new PhonewordPage().CreateViewController();
        mainPage.Title = "Phoneword";

        _navigation = new UINavigationController(mainPage);
        _window.RootViewController = _navigation;
        _window.MakeKeyAndVisible();

        return true;
    }
    ...
}
```

`FinishedLaunching` Метод выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метод.
- Ссылку на `AppDelegate` класс хранится в `static` `Instance` поля. Это позволит обеспечить механизм для других классов для вызова методов, определенных в `AppDelegate` класса.
- `UIWindow`, Создается это основной контейнер для представлений в приложениях машинным кодом iOS.
- `PhonewordPage` Класса, который является Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы, определенные в XAML, создается и преобразовать `UIViewController` с помощью `CreateViewController` метода расширения.
- `Title` Свойство `UIViewController` имеет значение, который будет отображаться на `UINavigationBar`.
- Объект `UINavigationController` создается для управления иерархической навигации. `UINavigationController` Класс управляет стек Просмотр контроллеров и `UIViewController` переданные в конструктор откроется изначально при `UINavigationController` загружается.
- `UINavigationController` Экземпляр задан в качестве верхнего уровня `UIViewController` для `UIWindow`и `UIWindow` задан в качестве ключа окна для приложения и станет видимым.

Один раз `FinishedLaunching` выполнен метод, пользовательский Интерфейс определен в Xamarin.Forms `PhonewordPage` будет отображаться класса, как показано на следующем снимке экрана:

[![](native-forms-images/ios-phonewordpage.png "iOS PhonewordPage")](native-forms-images/ios-phonewordpage-large.png#lightbox "iOS PhonewordPage")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), приведет к обработчиков событий в `PhonewordPage` выполнение кода. Например, когда пользователь касается **журнал звонков** выполняется кнопки следующий обработчик событий:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    AppDelegate.Instance.NavigateToCallHistoryPage();
}
```

`static` `AppDelegate.Instance` Поле позволяет `AppDelegate.NavigateToCallHistoryPage` вызываемого метода, как показано в следующем примере кода:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateViewController();
    callHistoryPage.Title = "Call History";
    _navigation.PushViewController(callHistoryPage, true);
}
```

`NavigateToCallHistoryPage` Метод преобразует Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-страницу, чтобы производный `UIViewController` с `CreateViewController` метода расширения и наборы `Title` свойство `UIViewController`. `UIViewController` Затем помещается в `UINavigationController` по `PushViewController` метод. Таким образом, определен пользовательский Интерфейс в Xamarin.Forms `CallHistoryPage` будет отображаться класса, как показано на следующем снимке экрана:

[![](native-forms-images/ios-callhistorypage.png "iOS CallHistoryPage")](native-forms-images/ios-callhistorypage-large.png#lightbox "iOS CallHistoryPage")

При `CallHistoryPage` отображается, нажимая на задний план отобразит стрелка `UIViewController` для `CallHistoryPage` класса из `UINavigationController`, возвращая пользователю `UIViewController` для `PhonewordPage` класса.

## <a name="android"></a>Android

В Android `OnCreate` переопределить в `MainActivity` класс обычно является местом для выполнения приложения при запуске задач, связанных с. В следующем примере кода показан `MainActivity` класса в примере приложения:

```csharp
public class MainActivity : AppCompatActivity
{
    public static MainActivity Instance;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);

        Forms.Init(this, bundle);
        Instance = this;

        SetContentView(Resource.Layout.Main);
        var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
        SetSupportActionBar(toolbar);
        SupportActionBar.Title = "Phoneword";

        var mainPage = new PhonewordPage().CreateFragment(this);
        FragmentManager
            .BeginTransaction()
            .Replace(Resource.Id.fragment_frame_layout, mainPage)
            .Commit();
        ...
    }
    ...
}
```

`OnCreate` Метод выполняет следующие задачи:

- Xamarin.Forms инициализируется путем вызова `Forms.Init` метод.
- Ссылку на `MainActivity` класс хранится в `static` `Instance` поля. Это позволит обеспечить механизм для других классов для вызова методов, определенных в `MainActivity` класса.
- `Activity` Содержимое задано из разметки ресурса. В примере приложения макет состоит из `LinearLayout` , содержащий `Toolbar`и `FrameLayout` в качестве контейнера фрагмента.
- `Toolbar` Извлекается и задать в качестве панели действий для `Activity`, и задайте заголовок панели действий.
- `PhonewordPage` Класса, который является Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы, определенные в XAML, создается и преобразовать `Fragment` с помощью `CreateFragment` метода расширения.
- `FragmentManager` Класс создает и фиксирует транзакцию, которая заменяет `FrameLayout` экземпляра с `Fragment` для `PhonewordPage` класса.

Дополнительные сведения о фрагментах см. в разделе [фрагментов](~/android/platform/fragments/index.md).

> [!NOTE]
> В дополнение к `CreateFragment` Xamarin.Forms метод расширения также включает `CreateSupportFragment` метод. `CreateFragment` Метод создает `Android.App.Fragment` , можно использовать в приложениях, предназначенных для API 11 и выше. `CreateSupportFragment` Метод создает `Android.Support.V4.App.Fragment` может использоваться в приложениях, предназначенных для версий API до 11.

Один раз `OnCreate` выполнен метод, пользовательский Интерфейс определен в Xamarin.Forms `PhonewordPage` будет отображаться класса, как показано на следующем снимке экрана:

[![](native-forms-images/android-phonewordpage.png "Android PhonewordPage")](native-forms-images/android-phonewordpage-large.png#lightbox "Android PhonewordPage")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), приведет к обработчиков событий в `PhonewordPage` выполнение кода. Например, когда пользователь касается **журнал звонков** выполняется кнопки следующий обработчик событий:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    MainActivity.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainActivity.Instance` Поле позволяет `MainActivity.NavigateToCallHistoryPage` вызываемого метода, как показано в следующем примере кода:

```csharp
public void NavigateToCallHistoryPage()
{
    var callHistoryPage = new CallHistoryPage().CreateFragment(this);
    FragmentManager
        .BeginTransaction()
        .AddToBackStack(null)
        .Replace(Resource.Id.fragment_frame_layout, callHistoryPage)
        .Commit();
}
```

`NavigateToCallHistoryPage` Метод преобразует Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-страницу, чтобы производный `Fragment` с `CreateFragment` метод расширения и добавляет `Fragment` на фрагмент обратно в стек. Таким образом, определен пользовательский Интерфейс в Xamarin.Forms `CallHistoryPage` будет отображаться, как показано на следующем снимке экрана:

[![](native-forms-images/android-callhistorypage.png "Android CallHistoryPage")](native-forms-images/android-callhistorypage-large.png#lightbox "Android CallHistoryPage")

При `CallHistoryPage` отображается, нажимая на задний план стрелка отобразит `Fragment` для `CallHistoryPage` из этот фрагмент стек возвращение пользователю `Fragment` для `PhonewordPage` класса.

### <a name="enabling-back-navigation-support"></a>Включение поддержки переходов назад

`FragmentManager` Класс имеет `BackStackChanged` события, которое возникает при каждом изменении содержимого этот фрагмент стек. `OnCreate` Метод в `MainActivity` класс содержит анонимный обработчик этого события:

```csharp
FragmentManager.BackStackChanged += (sender, e) =>
{
    bool hasBack = FragmentManager.BackStackEntryCount > 0;
    SupportActionBar.SetHomeButtonEnabled(hasBack);
    SupportActionBar.SetDisplayHomeAsUpEnabled(hasBack);
    SupportActionBar.Title = hasBack ? "Call History" : "Phoneword";
};
```

Этот обработчик событий отображает кнопки "Назад" на панели действий, при условии, что имеется один или несколько `Fragment` экземпляров в фрагменте обратно в стек. Ответ, нажав кнопку "Назад", обрабатывается `OnOptionsItemSelected` переопределения:

```csharp
public override bool OnOptionsItemSelected(Android.Views.IMenuItem item)
{
    if (item.ItemId == global::Android.Resource.Id.Home && FragmentManager.BackStackEntryCount > 0)
    {
        FragmentManager.PopBackStack();
        return true;
    }
    return base.OnOptionsItemSelected(item);
}
```

`OnOptionsItemSelected` Переопределение вызывается каждый раз при выборе элемента в меню «Параметры». Эта реализация извлекает текущий фрагмент из этот фрагмент стек, если был выбран кнопки "Назад", а также один или несколько `Fragment` экземпляров в фрагменте обратно в стек.

### <a name="multiple-activities"></a>Несколько действий

Если приложение состоит из нескольких действий [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производных страниц могут быть внедрены в каждое из действий. В этом сценарии `Forms.Init` метод должны вызываться только в `OnCreate` переопределить первого `Activity` , внедряет Xamarin.Forms `ContentPage`. Однако при этом существуют следующие последствия:

- Значение `Xamarin.Forms.Color.Accent` будет взято из `Activity` вызвавшую `Forms.Init` метод.
- Значение `Xamarin.Forms.Application.Current` будет связан с `Activity` вызвавшую `Forms.Init` метод.

### <a name="choosing-a-file"></a>Выбор файла

При внедрении [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы, использующей [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) , требуется для поддержки HTML «Выберите файл» кнопку, `Activity` необходимо переопределить `OnActivityResult` метод:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    ActivityResultCallbackRegistry.InvokeCallback(requestCode, resultCode, data);
}
```

## <a name="uwp"></a>UWP

На UWP, собственный `App` класс обычно является местом для выполнения приложения при запуске задач, связанных с. Xamarin.Forms обычно инициализации, в приложениях Xamarin.Forms UWP в `OnLaunched` переопределить в машинном коде `App` класса, чтобы передать `LaunchActivatedEventArgs` аргумент `Forms.Init` метод. По этой причине собственных приложений UWP, использующие Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производном страницы проще всего можно вызвать `Forms.Init` метод `App.OnLaunched` метод.

По умолчанию, собственный `App` класса запускает `MainPage` класс как первая страница приложения. В следующем примере кода показан `MainPage` класса в примере приложения:

```csharp
public sealed partial class MainPage : Page
{
    public static MainPage Instance;

    public MainPage()
    {
        this.InitializeComponent();
        this.NavigationCacheMode = NavigationCacheMode.Enabled;
        Instance = this;
        this.Content = new Phoneword.UWP.Views.PhonewordPage().CreateFrameworkElement();
    }
    ...
}
```

`MainPage` Конструктор выполняет следующие задачи:

- На странице включено кэширование, чтобы новый `MainPage` не создается, когда пользователь переходит на страницу.
- Ссылку на `MainPage` класс хранится в `static` `Instance` поля. Это позволит обеспечить механизм для других классов для вызова методов, определенных в `MainPage` класса.
- `PhonewordPage` Класса, который является Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страницы, определенные в XAML, создается и преобразовать `FrameworkElement` с помощью `CreateFrameworkElement` метод расширения, а затем задайте, как содержимое `MainPage` класса.

Один раз `MainPage` конструктор выполнен, определен пользовательский Интерфейс в Xamarin.Forms `PhonewordPage` будет отображаться класса, как показано на следующем снимке экрана:

[![](native-forms-images/uwp-phonewordpage.png "UWP PhonewordPage")](native-forms-images/uwp-phonewordpage-large.png#lightbox "UWP PhonewordPage")

Взаимодействие с пользовательским Интерфейсом, например, коснувшись [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/), приведет к обработчиков событий в `PhonewordPage` выполнение кода. Например, когда пользователь касается **журнал звонков** выполняется кнопки следующий обработчик событий:

```csharp
void OnCallHistory(object sender, EventArgs e)
{
    Phoneword.UWP.MainPage.Instance.NavigateToCallHistoryPage();
}
```

`static` `MainPage.Instance` Поле позволяет `MainPage.NavigateToCallHistoryPage` вызываемого метода, как показано в следующем примере кода:

```csharp
public void NavigateToCallHistoryPage()
{
    this.Frame.Navigate(new CallHistoryPage());
}
```

Навигация в UWP обычно выполняется с `Frame.Navigate` метод, который принимает `Page` аргумент. Определяет Xamarin.Forms `Frame.Navigate` метод расширения, который принимает [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производного экземпляра страницы. Таким образом, когда `NavigateToCallHistoryPage` выполняется метод, пользовательский Интерфейс, определенный в Xamarin.Forms `CallHistoryPage` будет отображаться, как показано на следующем снимке экрана:

[![](native-forms-images/uwp-callhistorypage.png "UWP CallHistoryPage")](native-forms-images/uwp-callhistorypage-large.png#lightbox "UWP CallHistoryPage")

При `CallHistoryPage` отображается, коснувшись обратной стрелки отобразит `FrameworkElement` для `CallHistoryPage` из стека назад в приложении, возвращая пользователю `FrameworkElement` для `PhonewordPage` класса.

### <a name="enabling-back-navigation-support"></a>Включение поддержки переходов назад

На UWP приложения необходимо включить переходов назад все оборудование и программное обеспечение назад кнопками для различных устройств. Это можно сделать путем регистрации обработчика событий для `BackRequested` событие, которое может быть выполнена в `OnLaunched` метод в машинном коде `App` класса:

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    if (rootFrame == null)
    {
        ...      
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;

        SystemNavigationManager.GetForCurrentView().BackRequested += OnBackRequested;
    }
    ...
}
```

При запуске приложения `GetForCurrentView` метод извлекает `SystemNavigationManager` объект связанный с текущим представлением, а затем регистрирует обработчик событий для `BackRequested` события. Приложение получает это событие, только если его активным, а в ответ, вызывает `OnBackRequested` обработчик событий:

```csharp
void OnBackRequested(object sender, BackRequestedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        e.Handled = true;
        rootFrame.GoBack();
    }
}
```

`OnBackRequested` Вызовов обработчика событий `GoBack` метода на корневой кадр приложения и наборы `BackRequestedEventArgs.Handled` свойства `true` пометить событие как обработанное. Чтобы пометить событие как обработанное может привести к системе покидая приложения (на семейства мобильных устройств) или пропуск события (семейство устройств рабочего стола).

Приложение зависит от системы, предоставленной кнопка "Назад" на телефоне, но выбирает, нужно ли отображать кнопки "Назад" в строке заголовка на настольных компьютеров. Это достигается путем установки `AppViewBackButtonVisibility` в одно из `AppViewBackButtonVisibility` значений перечисления:

```csharp
void OnNavigated(object sender, NavigationEventArgs e)
{
    SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility =
        ((Frame)sender).CanGoBack ? AppViewBackButtonVisibility.Visible : AppViewBackButtonVisibility.Collapsed;
}
```

`OnNavigated` Обработчик событий, который выполняется в ответ на `Navigated` события, обновляет видимость заголовка кнопки "Назад", когда происходит переход на страницу. Благодаря данному Назад кнопка панели заголовка отображается, если в приложении стек не пуст, либо удален из строки заголовка, если в приложении стек пуст.

Дополнительные сведения о поддержке переходов назад на UWP в разделе [журнал переходов и обратной навигации для приложений UWP](/windows/uwp/design/basics/navigation-history-and-backwards-navigation/).

## <a name="summary"></a>Сводка

Собственный формы позволяют Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-производный страниц для использования собственных проектов Xamarin.iOS, Xamarin.Android и универсальной платформы Windows (UWP). Можно использовать в собственных проектах `ContentPage`-производный страниц, которые напрямую добавляются в проект или из проекта .NET Standard библиотеки или общий проект. В этой статье описано, как использовать `ContentPage`-производный страниц, которые напрямую добавляются в собственных проектах, а также для перемещения между ними.


## <a name="related-links"></a>Связанные ссылки

- [NativeForms (пример)](https://developer.xamarin.com/samples/xamarin-forms/Native2Forms/)
- [Исходные представления](~/xamarin-forms/platform/native-views/index.md)
