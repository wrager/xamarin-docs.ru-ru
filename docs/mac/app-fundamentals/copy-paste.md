---
title: путем копирования и вставки;
description: В этой статье рассматривается работа с монтажного стола для предоставления копирования и вставки в приложении Xamarin.Mac. Показано, как работать со стандартными типами данных, могут совместно использоваться несколькими приложениями и как обеспечить поддержку пользовательских данных в приложении.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cf81666403f687ce997e20f6f5f097dc9fcf1421
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="copy-and-paste"></a>путем копирования и вставки;

_В этой статье рассматривается работа с монтажного стола для предоставления копирования и вставки в приложении Xamarin.Mac. Показано, как работать со стандартными типами данных, могут совместно использоваться несколькими приложениями и как обеспечить поддержку пользовательских данных в приложении._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin.Mac, доступны такую же монтажном столе (копировать и вставить) поддержку, работающий в Objective-C имеет.

В этой статье мы обсуждаемые два основных способа использования монтажного стола в приложении Xamarin.Mac:

1. **Стандартные типы данных** -так, как монтажном столе обычно выполняются операции между двумя несвязанными приложениями, ни приложения определяет типы данных, которые поддерживают другой. Чтобы увеличить вероятность для общего доступа, монтажного стола может содержать несколько представлений данного элемента (с помощью стандартный набор общих типов данных), это позволяет приложению требуется много выбирает версию, которая лучше всего подходит для своих потребностей.
2. **Пользовательские данные** — для поддержки копирования и вставки сложных данных в рамках вашей Xamarin.Mac, можно определить тип пользовательских данных, который будет обрабатываться монтажного стола. Например приложении рисования векторных, позволяет пользователю скопировать и вставить сложные фигуры, состоящие из нескольких типов данных и точки.

