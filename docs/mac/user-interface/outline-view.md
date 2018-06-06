---
title: Представления структуры в Xamarin.Mac
description: В этой статье рассматривается работа с представления структуры в приложении Xamarin.Mac. Он описывает создание и поддержание представления структуры в Xcode и интерфейс построителя и работа с ними программным способом.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a12eee5f473ffdc6a235faeb55c0a3d6754f4629
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792831"
---
# <a name="outline-views-in-xamarinmac"></a>Представления структуры в Xamarin.Mac

_В этой статье рассматривается работа с представления структуры в приложении Xamarin.Mac. Он описывает создание и поддержание представления структуры в Xcode и интерфейс построителя и работа с ними программным способом._

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к той же структуры представления, которые разработчик, работающий *Objective-C* и *Xcode* does. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ для создания и поддержания представления структуры (или их при необходимости создать непосредственно в код C#).

Структура — это тип таблицы, в котором пользователь развернуть или свернуть строки иерархических данных. Как представление таблицы структура отображаются данные для набора связанных элементов, строк, представляющих отдельных элементов и столбцами, представляющими атрибуты этих элементов. В отличие от таблицы представления элементы в представлении структуры не в виде плоского списка, они организованы в иерархии, такие как файлы и папки на жестком диске.

[![](outline-view-images/populate03.png "Запустите пример приложения")](outline-view-images/populate03.png#lightbox)

В этой статье мы обсудим основы работы с представлениями структуры в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Введение в режиме структуры

Структура — это тип таблицы, в котором пользователь развернуть или свернуть строки иерархических данных. Как представление таблицы структура отображаются данные для набора связанных элементов, строк, представляющих отдельных элементов и столбцами, представляющими атрибуты этих элементов. В отличие от таблицы представления элементы в представлении структуры не в виде плоского списка, они организованы в иерархии, такие как файлы и папки на жестком диске.

Если элемент в представлении структуры содержит другие элементы, его можно разворачивать и сворачивать пользователем. Расширяемый элемент отображает треугольник, указывающая вправо, когда элемент свернут и указывает вниз при развертывании элемента. Щелкните треугольник в результате элемент можно разворачивать и сворачивать.

Представление структуры (`NSOutlineView`) является подклассом представления таблицы (`NSTableView`) и таким образом, большую часть своего поведения наследуется от родительского класса. В результате многие операции, поддерживаемые представление таблицы, как выбор строк или столбцов, изменения положения столбцов, перетаскивая заголовки столбцов, т. д., также поддерживаются в режиме структуры. Приложение Xamarin.Mac имеет управлять этими функциями и можно настроить представление структуры параметров (в коде или в интерфейс построителя), чтобы разрешать или запрещать определенные операции.

Структура не сохраняет свой данных, вместо этого он основывается на источник данных (`NSOutlineViewDataSource`) для предоставления строк и столбцов, необходимых по мере необходимости.

Можно настроить поведение в режиме структуры, предоставляя подкласс делегата представление структуры (`NSOutlineViewDelegate`) для поддержки управления столбца структуры, введите для выбора функции, выбор строк и редактирования, настраиваемое отслеживание и настраиваемых представлений для отдельных столбцы и строки.

Так как структура реализованы многие аспекты его поведения и функциональных возможностей с представлением таблицы, можно пройти через наших [представления таблиц](~/mac/user-interface/table-view.md) документации, прежде чем переходить к этой статье.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Создание и обслуживание представления структуры в Xcode

При создании нового приложения Xamarin.Mac Cocoa, вы получаете стандартное окно пустым, по умолчанию. В данном окне определяется в `.storyboard` файл автоматически включен в проект. Чтобы изменить проект windows в **обозревателе решений**, дважды щелкните `Main.storyboard` файла:

[![](outline-view-images/edit01.png "При выборе основных раскадровки")](outline-view-images/edit01.png#lightbox)

Откроется окно конструктора в построителе Xcode элемента интерфейса:

[![](outline-view-images/edit02.png "Изменение пользовательского интерфейса в Xcode")](outline-view-images/edit02.png#lightbox)

Тип `outline` в **библиотеки инспектора** поле поиска для облегчения поиска представление структуры элементов управления:

[![](outline-view-images/edit03.png "При выборе структура из библиотеки")](outline-view-images/edit03.png#lightbox)

Перетащите представление-контроллер в режиме структуры **интерфейс редактора**, сделать ее заполнения области содержимого View-Controller и присвойте ему значение где сжимается и увеличивается с окном **Редактор ограничений**:

[![](outline-view-images/edit04.png "Изменение ограничений")](outline-view-images/edit04.png#lightbox)

Выберите режим структуры **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](outline-view-images/edit05.png "Инспектор атрибута")](outline-view-images/edit05.png#lightbox)

- **Столбец структуры** -столбец таблицы, в котором отображаются иерархические данные.
- **Столбец структуры Автосохранение** — Если `true`, столбец структуры будет автоматически сохраняется и восстанавливается между запуском приложения.
- **Отступ** -значение отступа столбцы в развернутый элемент.
- **Отступ ячеек соответствует** — Если `true`, отступ метки будут иметь отступ вместе с ячейки.
- **Автоматически сохранять элементы развернут** — Если `true`, состояние свернутого элементы будут автоматически сохраняется и восстанавливается между запуском приложения.
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
> Если не обслуживание устаревшее приложение Xamarin.Mac, `NSView` представления на основе структуры должны использоваться через `NSCell` на основе представления таблицы. `NSCell` считается прежних версий и может не поддерживаться Забегая вперед.

Выберите столбец таблицы в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](outline-view-images/edit06.png "Инспектор атрибута")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "Инспектор атрибута")](outline-view-images/edit07.png#lightbox)

Это все свойства стандартное представление. Имеется также возможность изменения размера строк для этого столбца здесь.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField`) в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](outline-view-images/edit08.png "Инспектор атрибута")](outline-view-images/edit08.png#lightbox)

Вы получите все свойства стандартных текстовое поле для задания здесь. По умолчанию стандартный текстовое поле используется для отображения данных для ячейки в столбце.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в **иерархии интерфейса** и следующие свойства доступны в **инспектора атрибут**:

[![](outline-view-images/edit09.png "Инспектор атрибута")](outline-view-images/edit09.png#lightbox)

Доступны параметры наиболее важных здесь.

- **Макет** — укажите, каким компонуется ячеек данного столбца.
- **Использует один линейный режим** — Если `true`, ячейки ограничено до одной строки.
- **Первый ширину макета среды выполнения** — Если `true`, ячейки, предпочтут ширину, задайте для него (вручную или автоматически) при его отображается при первом запуске приложения.
- **Действие** -контролирует, когда изменение **действия** отправляется для ячейки.
- **Поведение** -определяет, если ячейка находится и редактировать.
- **Форматированный текст** — Если `true`, ячейки могут отображать текст, отформатированный и со стилем.
- **Отменить** — Если `true`, ячейки берет на себя ответственность за его — отменить поведение.

Выберите представление ячейки таблицы (`NSTableFieldCell`) в нижней части столбца таблицы в **иерархии интерфейса**:

[![](outline-view-images/edit11.png "Выбор представления ячейки таблицы")](outline-view-images/edit10.png#lightbox)

Это дает возможность изменить представление ячейки таблицы, используемый как базовый _шаблон_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Добавление действий и выходов

Просто как любые другие Cocoa элемент управления пользовательского интерфейса, нам нужно предоставлять нашей представление структуры и его столбцы и ячейки для C# кода с помощью **действия** и **торговцам** (с учетом функциональность, необходимую).

Процесс аналогичен для любого элемента режим структуры, необходимо предоставить:

1. Переключитесь в **помощника редактор** и убедитесь, что `ViewController.h` выбран файл: 

    [![](outline-view-images/edit11.png "Выбор правильного h-файл")](outline-view-images/edit11.png#lightbox)
2. Выберите представление структуры в **иерархии интерфейса**управления щелкните и перетащите `ViewController.h` файла.
3. Создание **розетки** представление структуры, вызываемых `ProductOutline`: 

    [![](outline-view-images/edit13.png "Настройка выхода")](outline-view-images/edit13.png#lightbox)
4. Создание **торговцам** таблицы столбцы, а также называется `ProductColumn` и `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Настройка выхода")](outline-view-images/edit14.png#lightbox)
5. Сохраните вы изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Далее мы записываем кода отображения некоторых данных для структуры при запуске приложения.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Заполнение представление структуры

Наш режим структуры, созданной в интерфейс построителя и доступными через **розетки**, Далее нам нужно создать код для заполнения.

Во-первых давайте создадим новый `Product` класс для хранения информации для отдельных строк и группы продуктов sub. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `Product` для **имя** и нажмите кнопку **New** кнопки:

[![](outline-view-images/populate01.png "Создание пустой класс")](outline-view-images/populate01.png#lightbox)

Сделать `Product.cs` внешний файл следующим образом:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

Затем нужно создать подкласс `NSOutlineDataSource` для предоставления данных для наших структуры по запросу. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `ProductOutlineDataSource` для **имя** и нажмите кнопку **New** кнопки.

Изменить `ProductTableDataSource.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }
                
        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }
        
        }
        #endregion
    }
}
```

Этот класс имеет хранилище для наших представление структуры элементов и переопределяет `GetChildrenCount` для возврата количества строк в таблице. `GetChild` Возвращает заданного родительского или дочернего элемента (по запросу представление структуры) и `ItemExpandable` определяет указанный элемент как родительский или дочерний элемент.

Наконец, нужно создать подкласс `NSOutlineDelegate` для обеспечения поведения нашей структуры. В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **добавить** > **новый файл...** Выберите **Общие** > **пустым классом**, введите `ProductOutlineDelegate` для **имя** и нажмите кнопку **New** кнопки.

Изменить `ProductOutlineDelegate.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Когда создается экземпляр `ProductOutlineDelegate`, мы также передать экземпляр `ProductOutlineDataSource` , предоставляющая данные для структуры. `GetView` Метод отвечает за возвращение представления (данных) для отображения в ячейку столбца и строки. Если это возможно для отображения ячейки будут повторно использовать существующее представление, если не должно быть создано новое представление.

Чтобы заполнить структуру, изменим `MainWindow.cs` и создайте `AwakeFromNib` внешний метод следующим образом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Если мы запустим приложение, отображаются следующие сведения:

[![](outline-view-images/populate02.png "Свернутое представление")](outline-view-images/populate02.png#lightbox)

Если мы развернуть узел в представлении структуры, он будет выглядеть следующим образом:

[![](outline-view-images/populate03.png "Расширенное представление")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте позволить пользователю сортировать данные в структуре, щелкнув заголовок столбца. Во-первых, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите `Product` столбца, введите `Title` для **ключа сортировки**, `compare:` для **селектор** и выберите `Ascending` для **порядок**:

[![](outline-view-images/sort01.png "Установка ключа порядка сортировки")](outline-view-images/sort01.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDataSource.cs` файл и добавьте следующие методы:

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

`Sort` Метод позволяют сортировать данные в источнике данных, на основе заданного `Product` поля класса в возрастающем или убывающем порядке. Переопределенный `SortDescriptorsChanged` метод будет вызываться каждый раз щелкает использование заголовков столбцов. Он будет передан **ключ** значение, которое задается в построителе интерфейс и порядок сортировки для этого столбца.

Если запустить приложение и щелкните заголовок столбца, строки будут отсортированы по этому столбцу:

[![](outline-view-images/sort02.png "Пример отсортированных выходных данных")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбрать одну строку, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим структуры **иерархии интерфейса** и снимите флажок **несколько** флажок в **инспектора атрибут**:

[![](outline-view-images/select01.png "Инспектор атрибута")](outline-view-images/select01.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.


Далее следует изменить `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбирать любой отдельной строки в режиме структуры. Вернуть `false` для `ShouldSelectItem` для хотя бы один элемент, который необходимо исключить из пользователя, чтобы иметь возможность выбрать или `false` для каждого элемента, если не нужно, чтобы пользователь имел возможность выбрать какие-либо элементы.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим структуры **иерархии интерфейса** и проверьте **несколько** флажок в **инспектора атрибут**:

[![](outline-view-images/select02.png "Инспектор атрибута")](outline-view-images/select02.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.


Далее следует изменить `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Это позволит пользователю выбирать любой отдельной строки в режиме структуры. Вернуть `false` для `ShouldSelectRow` для хотя бы один элемент, который необходимо исключить из пользователя, чтобы иметь возможность выбрать или `false` для каждого элемента, если не нужно, чтобы пользователь имел возможность выбрать какие-либо элементы.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Для выбора строк

Если вы хотите разрешить пользователю ввести символ с выбранной представление структуры и выберите первую строку с этим символом, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим структуры **иерархии интерфейса** и проверьте **выберите тип** флажок в **инспектора атрибут**:

[![](outline-view-images/type01.png "Изменение типа строки")](outline-view-images/type01.png#lightbox)

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

`GetNextTypeSelectMatch` Принимает заданного `searchString` и возвращает первый элемент `Product` с этой строки в его `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетащить изменить порядок столбцов в представлении структуры, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в построителе интерфейса. Выберите режим структуры **иерархии интерфейса** и проверьте **переупорядочивание** флажок в **инспектора атрибут**:

[![](outline-view-images/reorder01.png "Инспектор атрибута")](outline-view-images/reorder01.png#lightbox)

Если мы ценны для **Автосохранение** свойства и проверить **сведения о столбце** поле, любые изменения, мы предоставим макет таблицы будут автоматически сохранены для нас и восстановления приложения следующий раз При запуске.

Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductOutlineDelegate.cs` и добавьте следующий метод:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

`ShouldReorder` Метод должен возвращать `true` для любого столбца, его необходимо разрешить для перетаскивания переупорядочить в `newColumnIndex`, в противном случае возвращаемое `false`;

Если мы запустим приложение, можно перетащить заголовки столбцов вокруг переупорядочить столбцы:

[![](outline-view-images/reorder02.png "Пример изменения порядка столбцов")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Редактировать ячейки

Если вы хотите разрешить пользователю изменять значения для данной ячейки, изменять `ProductOutlineDelegate.cs` и измените `GetViewForItem` метод следующим образом:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Теперь если мы запустим приложение, пользователь может изменять ячейки в табличном представлении:

[![](outline-view-images/editing01.png "Пример изменения ячейки")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Использование изображений в представления структуры

Включить изображение в рамках ячейки в `NSOutlineView`, вам потребуется изменить как данные возвращаются в представлении структуры `NSTableViewDelegate's` `GetView` метод, используемый `NSTableCellView` вместо стандартное `NSTextField`. Пример:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Дополнительные сведения см. в разделе [изображения с помощью представления структуры](~/mac/app-fundamentals/image.md) часть наших [работе с изображением](~/mac/app-fundamentals/image.md) документации.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Представления структуры привязки данных

С помощью методов кодирования ключ-значение и привязка данных в приложении Xamarin.Mac, можно значительно уменьшить объем кода, который необходимо написать и хранить для заполнения и работать с элементами пользовательского интерфейса. Имеется также преимущество дальнейшего разделения данных резервное копирование (_модели данных_) вашего спереди завершения пользовательского интерфейса (_Model-View-Controller_), что приведет к проще поддерживать более гибкие приложения конструктор.

Ключ-значение кодирования (KVC) — это механизм для доступа к свойствам объекта неявно, с помощью ключей (специальный формат строки) для определения свойства вместо обращения к ним через переменные экземпляра или методы доступа (`get/set`). Путем реализации кодирования ключ-значение соответствует методов доступа в приложении Xamarin.Mac, можно получить доступ с другими функциями macOS рассмотрение ключ-значение (KVO), привязка данных, основные данные, Cocoa привязок и применимость сценариев.

Дополнительные сведения см. в разделе [привязку данных представление структуры](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) часть наших [привязки данных и кодирование ключ-значение](~/mac/app-fundamentals/databinding.md) документации.

<a name="Summary" />

## <a name="summary"></a>Сводка

Подробное описание работы с представлениями структуры в приложении Xamarin.Mac вступила в этой статье. Мы уже видели различных типов и используется для представления структуры, как создавать и поддерживать представления структуры в построителе интерфейса в Xcode и способы работы с представления структуры в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [MacOutlines (пример)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (пример)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в режиме структуры](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
