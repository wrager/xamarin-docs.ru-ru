---
title: ".Storyboard/.xib-less пользовательского интерфейса"
description: "В этой статье описывается создание пользовательского интерфейса в приложении Xamarin.Mac непосредственно из кода C#, без .storyboard файлы, файлы .xib или интерфейс построителя."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 544aad278b9bc66120e188eec54fa68be71dc625
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>.Storyboard/.xib-less пользовательского интерфейса

_В этой статье описывается создание пользовательского интерфейса в приложении Xamarin.Mac непосредственно из кода C#, без .storyboard файлы, файлы .xib или интерфейс построителя._


## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin.Mac, имеют доступ к одной элементы пользовательского интерфейса и средств, предоставляемых разработчик, работающий *Objective-C* и *Xcode* does. Как правило при создании приложения Xamarin.Mac, используемого в Xcode интерфейс построителя .storyboard или .xib файлов для создания и поддержания вы пользовательского интерфейса приложения.

Также имеется возможность создать некоторые или все из пользовательского интерфейса приложения Xamarin.Mac непосредственно в коде C#. В этой статье мы обсудим основные принципы создания пользовательских интерфейсов и элементов пользовательского интерфейса в коде C#.

[![Visual Studio для редактора кода Mac](xibless-ui-images/intro01.png "Visual Studio для Mac редактор кода")](xibless-ui-images/intro01-large.png)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Переключение окна для использования кода

При создании нового приложения Xamarin.Mac Cocoa, вы получаете стандартное окно пустым, по умолчанию. В данном окне определяется в **Main.storyboard** (или традиционно **MainWindow.xib**) файл, автоматически включаются в проект. Сюда также входят **ViewController.cs** файл, который управляет основного представления приложения (или повторно традиционно **MainWindow.cs** и **MainWindowController.cs** файла).

Чтобы перейти в окно Xibless для приложения, выполните следующее:

1. Откройте приложение, которое вы хотите остановить использование `.stroyboard` или .xib файлов для определения пользовательского интерфейса в Visual Studio для Mac.
2. В **Pad решения**, щелкните правой кнопкой мыши **Main.storyboard** или **MainWindow.xib** файла и выберите **удалить**: 

    ![Удаление основной раскадровки или окно](xibless-ui-images/switch01.png "удаление главной раскадровки или окна")
3. Из **диалогового окна удалите**, нажмите кнопку **удалить** кнопку, чтобы полностью удалить .storyboard или .xib из проекта: 

    ![Подтверждение удаления](xibless-ui-images/switch02.png "подтверждения удаления")

Теперь нам нужно будет изменить **MainWindow.cs** файл, чтобы определить макет окна и изменить **ViewController.cs** или **MainWindowController.cs** файла для создания экземпляр нашей `MainWindow` класса, так как файл .storyboard или .xib больше не используется.

Современные приложения Xamarin.Mac, использующих раскадровки для собственный пользовательский интерфейс не может содержать автоматически **MainWindow.cs**, **ViewController.cs** или **MainWindowController.cs** файлов. При необходимости, просто добавьте новый пустой класс C# в проект (**добавить** > **новый файл...**   >  **Общие** > **пустым классом**) и назовите его таким же, как отсутствующий файл. 


### <a name="defining-the-window-in-code"></a>Определение окна в коде

Далее следует изменить **MainWindow.cs** файла и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Давайте рассмотрим некоторые из основных элементов.

Во-первых, мы добавили несколько _вычисляемого свойства_ который будет функционировать как торговцам (как при окна был создан в файле .storyboard или .xib):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Это даст нам доступ к элементам пользовательского интерфейса, которые мы собираемся отображения в окне. Поскольку окна не выполняется завышенными из файла .storyboard или .xib, мы должны может создать его экземпляр (как в дальнейшем будет показано `MainWindowController` класса). Это новый метод конструктора не:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Это, где будет создать макет окна и разместить все элементы пользовательского интерфейса, необходимые для создания необходимых пользовательского интерфейса. Прежде чем мы можем добавить все элементы пользовательского интерфейса окна, оно должно _представление содержимого_ для хранения элементов:

```csharp
ContentView = new NSView (Frame);
```

Это создает представление содержимого, который будет заполнять окна. Теперь добавим наш первый элемент пользовательского интерфейса `NSButton`, в окно:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Сначала необходимо отметить том, что, в отличие от операций ввода-вывода, macOS используется математической нотации для определения его система координат окна. Поэтому исходная точка — в нижнем левом нижнем углу окна, с увеличением значения вправо и в правом верхнем углу окна. Когда мы создаем новый `NSButton`, мы это учитывать как мы определить его положение и размер на экране.

`AutoresizingMask = NSViewResizingMask.MinYMargin` Свойство указывает кнопке, нужным остаются в том же расположении, из верхней части окна при изменении размера окна по вертикали. Опять же, что это является обязательным, поскольку (0,0) находится в левом нижнем углу окна.

Наконец `ContentView.AddSubview (ClickMeButton)` добавляет метод `NSButton` в представлении содержимого, так что он будет отображаться на экране, когда приложение выполнения и окно.

Далее добавляется Метка окна, которое будет отображаться количество раз, `NSButton` была нажата: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

Поскольку macOS не имеет определенной _метка_ элемент пользовательского интерфейса, мы добавили специально оформленного нередактируемые `NSTextField` в качестве метки. Точно так же, как кнопка до размера и расположения принимает во внимание (0,0) находится в левом нижнем углу окна. `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` Использует свойство **или** оператор, чтобы объединить два `NSViewResizingMask` функции. Это сделает метки остаются в том же расположении, из верхней части окна при изменении размера окна по вертикали и сжатия и увеличения ширины при изменении размера окна по горизонтали.