[![Пример выполняемого приложения](copy-paste-images/intro01.png "примере выполняемого приложения")](copy-paste-images/intro01-large.png#lightbox)

В этой статье мы обсудим основы работы с монтажного стола в приложении Xamarin.Mac для поддержки копирования и вставки. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` атрибуты используется для подключения классов C# для Objective-C-объекты и пользовательского интерфейса элементов.

## <a name="getting-started-with-the-pasteboard"></a>Приступая к работе с монтажного стола

Монтажного стола представляет стандартный механизм для обмена данными в рамках данного приложения или между приложениями. Обычно для вставки в приложении Xamarin.Mac используется для обработки копирования и вставки, однако также поддерживается ряд других операций (например, перетащите & Drop и службы приложений).

Поможет вам быстро off нуля, будет начинаться с простых практических Общие сведения с помощью монтажных столах в приложении Xamarin.Mac. Более поздней версии мы предоставим подробное объяснение, как работает монтажного стола и методы, используемые.

В этом примере мы создадим простой документ на основе приложение, которое управляет окно, содержащее представление изображения. Пользователь будет иметь возможность копирования и вставки изображений между документами в приложении и или из других приложений или несколько окон внутри одного приложения.

### <a name="creating-the-xamarin-project"></a>Создание проекта Xamarin

Во-первых мы собираемся создавать документ на основе Xamarin.Mac приложения, она будет добавление копирования и вставьте поддержка.

Выполните следующие действия:

1. Запустите Visual Studio для Mac и нажмите кнопку **новый проект...**  ссылку.
2. Выберите **Mac** > **приложения** > **приложения Cocoa**, нажмите кнопку **Далее** кнопки: 

    [![Создание нового проекта приложения Cocoa](copy-paste-images/sample01.png "Создание нового проекта приложения Cocoa")](copy-paste-images/sample01-large.png#lightbox)
3. Введите `MacCopyPaste` для **имя проекта** и сохранить все остальное в качестве значения по умолчанию. Нажмите кнопку Далее: 

    [![Задание имени проекта](copy-paste-images/sample01a.png "настройки имени проекта")](copy-paste-images/sample01a-large.png#lightbox)

4. Нажмите кнопку **создать** кнопки: 

    [![Подтверждение параметров нового проекта](copy-paste-images/sample02.png "Подтверждение параметров нового проекта")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Добавить NSDocument

Теперь мы будем добавлять пользовательские `NSDocument` класс, который будет выступать в качестве фона хранилище для пользовательского интерфейса приложения. Он содержит представление одного образа и узнать, как скопировать изображение из представления в области вставки и берется из области вставки изображения и отобразить его в графическом режиме.

Правой кнопкой мыши проект Xamarin.Mac в **Pad решения** и выберите **добавить** > **новый файл...** :

![Добавление в проект NSDocument](copy-paste-images/sample03.png "NSDocument Добавление в проект")

Введите в поле **Имя** значение `ImageDocument` и нажмите кнопку **Новый**. Изменить **ImageDocument.cs** класса и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Ниже подробно давайте рассмотрим некоторые из кода.

В следующем коде представлен свойство для проверки существования данных изображения монтажного стола по умолчанию, если образ доступен, `true` в противном случае возвращается `false`:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Следующий код копирует изображение из представления изображение, вложенное в области вставки:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

И следующий код вставляет изображение из области вставки и отображает его в представлении вложенного изображения (если монтажного стола содержит допустимое изображение):

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Этот документ на месте мы создадим пользовательский интерфейс для приложения Xamarin.Mac.

### <a name="building-the-user-interface"></a>Создание пользовательского интерфейса

Дважды щелкните **Main.storyboard** файл, чтобы открыть его в Xcode. Затем можно также добавить панель инструментов и изображения и настройте их следующим образом:

[![Изменение панели инструментов](copy-paste-images/sample04.png "редактирование панели инструментов")](copy-paste-images/sample04-large.png#lightbox)

Добавление копии и вставьте **элементом панели инструментов изображения** в левой части панели инструментов. Мы будем работать с их как сочетания клавиш для копирования и вставки из меню "Правка". Добавьте четыре **элементы панели инструментов изображения** в правой части панели инструментов. Мы будем использовать их для заполнения изображения с некоторых изображений по умолчанию.

Дополнительные сведения о работе с панелями инструментов см. в разделе нашей [панели инструментов](~/mac/user-interface/toolbar.md) документации.

Теперь давайте также предоставляют следующие выходов и действий для наших элементы панели инструментов и изображения:

[![Создание выходов и действий](copy-paste-images/sample05.png "Создание выходов и действий")](copy-paste-images/sample05-large.png#lightbox)

Дополнительные сведения о работе с выходов и действия см. в разделе [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) часть наших [Hello, Mac](~/mac/get-started/hello-mac.md) документации.

### <a name="enabling-the-user-interface"></a>Включение пользовательского интерфейса

С нашей пользовательского интерфейса, созданные в Xcode и наш элемент пользовательского интерфейса, доступными через выходов и действия необходимо добавить код, чтобы обеспечить пользовательский Интерфейс. Дважды щелкните **ImageWindow.cs** файла в **Pad решение** и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Давайте рассмотрим этот код, подробно ниже.

Во-первых, мы предоставляем экземпляр `ImageDocument` класс, который мы создали выше:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

С помощью `Export`, `WillChangeValue` и `DidChangeValue`, у нас есть установки `Document` свойство для привязки данных в Xcode и ключ-значение кодировки.

Мы также предоставляем изображения из образа, также мы добавили в нашем пользовательском Интерфейсе в Xcode с использованием следующего свойства:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Когда загружается и отображается окно Main, создадим экземпляр нашей `ImageDocument` класса и подключить образ пользовательского интерфейса также и к нему с помощью следующего кода:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Наконец, в ответ на щелчок на копирование и вставка элементов панели инструментов вызывается экземпляр `ImageDocument` класса выполняют реальную работу:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Включение меню «файл» и «изменение

Последнее, что необходимо сделать это Включение **New** элемент меню из **файл** меню (для создания новых экземпляров нашей главного окна) и включить **Вырезать**, **копирования**  и **вставить** пункты **изменить** меню.

Чтобы включить **New** меню элемента, изменение **AppDelegate.cs** файл и добавьте следующий код:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Дополнительные сведения см. в разделе [работать с несколькими окнами](~/mac/user-interface/window.md) часть наших [Windows](~/mac/user-interface/window.md) документации.

Чтобы включить **Вырезать**, **копирования** и **вставить** пунктов меню, изменить **AppDelegate.cs** файл и добавьте следующий код:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Для каждого пункта меню, мы получить окно текущего переднего плана, ключевые и приведите его к нашей `ImageWindow` класса:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Здесь мы называем `ImageDocument` экземпляра класса окна для обработки копирования и вставки действия. Пример: 

```csharp
window.Document.CopyImage (sender);
```

Нам нужен только **Вырезать**, **копирования** и **вставить** данных монтажного стола по умолчанию или в виде образа также текущее активное окно изображения пунктов меню, должна быть доступна в том случае, если имеется.

Давайте добавим **EditMenuDelegate.cs** проект Xamarin.Mac файл и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Опять же, получить текущее окно переднего плана и использовать его `ImageDocument` экземпляра класса, существует ли данные требуется изображения. Затем мы используем `MenuWillHighlightItem` для включения или отключения каждого элемента на основании этого состояния.

Изменить **AppDelegate.cs** и создайте `DidFinishLaunching` внешний метод следующим образом:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Во-первых мы отключить автоматическое включение и отключение элементов меню в меню "Правка". Далее мы присоединить экземпляр `EditMenuDelegate` класс, созданной ранее.

Дополнительные сведения см. в разделе нашей [меню](~/mac/user-interface/menu.md) документации.

### <a name="testing-the-app"></a>Тестирование приложения

Все на месте мы готовы для тестирования приложения. Построить и запустить приложение, и это основной интерфейс отображается:

![Запуск приложения](copy-paste-images/run01.png "работающим с приложением")

При открытии меню "Правка", обратите внимание, что **Вырезать**, **копирования** и **вставить** отключены, так как отсутствует изображение не в образе, хорошо или вставки по умолчанию:

![Открытие меню "Правка"](copy-paste-images/run02.png "Открытие меню "Правка"")

Если добавить изображение к образу также и снова откройте меню "Правка", элемент, который будет включен:

![Отображение элементов меню Правка включены](copy-paste-images/run03.png "отображения элементов меню Правка включены")

При копировании изображения и выберите **New** из меню «файл» можно вставить этот образ в новое окно:

![Вставка изображения в новом окне](copy-paste-images/run04.png "вставки изображения в новом окне")

Подробное описание работы с монтажного стола в приложении Xamarin.Mac перейти в следующих разделах.

## <a name="about-the-pasteboard"></a>О монтажного стола

В macOS (прежнее название-OS X) монтажного стола (`NSPasteboard`) поддерживает несколько серверных процессов, таких как копирование и вставка, Drag & Drop и служб приложений. В следующих разделах перейти на более подробно рассмотрим некоторые основные понятия монтажном столе.

### <a name="what-is-a-pasteboard"></a>Что такое монтажного стола?

`NSPasteboard` Класс предоставляет стандартный механизм для обмена данными между приложениями или в приложении. Основная функция монтажного стола предназначен для обработки операций копирования и вставки:

1. Когда пользователь выбирает элемент в приложении и использует **Вырезать** или **копирования** пункта меню монтажного стола размещаются один или несколько представлений выбранного элемента.
2. Если пользователь использует **вставить** элемента меню (в пределах одного приложения или любой другой), версия, она может обрабатывать данные копируются из монтажного стола и добавлен к приложению.

Менее очевидная монтажном столе использует содержит поиска, перетащите, операции перетаскивания и операций со службами приложений:

- Когда пользователь начинает операцию перетаскивания, перетаскиваемые данные копируются в буфер. Если операция перетаскивания завершается после перетаскивания на другое приложение, что приложение копирует данные из монтажного стола.
- Для служб перевода данных, которые будут преобразованы копируется в буфер запрашивающему приложению. Служба приложений, получает данные из монтажного стола, осуществляет трансляцию, а затем вставляет данные обратно монтажного стола.

В простейшей форме монтажных столах используются для перемещения данных в приложении или между приложениями и таким образом, существует в специальной глобальной области памяти вне процесса приложения. Хотя основные понятия монтажных столах, легко grasps, существует несколько более сложных сведения, которые необходимо учитывать. Они рассматриваются подробно ниже.

### <a name="named-pasteboards"></a>Именованный монтажных столах

Вставки могут быть открытыми или закрытыми и могут быть использованы для выполнения различных задач в приложении или между несколькими приложениями. macOS предоставляет несколько стандартных монтажных столах, каждый из которых конкретных, четко определенных использования:

- `NSGeneralPboard` -По умолчанию монтажного стола для **Вырезать**, **копирования** и **вставить** операций.
- `NSRulerPboard` -Поддерживает **Вырезать**, **копирования** и **вставить** операции над **линейки**.
- `NSFontPboard` -Поддерживает **Вырезать**, **копирования** и **вставить** операции над `NSFont` объектов.
- `NSFindPboard` -Поддерживает конкретного приложения найти панелей, которые могут совместно использовать искомый текст.
- `NSDragPboard` -Поддерживает **Drag & Drop** операций.

В большинстве случаев используется один монтажных столах системные. Однако в некоторых ситуациях, когда необходимо создавать свои собственные монтажных столах. В этих случаях можно использовать `FromName (string name)` метод `NSPasteboard` класса, чтобы создать пользовательские вставки с заданным именем.

Кроме того, можно вызвать `CreateWithUniqueName` метод `NSPasteboard` класса, чтобы создать уникальным именем вставки.

### <a name="pasteboard-items"></a>Монтажном столе элементов

Каждый фрагмент данных, когда приложение записывает в монтажного стола считается _элемента монтажного стола_ и монтажного стола может содержать несколько элементов одновременно. Таким образом приложение можно написать несколько версий данных, будут скопированы монтажного стола (например, обычный текст и форматированный текст) и получении приложения можно прочитать только данные возможность обработки (например, только обычный текст).

### <a name="data-representations-and-uniform-type-identifiers"></a>Представления данных и идентификаторы универсального типа

Монтажном столе операции обычно занимают приложения между двумя (или более), которые не имеют сведений о друг с другом или типы данных, что каждый обрабатывать. Как уже говорилось в предыдущем разделе, полностью использовать потенциал для совместного использования информации, монтажного стола может содержать несколько представлений скопированные и вставленные данные.

Каждое представление определяется через универсальный тип идентификатора (UTI), который представляет собой не более простая строка, однозначно определяющее тип даты, которые постоянно получают (Дополнительные сведения см. в разделе Apple [Обзор идентификаторы универсальный тип ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) документации). 

При создании пользовательского типа данных (например, графического объекта в векторе Рисование приложения), можно создать собственные UTI для уникальной идентификации в копии и операции вставки.

Когда приложение готовится к вставить данные, скопированные из монтажного стола, его необходимо найти представление, которое наилучшим образом соответствует его возможностей (если таковые существуют). Как правило это будет обширный доступен тип (например форматированный текст для приложения обработки текста), будет выполнено восстановление посредством простой формы, доступные как обязательный (обычный текст для простого текстового редактора).

<a name="Promised_Data" />

### <a name="promised-data"></a>Обещанные данные

Вообще говоря можно указать столько представления данных, копируются как можно добиться максимальной обмена данными между приложениями. Однако из-за ограничения или память, он может быть неэффективным фактически записать каждого типа данных в область вставки.

В этом случае первое представление данных можно поместить монтажного стола и принимающее приложение может запросить другое представление, которое может быть созданный на лету непосредственно перед операцией вставки.

При добавлении первого элемента в монтажного стола указывается один или несколько из других представлений доступных предоставляются по объекту, который соответствует `NSPasteboardItemDataProvider` интерфейса. Эти объекты содержат дополнительные представления запросу, по запросу принимающее приложение.

### <a name="change-count"></a>Число изменений

Хранит каждый монтажного стола _число изменений_ объявлен времени, с шагом каждого нового владельца. Приложение может определить, если область вставки содержимое изменилось со времени последнего, он проверяет, обращаясь к значению счетчика изменения.

Используйте `ChangeCount` и `ClearContents` методы `NSPasteboard` класса, чтобы изменить число изменений данной монтажного стола.

## <a name="copying-data-to-a-pasteboard"></a>Копирование данных монтажного стола

С первого доступа монтажного стола, сняв все существующее содержимое и записи столько представления данных при необходимости в буфер выполнения операции копирования.

Пример:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

Как правило вы просто начну для общие монтажного стола, как сделано в предыдущем примере. Любой объект, который отправляется `WriteObjects` метод *должен* соответствует `INSPasteboardWriting` интерфейса. Несколько встроенных классов (такие как `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, и `NSPasteboardItem`) автоматически наследуют этот интерфейс.

