---
title: "Работа с оповещениями"
description: "В этой статье рассматривается работа с UIAlertController для отображения пользователю в Xamarin.tvOS предупреждающее сообщение."
ms.topic: article
ms.prod: xamarin
ms.assetid: F969BB28-FF2C-4A7D-88CA-F8076AD48538
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 6dabba30c5242d6e7e9ef42a4025f87826a5b89e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-alerts"></a>Работа с оповещениями

_В этой статье рассматривается работа с UIAlertController для отображения пользователю в Xamarin.tvOS предупреждающее сообщение._


Если необходимо привлечь внимание пользователей tvOS или задать разрешения на выполнение разрушительных действия (например, при удалении файла), могут быть представлены предупреждающее сообщение с помощью `UIAlertViewController`:

[![](alerts-images/alert01.png "Пример UIAlertViewController")](alerts-images/alert01.png#lightbox)

Если добавление для отображения сообщения можно добавить кнопки и текстовые поля на предупреждение, чтобы разрешить пользователю реагировать на действия и предоставлять отзывы.

<a name="About-Alerts" />

## <a name="about-alerts"></a>О предупреждениях

Как уже говорилось выше, предупреждения используются для получения внимания пользователей и сообщите ему состояние приложения или запросить отзыв. Оповещения должен представить заголовок, при необходимости иметь сообщения и один или несколько кнопок и текстовых полей.

[![](alerts-images/alert04.png "Пример оповещения")](alerts-images/alert04.png#lightbox)

Apple имеет следующие рекомендации по работе с оповещениями.

- **Оповещения только в случае необходимости используйте** -предупреждений нарушать поток пользователя с приложением и прерываний взаимодействие с пользователем и таким образом, следует использовать только для важных ситуациях, например, уведомления об ошибках, покупки из приложений и команды. 
- **Предоставляет полезных** — Если оповещение представляет параметры для пользователя, а ваш следует убедиться, что каждый параметр содержит важные сведения и полезные действия для пользователя выполнить.

<a name="Alert-Titles-and-Messages" />

### <a name="alert-titles-and-messages"></a>Заголовки предупреждений и сообщений

Apple имеет следующие рекомендации для представления этого предупреждения заголовка и необязательного сообщения.

- **Используйте названия Multiword** -заголовок оповещения должны получить точку ситуации через четко оставаясь простой. Заголовок одно слово редко предоставляет достаточно сведений.
- **Использовать описательные заголовки, не требующих сообщение** — там, где это возможно, следует рассмотреть возможность включения предупреждения заголовок описательные недостаточно, дополнительный текст сообщения не требуется. 
- **Преобразовать сообщение короткое, полное предложение** — Если требуется дополнительное сообщение для получения точки предупреждения по, сохранить его как можно более простыми и сделать его полное предложение с правильный регистр символов и знаков препинания.

<a name="Alert-Buttons" />

### <a name="alert-buttons"></a>Предупреждения кнопки

Apple состоит из следующих предложений для добавления кнопок на предупреждение.

- **Ограничение на две кнопки** — там, где это возможно, ограничить оповещения до двух кнопок. Одну кнопку оповещения содержат сведения, но никакие действия. Предупреждения об изменении две кнопки предоставляют простой Да\Нет действия.
- **Использовать Succinct логических названия кнопки** -простой, лучше всего один-два слова кнопка заголовки, которые четко описывать действие кнопки. Дополнительные сведения см. в разделе нашей [работа с кнопками](~/ios/tvos/user-interface/buttons.md) документации.
- **Четко пометить разрушительных кнопки** — для кнопки, выполняющие разрушительных действие (например, при удалении файла) четко обозначить их с `UIAlertActionStyle.Destructive` стиля.

<a name="Displaying-an-Alert" />

## <a name="displaying-an-alert"></a>Отображение оповещения

Чтобы отобразить оповещения, создайте экземпляр `UIAlertViewController` и настройте его, добавляя действия (кнопки) и выбора стиля предупреждения. Например следующий код отображает предупреждение OK или "Отмена":

```csharp
const string title = "A Short Title is Best";
const string message = "A message should be a short, complete sentence.";
const string acceptButtonTitle = "OK";
const string cancelButtonTitle = "Cancel";
const string deleteButtonTitle = "Delete";
...
        
var alertController = UIAlertController.Create (title, message, UIAlertControllerStyle.Alert);

// Create the action.
var acceptAction = UIAlertAction.Create (acceptButtonTitle, UIAlertActionStyle.Default, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

var cancelAction = UIAlertAction.Create (cancelButtonTitle, UIAlertActionStyle.Cancel, _ => 
    Console.WriteLine ("The \"OK/Cancel\" alert's other action occurred.")
);

// Add the actions.
alertController.AddAction (acceptAction);
alertController.AddAction (cancelAction);
PresentViewController (alertController, true, null);
```

Давайте рассмотрим этот код подробно. Во-первых мы создадим новое предупреждение с указанным заголовком и сообщением:

```csharp
UIAlertController.Create (title, message, UIAlertControllerStyle.Alert)
```

Теперь для каждой кнопки, мы должны отображаться в диалоговом окне Создать действие определение название кнопки, его стиль, а также действия, которые необходимо выполнить при нажатии кнопки.

```csharp
UIAlertAction.Create ("Button Title", UIAlertActionStyle.Default, _ => 
    // Do something when the button is pressed
    ...
);
```

`UIAlertActionStyle` Перечисления позволяют задать стиль кнопки как один из следующих:

- **По умолчанию** -кнопка будет кнопкой по умолчанию, выбранные при отображении предупреждения.
- **Отмена** -кнопка является кнопкой "Отмена" для предупреждения.
- **Разрушение** -выделение кнопок как разрушительных действие, например, при удалении файла. В настоящий момент tvOS отображает разрушительных кнопку с красным фоном.

`AddAction` Метод добавляет указанное действие для `UIAlertViewController` и, наконец, `PresentViewController (alertController, true, null)` метод отображает данного предупреждения для пользователя.

<a name="Adding-Text-Fields" />

## <a name="adding-text-fields"></a>Добавление текстовых полей

Кроме действий (кнопки) на предупреждение, можно добавлять текстовые поля с оповещением, чтобы разрешить пользователю заполните сведения, такие как идентификаторы пользователей и пароли:

[![](alerts-images/alert02.png "Текстовое поле в предупреждении")](alerts-images/alert02.png#lightbox)

Если пользователь выбирает текстовое поле, стандартная tvOS клавиатуры будет отображаться пользователям вводить значение для поля:

[![](alerts-images/alert03.png "Ввод текста")](alerts-images/alert03.png#lightbox)

Следующий код отображает ОК/Отмена предупреждений одно текстовое поле для ввода значения:

```csharp
UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
UITextField field = null;

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;

    // Initialize field
    field.Placeholder = placeholder;
    field.Text = text;
    field.AutocorrectionType = UITextAutocorrectionType.No;
    field.KeyboardType = UIKeyboardType.Default;
    field.ReturnKeyType = UIReturnKeyType.Done;
    field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

});

// Add cancel button
alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
    // User canceled, do something
    ...
}));

// Add ok button
alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
    // User selected ok, do something
    ...
}));

// Display the alert
controller.PresentViewController(alert,true,null);
```

`AddTextField` Метод добавляет новое текстовое поле оповещение, которое затем можно настроить, задав свойства, такие как заполнитель, текст (текст, который появляется, когда поле пусто), текстовые значения по умолчанию и типа клавиатуры. Пример:

```csharp
// Initialize field
field.Placeholder = placeholder;
field.Text = text;
field.AutocorrectionType = UITextAutocorrectionType.No;
field.KeyboardType = UIKeyboardType.Default;
field.ReturnKeyType = UIReturnKeyType.Done;
field.ClearButtonMode = UITextFieldViewMode.WhileEditing;
```

Чтобы мы могут применяться значение текстового поля в более поздней версии, мы также сохраняется копия используя следующий код:

```csharp
UITextField field = null;
...

// Add and configure text field
alert.AddTextField ((textField) => {
    // Save the field
    field = textField;
    ...
});
```

Когда пользователь укажет значение в текстовом поле, можно использовать `field` переменной для доступа к этому значению.

<a name="Alert-View-Controller-Helper-Class" />

## <a name="alert-view-controller-helper-class"></a>Вспомогательный класс контроллера представление предупреждений

Поскольку отображение простой, распространенные типы оповещений с помощью `UIAlertViewController` может привести к бит повторяющийся код, можно использовать вспомогательный класс для сокращения объема повторяющихся частей кода. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;

namespace UIKit
{
    /// <summary>
    /// Alert view controller is a reusable helper class that makes working with <c>UIAlertViewController</c> alerts
    /// easier in a tvOS app.
    /// </summary>
    public class AlertViewController
    {
        #region Static Methods
        public static UIAlertController PresentOKAlert(string title, string description, UIViewController controller) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Configure the alert
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(action) => {}));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentOKCancelAlert(string title, string description, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentDestructiveAlert(string title, string description, string destructiveAction, UIViewController controller, AlertOKCancelDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false);
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create(destructiveAction,UIAlertActionStyle.Destructive,(actionOK) => {
                // Any action?
                if (action!=null) {
                    action(true);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }

        public static UIAlertController PresentTextInputAlert(string title, string description, string placeholder, string text, UIViewController controller, AlertTextInputDelegate action) {
            // No, inform the user that they must create a home first
            UIAlertController alert = UIAlertController.Create(title, description, UIAlertControllerStyle.Alert);
            UITextField field = null;

            // Add and configure text field
            alert.AddTextField ((textField) => {
                // Save the field
                field = textField;

                // Initialize field
                field.Placeholder = placeholder;
                field.Text = text;
                field.AutocorrectionType = UITextAutocorrectionType.No;
                field.KeyboardType = UIKeyboardType.Default;
                field.ReturnKeyType = UIReturnKeyType.Done;
                field.ClearButtonMode = UITextFieldViewMode.WhileEditing;

            });

            // Add cancel button
            alert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel,(actionCancel) => {
                // Any action?
                if (action!=null) {
                    action(false,"");
                }
            }));

            // Add ok button
            alert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default,(actionOK) => {
                // Any action?
                if (action!=null && field !=null) {
                    action(true, field.Text);
                }
            }));

            // Display the alert
            controller.PresentViewController(alert,true,null);

            // Return created controller
            return alert;
        }
        #endregion

        #region Delegates
        public delegate void AlertOKCancelDelegate(bool OK);
        public delegate void AlertTextInputDelegate(bool OK, string text);
        #endregion
    }
}
```

С помощью этого класса, отображения и реагирования на предупреждения, простой может быть выполнена следующим образом:

```csharp
#region Custom Actions
partial void DisplayDestructiveAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentDestructiveAlert("A Short Title is Best","The message should be a short, complete sentence.","Delete",this, (ok) => {
        Console.WriteLine("Destructive Alert: The user selected {0}",ok);
    });
}

partial void DisplayOkCancelAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKCancelAlert("A Short Title is Best","The message should be a short, complete sentence.",this, (ok) => {
        Console.WriteLine("OK/Cancel Alert: The user selected {0}",ok);
    });
}

partial void DisplaySimpleAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentOKAlert("A Short Title is Best","The message should be a short, complete sentence.",this);
}

partial void DisplayTextInputAlert (Foundation.NSObject sender) {
    // User helper class to present alert
    AlertViewController.PresentTextInputAlert("A Short Title is Best","The message should be a short, complete sentence.","placeholder", "", this, (ok, text) => {
        Console.WriteLine("Text Input Alert: The user selected {0} and entered `{1}`",ok,text);
    });
}
#endregion
```


<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье рассматривается работа с `UIAlertController` для отображения пользователю в Xamarin.tvOS предупреждающее сообщение. Во-первых оно было показано, как отображать простое предупреждение и добавить кнопки. Далее показано, как добавить оповещение текстовые поля. Наконец показано, как использовать вспомогательный класс для сокращения объема повторяющихся частей кода, необходимого для отображения предупреждения.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
