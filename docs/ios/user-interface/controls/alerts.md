---
title: Отображение оповещений в Xamarin.iOS
description: В этом документе описывается отображение оповещений в Xamarin.iOS с помощью UIAlertController API-интерфейсы, представленные в iOS 8.
ms.prod: xamarin
ms.assetid: 61C671E9-3757-4052-86E4-28640025A34A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 88233cb1ac31b2669fdc38bbc9b0835a45c6b0ce
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789600"
---
# <a name="displaying-alerts-in-xamarinios"></a>Отображение оповещений в Xamarin.iOS

Начиная с iOS 8, UIAlertController имеет завершенного замененного UIActionSheet и UIAlertView оба из которых являются устаревшими.

В отличие от классов, которые он заменен которых являются подклассами UIView, UIAlertController является подклассом UIViewController.

Используйте `UIAlertControllerStyle` для указания типа предупреждение для отображения. Ниже приведены эти типы оповещений.

- **UIAlertControllerStyleActionSheet**
    * Предварительная iOS 8. это было бы UIActionSheet
- **UIAlertControllerStyleAlert**
    * Предварительная iOS 8. это было бы UIAlertView 

Существует три необходимые действия, выполняемые при создании предупреждения контроллера:

- Создайте и настройте оповещения с a:
    * заголовок
    * сообщение
    * preferredStyle
    
- (Необязательно) Добавление текстового поля
- Добавьте необходимые действия
- Представления View Controller

Простейший оповещение содержит одну кнопку, как показано на этом снимке экрана:

 ![Предупреждение с одной кнопки](alerts-images/alert1.png)

Код, чтобы показать простое предупреждение выглядит следующим образом:

```csharp
okayButton.TouchUpInside += (sender, e) => {

    //Create Alert
    var okAlertController = UIAlertController.Create ("Title", "The message", UIAlertControllerStyle.Alert);

    //Add Action
    okAlertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Default, null));

    // Present Alert
    PresentViewController (okAlertController, true, null);
};
```

Отображение оповещение с несколькими действиями, выполняется таким же образом, но добавить два действия. Например на следующем рисунке показан оповещение две кнопки:

 ![ Предупреждение с две кнопки](alerts-images/alert2.png)

```csharp
okayCancelButton.TouchUpInside += ((sender, e) => {

    //Create Alert
    var okCancelAlertController = UIAlertController.Create("Alert Title", "Choose from two buttons", UIAlertControllerStyle.Alert);

    //Add Actions
    okCancelAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, alert => Console.WriteLine ("Okay was clicked")));
    okCancelAlertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, alert => Console.WriteLine ("Cancel was clicked")));

    //Present Alert
    PresentViewController(okCancelAlertController, true, null);
});
```

Оповещения можно также отобразить лист действия, аналогичные на следующий снимок экрана:

 ![Предупреждение листа](alerts-images/alert3.png)

Добавлены кнопки предупреждение с `AddAction` метод:

```csharp
actionSheetButton.TouchUpInside += ((sender, e) => {

    // Create a new Alert Controller
    UIAlertController actionSheetAlert = UIAlertController.Create("Action Sheet", "Select an item from below", UIAlertControllerStyle.ActionSheet);

    // Add Actions
    actionSheetAlert.AddAction(UIAlertAction.Create("OK",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item One pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("custom button 1",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Two pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Default, (action) => Console.WriteLine ("Item Three pressed.")));

    actionSheetAlert.AddAction(UIAlertAction.Create("Cancel",UIAlertActionStyle.Cancel, (action) => Console.WriteLine ("Cancel button pressed.")));

    // Required for iPad - You must specify a source for the Action Sheet since it is
    // displayed as a popover
    UIPopoverPresentationController presentationPopover = actionSheetAlert.PopoverPresentationController;
    if (presentationPopover!=null) {
        presentationPopover.SourceView = this.View;
        presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Up;
    }

    // Display the alert
    this.PresentViewController(actionSheetAlert,true,null);
});
```

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://developer.xamarin.com/samples/Controls/)
- [Предупреждения контроллера](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
