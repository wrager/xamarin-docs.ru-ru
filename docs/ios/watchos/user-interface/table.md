---
title: watchOS таблицы элементов управления в Xamarin
description: В этом документе описывается использование элементов управления watchOS таблицы в Xamarin. Он описывает таблицу, добавив контроллера строки, создание и заполнение строк, отвечать на нажатия и многое другое.
ms.prod: xamarin
ms.assetid: 7C14126D-9591-4387-A588-3C4521F11C55
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: afb8f9a96fa14877cbd0352869e23972719a4480
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791362"
---
# <a name="watchos-table-controls-in-xamarin"></a>watchOS таблицы элементов управления в Xamarin

WatchOS `WKInterfaceTable` управления гораздо проще, чем их аналоги операций ввода-вывода, но выполняет ту же роль. Он создает прокрутки список строк, может иметь пользовательские макеты и который реагировать на события касания.

![](table-images/table-list-sml.png "Список наблюдения за таблицы") ![](table-images/table-detail-sml.png)
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="adding-a-table"></a>Добавление таблицы

Перетащите **таблицы** управления сцены. По умолчанию будет выглядеть, как это (отображение макета одна неопределенная строка):

[![](table-images/add-table-sml.png "Добавление таблицы")](table-images/add-table.png#lightbox)

Присвойте имя таблицы в **свойства** панель **имя** поле, чтобы его можно ссылаться в коде.

## <a name="add-a-row-controller"></a>Добавить контроллер строк

Таблица автоматически включает одну строку, представленный контроллер строк, содержащий **группы** элемента управления по умолчанию.

Для задания **класса** контроллера строки, выберите строку в **Структура документа** и введите имя класса в **свойства** заполнения:

[![](table-images/add-row-controller-sml.png "Введите имя класса в области свойств")](table-images/add-row-controller.png#lightbox)

После задания класса для строки контроллера IDE создаст соответствующий файл C# в проекте. Перетащите элементы управления (например, метки) в строку и присвойте им имена, поэтому они могут ссылаться в коде.




## <a name="create-and-populate-rows"></a>Создание и заполнение строк

`SetNumberOfRows` создает классы контроллера строку для каждой строки, с помощью `Identifier` для выбора правильной. Если Вы дали контроллере строку пользовательского `Identifier`, изменить **по умолчанию** во фрагменте кода ниже с идентификатором, который вы использовали. `RowController` *Для каждой строки* создается, когда `SetNumberOfRows` вызывается и таблице.

```csharp
myTable.SetNumberOfRows ((nint)rows.Count, "default");
        // loads row controller by identifier
```

> [!IMPORTANT]
> Строки таблицы не виртуализованных как в iOS. Попробуйте ограничить число строк (Apple рекомендует меньше 20).

После создания строк необходимо для заполнения каждой ячейки (например `GetCell` в iOS). Этот фрагмент кода из [WatchTables пример](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/) обновления метки в каждой строке

```csharp
for (var i = 0; i < rows.Count; i++) {
    var elementRow = (RowController)myTable.GetRowController (i);
    elementRow.myRowLabel.SetText (rows [i]);
}
```

> [!IMPORTANT]
> С помощью `SetNumberOfRows` и затем циклически с помощью `GetRowController` вызывает всю таблицу для отправки в часы. На последующих представления таблицы, если вам нужно добавить или удалить отдельные строки используйте `InsertRowsAt` и `RemoveRowsAt` для повышения производительности.


## <a name="respond-to-taps"></a>Ответ на касания

Можно ответить на выбор строк двумя способами:

- Реализация `DidSelectRow` метода контроллера интерфейса, или
- Создание segue на раскадровку и внедрение `GetContextForSegue` Если выбор строк, чтобы открыть другой сцены.

### <a name="didselectrow"></a>DidSelectRow

Для программной обработки выбора строк, реализовать `DidSelectRow` метод. Чтобы открыть новый сцены, используйте `PushController` и передайте сцены идентификатор и контекст данных для использования:

```csharp
public override void DidSelectRow (WKInterfaceTable table, nint rowIndex)
{
    var rowData = rows [(int)rowIndex];
    Console.WriteLine ("Row selected:" + rowData);
    // if selection should open a new scene
    PushController ("secondInterface", rows[(int)rowIndex]);
}
```

### <a name="getcontextforsegue"></a>GetContextForSegue

Перетащите segue на раскадровку вашей строки таблицы в другой сцены (удерживая нажатой **управления** ключа во время перетаскивания).
Обязательно выберите segue и назначение ему идентификатора **свойства** pad (такие как `secondLevel` в приведенном ниже примере).

В контроллере интерфейса реализации `GetContextForSegue` метод и возврат контекст данных, должна быть реализована в сцену, представленный segue.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier, WKInterfaceTable table, nint rowIndex)
{
    if (segueIdentifier == "secondLevel") {
        return new NSString (rows[(int)rowIndex]);
    }
    return null;
}
```

Данные передаются в сцену цели storyboard в его `Awake` метод.

## <a name="multiple-row-types"></a>Несколько типов строк

По умолчанию элемента управления table имеет тип одной строки, можно создать. Чтобы добавить дополнительные строки «шаблоны» используют **строк** поле **свойства** прокладки, чтобы создать дополнительные контроллеры строки:

![](table-images/prototype-rows1.png "Установка числа строк прототипа")

Установка **строк** свойства **3** создаст заполнители дополнительная строка для перетаскивания элементов управления в. Для каждой строки, задайте **класса** в **свойства** прокладки, чтобы убедиться, создается класс контроллера строки.

![](table-images/prototype-rows2.png "Прототип строк в конструкторе")

Для заполнения таблицы с использованием типов в другую строку `SetRowTypes` метод, чтобы указать тип контроллера строк, который будет использоваться для каждой строки в таблице. Позволяет указать, какой контроллер строк следует использовать для каждой строки идентификаторов строк.

Число элементов в массиве должно совпадать с числом строк в таблице планируемого:

```csharp
myTable.SetRowTypes (new [] {"type1", "default", "default", "type2", "default"});
```

При заполнении таблицы с несколькими контроллерами строк, необходимо иметь для отслеживания типа нужно заполнить пользовательского интерфейса:

```csharp
for (var i = 0; i < rows.Count; i++) {
    if (i == 0) {
        var elementRow = (Type1RowController)myTable.GetRowController (i);
        // populate UI controls
    } else if (i == 3) {
        var elementRow = (Type2RowController)myTable.GetRowController (i);
        // populate UI controls
    } else {
        var elementRow = (DefaultRowController)myTable.GetRowController (i);
        // populate UI controls
    }
}
```


## <a name="vertical-detail-paging"></a>Вертикальная детализации разбиения на страницы

watchOS 3 появилась новая возможность для таблиц: возможность прокручивать страницы сведений о связанных с каждой строки без необходимости вернуться назад к таблице и выбрать другую строку. На экране сведений о можно прокручивать проведение пальцем по экрану вверх и вниз или с помощью цифровых Главная.

![](table-images/table-scroll-sml.png "Пример вертикальной подробное разбиение по страницам") ![](table-images/table-detail-sml.png)

> [!IMPORTANT]
> Эта функция доступна в настоящее время только путем редактирования раскадровки в построителе интерфейс Xcode.

Чтобы включить эту функцию, установите `WKInterfaceTable` на рабочую область конструирования и деления **вертикальной подробное разбиение по страницам** параметр:

![](table-images/vertical-detail-paging-sml.png "При выборе параметра вертикальной подробное разбиение по страницам")

Как [поясненные Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable#1682023) навигации таблицы необходимо использовать segues для работы функции разбиения на страницы. Перепишите существующий код, использующий `PushController` вместо этого использовать segues.

<a name="add_row_controller" />

## <a name="appendix-row-controller-code-example"></a>Приложение: Пример кода контроллера строки

При создании контроллера строк в конструкторе, интегрированной среды разработки автоматически создаст двух файлах кода. Ниже приведен код в эти файлы, созданные для ссылки.

Первая будет называться для класса, например **RowController.cs**, следующим образом:

```csharp
using System;
using Foundation;

namespace WatchTablesExtension
{
    public partial class RowController : NSObject
    {
        public RowController ()
        {
        }
    }
}
```

Другой **. designer.cs** файл является определение разделяемого класса, содержащий выходов и действия, которые создаются в рабочей области конструктора, как показано в примере с одним `WKInterfaceLabel` управления:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using UIKit;

namespace WatchTables.OnWatchExtension
{
    [Register ("RowController")]
    partial class RowController
    {
        [Outlet]
        [GeneratedCode ("iOS Designer", "1.0")]
        public WatchKit.WKInterfaceLabel MyLabel { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (MyLabel != null) {
                MyLabel.Dispose ();
                MyLabel = null;
            }
        }
    }
}
```

Выходов и указано здесь действия можно ссылаться в коде - Однако **. designer.cs** не следует непосредственно изменять файл.



## <a name="related-links"></a>Связанные ссылки

- [WatchTables (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Таблица документации Apple](https://developer.apple.com/reference/watchkit/wkinterfacetable)
