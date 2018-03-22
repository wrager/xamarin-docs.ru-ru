---
title: "Высота строки автоматического изменения размера"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE45A385-D40A-482A-90A0-E8382C2BFFB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f1b35905d14086dcfc0cb749c8e4cc7de1608dd5
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="auto-sizing-row-height"></a>Высота строки автоматического изменения размера

Начиная с iOS 8, Apple появилась возможность создания представления таблицы (`UITableView`), может автоматически увеличиваться или уменьшаться высота данной строки, в соответствии с размером его содержимого с помощью автоматический макет, размер классы и ограничений.

iOS 11 добавлена возможность для строк, чтобы автоматически развернуть. Заголовки, нижние колонтитулы и ячейки можно теперь автоматически определяться на основе их содержимого. Тем не менее если таблица создается в построитель интерфейс iOS конструктор, или если она имеет фиксированную высоту строк необходимо вручную включить self изменение размера ячеек, как описано в этом руководстве.

## <a name="cell-layout-in-the-ios-designer"></a>Макет ячеек в конструкторе iOS

Открытие раскадровки для таблицы представления, необходимо иметь автоматически строки-resize в конструкторе, iOS выберите ячейки *прототип* и разработки макета ячейки. Пример:

[![](autosizing-row-height-images/table01.png "Проект прототипа ячейки")](autosizing-row-height-images/table01.png#lightbox)

Для каждого элемента в прототипе добавления ограничений для поддержания элементы в правильном положении при представлении размеров для поворота или другой iOS размеру экрана устройства. Например, закрепление `Title` верхнего левого, так и справа от ячейки *представление содержимого*:

[![](autosizing-row-height-images/table02.png "Закрепление заголовок в верхней, слева и справа от представления содержимого ячеек")](autosizing-row-height-images/table02.png#lightbox)

В случае в нашем примере таблицы небольшой `Label` (под `Title`) — это поле, можно сжать и увеличиваться, чтобы увеличить или уменьшить высоту строки. Чтобы этого добиться, добавьте следующие ограничения, которые необходимо закрепить левой, правой, верхней и нижней метки:

[![](autosizing-row-height-images/table03.png "Эти ограничения, чтобы закрепить левой, правой, верхней и нижней метки")](autosizing-row-height-images/table03.png#lightbox)

Теперь, когда мы полностью ограниченное элементы в ячейке, нам нужно уточнить, какой элемент следует увеличить. Чтобы сделать это, задайте **содержимого приоритет надо тебя обнять** и **содержимого приоритет сопротивление сжатия** по мере необходимости **макета** части панели свойств:

[![](autosizing-row-height-images/table03a.png "Области макета панели свойств")](autosizing-row-height-images/table03a.png#lightbox)

Присвойте элементу, который требуется развернуть, чтобы иметь **ниже** значение приоритета, надо тебя обнять и **нижней** значение приоритета сопротивление сжатия.

Далее, нам нужно выбрать ячейку прототип и присвойте ему уникальный **идентификатор**:

[![](autosizing-row-height-images/table04.png "Предоставляя прототип ячейки уникальный идентификатор")](autosizing-row-height-images/table04.png#lightbox)

В случае нашего примера `GrowCell`. Мы будем использовать это значение позже, когда мы заполните таблицу.

> [!IMPORTANT]
> Если таблица содержит более чем один тип ячейки (**прототип**), необходимо обеспечить каждого типа имеет собственный уникальный `Identifier` для автоматического изменения размеров строк для работы.

Для каждого элемента нашей прототип ячейки назначить **имя** для представления этого кода на языке C#. Пример:

[![](autosizing-row-height-images/table05.png "Укажите имя для представления этого кода на языке C#")](autosizing-row-height-images/table05.png#lightbox)

Добавьте пользовательский класс для `UITableViewController`, `UITableView` и `UITableCell` (Prototype). Пример: 

[![](autosizing-row-height-images/table06.png "Добавление пользовательского класса UITableViewController, UITableView и UITableCell")](autosizing-row-height-images/table06.png#lightbox)

Наконец, чтобы все ожидаемые содержимое отображается в метку, установите **строки** свойства `0`:

[![](autosizing-row-height-images/table06.png "Свойство строки значение 0")](autosizing-row-height-images/table06a.png#lightbox)

С помощью пользовательского интерфейса определен добавим код, автоподбора размера высоты строки.

## <a name="enabling-auto-resizing-height"></a>Включить автоматическое изменение размеров высоты

В любом нашей таблице представления источника данных (`UITableViewDatasource`) или источник (`UITableViewSource`), когда мы dequeue ячейки, необходимо использовать `Identifier` , определенный в конструкторе. Пример:

```csharp
public string CellID {
    get { return "GrowCell"; }
}
...

public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (CellID, indexPath) as GrowRowTableCell;
    var item = Items [indexPath.Row];

    // Setup
    cell.Image = UIImage.FromFile(item.ImageName);
    cell.Title = item.Title;
    cell.Description = item.Description;

    return cell;
}
```

По умолчанию представление таблицы будут задаваться для автоматическое изменение размеров высоты строки. Чтобы это проверить, `RowHeight` должно быть равно `UITableView.AutomaticDimension`. Нам также нужно указать `EstimatedRowHeight` свойство в нашем `UITableViewController`. Пример:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Initialize table
    TableView.DataSource = new GrowRowTableDataSource(this);
    TableView.Delegate = new GrowRowTableDelegate (this);
    TableView.RowHeight = UITableView.AutomaticDimension;
    TableView.EstimatedRowHeight = 40f;
    TableView.ReloadData ();
}
```

Эта оценка не должен быть точным, просто грубую оценку средняя высота каждой строки в представлении.

Этот код в месте, при запуске приложения, каждая строка будет сжать и расширяться в зависимости от высоты последняя метка в прототипе ячейки. Пример:

[![](autosizing-row-height-images/table07.png "Запустите образец таблицы")](autosizing-row-height-images/table07.png#lightbox)


## <a name="related-links"></a>Связанные ссылки

- [GrowRowTable (пример)](https://developer.xamarin.com/samples/monotouch/GrowRowTable/)
