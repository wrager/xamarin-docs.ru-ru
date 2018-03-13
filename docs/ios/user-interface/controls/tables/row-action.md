---
title: "Работа с действиями строк"
description: "В этом руководстве демонстрируется создание пользовательского проведите действия для строки таблицы с UISwipeActionsConfiguration или UITableViewRowAction"
ms.topic: article
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 23a8fcd0633757bfffdb1761c3fc811268341b96
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-row-actions"></a>Работа с действиями строк

_В этом руководстве демонстрируется создание пользовательского проведите действия для строки таблицы с UISwipeActionsConfiguration или UITableViewRowAction_

![Демонстрация действия проведите по строкам](row-action-images/action02.png)

iOS предоставляет два способа для выполнения действий над таблицей: `UISwipeActionsConfiguration` и `UITableViewRowAction`.

`UISwipeActionsConfiguration` появился в iOS 11 и используется для определения набора действий, которые необходимо выполнить в случае, если пользователь вставляет _в любом направлении_ над строкой в табличном представлении. Это поведение аналогично собственного Mail.app 

`UITableViewRowAction` Класс используется для определения действие, которое будет иметь место, когда пользователь вставляет по горизонтали для строки в таблицу.
Например, когда изменение таблицы, считывания влево в строке отображает **удалить** кнопку по умолчанию. При присоединении несколько экземпляров `UITableViewRowAction` класса `UITableView`, может быть определено несколько пользовательских действий, каждый из которых свой собственный текст, форматирование и поведение.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

Существует три действия, необходимые для реализации действия проведите с `UISwipeActionsConfiguration`:

1. Переопределить `GetLeadingSwipeActionsConfiguration` и/или `GetTrailingSwipeActionsConfiguration` методы. Эти методы возвращают `UISwipeActionsConfiguration`. 
2. Создать экземпляр `UISwipeActionsConfiguration` должны быть возвращены. Этот класс принимает массив `UIContextualAction`.
3. Создайте таблицу `UIContextualAction`.

Это описано в следующих разделах более подробно.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. Реализация методов SwipeActionsConfigurations

`UITableViewController` (а также `UITableViewSource` и `UITableViewDelegate`) содержат два метода: `GetLeadingSwipeActionsConfiguration` и `GetTrailingSwipeActionsConfiguration`, которые используются для реализации набор действий, проведите на строку представления таблицы. Начальные действия проведите относится считывание из левой части экрана на языке, слева направо и из правой части экрана языке справа налево. 

Следующий пример (из [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) образец) демонстрируется реализация начальные конфигурации считывание. Создаются два действия, из контекстных команд, которые рассматриваются [ниже](#create-uicontextualaction). Эти действия передаются в только что инициализированный [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), который используется как возвращаемое значение.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });
    
    leadingSwipe.PerformsFirstActionWithFullSwipe = false;
    
    return leadingSwipe;
}  
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. Создать экземпляр `UISwipeActionsConfiguration`

Создать экземпляр `UISwipeActionsConfiguration` с помощью `FromActions` метод, чтобы добавить новый массив `UIContextualAction`s, как показано в следующем фрагменте кода:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

Это важно отметить, что порядок отображения действий зависит от способа передачи в массив. Например код вставляет начальные выше отображаются действия так, как:

![Начальные действия проведите отображаются на строку таблицы](row-action-images/action03.png)

В конце вставляет, действия будут отображаться как показано на следующем рисунке:

![в конце действия проведите отображаются на строку таблицы](row-action-images/action04.png)

Этот фрагмент кода также используется новый `PerformsFirstActionWithFullSwipe` свойство. По умолчанию это свойство имеет значение true, то есть, первое действие в массиве будет происходить, когда пользователь предъявляет полностью в строке. Если у вас есть действие, которое не является необратимым (например «Delete», это может быть идеальным и поэтому следует задать значение `false`.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>Создание `UIContextualAction`

Контекстно-зависимое действие — где фактически создать действие, которое будет отображаться, когда пользователь предъявляет строки таблицы.

Для инициализации действие, которое необходимо предоставить `UIContextualActionStyle`, заголовок, а также `UIContextualActionHandler`. `UIContextualActionHandler` Принимает три параметра: действие, представление, отображаемое в действие и обработчик завершения:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null)); 
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);
                            
                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

Можно изменить различные свойства visual, такие как цвет фона или фоновое изображение действия. В приведенном выше фрагменте кода демонстрируется добавление изображения к действию и установка его цвета фона на синий.

После создания контекстно-зависимое действий их можно использовать для инициализации `UISwipeActionsConfiguration` в `GetLeadingSwipeActionsConfiguration` метод.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

Чтобы определить один или несколько пользовательских строк действия для `UITableView`, необходимо создать экземпляр `UITableViewDelegate` класса и переопределить `EditActionsForRow` метод. Пример:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

Статический `UITableViewRowAction.Create` метод используется для создания нового `UITableViewRowAction` , будет отображаться **Hi** когда пользователь вставляет оставшееся по горизонтали на строки в таблице. Позже новый экземпляр `TableDelegate` создается и прикрепляется к `UITableView`. Пример:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

При запуске приведенного выше кода и вставляет пользователя, оставшегося на строку таблицы **Hi** кнопка будет отображаться вместо **удалить** кнопки, которая отображается по умолчанию:

[![](row-action-images/action01.png "Кнопка Hi, будет отображаться вместо «удалить»")](row-action-images/action01.png#lightbox)

Если пользователь нажимает кнопку **Hi** кнопки `Hello World!` записываются в консоль в Visual Studio для Mac или Visual Studio при запуске приложения в режиме отладки.



## <a name="related-links"></a>Связанные ссылки

- [TableSwipeActions (пример)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (пример)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
