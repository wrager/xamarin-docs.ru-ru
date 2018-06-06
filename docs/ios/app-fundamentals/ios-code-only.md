---
title: Создание операций ввода-вывода пользовательских интерфейсов в коде в Xamarin.iOS
description: В этом документе описывается использование кода для создания пользовательского интерфейса для приложения Xamarin.iOS. В нем описывается просмотр контроллеров построения представления иерархии, обработка, поворот и многое другое.
ms.prod: xamarin
ms.assetid: 7CB1FEAE-0BB3-4CDC-9076-5BD555003F1D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 5e8abc2cea2e2ca8abfada8bc85379d93d183768
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784638"
---
# <a name="creating-ios-user-interfaces-in-code-in-xamarinios"></a>Создание операций ввода-вывода пользовательских интерфейсов в коде в Xamarin.iOS

Пользовательский интерфейс приложения iOS подобно storefront — приложение обычно получает одно окно, но ее можно заполнить окно много объектов, его необходимо, а объекты и порядков может изменяться в зависимости от приложения требуется для отображения. В этом сценарии объекты — то, что видит пользователь — называются представлениями. Для построения одного экрана в приложении, представления, располагаются друг над другом содержимого представления иерархии и иерархии находится под управлением одного контроллера представления. Приложения с несколькими экранами используют несколько иерархий представлений содержимого, каждая из которых имеет свой контроллер представления. Приложение размещает представления в окне, чтобы создать другую иерархию представлений содержимого в зависимости от экрана, на котором находится пользователь.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

На следующей схеме показаны связи между окном, представлениями, вложенными представлениями и контроллером представления, которые позволяют вывести пользовательский интерфейс на экран устройства: 