Опять же `ContentView.AddSubview (ClickMeLabel)` добавляет метод `NSTextField` в представлении содержимого, так что он будет отображаться на экране при запуске приложения и открывается окно.


### <a name="adjusting-the-window-controller"></a>Настройка контроллера окна

Так как в структуре `MainWindow` не загружается из файла .storyboard или .xib, нам нужно будет внести некоторые изменения в окно контроллера. Изменить **MainWindowController.cs** файла и сделать его выглядеть следующим образом:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Разрешить рассматриваются ключевые элементы этого изменения.

Во-первых, определим новый экземпляр `MainWindow` класса и назначьте его к контроллеру базового окна `Window` свойство:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Мы определяем расположение окна экрана с `CGRect`. Так же, как система координат окна экрана определяет (0,0) в левом нижнем углу. Затем определяется стиль окна с помощью **или** оператор для объединения двух или более `NSWindowStyle` функции:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Следующие `NSWindowStyle` функции доступны:

- **Без рамки** -окна будут иметь без границы.
- **Под названием** -окна будут иметь строку заголовка.
- **Closable** -окне есть кнопка закрытия и может быть закрыт.
- **Miniaturizable** -окне есть кнопка Miniaturize и можно свести к минимуму.
- **Изменяемого размера** -кнопка Изменить размер и быть изменяемого размера окна.
- **Программа** -окно является окном стиль служебной программы (панель).
- **DocModal** -Если окна — это панель, она будет модальное документа вместо модальное системы. 
- **NonactivatingPanel** -Если окно является панель, она не станут главного окна.
- **TexturedBackground** -окна будут иметь текстуры фона.
- **Немасштабируемого** -не масштабируется окна.
- **UnifiedTitleAndToolbar** -области заголовка и панель инструментов окна будет присоединен.
- **Hud** -индикации панели отображается окно.
- **FullScreenWindow** -окна можно ввести в полноэкранном режиме.
- **FullSizeContentView** -представление содержимого окна находится за строку заголовка и область панели инструментов.

Последние два свойства определяют _тип буферизации_ окна и если рисование окна будет отложено. Дополнительные сведения о `NSWindows`, см. в разделе Apple [Знакомство с Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) документации.

Наконец, поскольку окне не выполняется завышенными из файла .storyboard или .xib, нам нужно имитировать его в нашем **MainWindowController.cs** путем вызова windows `AwakeFromNib` метод:

```csharp
Window.AwakeFromNib ();
```

Это позволит вам код на соответствие окна точно так же, как стандартное окно, загруженного из файла .storyboard или .xib.


### <a name="displaying-the-window"></a>Отображение окна

Позволяет удалить файл .storyboard или .xib и **MainWindow.cs** и **MainWindowController.cs** файлы изменены, вы будете использовать окна так же, как и любой обычного окна, созданной в Построитель Xcode элемента интерфейса с файлом .xib.

Следующие создать новый экземпляр окна и его контроллера и отображение окна на экране.

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

На этом этапе Если приложение запускается и нажата кнопка несколько раз, ниже будет отображаться:

![Пример приложения запустите](xibless-ui-images/run01.png "запуска примера приложения")


## <a name="adding-a-code-only-window"></a>Добавление только окно кода

Если нужно добавить в существующее приложение Xamarin.Mac только код, xibless окно, щелкните правой кнопкой мыши проект в **Pad решения** и выберите **добавить** > **новый файл...** . В **новый файл** диалогового окна выберите **Xamarin.Mac** > **Cocoa окна с контроллером**, как показано ниже:

![Добавление нового окна контроллера](xibless-ui-images/add01.png "добавлении нового контроллера окна") 

Как и ранее, будут удалены из проекта по умолчанию .storyboard или .xib файла (в этом случае **SecondWindow.xib**) и следуйте инструкциям [переключения окна, чтобы использовать код](#Switching_a_Window_to_use_Code) выше для покрытия Определение окна кода.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Добавление элемента пользовательского интерфейса окна в коде

Окно было ли в коде или загрузке из файла .storyboard или .xib, возможны ситуации где нужно добавить элемент пользовательского интерфейса окна из кода. Пример:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Приведенный выше код создает новый `NSButton` и добавляет его в `MyWindow` экземпляр окна для отображения. По сути любой элемент пользовательского интерфейса, который можно задать в построителе Xcode интерфейс в файле .storyboard или .xib можно создать в коде и отображается в окне.


## <a name="defining-the-menu-bar-in-code"></a>Определение строки меню в коде

Из-за текущих ограничений в Xamarin.Mac не рекомендуется создавать приложения Xamarin.Mac меню панели —`NSMenuBar`— в коде, но продолжать использовать **Main.storyboard** или **MainMenu.xib** файл для его определения. С другой стороны, можно добавлять и удалять меню и элементов меню в коде C#.

Например, изменить **AppDelegate.cs** и создайте `DidFinishLaunching` внешний метод следующим образом:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Выше создает меню строки состояния из кода и отображается при запуске приложения. Дополнительные сведения о работе с меню см. в разделе нашей [меню](~/mac/user-interface/menu.md) документации.


## <a name="summary"></a>Сводка

В этой статье были предприняты никакие подробное описание создания пользовательского интерфейса приложения Xamarin.Mac в коде C# в противоположность использованию построителя интерфейса в Xcode с .storyboard или .xib файлов.



## <a name="related-links"></a>Связанные ссылки

- [MacXibless (пример)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Меню](~/mac/user-interface/menu.md)
- [macOS рекомендациям по интерфейсам](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Введение в Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
