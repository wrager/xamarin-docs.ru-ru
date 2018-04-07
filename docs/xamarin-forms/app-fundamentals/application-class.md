---
title: Класс App
description: Возможности класса приложения по умолчанию, который может быть C# или XAML
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 1e1039f513534885dffe9fef348d567243651e22
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="app-class"></a>Класс App

`Application` Базовый класс предоставляет следующие возможности, которые предоставляются в используемом по умолчанию проекты `App` подкласс:

* Объект `MainPage` свойство, которое можно задать начальную страницу приложения.
* Постоянные [ `Properties` словарь](#Properties_Dictionary) для хранения простых значений через изменения состояния жизненного цикла.
* Статический `Current` свойство, которое содержит ссылку на текущий объект приложения.

Также предоставляет [методов жизненного цикла](~/xamarin-forms/app-fundamentals/app-lifecycle.md) например `OnStart`, `OnSleep`, и `OnResume` а также события модальное навигации.

В зависимости от того, шаблон, который вы выбрали, `App` класса, можно задать одним из двух способов:

* **C#**, или
* **XAML И C#**

Для создания **приложения** класса с помощью XAML, значение по умолчанию **приложения** класса должны быть заменены XAML **приложения** класс и связанный код программной части, как показано в следующем примере кода:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

В следующем примере кода показано связанного кода.

```csharp
public partial class App : Application
{
    public App ()
    {
        InitializeComponent ();
        MainPage = new HomePage ();
    }
    ...
}
```

А также параметр [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) свойства, код программной части также необходимо вызвать `InitializeComponent` метод, чтобы загрузить и проанализировать связанные XAML.

## <a name="mainpage-property"></a>Свойство MainPage

`MainPage` Свойство `Application` класса задает корневую страницу приложения.

Например, можно создать логику в вашей `App` класса для отображения на другую страницу, в зависимости от того, является ли пользователь вошел или не.

`MainPage` Свойство должно быть установлено `App` конструктор,

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }
}
```

<a name="Properties_Dictionary" />

## <a name="properties-dictionary"></a>Словарь свойств

`Application` Имеет подкласса статическое `Properties` словарь, который может использоваться для хранения данных, в частности, для использования в `OnStart`, `OnSleep`, и `OnResume` методы. Это может осуществляться в любом месте кода Xamarin.Forms с помощью `Application.Current.Properties`.

`Properties` Использует словарь `string` ключ и сохраняет `object` значение.

Например, можно задать постоянный `"id"` свойства в любом месте в коде (при выборе элемента на странице `OnDisappearing` метод, или в `OnSleep` метод) следующим образом:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

В `OnStart` или `OnResume` методов, это значение можно затем использовать для повторного создания пользователем иным образом. `Properties` Хранилищ словарь `object`s, поэтому необходимо выполнить приведение значения перед его использованием.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
    var id = Application.Current.Properties ["id"] as int;
    // do something with id
}
```

Всегда проверяйте наличие ключа перед доступом к нему, чтобы предотвратить непредвиденные ошибки.

> [!NOTE]
> `Properties` Словарь может сериализовать только типы-примитивы для хранения данных. Для хранения других типов (такие как `List<string>`) может завершиться ошибкой без вмешательства пользователя.

<!-- bugzilla 28657 -->

### <a name="persistence"></a>Сохраняемость

`Properties` Словарь автоматически сохраняются на устройстве.
Данные, добавленные в словарь, будут доступны при переходе приложения в фоновом режиме, или даже после его перезапуска.

Xamarin.Forms 1.4 появился дополнительный метод на `Application` класс - `SavePropertiesAsync()` -которого может быть вызван для сохранения заранее `Properties` словаря. Это позволяет сохранить свойства после важные обновления, чем риск их не начало сериализации из-за сбоя или закрывается Операционной системой.

