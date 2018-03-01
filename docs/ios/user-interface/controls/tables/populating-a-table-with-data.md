---
title: "Заполнение таблицы данных"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: fb0e4341d8d8ad0719f35c691add9bad1d3f85a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="populating-a-table-with-data"></a>Заполнение таблицы данных

Для добавления строк к `UITableView` необходимо реализовать `UITableViewSource` подкласс и вызывает методы, которые Просмотр таблицы для заполнения сам переопределения.

В настоящем руководстве описывается:

- Создание подклассов UITableViewSource
- Повторное использование ячейки
- Добавление индекса
- Добавление верхних и нижних колонтитулов


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Создание подклассов UITableViewSource

Объект `UITableViewSource` подкласс назначается для каждого `UITableView`. Представление таблицы запрашивает исходный класс, чтобы определить, как для своей отрисовки (для, например, сколько строк являются обязательными и высоту каждой строки, если он отличается от установленного по умолчанию). Самое главное источник предоставляет каждое представление ячейки заполняются данными.

Существует только два обязательных методы, необходимые для создания таблицы отображения данных.

-   **RowsInSection** — возвращаемое [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) число общее число строк данных в таблице будет отображаться.
-   **GetCell** — возвращаемое `UITableCellView` заполнение данными для соответствующего индекс строки, передаваемые методу.


Образец файла BasicTable **TableSource.cs** имеет простейшая реализация `UITableViewSource`. Он принимает массив строк для отображения в таблице и возвращает стиль ячейки по умолчанию, каждая строка можно увидеть в приведенном ниже фрагменте кода:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

Объект `UITableViewSource` можно использовать любой структуре данных из массива простых строк (как показано в следующем примере) <> списка или другие коллекции. Реализация `UITableViewSource` методы изолирует таблицы из структуры данных.

Чтобы использовать этот подкласс, создать массив строк для создания источника, а затем назначить экземпляр `UITableView`:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Результирующая таблица выглядит следующим образом:

 [ ![](populating-a-table-with-data-images/image3.png "Образец таблицы под управлением")](populating-a-table-with-data-images/image3.png)

Большинство таблиц позволяют пользователю touch строки, чтобы выбрать ее и выполнить другие действия (например, воспроизводить песню, или вызов контакт или отображение другой экран). Чтобы добиться этого, существуют некоторые моменты, которые нужно выполнить. Во-первых давайте создадим AlertController для отображения сообщения, когда пользователь щелкните строку, добавив следующую команду, чтобы `RowSelected` метод:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Теперь следует создайте экземпляр нашей представление-контроллер.

```csharp
HomeScreen owner;
```
Добавьте конструктор для класса UITableViewSource, который принимает контроллер представление как параметр и сохраняет его в поле:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Измените метод ViewDidLoad, где создается класс UITableViewSource для передачи `this` ссылку:

```csharp
table.Source = new TableSource(tableItems, this);
```
Наконец, обратно в вашей `RowSelected` метод, вызовите `PresentViewController` кэшированных поля:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Теперь пользователь могут соприкасаться строку и будет отображаться предупреждение:



 [ ![](populating-a-table-with-data-images/image4.png "Строки выбранного оповещения")](populating-a-table-with-data-images/image4.png)


## <a name="cell-reuse"></a>Повторное использование ячейки

В этом примере имеется только шесть элементов, поэтому нет, повторное использование ячейки не требуется. При отображении сотен или тысяч строк, однако было бы потерей памяти для создания сотен или тысяч `UITableViewCell` объектов, когда только некоторые поместиться на экране одновременно.

Чтобы избежать такой ситуации, когда ячейки исчезнет с экрана, на котором представления помещается в очередь для повторного использования. При переходе пользователя, вызывает таблицы `GetCell` для запроса нового представления для отображения — повторно использовать существующие ячейки, (который не отображается в настоящий момент) вызовите `DequeueReusableCell` метод. Если ячейки доступен для повторного использования, которые будут возвращены, в противном случае возвращается значение null и код необходимо создать новый экземпляр ячейки.

Этот фрагмент кода в примере демонстрируется шаблон:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