При создании настраиваемого класса данных монтажного стола должно удовлетворять `INSPasteboardWriting` интерфейс или заключаться в экземпляр `NSPasteboardItem` класса (см. [пользовательские типы данных](#Custom_Data_Types) разделе ниже).

## <a name="reading-data-from-a-pasteboard"></a>Чтение данных из монтажного стола

Как уже говорилось выше, полностью использовать потенциал для обмена данными между приложениями, несколько представлений копируемых данных могут записывать в буфер. Именно принимающему приложению выберите обширный версии, возможно, его возможностей (если таковые существуют).

### <a name="simple-paste-operation"></a>Операция вставки простой

Чтение данных из монтажном столе с помощью `ReadObjectsForClasses` метод. Потребуется два параметра:

1. Массив `NSObject` на основе типов классов, которые требуется выполнить чтение из монтажного стола. Вы должны заказать это с наиболее требуемый тип данных во-первых, остальные типы в снижения предпочтения.
2. Словарь, содержащий дополнительные ограничения (например, ограничение для конкретных типов содержимого URL-адрес) или пустой словарь, если требуются дополнительные ограничения отсутствуют.

Метод возвращает массив элементов, соответствующих критериям, которые мы переданный и поэтому содержит не более одинаковое количество типов данных, которые запрашиваются. Это также возможно, что ни один из запрошенных типов имеются, и возвращается пустой массив.

Например, следующий код проверяет `NSImage` существует в общие монтажного стола и отображает его в хорошо изображения в этом случае:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Запрашивает несколько типов данных

В зависимости от типа создаваемого приложения Xamarin.Mac может быть способен обрабатывать несколько представлений, вставляемые данные. В этом случае существует два сценария для извлечения данных из монтажного стола:

1. Сделать за один вызов `ReadObjectsForClasses` метод и предоставляя массив всех представлений, которые необходимы (в порядке предпочтения).
2. Выполнение нескольких вызовов `ReadObjectsForClasses` метод запрашивает другой массив типов, каждый раз.

В разделе **простой операции вставки** выше в разделе Дополнительные сведения о получении данных от монтажного стола.

### <a name="checking-for-existing-data-types"></a>Проверка существующих типов данных

Бывают случаи, которые можно проверить наличие монтажного стола представление данных фактически не считывая данные от монтажного стола (таких как включение **вставить** пункта меню только в том случае, если существует допустимых данных).

Вызовите `CanReadObjectForClasses` метод монтажного стола, содержит ли данный тип.

Например, следующий код определяет, содержит ли общие монтажного стола `NSImage` экземпляр:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Чтение из монтажного стола URL-адреса

На основе функции данного приложения Xamarin.Mac, может считать монтажного стола URL-адреса, но только если они отвечают заданным условиям (например, указывающий файлов или URL-адреса с определенным типом данных). В этом случае можно указать дополнительные условия поиска с использованием второго параметра `CanReadObjectForClasses` или `ReadObjectsForClasses` методы.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Пользовательские типы данных

Бывают случаи, когда необходимо сохранить свои собственные пользовательские типы в буфер из Xamarin.Mac приложения. Например вектор, рисование приложение, которое позволяет пользователю копировать и вставить графических объектов.

В этом случае необходимо разработать пользовательский класс данных, таким образом, чтобы он наследовал `NSObject` и он соответствует несколько интерфейсов (`INSCoding`, `INSPasteboardWriting` и `INSPasteboardReading`). При необходимости можно использовать `NSPasteboardItem` для инкапсуляции данных можно скопировать или вставить.

Ниже подробно рассматриваются оба варианта.

### <a name="using-a-custom-class"></a>С помощью пользовательского класса

В этом разделе будет расширение простого примера приложения, созданную в начале этого документа и добавление пользовательского класса для отслеживания сведений об образе, мы копирования и вставки между windows.

Добавьте новый класс в проект и назовите его **ImageInfo.cs**. Измените файл и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

В следующих разделах мы сделаете подробное описание этого класса.

#### <a name="inheritance-and-interfaces"></a>Наследование и интерфейсы

Прежде чем настраиваемого класса данных может записываться или считываться монтажного стола, она должна соответствовать `INSPastebaordWriting` и `INSPasteboardReading` интерфейсов. Кроме того, он должен наследовать из `NSObject` также должен соответствовать `INSCoding` интерфейс:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Класс также должны быть предоставлены Objective-C помощью `Register` директивы и он должен предоставлять необходимые свойства и методы, с помощью `Export`. Пример:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Мы предоставляете два поля данных, этот класс будет содержать - имя образа и его тип (jpg, png, и т. д.). 

Дополнительные сведения см. в разделе [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документации, объясняется, его `Register` и `Export` атрибуты используется для подключения классов C# для Objective-C-объекты и пользовательского интерфейса элементов.

#### <a name="constructors"></a>Конструкторы

Два конструктора (должным образом передаваться Objective-C) будут необходимы в наших пользовательских классов данных, чтобы оно может быть прочитано из монтажного стола:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Во-первых, мы предоставляем _пустой_ конструктор в метод Objective-C по умолчанию `init`.

Далее мы предоставляем `NSCoding` совместимый конструктор, который будет использоваться для создания нового экземпляра объекта из монтажного стола, при вставке под именем экспортируемого `initWithCoder`.

Этот конструктор использует `NSCoder` (созданный при помощи `NSKeyedArchiver` при записи в буфер), извлекает данные пару ключ значение и сохраняет его в поля свойств класса данных.

#### <a name="writing-to-the-pasteboard"></a>Запись в буфер

Подчиняясь `INSPasteboardWriting` интерфейса, нам нужно предоставлять два метода и при необходимости третий метод, чтобы класс могут записываться в буфер.

Во-первых мы должны сообщить монтажного стола тип данных, представления, которые могут быть записаны пользовательского класса.

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Каждое представление определяется через универсальный тип идентификатора (UTI), который представляет собой не более простая строка, однозначно определяющее тип данных, которые постоянно получают (Дополнительные сведения см. в разделе Apple [Обзор идентификаторы универсальный тип ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) документации).

В нашем настраиваемого формата, мы создаем собственных UTI: «com.xamarin.image-info» (Обратите внимание, что обратную нотацию так же, как идентификатор приложения). Наш класс также может записывать стандартную строку в буфер (`public.text`). 

Теперь нам нужны для создания объекта в запрошенном пользователем формате, фактически возвращает записанных в буфер:

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Для `public.text` типа, мы возвращается простой, в формате `NSString` объекта. Для настраиваемых `com.xamarin.image-info` типа, мы используем `NSKeyedArchiver` и `NSCoder` интерфейс для кодирования пользовательский класс данных архива пару ключ значение. Мы должны реализовать следующий метод фактически обработать кодировку.

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Пар отдельных ключей и значений записываются кодировщик и второй конструктор, который мы добавили выше декодировать.

Кроме того мы могут включать следующий метод для определения параметров при записи данных в буфер:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

В настоящее время единственным `WritingPromised` параметр доступен и должен использоваться при данного типа только обещают и не были фактически записанных в буфер. Дополнительные сведения см. в разделе [ожидаемая данных](#Promised_Data) выше.

С помощью этих методов на месте записываемый в буфер нашего пользовательского класса можно использовать следующий код:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Чтение из монтажного стола

Подчиняясь `INSPasteboardReading` интерфейса, необходимо предоставить три метода, чтобы класс пользовательские данные могут быть прочитаны из монтажного стола.

Во-первых мы должны сообщить монтажного стола тип данных, представления, которые пользовательский класс может считывать из буфера обмена.

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Опять же, определяются как простой UTIs, поэтому они находятся те же типы, определенные в **записи в буфер** выше.

Далее мы должны сообщить монтажного стола _как_ каждого типа UTI будет считываться с помощью следующего метода:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Для `com.xamarin.image-info` типа, мы указываем монтажного стола декодировать пара ключ значение, которое мы создали с `NSKeyedArchiver` при создании класса в буфер, вызвав `initWithCoder:` конструктор, который мы добавили в класс.

Наконец необходимо добавить следующий метод для чтения представления данных UTI от монтажного стола:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

С помощью всех этих методов на месте класса пользовательские данные могут быть прочитаны из монтажного стола, используя следующий код:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>С помощью NSPasteboardItem

Возможны ситуации, когда нужно написать пользовательские элементы в буфер, который не является основанием для создания пользовательского класса или вы хотите предоставить данные в формате, только при необходимости. В подобных случаях можно использовать `NSPasteboardItem`.

Объект `NSPasteboardItem` предоставляет возможность точного управления данных, записанных в буфер и предназначена для временного доступа - он должен быть удален с после записи в буфер.

#### <a name="writing-data"></a>Запись данных

Для записи пользовательских данных `NSPasteboardItem` необходимо предоставить пользовательский `NSPasteboardItemDataProvider`. Добавьте новый класс в проект и назовите его **ImageInfoDataProvider.cs**. Измените файл и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Как это делалось с классом пользовательские данные, необходимо использовать `Register` и `Export` директивы для представления этого цель-C. Класс должен наследоваться от `NSPasteboardItemDataProvider` и должны быть реализованы `FinishedWithDataProvider` и `ProvideDataForType` методы.

Используйте `ProvideDataForType` метод для предоставления данных, который будет упакован в `NSPasteboardItem` следующим образом:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

В этом случае мы хранения следующая информация о нашем изображения (имя и ImageType) и записи их в простую строку (`public.text`).

Тип записи данных в буфер, используйте следующий код:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Чтение данных

Чтобы прочитать данные из монтажного стола, используйте следующий код:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Сводка

В этой статье были предприняты никакие подробное описание работы с монтажного стола в приложении Xamarin.Mac для поддержки копирования и вставки. Впервые простой пример для ознакомления с помощью стандартных монтажных столах операций. Далее ушло подробное рассмотрение монтажного стола, а также на чтение и запись данных из него. Наконец хотя рассматривались с помощью пользовательского типа данных для поддержки копирования и вставки, сложных типов данных в приложении.



## <a name="related-links"></a>Связанные ссылки

- [MacCopyPaste (пример)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Руководство по программированию на монтажном столе](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS рекомендациям по интерфейсам](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
