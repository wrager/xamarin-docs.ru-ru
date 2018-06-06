---
title: Создание объектов пользовательского интерфейса в Xamarin.iOS
description: Этот документ содержит общие сведения о том, как создавать пользовательский интерфейс в Xamarin.iOS. Он описывает iOS конструктор, интерфейс построителя Xcode, C# и раскадровки.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c688dcdf7498b0a2860d1878d893beae4f5cf8fc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790156"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Создание объектов пользовательского интерфейса в Xamarin.iOS

Apple группы связанных функциональные элементы в «платформы», которые приравниваются к Xamarin.iOS пространства имен. `UIKit` представляет пространство имен, содержащее все элементы управления пользовательского интерфейса для операций ввода-вывода.

Не забудьте включить следующее всякий раз, когда код должен ссылаться на элемент управления интерфейса пользователя, таких как кнопки, метки или с помощью инструкции:

```csharp
using UIKit;
```

Все элементы управления, описанных в этой главе находятся в пространстве имен UIKit и имеет имя класса элемента управления каждого пользователя `UI` префикс.

Можно изменять элементы управления пользовательского интерфейса и макеты тремя способами:

-  **[Xamarin iOS конструктор](~/ios/user-interface/designer/index.md)**  — Xamarin используйте встроенные макет конструктора для разработки экранов. Дважды щелкните раскадровки или XIB файлов для редактирования с помощью встроенного конструктора.
-  **Интерфейс построителя Xcode** — перетащите на экран макеты с помощью построителя интерфейса элемента управления. Откройте в Xcode раскадровки или XIB файла щелкните правой кнопкой мыши файл в **Pad решения** и выбрав **открыть с помощью > Xcode интерфейс построителя**.
-  **С помощью C#** — элементы управления можно также программно создан с кодом и добавить представление иерархии.

Можно добавить новые файлы раскадровки и XIB путем щелчка правой кнопкой мыши проект iOS и выбрав **Добавить > новый файл...** .

Какой бы метод вы используете, управление свойствами и событиями можно по-прежнему работать с помощью C# в логике приложения.

## <a name="using-xamarin-ios-designer"></a>С помощью Xamarin iOS конструктора