`cellIdentifier` Сути создает отдельных очередей для различных типов ячейки. В этом примере все ячейки выглядеть же жестко задано таким образом, только один идентификатор используется. Если существуют различные типы ячейки они должны каждый иметь другой идентификатор строки, при их создании и когда они запрашиваются из очереди повторного использования.

### <a name="cell-reuse-in-ios-6"></a>Повторное использование ячейки в iOS 6 +

iOS 6 добавлены узор для повторного использования ячейки, аналогично один Знакомство с представлениями коллекции. Несмотря на то, что существующие повторно использовать шаблон, показанный выше по-прежнему поддерживается для обратной совместимости, этот новый шаблон является предпочтительным, как он исключает необходимость применения проверки на значения null в ячейке.

С новым шаблоном приложение регистрирует класс ячейки или xib для использования с помощью вызова `RegisterClassForCellReuse` или `RegisterNibForCellReuse` в конструкторе контроллера. Затем, когда вывода ячейки в `GetCell` метод просто вызов `DequeueReusableCell` передавая код регистрации класс ячейки или xib и пути индекса.

Например следующий код регистрирует пользовательский класс ячейки в UITableViewController:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

С классом MyCell регистрации, ячейки могут быть выведенным из очереди в `GetCell` метод `UITableViewSource` не требуется проверка лишние значения null, как показано ниже:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Имейте в виду, при использовании пользовательский класс ячейки с новым шаблоном повторного использования, вам необходимо реализовать конструктор, принимающий `IntPtr`, как показано в приведенном ниже фрагменте кода, в противном случае Objective-C будет невозможно создать экземпляр класса ячейки:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Можно просмотреть примеры разделов, описанные выше в **BasicTable** образца, связанные с этой статьей.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Добавление индекса

Индекс помогает пользователю прокручивать длинных списках, обычно в алфавитном порядке несмотря на то, что можно индексировать условием независимо от необходимости. **BasicTableIndex** образец загружает гораздо более длинный список элементов из файла для демонстрации индекс. Каждый элемент в индексе соответствует «раздел» таблицы.

 [ ![](populating-a-table-with-data-images/image5.png "Отображение")](populating-a-table-with-data-images/image5.png)

Для поддержки разделы данных таблицы необходимо группировать, поэтому создает образец BasicTableIndex `Dictionary<>` из массива строк, используя первую букву каждого элемента в качестве ключа словаря:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

`UITableViewSource` Подкласс то требуется добавить или изменить, чтобы использовать следующие методы `Dictionary<>` :

-   **NumberOfSections** — этот метод является необязательным, по умолчанию в таблице предполагается один раздел. При отображении индекса этот метод должен возвращать число элементов в индексе (например, 26 Если индекс содержит все буквы английского алфавита).
-   **RowsInSection** — возвращает количество строк в данной секции.
-   **SectionIndexTitles** — возвращает массив строк, которые будут использоваться для отображения индекса. Образец кода возвращает массив букв.


Обновленные методы в образце файла **BasicTableIndex/TableSource.cs** выглядеть следующим образом:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Индексы обычно используются только с простой таблицы стилей.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Добавление верхних и нижних колонтитулов

Верхние и нижние колонтитулы могут использоваться для визуальной группировки строк в таблице. Необходимые структуры данных очень сходно с добавлением индекса — `Dictionary<>` работает очень хорошо. Вместо алфавита для группировки ячейки, в этом примере будет группировать овощей botanical типом.
Вывод выглядит следующим образом.

 [ ![](populating-a-table-with-data-images/image6.png "Образец верхние и нижние колонтитулы")](populating-a-table-with-data-images/image6.png)

Чтобы отобразить верхние и нижние колонтитулы `UITableViewSource` подкласс требует эти дополнительные методы:

-   **TitleForHeader** — возвращает текст для использования в качестве заголовка
-   **TitleForFooter** — возвращает текст, который используется в качестве нижнего колонтитула.


Обновленные методы в образце файла **BasicTableHeaderFooter/Code/TableSource.cs** выглядеть следующим образом:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Можно настроить внешний вид колонтитулы с представлением объекта, с помощью `GetViewForHeader` и `GetViewForFooter` метода, переопределяет `UITableViewSource`.


## <a name="related-links"></a>Связанные ссылки

- [WorkingWithTables (пример)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
