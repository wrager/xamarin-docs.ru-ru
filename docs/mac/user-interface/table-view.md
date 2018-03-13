---
title: "Представления таблиц"
description: "В этой статье рассматривается работа с представлениями таблицы в приложении Xamarin.Mac. Он описывает создание представлений таблиц в Xcode и интерфейс построителя и работать с ними в коде."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4764a4babc9f6b06c7a9299feab1320971b0bf75
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="table-views"></a>Представления таблиц

_В этой статье рассматривается работа с представлениями таблицы в приложении Xamarin.Mac. Он описывает создание представлений таблиц в Xcode и интерфейс построителя и работать с ними в коде._

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к той же таблицы представления, которые разработчик, работающий *Objective-C* и *Xcode* does. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ для создания и поддержания представления таблицы (или их при необходимости создать непосредственно в код C#).

Представление таблицы отображает данные в табличном формате, содержащий один или несколько столбцов данных в нескольких строках. В зависимости от типа создаваемого представления таблицы пользователя можно сортировать по столбцу, реорганизация столбцы, добавлять столбцы, удалить столбцы или изменить данные, содержащиеся в таблице.

[![](table-view-images/intro01.png "Пример таблицы")](table-view-images/intro01.png#lightbox)

В этой статье мы обсудим основы работы с представлениями таблицы в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Общие сведения о представлениях таблиц

Представление таблицы отображает данные в табличном формате, содержащий один или несколько столбцов данных в нескольких строках. Представления таблицы отображаются внутри представления прокрутки (`NSScrollView`) и начиная с macOS 10.7, можно использовать любой `NSView` вместо ячеек (`NSCell`) для отображения строк и столбцов. С другой стороны, вы можете продолжать использовать `NSCell` тем не менее, вы будете обычно подкласс `NSTableCellView` и создания пользовательских строк и столбцов.

Представление таблицы не сохраняет свой данных, вместо этого он основывается на источник данных (`NSTableViewDataSource`) для предоставления строк и столбцов, необходимых по мере необходимости.

Можно настроить поведение представление таблицы, предоставляя подкласс делегата представления таблицы (`NSTableViewDelegate`) для поддержки управления столбца таблицы, введите для выбора функции, выбор строк и редактирования, настраиваемое отслеживание и настраиваемых представлений для отдельных столбцов и строки.

При создании представления таблиц, Apple предлагает следующие:

* Позволить пользователю сортировать таблицу, щелкнув заголовок столбца.
* Создайте заголовки столбцов, которые существительных или коротких субстантивных словосочетаний, описывающие данные, отображаемые в этом столбце.

Дополнительные сведения см. в разделе [содержимого представления](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Создание и обслуживание представления таблиц в Xcode

При создании нового приложения Xamarin.Mac Cocoa, вы получаете стандартное окно пустым, по умолчанию. В данном окне определяется в `.storyboard` файл автоматически включен в проект. Чтобы изменить проект windows в **обозревателе решений**, дважды щелкните `Main.storyboard` файла:

[![](table-view-images/edit01.png "При выборе основных раскадровки")](table-view-images/edit01.png#lightbox)

Откроется окно конструктора в построителе Xcode элемента интерфейса:

[![](table-view-images/edit02.png "Изменение пользовательского интерфейса в Xcode")](table-view-images/edit02.png#lightbox)

Тип `table` в **библиотеки инспектора** поле поиска для облегчения поиска элементов управления, представление таблицы:

[![](table-view-images/edit03.png "При выборе таблицы представления из библиотеки")](table-view-images/edit03.png#lightbox)

Перетащите таблицы представления View-Controller в **интерфейс редактора**, сделать ее заполнения области содержимого View-Controller и присвойте ему значение где сжимается и увеличивается с окном **Редактор ограничений**:

[![](table-view-images/edit04.png "Изменение ограничений")](table-view-images/edit04.png#lightbox)

Выберите режим таблицы **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](table-view-images/edit05.png "Инспектор атрибута")](table-view-images/edit05.png#lightbox)

- **Режим содержимого** -можно использовать либо представления (`NSView`) или ячейки (`NSCell`) для отображения данных в строках и столбцах. Начиная с macOS 10.7, следует использовать представления.
- **Смещает группы строк** — Если `true`, представление таблицы будет рисования сгруппированные ячеек, как если бы они с плавающей запятой.
- **Столбцы** -определяет количество отображаемых столбцов.
- **Заголовки** — Если `true`, столбцы будет иметь заголовки.
- **Изменение порядка** — Если `true`, пользователь сможет перетаскивать изменения порядка столбцов в таблице.
- **Изменение размеров** — Если `true`, пользователь сможет перетаскивать заголовки столбцов, чтобы изменить размер столбцов.
- **Установка размеров столбцов** -определяет, каким образом таблица будет автоматически размер столбцов.
- **Выделите** -элементов управления использует тип выделения в таблице, если ячейка выбрана.
- **Альтернативные строки** — Если `true`, когда-либо другие строки будут иметь другой цвет фона.
- **Горизонтальную сетку** -выбирает тип границы, нарисованной между ячейками по горизонтали.
- **Вертикальную сетку** -выбирает тип границы, нарисованной по вертикали между ячейками.
- **Цвет сетки** -задает цвет границы ячейки.
- **Фон** -задает цвет фона ячеек.
- **Выбор** -позволяют контролировать, как пользователь может выбрать ячейки в таблице, что:
    - **Несколько** — Если `true`, пользователь может выбрать несколько строк и столбцов.
    - **Столбец** — Если `true`, пользователь может выбрать столбцы.
    - **Введите Select** — Если `true`, пользователь может ввести символ, чтобы выбрать строку.
    - **Пустой** — Если `true`, пользователю не требуется для выбора строк или столбцов, таблица позволяет для выбора не вообще.
- **Эта функция** -имя, которое имеет формат таблицы автоматически сохранять в папке.
- **Сведения о столбце** — Если `true`, порядок и ширины столбцов будут сохранены автоматически.
- **Разрывы строки** — укажите, каким ячейки обрабатывает разрывы строки.
- **Усекает последней видимой строки** — Если `true`, ячейки будут усечены, данные могут не помещаются в его границах.

> [!IMPORTANT]
> Если не обслуживание устаревшее приложение Xamarin.Mac, `NSView` на основе таблицы представления, которые должны использоваться по `NSCell` на основе представления таблицы. `NSCell` считается прежних версий и может не поддерживаться Забегая вперед.

Выберите столбец таблицы в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](table-view-images/edit06.png "Инспектор атрибута")](table-view-images/edit06.png#lightbox)

- **Заголовок** -задает заголовок столбца.
- **Выравнивание** -задать выравнивание текста внутри ячеек.
- **Название шрифта** -Выбор шрифта для текста заголовка ячейки.
- **Ключ сортировки** -ключ, используемый для сортировки данных в столбце. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Селектор** -— **действия** используется для выполнения сортировки. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Порядок** -используется порядок сортировки для столбцов данных.
- **Изменение размеров** -выбирает тип изменения размера столбца.
- **Для редактирования** — Если `true`, пользователь может изменять ячейки в таблице на основе ячейки.
- **Скрытые** — Если `true`, столбец будет скрыт.

Можно также изменить размер столбца, перетаскивая дескриптор (вертикально по центру правой стороны столбца) влево или вправо его.

Давайте выберите каждый столбец в наших таблиц, представлений и присвойте первый столбец **заголовок** из `Product` и второй `Details`.

Выберите представление ячейки таблицы (`NSTableViewCell`) в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](table-view-images/edit07.png "Инспектор атрибута")](table-view-images/edit07.png#lightbox)

Это все свойства стандартное представление. Имеется также возможность изменения размера строк для этого столбца здесь.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField`) в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](table-view-images/edit08.png "Инспектор атрибута")](table-view-images/edit08.png#lightbox)

Вы получите все свойства стандартного текстового поля, чтобы задать здесь. По умолчанию стандартный текстовое поле используется для отображения данных для ячейки в столбце.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](table-view-images/edit09.png "Инспектор атрибута")](table-view-images/edit09.png#lightbox)

Доступны параметры наиболее важных здесь.

- **Макет** — укажите, каким компонуется ячеек данного столбца.
- **Использует один линейный режим** — Если `true`, ячейки ограничено до одной строки.
- **Первый ширину макета среды выполнения** — Если `true`, ячейки, предпочтут ширину, задайте для него (вручную или автоматически) при его отображается при первом запуске приложения.
- **Действие** -контролирует, когда изменение **действия** отправляется для ячейки.
- **Поведение** -определяет, если ячейка находится и редактировать.
- **Форматированный текст** — Если `true`, ячейки могут отображать текст, отформатированный и со стилем.
- **Отменить** — Если `true`, ячейки берет на себя ответственность за его — отменить поведение.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в нижней части столбца таблицы в **иерархии интерфейса**:

[![](table-view-images/edit10.png "Выбор представления ячейки таблицы")](table-view-images/edit10.png#lightbox)

Это дает возможность изменить представление ячейки таблицы, используемый как базовый _шаблон_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Добавление действий и выходов

Просто как любые другие Cocoa элемент управления пользовательского интерфейса, нам нужно предоставлять нашей табличное представление и его столбцы и ячейки для C# кода с помощью **действия** и **торговцам** (с учетом функциональность, необходимую).

Процесс аналогичен для любого элемента таблицы представления, необходимо предоставить:

1. Переключитесь в **помощника редактор** и убедитесь, что `ViewController.h` выбран файл: 

    [![](table-view-images/edit11.png "Редактор помощника")](table-view-images/edit11.png#lightbox)
2. Выберите представление таблицы в **иерархии интерфейса**управления щелкните и перетащите `ViewController.h` файла.
3. Создание **розетки** таблицы представления, вызываемых `ProductTable`: 

    [![](table-view-images/edit13.png "Настройка выхода")](table-view-images/edit13.png#lightbox)
4. Создание **торговцам** таблицы столбцы, а также называется `ProductColumn` и `DetailsColumn`: 

    [![](table-view-images/edit14.png "Настройка выхода")](table-view-images/edit14.png#lightbox)
5. Сохраните вы изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Далее мы записываем отображения кода некоторые данные в таблице после запуска приложения.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Заполнение таблицы представления

С нашей таблицы представления, разработан в построителе интерфейс и через **розетки**, Далее нам нужно создать код для заполнения.

Во-первых давайте создадим новый `Product` класс для хранения информации для отдельных строк. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `Product` для **имя** и нажмите кнопку **New** кнопки:

[![](table-view-images/populate01.png "Создание пустой класс")](table-view-images/populate01.png#lightbox)

Сделать `Product.cs` внешний файл следующим образом:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}

```

Затем нужно создать подкласс `NSTableDataSource` для предоставления данных для нашей таблице по запросу. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `ProductTableDataSource` для **имя** и нажмите кнопку **New** кнопки.

Изменить `ProductTableDataSource.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Этот класс имеет места для хранения элементов нашей табличное представление и переопределяет `GetRowCount` для возврата количества строк в таблице.

Наконец, нужно создать подкласс `NSTableDelegate` для обеспечения поведения нашей таблице. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `ProductTableDelegate` для **имя** и нажмите кнопку **New** кнопки.

Изменить `ProductTableDelegate.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Когда создается экземпляр `ProductTableDelegate`, мы также передать экземпляр `ProductTableDataSource` , предоставляющая данные для таблицы. `GetViewForItem` Метод отвечает за возвращение представления (данных) для отображения в ячейку столбца и строки. Если это возможно для отображения ячейки будут повторно использовать существующее представление, если не должно быть создано новое представление.

Чтобы вставить в таблицу, изменим `ViewController.cs` и создайте `AwakeFromNib` внешний метод следующим образом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Если мы запустим приложение, отображаются следующие сведения:

[![](table-view-images/populate02.png "Запустите образец приложения")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте позволить пользователю сортировать данные в таблице, щелкнув заголовок столбца. Во-первых, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите `Product` столбца, введите `Title` для **ключа сортировки**, `compare:` для **селектор** и выберите `Ascending` для **порядок**:

[![](table-view-images/sort01.png "Задание ключа сортировки")](table-view-images/sort01.png#lightbox)

Выберите `Details` столбца, введите `Description` для **ключа сортировки**, `compare:` для **селектор** и выберите `Ascending` для **порядок**:

[![](table-view-images/sort02.png "Задание ключа сортировки")](table-view-images/sort02.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDataSource.cs` файл и добавьте следующие методы:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

`Sort` Метод позволяют сортировать данные в источнике данных, на основе заданного `Product` поля класса в возрастающем или убывающем порядке. Переопределенный `SortDescriptorsChanged` метод будет вызываться каждый раз щелкает использование заголовков столбцов. Он будет передан **ключ** значение, которое задается в построителе интерфейс и порядок сортировки для этого столбца.

Если запустить приложение и щелкните заголовок столбца, строки будут отсортированы по этому столбцу:

[![](table-view-images/sort03.png "Запустите пример приложения")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбрать одну строку, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим таблицы **иерархии интерфейса** и снимите флажок **несколько** флажок в **инспектора атрибут**:

[![](table-view-images/select01.png "Инспектор атрибута")](table-view-images/select01.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.


Далее следует изменить `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Это позволит пользователю выбирать любой отдельной строки в представлении. Вернуть `false` для `ShouldSelectRow` для любой строке, вы не хотите иметь возможность выбрать пользователя или `false` для каждой строки, если не нужно, чтобы пользователь имел возможность выбрать все строки.

Представление таблицы (`NSTableView`) содержит следующие методы для работы с Выбор строк:

- `DeselectRow(nint)` -Отменяет выделение заданной строки в таблице.
- `SelectRow(nint,bool)` -Выбор данной строки. Передайте `false` для второго параметра, чтобы одновременно выбрать только одну строку.
- `SelectedRow` — Возвращает текущую строку, выбранного в таблице.
- `IsRowSelected(nint)` — Возвращает `true` при выборе данной строки.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим таблицы **иерархии интерфейса** и проверьте **несколько** флажок в **инспектора атрибут**:

[![](table-view-images/select02.png "Инспектор атрибута")](table-view-images/select02.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.


Далее следует изменить `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Это позволит пользователю выбирать любой отдельной строки в представлении. Вернуть `false` для `ShouldSelectRow` для любой строке, вы не хотите иметь возможность выбрать пользователя или `false` для каждой строки, если не нужно, чтобы пользователь имел возможность выбрать все строки.

Представление таблицы (`NSTableView`) содержит следующие методы для работы с Выбор строк:

- `DeselectAll(NSObject)` -Отменяет выбор всех строк в таблице. Используйте `this` для первого параметра для отправки в объект, выполняющий выбора. 
- `DeselectRow(nint)` -Отменяет выделение заданной строки в таблице.
- `SelectAll(NSobject)` -Выбирает все строки в таблице. Используйте `this` для первого параметра для отправки в объект, выполняющий выбора.
- `SelectRow(nint,bool)` -Выбор данной строки. Передайте `false` для второго параметра снимите флажок и выбрать только одну строку, передайте `true` к выделению и включить эту строку.
- `SelectRows(NSIndexSet,bool)` -Выбор данного набора строк. Передайте `false` для второго параметра снимите флажок и выбрать только следующие строки, передайте `true` к выделению и включить эти строки.
- `SelectedRow` — Возвращает текущую строку, выбранного в таблице.
- `SelectedRows` — Возвращает `NSIndexSet` содержащую индексы выбранных строк.
- `SelectedRowCount` — Возвращает число выбранных строк.
- `IsRowSelected(nint)` — Возвращает `true` при выборе данной строки.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Для выбора строк

Если вы хотите разрешить пользователю ввести символ с выбранного представления таблицы и выберите первую строку с этим символом, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим таблицы **иерархии интерфейса** и проверьте **выберите тип** флажок в **инспектора атрибут**:

[![](table-view-images/type01.png "Тип выбора параметра")](table-view-images/type01.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

`GetNextTypeSelectMatch` Принимает заданного `searchString` и возвращает строку первого `Product` с этой строки в его `Title`.

Если запустить приложение и введите символ, строка выбирается:

[![](table-view-images/type02.png "Запустите образец приложения")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетащить изменения порядка столбцов в представлении, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим таблицы **иерархии интерфейса** и проверьте **переупорядочивание** флажок в **инспектора атрибут**:

[![](table-view-images/reorder01.png "Инспектор атрибута")](table-view-images/reorder01.png#lightbox)

Если мы ценны для **Автосохранение** свойства и проверить **сведения о столбце** поле, любые изменения, мы предоставим макет таблицы будут автоматически сохранены для нас и восстановления приложения следующий раз При запуске.

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Метод должен возвращать `true` для любого столбца, его необходимо разрешить для перетаскивания переупорядочить в `newColumnIndex`, в противном случае возвращаемое `false`;

Если мы запустим приложение, можно перетащить заголовки столбцов вокруг переупорядочить столбцы:

[![](table-view-images/reorder02.png "Пример упорядоченный столбцов")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Редактировать ячейки

Если вы хотите разрешить пользователю изменять значения для данной ячейки, изменять `ProductTableDelegate.cs` и измените `GetViewForItem` метод следующим образом:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Теперь если мы запустим приложение, пользователь может изменять ячейки в табличном представлении:

[![](table-view-images/editing01.png "Пример изменения ячейки")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Использование изображений в табличном представлении

Включить изображение в рамках ячейки в `NSTableView`, вам потребуется изменить как данные возвращаются в табличном представлении `NSTableViewDelegate's` `GetViewForItem` метод, используемый `NSTableCellView` вместо стандартное `NSTextField`. Пример:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Дополнительные сведения см. в разделе [изображения с помощью представления таблиц](~/mac/app-fundamentals/image.md) часть наших [работе с изображением](~/mac/app-fundamentals/image.md) документации.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Добавление кнопку «Удалить» в строку

В зависимости от требований приложения некоторых ситуациях может потребоваться где необходимо ввести кнопка действия для каждой строки в таблице. В качестве примера данного объекта, давайте разверните примере представление таблицы, созданной ранее, чтобы включить **удалить** кнопку на каждой строке.

Во-первых, изменить `Main.storyboard` в построителе интерфейс Xcode выберите представление таблицы и увеличить число столбцов для трех (3). Затем измените **заголовок** нового столбца к `Action`:

[![](table-view-images/delete01.png "Изменение имени столбца")](table-view-images/delete01.png#lightbox)

Сохранить изменения в раскадровку и вернуться в Visual Studio для Mac синхронизировать изменения.

Далее следует изменить `ViewController.cs` и добавьте приведенный ниже открытый метод:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

В том же файле изменения создания нового делегата представление таблицы внутри `ViewDidLoad` метод следующим образом:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Теперь измените `ProductTableDelegate.cs` файл для включения частного подключения к контроллеру представление и перевод контроллера как параметр при создании нового экземпляра делегата:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

Добавьте следующий новый частный метод к классу.

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Это занимает все представление текста конфигурации, которые были ранее, выполняемая в `GetViewForItem` метод и помещает их в одном месте вызываемой (с момента последнего столбца таблицы не содержит представления текста, но кнопка).

Наконец, измените `GetViewForItem` метод и сделать его выглядеть следующим образом:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btw = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Давайте взглянем на несколько разделов этот код более подробно. Во-первых, если новый `NSTableViewCell` — создаваемого действия берется на основе имени столбца. Для первых двух столбцов (**продукта** и **сведения**), новый `ConfigureTextField` вызывается метод.

Для **действия** столбец, новый `NSButton` создается и добавляется в ячейку как Sub представление:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Кнопки `Tag` свойство используется для хранения числа строк, который обрабатывается в данный момент. Это число будет использоваться позже при запросе к строке, чтобы удалить кнопку `Activated` событий:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

Во время запуска обработчика событий мы получаем, кнопки и продукт, который находится в строке данной таблицы. Затем для пользователя подтверждения удаления строки выводится предупреждение. По желанию пользователя удалить строку, данная строка удаляется из источника данных и таблицы будут обновляться:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Наконец Если используется повторно вместо создания нового представления ячейки таблицы, следующий код настраивает его на основе столбца обрабатывается:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Для **действия** столбца, пока не просматриваются все представления Sub `NSButton` найден, то `Tag` свойство обновляется, чтобы они указывали на текущей строке.

Эти изменения на месте при запуске приложения каждая строка будет иметь **удалить** кнопки:

[![](table-view-images/delete02.png "В таблице, представлении с кнопками, удаление")](table-view-images/delete02.png#lightbox)

Когда пользователь щелкает **удалить** кнопки, предупреждение будет отображаться запросом на удаление данной строки:

[![](table-view-images/delete03.png "Создает предупреждение удаления строк")](table-view-images/delete03.png#lightbox)

Если пользователь решит delete, строка будет удалена и перерисовывается таблицы:

[![](table-view-images/delete04.png "После удаления строки таблицы")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Представления таблиц привязки данных

С помощью методов кодирования ключ-значение и привязка данных в приложении Xamarin.Mac, можно значительно уменьшить объем кода, который необходимо написать и хранить для заполнения и работать с элементами пользовательского интерфейса. Имеется также преимущество дальнейшего разделения данных резервное копирование (_модели данных_) вашего спереди завершения пользовательского интерфейса (_Model-View-Controller_), что приведет к проще поддерживать более гибкие приложения конструктор.

Ключ-значение кодирования (KVC) — это механизм для доступа к свойствам объекта неявно, с помощью ключей (специальный формат строки) для определения свойства вместо обращения к ним через переменные экземпляра или методы доступа (`get/set`). Путем реализации кодирования ключ-значение соответствует методов доступа в приложении Xamarin.Mac, можно получить доступ с другими функциями macOS рассмотрение ключ-значение (KVO), привязка данных, основные данные, Cocoa привязок и применимость сценариев.

Дополнительные сведения см. в разделе [привязка данных представления таблицы](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) часть наших [привязки данных и кодирование ключ-значение](~/mac/app-fundamentals/databinding.md) документации.

<a name="Summary" />

## <a name="summary"></a>Сводка

Подробное описание работы с представлениями таблицы в приложении Xamarin.Mac вступила в этой статье. Мы уже видели различных типов и использует таблицы представления, как создать и поддерживать представления таблицы в построителе интерфейс Xcode и способы работы с представлениями таблицы в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [MacTables (пример)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (пример)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления структур](~/mac/user-interface/outline-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