[![](ios-code-only-images/image9.png "На этой диаграмме показаны связи между окна, представлений, представлений и View Controller")](ios-code-only-images/image9.png#lightbox)

Эти представления иерархии может быть создан с помощью [Xamarin конструктор для операций ввода-вывода](~/ios/user-interface/designer/index.md) в Visual Studio, однако полезно иметь фундаментальное понимание работы полностью в коде. В этой статье рассматриваются некоторые основные точки для получения и работает с разработки интерфейса пользователя только для кода.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

На следующей схеме показаны связи между окном, представлениями, вложенными представлениями и контроллером представления, которые позволяют вывести пользовательский интерфейс на экран устройства: 

[![](ios-code-only-images/image9.png "На этой диаграмме показаны связи между окна, представлений, представлений и View Controller")](ios-code-only-images/image9.png#lightbox)

Эти представления иерархии может быть создан с помощью [Xamarin конструктор для операций ввода-вывода](~/ios/user-interface/designer/index.md) в Visual Studio для Mac, однако полезно иметь фундаментальное понимание работы полностью в коде. В этой статье рассматриваются некоторые основные точки для получения и работает с разработки интерфейса пользователя только для кода.

-----

## <a name="creating-a-code-only-project"></a>Создание проекта только в коде

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="ios-blank-project-template"></a>Пустой шаблон проекта iOS

Сначала создайте проект iOS в Visual Studio с помощью **файл > Новый проект > Visual C# > iPhone & iPad > (Xamarin) приложение iOS** проекта, показано ниже:

[![Диалоговое окно нового проекта](ios-code-only-images/blankapp.w157-sml.png)](ios-code-only-images/blankapp.w157.png#lightbox)

Выберите **пустое приложение** шаблона проекта:

[![Выберите диалоговое окно шаблона](ios-code-only-images/blankapp-2.w157-sml.png)](ios-code-only-images/blankapp-2.w157.png#lightbox)

Шаблон пустого проекта добавляет 4 файлы в проект.

[![Файлы проекта](ios-code-only-images/empty-project.w157-sml.png "файлы проекта")](ios-code-only-images/empty-project.w157.png#lightbox)


1. **AppDelegate.cs** -содержит `UIApplicationDelegate` подкласс, `AppDelegate` , который используется для обработки событий приложения от операций ввода-вывода. Окно приложения создается в `AppDelegate` `FinishedLaunching` метод.
1. **Main.cs** -содержит точку входа для приложения, который указывает класс для `AppDelegate` .
1. **Info.plist** -файл списка свойств, который содержит сведения о конфигурации приложения.
1. **Entitlements.plist** — файл списка свойств, содержащий сведения о возможностях и разрешения приложения.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

## <a name="ios-templates"></a>Шаблоны iOS


Visual Studio для Mac не предоставляет пустой шаблон. Все шаблоны поставляются с поддержкой раскадровки, предлагающей Apple как основной способ создания пользовательского интерфейса. Тем не менее можно создать пользовательский Интерфейс полностью в коде. 

Следующие шаги описывают Удаление раскадровки из приложения: 


1. Используйте шаблон одно представление приложение для создания нового iOS проекта:
    
    [![](ios-code-only-images/single-view-app.png "Используйте шаблон одного представления приложения")](ios-code-only-images/single-view-app.png#lightbox)

1. Удалить `Main.Storyboard` и `ViewController.cs` файлов. Сделать **не** удалить `LaunchScreen.Storyboard`. Представление-контроллер следует удалить, так как он приведен полный код для представления контроллеров, созданную в раскадровку:
1. Убедитесь в том выбрать **удалить** центре:
    
    [![](ios-code-only-images/delete.png "Выберите пункт Удалить всплывающее диалоговое окно")](ios-code-only-images/delete.png#lightbox)

1. В файле Info.plist, удалить данные внутри **данных развертывания > главного интерфейса** параметр:
    
    [![](ios-code-only-images/main-interface.png "Удалить данные внутри параметр главного интерфейса")](ios-code-only-images/main-interface.png#lightbox)

1. Наконец, добавьте следующий код, чтобы ваш `FinishedLaunching` метод в классе AppDelegate:
        
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            // create a new window instance based on the screen size
            window = new UIWindow(UIScreen.MainScreen.Bounds);

            // make the window visible
            window.MakeKeyAndVisible();

            return true;
        }

Код, который был добавлен в `FinishedLaunching` на этапе 5 выше, в противном случае минимальный объем кода, необходимого для создания окна для приложения iOS.


-----



iOS-приложений с помощью [шаблона MVC](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md#Model_View_Controller). На первом экране отображается приложение создается на основе контроллера представления корневого окна. В разделе [Привет, iOS (несколько экранов)](~/ios/get-started/hello-ios-multiscreen/index.md) руководства для шаблона Дополнительные сведения о MVC сам.

Реализация `AppDelegate` добавленные шаблон создает окно приложения существует только по одному для каждого приложения iOS, которое делает ее видимой следующим кодом:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    public override UIWindow Window
            {
                get;
                set;
            }

    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow(UIScreen.MainScreen.Bounds);

        // make the window visible
        window.MakeKeyAndVisible();

        return true;
    }
}
```

Если для запуска этого приложения сейчас, вы получили бы исключения, о том, что `Application windows are expected to have a root view controller at the end of application launch`. Давайте добавления контроллера и сделать ее контроллера представления корневого приложения.

## <a name="adding-a-controller"></a>Добавление контроллера

Приложение может содержать много контроллеров представления, но он должен иметь один корневой View-Controller для управления контроллерами представления.  Добавить контроллер в окно путем создания `UIViewController` экземпляра и установке флажка `window.RootViewController` свойство:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;

        Window.RootViewController = controller;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

Каждый контроллер имеет связанного представления, которое доступно в `View` свойство. Этот код изменяет представление `BackgroundColor` свойства `UIColor.LightGray` , чтобы он будет отображаться, как показано ниже:

 [![](ios-code-only-images/image1.png "Фон представления является видимым светло-серый")](ios-code-only-images/image1.png#lightbox)

Нужно присвоить любой `UIViewController` подкласс как `RootViewController` таким образом, включая контроллеры UIKit, а также те, мы сами активно записи. Например, следующий код добавляет `UINavigationController` как `RootViewController`:

```csharp
public class AppDelegate : UIApplicationDelegate
{
    // class-level declarations

    public override UIWindow Window
    {
        get;
        set;
    }

    public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
    {
        // create a new window instance based on the screen size
        Window = new UIWindow(UIScreen.MainScreen.Bounds);

        var controller = new UIViewController();
        controller.View.BackgroundColor = UIColor.LightGray;
        controller.Title = "My Controller";

        var navController = new UINavigationController(controller);

        Window.RootViewController = navController;

        // make the window visible
        Window.MakeKeyAndVisible();

        return true;
    }
}
```

В результате получается контроллер, вложенные в контроллер навигации, как показано ниже:

 [![](ios-code-only-images/image2.png "Контроллер, вложенные в контроллер навигации")](ios-code-only-images/image2.png#lightbox)

## <a name="creating-a-view-controller"></a>Создание контроллера представления

Теперь, когда мы рассмотрели добавления контроллера как `RootViewController` окна, давайте посмотрим, как создать настраиваемое представление контроллер в коде.

Добавьте новый класс с именем `CustomViewController` как показано ниже:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](ios-code-only-images/customviewcontroller.w157-sml.png "Добавьте новый класс с именем CustomViewController")](ios-code-only-images/customviewcontroller.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](ios-code-only-images/new-file.png "Добавьте новый класс с именем CustomViewController")](ios-code-only-images/new-file.png#lightbox)

-----

Класс должен наследоваться из `UIViewController`, который находится в `UIKit` пространства имен, как показано:

```csharp
using System;
using UIKit;

namespace CodeOnlyDemo
{
    class CustomViewController : UIViewController
    {
    }
}
```

<a name="Initializing_the_View"/>

## <a name="initializing-the-view"></a>Инициализация представления

`UIViewController` содержит метод с именем `ViewDidLoad` , вызывается, когда представление-контроллер сначала загружается в память. Это необходимо выполнить инициализацию представления, такие как настройка его свойств.

Например следующий код добавляет кнопку и обработчик событий для принудительной отправки нового контроллера представления в стек навигации при нажатии кнопки:

```csharp
using System;
using CoreGraphics;
using UIKit;

namespace CodyOnlyDemo
{
    public class CustomViewController : UIViewController
    {
        public CustomViewController ()
        {
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            View.BackgroundColor = UIColor.White;
            Title = "My Custom View Controller";

            var btn = UIButton.FromType (UIButtonType.System);
            btn.Frame = new CGRect (20, 200, 280, 44);
            btn.SetTitle ("Click Me", UIControlState.Normal);

            var user = new UIViewController ();
            user.View.BackgroundColor = UIColor.Magenta;

            btn.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (user, true);
            };

            View.AddSubview (btn);

        }
    }
}
```

Чтобы загрузить этот контроллер в приложение и продемонстрируйте простой навигации, создайте новый экземпляр `CustomViewController`. Создание нового контроллера навигации, передачи Просмотр экземпляра контроллера и задать новый контроллер навигации в окно `RootViewController` в `AppDelegate` как раньше:

```csharp
var cvc = new CustomViewController ();

var navController = new UINavigationController (cvc);

Window.RootViewController = navController;
```

Теперь при загрузке приложения, `CustomViewController` загружается в контроллере навигации:

 [![](ios-code-only-images/customvc.png "CustomViewController загружается в контроллере навигации")](ios-code-only-images/customvc.png#lightbox)
 
Нажав кнопку, будет _принудительной_ контроллер представление стек навигации:

[![](ios-code-only-images/customvca.png "Новое представление-контроллер в стек переходов")](ios-code-only-images/customvca.png#lightbox)

## <a name="building-the-view-hierarchy"></a>Создание иерархии представления

В приведенном выше примере мы начали создание пользовательского интерфейса в коде, путем добавления кнопки представление-контроллер.

пользовательские интерфейсы для операций ввода-вывода состоят из иерархии представления. Дополнительные представления, такие как заголовки, кнопки, ползунки, т. д., добавляются как представлений некоторых родительского представления.

Например, изменим `CustomViewController` для создания экрана входа в систему, где пользователь может ввести имя пользователя и пароль. Экран будет состоять из двух текстовых полей и кнопки.

### <a name="adding-the-text-fields"></a>Добавление текстовых полей

Во-первых, удалить, добавленный в обработчик кнопки и события [инициализации представления](#Initializing_the_View) раздела. 

Добавить элемент управления для имени пользователя, создание и инициализация `UITextField` и добавление в иерархию представления, как показано ниже:

```csharp
class CustomViewController : UIViewController
{
    UITextField usernameField;

    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        View.BackgroundColor = UIColor.Gray;

        nfloat h = 31.0f;
        nfloat w = View.Bounds.Width;

        usernameField = new UITextField
        {
            Placeholder = "Enter your username",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 82, w - 20, h)
        };

        View.AddSubview(usernameField);
    }
}
```

Когда мы создаем `UITextField`, мы устанавливаем `Frame` свойство, чтобы определить ее расположение и размер. В iOS координаты 0,0 находится в верхнем левом углу с + x справа и + y вниз. После установки параметра `Frame` а также несколько других свойств, мы называем `View.AddSubview` добавление `UITextField` представление иерархии. Это делает `usernameField` вложенное представление из `UIView` экземпляр `View` ссылок на свойства. Вложенное представление добавляется с z порядком, который больше, чем его родительского представления для отображения данных перед родительское представление на экране.

Приложение с `UITextField` включены показано ниже:

 [![](ios-code-only-images/image4.png "Приложение с UITextField включены")](ios-code-only-images/image4.png#lightbox)

Мы можем добавить `UITextField` пароля таким же образом, только в настоящее время мы устанавливаем `SecureTextEntry` для свойства значение true, как показано ниже:

```csharp
public class CustomViewController : UIViewController
{
    UITextField usernameField, passwordField;
    public override void ViewDidLoad()
    {
       // keep the code the username UITextField
        passwordField = new UITextField
        {
            Placeholder = "Enter your password",
            BorderStyle = UITextBorderStyle.RoundedRect,
            Frame = new CGRect(10, 114, w - 20, h),
            SecureTextEntry = true
        };

      View.AddSubview(usernameField); 
      View.AddSubview(passwordField);
   }
}

```

Установка `SecureTextEntry = true` скрывает текста, введенного в `UITextField` пользователем, как показано ниже:

 [![](ios-code-only-images/image4a.png "Параметр SecureTextEntry true скрывает текста, введенного пользователем")](ios-code-only-images/image4a.png#lightbox)

### <a name="adding-the-button"></a>Добавление кнопки

Далее мы добавим кнопку, поэтому пользователь может отправить имя пользователя и пароль. Добавляется кнопка Просмотр иерархии, как и любой другой элемент управления, передавая его как аргумент родительскому представлению `AddSubview` метод снова.

Следующий код добавляет кнопки и регистрирует обработчик событий для `TouchUpInside` событий:

```csharp
var submitButton = UIButton.FromType (UIButtonType.RoundedRect);

submitButton.Frame = new CGRect (10, 170, w - 20, 44);
submitButton.SetTitle ("Submit", UIControlState.Normal);

submitButton.TouchUpInside += (sender, e) => {
    Console.WriteLine ("Submit button pressed");
};

View.AddSubview(submitButton);
```

Всего этого экрана входа будет отображаться как показано ниже:

 [![](ios-code-only-images/image5.png "Экран входа")](ios-code-only-images/image5.png#lightbox)

В отличие от предыдущих версий iOS, цвет фона кнопки по умолчанию является прозрачным. Изменение кнопки `BackgroundColor` это изменение свойств:

```csharp
submitButton.BackgroundColor = UIColor.White;
```

Это приведет к квадратную кнопку, а не стандартное округленное Обведенная кнопки. Для получения прямоугольника с закругленными edge используйте следующий фрагмент кода:

```csharp
submitButton.Layer.CornerRadius = 5f;
```

С этими изменениями представление будет выглядеть следующим образом:

[![](ios-code-only-images/image6.png "Пример выполнения представления")](ios-code-only-images/image6.png#lightbox)
 
## <a name="adding-multiple-views-to-the-view-hierarchy"></a>Добавление нескольких представлений в представление иерархии

iOS предоставляет возможность добавить несколько представлений просмотреть иерархию с помощью `AddSubviews`.

```csharp
View.AddSubviews(new UIView[] { usernameField, passwordField, submitButton }); 
```

## <a name="adding-button-functionality"></a>Добавление функциональности кнопки

При нажатии кнопки, пользователям будет ожидают, что происходит. Например, отображается предупреждение навигации выполняется или на другой экран. 

Давайте добавим код необходимо поместить второй контроллер представление в стеке навигации.

Во-первых можно создайте второй контроллер представление:

```csharp
var loginVC = new UIViewController () { Title = "Login Success!"};
loginVC.View.BackgroundColor = UIColor.Purple;
```

Добавьте функциональные возможности для `TouchUpInside` событий:

```csharp
submitButton.TouchUpInside += (sender, e) => {
                this.NavigationController.PushViewController (loginVC, true);
            };
```

Ниже показана навигации.

[![](ios-code-only-images/navigation.png "На этой диаграмме показано навигации")](ios-code-only-images/navigation.png#lightbox)

Обратите внимание, что по умолчанию при использовании контроллера навигации iOS предоставляет приложению панели навигации и кнопка "Назад" для перемещения назад по стеку.

## <a name="iterating-through-the-view-hierarchy"></a>Итерации Просмотр иерархии

Это возможно, для перебора вложенное представление иерархии и выбора любого конкретного представления. Например, чтобы найти все `UIButton` и предоставить другой кнопки `BackgroundColor`, можно использовать следующий фрагмент кода

```csharp
foreach(var subview in View.Subviews)
{
    if (subview is UIButton)
    {
         var btn = subview as UIButton;
         btn.BackgroundColor = UIColor.Green;
    }
}
```

Это, однако будет работать, если выполняется итерация для представления `UIView` как все представления будут возвращены как `UIView` как объекты, добавленные в родительское представление сами наследовать `UIView`.

## <a name="handling-rotation"></a>Обработка поворота

При повороте устройства Альбомная, элементы управления не меняют размер должным образом, как показано на следующем снимке экрана:

 [![](ios-code-only-images/image7.png "При повороте устройства Альбомная, не соответствующим образом изменяет элементы управления")](ios-code-only-images/image7.png#lightbox)

Является одним из способов устранения этой проблемы, задав `AutoresizingMask` свойство в каждом представлении. В этом случае мы хотим элементов управления, и он растянется по горизонтали, поэтому мы бы установили для каждого `AutoresizingMask`. Следующий пример основан на `usernameField`, но одинаковыми должен применяться к мини-приложений в иерархии представления.

```csharp
usernameField.AutoresizingMask = UIViewAutoresizing.FlexibleWidth;
```

Теперь, когда мы повернуть устройство или симулятор, все растягивается с целью заполнения дополнительное пространство, как показано ниже:

 [![](ios-code-only-images/image8.png "Все элементы управления растягиваются для заполнения дополнительное пространство")](ios-code-only-images/image8.png#lightbox)

## <a name="creating-custom-views"></a>Создание настраиваемых представлений

Помимо использования элементов управления, которые являются частью UIKit, можно также использовать настраиваемые представления. Представления можно создать путем наследования от `UIView` и переопределение `Draw`. Давайте создать настраиваемое представление и добавить его в иерархию представления для демонстрации.

### <a name="inheriting-from-uiview"></a>Наследование от UIView

Первое, что нам нужно будет создать класс настраиваемого представления. Мы сделаем это с помощью **класса** шаблон в Visual Studio для добавления пустой класс с именем `CircleView`. Базовый класс должен иметь значение `UIView`, которую мы возобновить находится в `UIKit` пространства имен. Нам также нужно `System.Drawing` также пространство имен. Другие различных `System.*` пространства имен не будет используется в этом примере, поэтому вы можете удалить их.

Класс должен выглядеть следующим образом:

```csharp
using System;

namespace CodeOnlyDemo
{
    class CircleView : UIView
    {
    }
}
```

### <a name="drawing-in-a-uiview"></a>Рисование в UIView

Каждый `UIView` имеет `Draw` метода, вызываемого при его отрисовке. `Draw` вызывать не следует напрямую. Он вызывается системой во время выполнения цикла обработки. Во время первого выполнения выполнения цикла после добавления представления просмотр иерархии, его `Draw` вызывается метод. Последующие вызовы `Draw` возникают, когда представление является помеченные для рисования с помощью вызова `SetNeedsDisplay` или `SetNeedsDisplayInRect` в представлении.

Мы можем добавить код прорисовки наши представления, добавив такой код внутри переопределенный `Draw` метода, как показано ниже:

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    //get graphics context
    using (var g = UIGraphics.GetCurrentContext())
    {
        // set up drawing attributes
        g.SetLineWidth(10.0f);
        UIColor.Green.SetFill();
        UIColor.Blue.SetStroke();

        // create geometry
        var path = new CGPath();
        path.AddArc(Bounds.GetMidX(), Bounds.GetMidY(), 50f, 0, 2.0f * (float)Math.PI, true);

        // add geometry to graphics context and draw
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);
    }
}
```

Поскольку `CircleView` — `UIView`, также можно задать `UIView` также свойства. Например, можно выбрать вариант `BackgroundColor` в конструкторе:

```csharp
public CircleView()
{
    BackgroundColor = UIColor.White;
}
```

Для использования `CircleView` мы только что создали, мы можем либо добавить его как вложенное представление иерархии представления в существующем контроллере, как мы сделали с `UILabels` и `UIButton` ранее, или его можно загрузить как представление нового контроллера. Давайте сделано позже.

### <a name="loading-a-view"></a>Загрузка представления

 `UIViewController` метод с именем `LoadView` , называется контроллером для создания представления. Это необходимо создать представление и назначить его к контроллеру `View` свойство.

Во-первых, нужен контроллер, поэтому следует создать новый пустой класс с именем `CircleController`.

В `CircleController` добавьте следующий код, чтобы задать `View` для `CircleView` (не следует вызывать `base` реализацию в переопределении):

```csharp
using UIKit;

namespace CodeOnlyDemo
{
    class CircleController : UIViewController
    {
        CircleView view;

        public override void LoadView()
        {
            view = new CircleView();
            View = view;
        }
    }
}
```

Наконец мы должны представлять контроллера во время выполнения. Давайте это сделать, добавив обработчик событий для кнопки "Отправить", добавленный ранее, как показано ниже:

```csharp
submitButton.TouchUpInside += delegate
{
    Console.WriteLine("Submit button clicked");

    //circleController is declared as class variable
    circleController = new CircleController();
    PresentViewController(circleController, true, null);
};
```

Теперь когда мы запустите приложение и коснитесь кнопки "Отправить", отображается новое представление с окружностью:

 [![](ios-code-only-images/circles.png "Появится новое представление с окружностью")](ios-code-only-images/circles.png#lightbox)

## <a name="creating-a-launch-screen"></a>Создание экрана запуска

Объект [экран запуска](~/ios/app-fundamentals/images-icons/launch-screens.md) отображается при запуске приложение как способ отображения пользователям, что это отвечать на запросы. Поскольку при загрузке приложения отображается экран запуска, не может быть создан в коде, как приложение по-прежнему загружается в память. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Если вашей Создание проекта в Visual Studio, на экран запуска предоставляется в виде файла .xib, которую можно найти в iOS **ресурсы** папку внутри проекта. 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Если ваш Создание проекта iOS в Visual Studio для Mac, на экран запуска предоставляется автоматически в виде файла раскадровки. 

-----

Это можно изменить с помощью двойных щелкните ее и открыв его в конструкторе iOS.

Apple рекомендует .xib или файл раскадровки используется для приложений, предназначенных для iOS 8 или более поздней версии, при запуске файла, либо в конструкторе iOS вы используете классы размер и автоматический макет для адаптации макета, чтобы он выглядит неплохо и отображается неправильно, для всех устройств размеры. Изображение статических запуска можно использовать в дополнение к .xib или раскадровки для обеспечения поддержки для приложений, предназначенных для более ранних версий.

Дополнительные сведения о создании экрана запуска см. ниже документы:

- [Создание экрана запуска с помощью .xib](https://developer.xamarin.com/recipes/ios/general/templates/launchscreen-xib/)
- [Управление экраны запуска с помощью раскадровки](~/ios/app-fundamentals/images-icons/launch-screens.md)

> [!IMPORTANT]
> Начиная с версии iOS 9 Apple рекомендует что раскадровки следует использовать в качестве основного способа создания экрана запуска.

### <a name="creating-a-launch-image-for-pre-ios-8-applications"></a>Создание образа запуска для предварительной iOS 8 приложений

Если вы приложение предназначено для версий до iOS 8 статическое изображение можно использовать в дополнение к .xib или экран запуска раскадровки. 

Это статическое изображение можно задать в файле Info.plist или как каталог активов (для iOS 7) в приложении. Необходимо будет предоставить для каждого устройства размер (320 x 480, 640 x 960 640 x 1136), приложение может выполняться на отдельные изображения. Дополнительные сведения о размерах экрана запуска [запуска изображения](~/ios/app-fundamentals/images-icons/launch-screens.md) руководства.

> [!IMPORTANT]
> Если ваше приложение имеет без запуска экрана, вы можете заметить, что оно не помещается полностью экрана. Если это так, вам следует убедиться, что включить, по крайней мере, 640 x 1136 изображение с именем `Default-568@2x.png` для вашей Info.plist. 



## <a name="summary"></a>Сводка

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В этой статье рассматриваются способы разработки приложений iOS программно в Visual Studio. Мы рассмотрели способ построения проекта из пустой шаблон проекта, инструкции для создания и добавления контроллера представления корневого в окно. Затем мы показали, как использовать элементы управления из UIKit для создания представления иерархии в контроллере для разработки на экран приложения. Далее мы рассмотрели, как делать представления макета соответствующим образом в разные ориентации, и мы узнали, как создать настраиваемое представление путем создания подкласса `UIView`, а также для загрузки представление в контроллере. Наконец, мы изучена Добавление экран запуска приложения.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

В этой статье описывается, как разрабатывать приложения iOS программно в Visual Studio для Mac. Мы рассмотрели способ построения проекта из шаблона единое представление инструкции для создания и добавления контроллера представления корневого в окно. Затем мы показали, как использовать элементы управления из UIKit для создания представления иерархии в контроллере для разработки на экран приложения. Далее мы рассмотрели, как делать представления макета соответствующим образом в разные ориентации, и мы узнали, как создать настраиваемое представление путем создания подкласса `UIView`, а также для загрузки представление в контроллере. Наконец, мы изучена Добавление экран запуска приложения.

-----




## <a name="related-links"></a>Связанные ссылки

- [SimpleLogin (пример)](https://developer.xamarin.com/samples/monotouch/SimpleLogin)
