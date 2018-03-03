---
title: "Создание пользовательских элементов управления"
description: "В этой статье описываются способы создания пользовательских элементов управления и работы с ними в построителе интерфейса."
ms.topic: article
ms.prod: xamarin
ms.assetid: 675B9405-D9A7-49F0-94AD-417F10A71D11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f3d6301bc2c0237a268669fff437801bfb2657d1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="creating-custom-controls"></a>Создание пользовательских элементов управления

_В этой статье описываются способы создания пользовательских элементов управления и работы с ними в построителе интерфейса._

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к тому же пользовательские элементы управления, работающий в *Objective-C*, *Swift* и *Xcode* does . Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ создавать и поддерживать свои пользовательские элементы управления (или при необходимости их непосредственно в коде C#).

Хотя macOS предоставляет множество встроенных пользовательских элементов управления, могут оказаться раз, необходимо создать пользовательский элемент управления для предоставляют функциональные возможности, не предоставляемые out of box или пользовательскую тему пользовательского интерфейса (например, игры интерфейс).

[ ![](custom-controls-images/intro01.png "Пример пользовательского элемента управления пользовательского интерфейса")](custom-controls-images/intro01.png)

В этой статье мы обсудим основные принципы создания многократно используемых пользовательского элемента управления пользовательского интерфейса в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Общие сведения о пользовательских элементов управления

Как уже говорилось выше, может быть раз, когда необходимо создать повторно используемый пользовательского элемента управления пользовательского интерфейса для обеспечения уникальные функциональные возможности пользовательского интерфейса приложение Xamarin.Mac или создайте пользовательскую тему пользовательского интерфейса (например, игры интерфейс).

В таких ситуациях может легко наследовать от `NSControl` и создайте настраиваемое средство, которые можно добавить к пользовательскому Интерфейсу приложения посредством кода C# или с помощью построителя интерфейса в Xcode. Путем наследования от `NSControl` пользовательский элемент управления автоматически будет иметь все стандартные функции, которыми обладает встроенного элемента управления пользовательского интерфейса (например, `NSButton`).

Если пользовательский элемент управления пользовательского интерфейса просто отображает сведения (такие как создание пользовательских диаграмм и графическое средство), может потребоваться наследовать от `NSView` вместо `NSControl`.

Независимо от того, какой базовый класс используется основные шаги для создания пользовательского элемента управления одинаково.

В этой статье будут создания пользовательского компонента отражение коммутатор, обеспечивающий уникальный темы пользовательского интерфейса и пример построения полнофункциональным пользовательского интерфейса пользовательского элемента управления.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Создание пользовательского элемента управления

Поскольку мы создаем пользовательский элемент управления будет отвечать на запросы на ввод данных пользователем (нажатия кнопки мыши), мы собираемся наследовать от `NSControl`. Таким образом наш пользовательский элемент управления автоматически будет иметь все стандартные функции, которыми обладает встроенного элемента управления пользовательского интерфейса и отвечать как macOS стандартные элементы управления.

В Visual Studio для Mac откройте проект Xamarin.Mac, для создания пользовательского интерфейса пользовательского элемента управления для (или создайте новую). Добавьте новый класс и назовите его `NSFlipSwitch`:

[ ![](custom-controls-images/custom01.png "Добавление нового класса")](custom-controls-images/custom01.png)

Далее следует изменить `NSFlipSwitch.cs` класса и сделать его выглядеть следующим образом:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Прежде всего необходимо обратить внимание о нашем пользовательском классе, в том, что мы наследуют от `NSControl` и с помощью **зарегистрировать** команду, чтобы предоставить этот класс для Objective-C и Xcode интерфейс построителя:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

В следующих разделах мы ознакомитесь с остальной части приведенный выше код, подробно.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Отслеживание состояния элемента управления

Поскольку наш пользовательский элемент управления переключателя, мы должны возможность отслеживать состояние коммутатора On/Off. Мы обработки на следующий код в `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

При изменении состояния переключателя, нам нужна возможность обновить пользовательский Интерфейс. Мы делаем, заставив перерисовывает его пользовательского интерфейса с помощью элемента управления `NeedsDisplay = true`.

Если наш элемент управления, которые требуется один Вкл/Выкл состояния (например многими состояниями ключ с положениями 3), можно было бы использовать **перечисления** для отслеживания состояния. Например, простой **bool** будет выполнять.

Мы также добавили вспомогательный метод для замены состояние переключение между Включение и отключение:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Более поздней версии мы будет дополнен этот вспомогательный класс для информирования вызывающего объекта при изменении состояния параметров.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Рисование интерфейса элемента управления

Мы будем использовать графические Core рисования подпрограммы для рисования наш пользовательский элемент управления пользовательского интерфейса во время выполнения. Прежде чем это можно сделать, необходимо включить слои можем контролировать. Для этого с помощью следующего закрытого метода:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Этот метод вызывается из каждого элемента управления конструкторов, чтобы обеспечить правильную настройку элемента управления. Пример:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Далее, нам нужно переопределить `DrawRect` метод и добавить основные графические подпрограммы для изображения элемента управления:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Мы будем Настройка визуальное представление для элемента управления при изменении его состояния (например, начиная от **на** для **Off**). Каждый раз об изменении состояния, мы можем `NeedsDisplay = true` принудительно для повторной отрисовки для нового состояния элемента управления.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Отвечать на действия пользователя

Существует два основных способа, можно добавить введенных пользователем данных в нашем пользовательского элемента управления: **переопределить подпрограммы обработки мыши** или **распознавателей жестов**. Какой метод используется, будет основываться на функциональные возможности, необходимые для можем контролировать.

> [!IMPORTANT]
> Для любой пользовательский элемент управления, вы создаете, следует использовать **переопределить методы** _или_ **распознавателей жестов**, но не оба одновременно времени они могут конфликтовать друг с другом.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Обработка введенных пользователем данных с помощью переопределения методов

Объекты, наследуемые от `NSControl` (или `NSView`) существует несколько переопределять методы для обработки мыши или клавиши ВВОД. Для наших примера элемента управления требуется отразить состояние переключение между **на** и **Off** при нажатии на элемент управления с левой кнопки мыши. Можно добавить следующие переопределяет методы `NSFliwSwitch` классе позволяет обработать это:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

В приведенном выше коде мы называем `FlipSwitchState` (описанный выше) отражение Вкл/Выкл состояние коммутатора в метод `MouseDown` метод. Это также приведет к перерисовки для отражения текущего состояния элемента управления.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Обработка введенных пользователем данных с распознавателей жестов

При необходимости распознавателей жестов можно использовать для обработки действий пользователя с элементом управления. Удалить переопределения добавленных выше, измените `Initialize` метод и сделать его выглядеть следующим образом:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Здесь мы создаем новый `NSClickGestureRecognizer` и вызов нашей `FlipSwitchState` метод, чтобы изменить состояние параметра, когда пользователь щелкает его с левой кнопки мыши. `AddGestureRecognizer (click)` Метод добавляет распознаватель жестов в элемент управления.

Опять же какой метод, мы используем зависит от мы пытаемся выполнить с помощью наших пользовательских элементов управления. Если мы должны низким уровнем доступа для взаимодействия с пользователем, используйте методы переопределения. Если необходимо предварительно определенной функции, такие как щелчки мышью используйте распознавателей жестов.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>Отвечает на события изменения состояния

Когда пользователь изменяет состояние наш пользовательский элемент управления, мы также нужна возможность реагировать на изменения состояния в коде (например, при выполнении нечто при нажатии на кнопку). 

Для поддержки этой функции, изменить `NSFlipSwitch` и добавьте следующий код:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Далее следует изменить `FlipSwitchState` метод и сделать его выглядеть следующим образом:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Во-первых, мы предоставляем `ValueChanged` событий, что обработчик, можно добавить в код C#, чтобы мы могли выполнять действие, когда пользователь изменяет состояние коммутатора.

Во-вторых, поскольку наш пользовательский элемент управления наследуется от `NSControl`, он автоматически использует **действия** могут назначаться в построителе интерфейса в Xcode. Вызов его **действия** при изменении состояния, мы используем следующий код:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Во-первых, мы проверяем Если **действия** были назначены элементу управления. Далее мы называем **действия** если он определен.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>С помощью пользовательского элемента управления

С нашей пользовательский элемент управления полностью определена можно либо добавить его нашего приложения Xamarin.Mac пользовательского интерфейса с использованием кода C#, или в Xcode интерфейс построителя.

Чтобы добавить элемент управления с помощью интерфейса построитель, сначала выполните чистую сборку проекта Xamarin.Mac, а затем дважды щелкните `Main.storyboard` файл, чтобы открыть ее в построителе интерфейс для изменения:

[ ![](custom-controls-images/custom02.png "Редактирование раскадровки в Xcode")](custom-controls-images/custom02.png)

Затем перетащите `Custom View` в пользовательском интерфейсе конструктора:

[ ![](custom-controls-images/custom03.png "При выборе представления из библиотеки")](custom-controls-images/custom03.png)

С помощью пользовательского представления по-прежнему выбран, переключитесь в **удостоверение инспектора** и изменить представление **класса** для `NSFlipSwitch`:

[ ![](custom-controls-images/custom04.png "Представление класс параметров")](custom-controls-images/custom04.png)

Переключитесь в **помощника редактор** и создать **розетки** для пользовательского элемента управления (Убедитесь, что привязки в `ViewControler.h` файл и не `.m` файл):

[ ![](custom-controls-images/custom05.png "Настройка нового выхода")](custom-controls-images/custom05.png)

Сохранить изменения, вернитесь в Visual Studio для Mac и разрешить изменения в синхронизации. Изменить `ViewController.cs` и создайте `ViewDidLoad` внешний метод следующим образом:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

Здесь мы реагировать на `ValueChanged` мы определенного выше на события `NSFlipSwitch` класса и записать текущий **значение** при нажатии на элемент управления.

При необходимости может возвращать интерфейс построителя и определить **действия** на элементе управления:

[ ![](custom-controls-images/custom06.png "Настройка нового действия")](custom-controls-images/custom06.png)

Опять же, изменить `ViewController.cs` и добавьте следующий метод:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Следует использовать либо **событий** или определить **действия** в построителе интерфейса, но не следует использовать оба метода, в то же время или они могут конфликтовать друг с другом.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье были предприняты никакие подробное описание создание многократно используемых пользовательского элемента управления пользовательского интерфейса в приложении Xamarin.Mac. Мы узнали, как рисовать пользовательские элементы управления пользовательского интерфейса, два основных способа реагировать на ввод данных пользователем и мыши и способ предоставления нового элемента управления к действиям в построителе интерфейса в Xcode.

## <a name="related-links"></a>Связанные ссылки

- [MacCustomControl (пример)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по интерфейсу отдела OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Обработка событий мыши](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
