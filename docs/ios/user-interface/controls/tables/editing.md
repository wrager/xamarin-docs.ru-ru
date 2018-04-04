---
title: Редактирование
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 161de0209217dde671b976afad90eaad18d8c7b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="editing"></a>Редактирование

Возможности редактирования в таблице включены путем переопределения методов в `UITableViewSource` подкласс. Простой режим редактирования жест проведите для удаления, можно реализовать с помощью переопределения одного метода.
Более сложные изменения (включая перемещение строк) можно сделать с таблицей в режиме редактирования.

## <a name="swipe-to-delete"></a>Проведите для удаления

Проведите, чтобы удалить компонент естественным жест в iOS, которое рассчитывать пользователи. 

 [![](editing-images/image10.png "Пример проведите для удаления")](editing-images/image10.png#lightbox)

Существует три метода переопределения, влияющие на жестов проведите, чтобы показать **удалить** кнопки в ячейке:

-   **CommitEditingStyle** — обнаруживает исходной таблицы, если этот метод переопределяется и автоматически включает жестов проведите для удаления. Реализация этого метода должна вызывать `DeleteRows` на `UITableView` для ячейки исчезают, а также удалять базовых данных из модели (например, массив, словарь или базы данных). 
-   **CanEditRow** — Если CommitEditingStyle переопределяется, предполагается, что все строки для редактирования. Если этот метод реализуется и возвращает значение false (для некоторых конкретных строк или для всех строк), затем проведите для удаления жестов будет недоступен в этой ячейке. 
-   **TitleForDeleteConfirmation** : при необходимости указывает текст для **удалить** кнопки. Если этот метод не реализован на кнопке отображается надпись будет «Delete». 


Эти методы реализованы в `TableSource` класса следующим образом:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

В этом примере `UITableViewSource` был обновлен для использования `List<TableItem>` (а не строковый массив) как источника данных, поскольку он поддерживает добавление и удаление элементов из коллекции.


## <a name="edit-mode"></a>Режим редактирования

При таблицы в режиме редактирования пользователь видит красный графического элемента «остановить» на каждой строке, который раскрывает кнопку «Удалить», когда затронутых. В таблице также отображается значок «маркер», чтобы указать, что строки можно перетаскивать для изменения порядка.
**TableEditMode** Образец реализует эти функции, как показано.

 [![](editing-images/image11.png "Образец TableEditMode реализует эти функции, как показано")](editing-images/image11.png#lightbox)

Существует несколько различных методов на `UITableViewSource` , влияющие на поведение режима редактирования таблицы:

-   **CanEditRow** — ли каждая строка может редактироваться. Возвращает значение false, чтобы предотвратить проведите для удаления и удаления в режиме редактирования. 
-   **CanMoveRow** — возвращаемое значение true для включения маркера перемещения или false, чтобы запретить перемещение. 
-   **EditingStyleForRow** — Если таблица находится в режиме редактирования определяет возвращаемое значение из этого метода, в ячейке отображается значок удаления красный или зеленый значок добавления. Возвращает `UITableViewCellEditingStyle.None` Если строки не должно быть для редактирования. 
-   **MoveRow** — вызывается, когда строка перемещается, что структуры данных можно изменять в соответствии с данными, как оно отображается в таблице. 


Реализация для первых трех методов является относительно прост — Если вы хотите использовать `indexPath` для изменения поведения конкретных строк, просто следует жестко кодировать возвращаемые значения для всей таблицы.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

`MoveRow` Реализации немного сложнее, так как он должен для изменения структуры данных в соответствии с новым порядком. Поскольку данных реализована в виде `List` в следующем примере кода элемент данных старого расположения и вставляет его в новом расположении. При хранении данных в таблицу базы данных SQLite со столбцом «order» (например), этот метод вместо этого необходимо выполнять некоторые операции SQL для изменения порядка номеров в этом столбце.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Наконец, чтобы получить таблицу в режим редактирования, **изменить** кнопку должен вызывать `SetEditing` следующим образом

```csharp
table.SetEditing (true, true);
```

и когда пользователь закончил редактирование, **сделать** кнопки следует отключить режим редактирования:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Стиль редактирования вставки строк

Вставки строки из таблицы является нередко пользовательский интерфейс — основной пример в приложениях стандартный iOS **изменить контакт** экрана. На этом снимке экрана показано, как работает функция вставки строк — в режиме изменения режима нет дополнительных строк, (при нажатии) вставляется дополнительные строки данных. При завершении редактирования, временный **(добавить новые)** строка удаляется.

 [![](editing-images/image12.png "По завершении редактирования временный добавить новые удаления строки")](editing-images/image12.png#lightbox)

Существует несколько различных методов на `UITableViewSource` , влияющие на поведение режима редактирования таблицы. Эти методы были реализованы как показано ниже в примере кода.

-   **EditingStyleForRow** — возвращает `UITableViewCellEditingStyle.Delete` для строки, содержащие данные и возвращает `UITableViewCellEditingStyle.Insert` для последней строки (которая будет добавляться в частности вести себя как кнопка вставки). 
-   **CustomizeMoveTarget** — хотя пользователь перемещает возвращаемое значение из этого метода необязательно ячейки можно переопределить их выбор расположения. Это означает, что их можно предотвратить «удаление» ячейки в определенных позициях — как показано в примере, блокирует перемещение после любой строке **(добавить новые)** строки. 
-   **CanMoveRow** — возвращаемое значение true для включения маркера перемещения или false, чтобы запретить перемещение. В примере последней строки содержится знак «перемещения» скрыта, так как он предназначен для сервера как только кнопка вставки. 


Мы также добавить две настраиваемые методы для добавления строки «insert» и удалите его еще раз больше не требуется. Они вызываются из **изменить** и **сделать** кнопки:

-   **WillBeginTableEditing** — Если **изменить** кнопка затронутых он вызывает `SetEditing` для размещения таблицы в режиме редактирования. Это вызывает метод WillBeginTableEditing, где мы можем отобразить **(добавить новые)** строку в конец таблицы в качестве «кнопка вставки». 
-   **DidFinishTableEditing** — когда затронутых «Готово» `SetEditing` вызывается снова, чтобы отключить режим редактирования. Пример кода удаляет **(добавить новые)** строки в таблице, при редактировании больше не требуется. 


Эти переопределения методов, реализованных в образце файла **TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Эти две пользовательские методы используются для добавления и удаления **(добавить новые)** строки, когда таблицы в режиме редактирования включен или отключен:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Наконец, этот код создает экземпляр **изменить** и **сделать** кнопки с лямбда-выражения, включающие или отключающие режим редактирования, когда они затронутых:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Этот шаблон пользовательского интерфейса вставки строк не используется очень часто, но можно также использовать `UITableView.BeginUpdates` и `EndUpdates` методы для анимации, операция вставки или удаления ячеек в любой таблице. Правило с помощью этих методов является возвращение разницу по `RowsInSection` между `BeginUpdates` и `EndUpdates` вызовы должны совпадать на общее количество ячеек, добавлять или удалять с `InsertRows` и `DeleteRows` методы. Если базовый источник данных не изменяется для соответствия Вставка или удаление таблицы представления завершится ошибкой.


## <a name="related-links"></a>Связанные ссылки

- [WorkingWithTables (пример)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