Можно найти ссылки на использование `Properties` словаря в **Создание мобильных приложений с помощью Xamarin.Forms** книга главах [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf), [15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), и [20 ](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)и в соответствующем [образцы](https://github.com/xamarin/xamarin-forms-book-preview-2).



## <a name="the-application-class"></a>Класс Application

Полный `Application` реализацию класса приведен ниже для справки:

```csharp
public class App : Xamarin.Forms.Application
{
    public App ()
    {
        MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
    }

    protected override void OnStart()
    {
        // Handle when your app starts
        Debug.WriteLine ("OnStart");
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Debug.WriteLine ("OnSleep");
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
        Debug.WriteLine ("OnResume");
    }
}

```

Этот класс затем создается в каждом проекте конкретную платформу и переданный `LoadApplication` метод, который, если `MainPage` загружается и отображается для пользователя.
В следующих разделах показан код для каждой платформы. Последние шаблоны решения Xamarin.Forms уже содержит этот код, предварительно настроенное для приложения.


### <a name="ios-project"></a>Проект iOS

IOS `AppDelegate` класс теперь наследует от `FormsApplicationDelegate`. Он должен выполнить следующее.

* Вызовите `LoadApplication` с экземпляром `App` класса.

* Всегда возвращает `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="android-project"></a>Проект Android

Android `MainActivity` теперь наследует от `FormsApplicationActivity`. В `OnCreate` переопределить `LoadApplication` метод вызывается с помощью экземпляра `App` класса.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> Имеется более новом [ `FormsAppCompatActivity` ](~/xamarin-forms/platform/android/appcompat.md) базовый класс, который может использоваться для лучшей поддержки Android материал конструктора.
> Это значение станет Android шаблон по умолчанию в будущем, но можно сделать [эти инструкции](~/xamarin-forms/platform/android/appcompat.md) для обновления существующих приложений Android.


### <a name="windows-phone-project"></a>Проект Windows Phone

Главной странице в проекте Windows Phone (на основе Silverlight) должен быть производным от `FormsApplicationPage`. Это означает, XAML и C# для `MainPage` ссылки `FormsApplicationPage` класса, как показано.

XAML использует пользовательского пространства имен так, чтобы использовалось корневой элемент `FormsApplicationPage` класса:

```xaml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

C# наследует от `FormsApplicationPage` класса и вызывает `LoadApplication` для создания экземпляра вашей Xamarin.Forms `App`. Обратите внимание, что рекомендуется всегда явно использовать пространство имен приложения для уточнения `App` поскольку приложения Windows Phone, также имеют свои собственные `App` класса, не связанным с помощью Xamarin.Forms.

```csharp
public partial class MainPage :
    global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3, use the correct namespace
    }
 }
```

### <a name="windows-81-project"></a>Проект Windows 8.1

Главной странице в [Windows 8.1 (на основе WinRT)](~/xamarin-forms/platform/windows/installation/tablet.md) проектов теперь должен быть производным от `WindowsPage`. Это означает XAML для `MainPage` ссылки `WindowsPage` класса, как показано:

XAML использует пользовательского пространства имен так, чтобы использовалось корневой элемент `FormsApplicationPage` класса:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
   ...>
</forms:WindowsPage>
```

Построение codebehind C# необходимо вызвать `LoadApplication` для создания экземпляра вашей Xamarin.Forms `App`. Обратите внимание, что рекомендуется всегда явно использовать пространство имен приложения для уточнения `App` потому, что приложения UWP также свои собственные `App` класса, не связанным с помощью Xamarin.Forms.

```csharp
public partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadApplication(new YOUR_APP_NAMESPACE.App());
    }
 }
```

Обратите внимание, что `Forms.Init()` должен вызываться **App.xaml.cs** около строки 65.

### <a name="universal-windows-project-uwp-for-windows-10"></a>Проект универсального приложения Windows (UWP) для Windows 10

[Универсальных проектов Windows](~/xamarin-forms/platform/windows/installation/universal.md) поддержки в Xamarin.Forms в настоящее время находится в предварительной версии.

Главной странице в проекте UWP, должен быть производным от `WindowsPage`. Это означает, XAML и C# для `MainPage` ссылки `FormsApplicationPage` класса, как показано.

XAML использует пользовательского пространства имен так, чтобы использовалось корневой элемент `FormsApplicationPage` класса:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

Построение codebehind C# необходимо вызвать `LoadApplication` для создания экземпляра вашей Xamarin.Forms `App`. Обратите внимание, что рекомендуется всегда явно использовать пространство имен приложения для уточнения `App` потому, что приложения UWP также свои собственные `App` класса, не связанным с помощью Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Обратите внимание, что `Forms.Init()` должен вызываться **App.xaml.cs** около строки 63.