Чтобы начать создание пользовательского интерфейса в конструкторе iOS, дважды щелкните файл раскадровки. Элементы управления можно перетаскивать на поверхность разработки из **элементов** как показано ниже:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

 [![](creating-ui-objects-images/image2b.png "Панель элементов")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [![](creating-ui-objects-images/image2b-vs.png "Панель элементов - Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

При выборе элемента управления в рабочей области конструирования **панель свойств** отобразятся атрибуты этого элемента управления. **Мини-приложения > удостоверение > имя** поле, которое заполняется на снимке экрана ниже, используется в качестве *розетки* имя. Вот как можно ссылаться на элемент управления в C#.

 [![](creating-ui-objects-images/image3b.png "Панель свойств мини-приложения")](creating-ui-objects-images/image3b.png#lightbox)

Подробно изучить с помощью конструктора операций ввода-вывода, см. в разделе [введение в конструктор iOS](~/ios/user-interface/designer/introduction.md) руководства.

## <a name="using-xcode-interface-builder"></a>С помощью построителя Xcode интерфейса

Если вы не знакомы с использованием интерфейса построитель, ссылаются на Apple [интерфейс построителя](https://developer.apple.com/xcode/interface-builder/) документов.

Чтобы открыть раскадровку в Xcode, щелкните правой кнопкой мыши для доступа к контекстное меню для файла раскадровки и открыть с **Xcode интерфейс построителя**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

 [![](creating-ui-objects-images/imagexcode.png "Контекстное меню раскадровки: Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](creating-ui-objects-images/imagexcode-vs.png "Контекстное меню раскадровки: Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Элементы управления можно перетаскивать на поверхность разработки из **библиотека объектов** показано ниже:

 [![](creating-ui-objects-images/image5a.png "Библиотека объектов Xcode")](creating-ui-objects-images/image5a.png#lightbox)

При разработке пользовательского интерфейса с помощью построителя интерфейса, необходимо создать **розетки** для каждого элемента управления, который необходимо ссылаться на языке C#. Это можно сделать, включив **помощника редактор** с помощью центра **редактор** кнопки на кнопке панели инструментов Xcode:

 [![](creating-ui-objects-images/image6a.png "Кнопки помощника редактора")](creating-ui-objects-images/image6a.png#lightbox)

Щелкните объект пользовательского интерфейса; затем **управления перетащите** в h-файл. Чтобы ** управления перетащите **, удерживайте нажатой клавишу а затем нажмите и удерживайте на объект интерфейса пользователя, вы создаете розетки (или действия). Удерживайте нажатой клавишу при перетаскивании в файле заголовка. Готово, перетащив ниже `@interface` определения. Синей линией должно отобразиться с заголовок вставить розетки или розетки коллекции, как показано на снимке экрана ниже.

После отпускания кнопки click будет предложено указать имя выхода, который будет использоваться для создания свойство C#, которое можно ссылаться в коде:

 [![](creating-ui-objects-images/image8a.png "Создание выхода")](creating-ui-objects-images/image8a.png#lightbox)

Дополнительные сведения о как интерфейс построителя Xcode интегрируется с Visual Studio для Mac см. [создания кода Xib](~/ios/internals/xib-code-generation.md#generated) документа.

##  <a name="using-c"></a>С помощью C#

Если вы решите программным способом создать объект пользовательского интерфейса с помощью C# (в представлении или представлении контроллер, например), выполните следующие действия.

-  Объявите поле уровня класса для объекта интерфейса пользователя. Создание элемента управления, в `ViewDidLoad` пример. Объект можно ссылаться на протяжении жизненного цикла методы View Controller (например)
`ViewWillAppear`).
-  Создание `CGRect` , определяющий фрейм элемента управления (его координаты X и Y на экране, а также высоту и ширину). Необходимо убедиться, что у вас есть `using CoreGraphics` для этой директивы.
-  Вызовите конструктор для создания и назначения элемента управления.
-  Задайте необходимые свойства или обработчики событий.
-  Вызовите `Add()` Добавление элемента управления, чтобы просмотреть иерархию.

Ниже приведен простой пример создания `UILabel` в контроллере представление с использованием C#:

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>С помощью C# и раскадровки

При добавлении в область конструктора Просмотр контроллеров двух соответствующих файлов C# создаются в проекте. В этом примере `ControlsViewController.cs` и `ControlsViewController.designer.cs` были автоматически созданы:

 [![](creating-ui-objects-images/image9b.png "Разделяемый класс ViewController")](creating-ui-objects-images/image9b.png#lightbox)

`MainViewController.cs` Файл предназначен для *кода*. Это место, куда `View` методов жизненного цикла, такие как `ViewDidLoad` и `ViewWillAppear` реализованы и где можно добавить собственные свойства, поля и методы.

`ControlsViewController.designer.cs` — Созданный код, содержащий разделяемый класс. Если имя элемента управления в области конструктора область в Visual Studio для Mac, или создать розетки или действие в Xcode, соответствующему свойству или разделяемого метода, добавляется файл конструктора (designer.cs). В следующем примере кода показан пример кода, созданного для обеих кнопок и представления текста, где одну из кнопок также имеет `TouchUpInside` событий.

Эти элементы разделяемого класса, включите код, чтобы ссылаться на элементы управления и реагировать на действия, которые объявлены в области конструктора:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

`designer.cs` Файл не может быть изменен вручную — интегрированная среда разработки (Visual Studio для Visual Studio или Mac) отвечает его синхронизации с раскадровкой.

При добавлении объектов пользовательского интерфейса программными средствами `View` или `ViewController`, создать экземпляр и самостоятельно управлять ссылок на объекты, и таким образом файл конструктора не является обязательным.



## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://developer.xamarin.com/samples/Controls/)
