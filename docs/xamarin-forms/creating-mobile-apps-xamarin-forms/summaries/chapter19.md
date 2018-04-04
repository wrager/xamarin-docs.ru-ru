---
title: Сводка Глава 19. Представления коллекций
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 0AEC3A5C-586E-4D0F-9895-67E99A053A79
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 8aabf469f877fdb3ec703c2e3fdab13fcb25975a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-19-collection-views"></a>Сводка Глава 19. Представления коллекций

Xamarin.Forms определяет три представления, которые обслуживают коллекции и отобразить их элементы:

- [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) Представляет относительно короткий список элементов строковых, пользователь может выбрать один
- [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Чаще всего длинный список элементов, обычно того же типа и форматирования, а также позволяет пользователю выбрать один
- [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) — это совокупность *ячеек* (обычно из различных типов и внешний вид) для отображения данных или управления ввод данных пользователем

Обычно для использования приложениями MVVM `ListView` для отображения можно выбрать коллекцию объектов.

## <a name="program-options-with-picker"></a>Параметры программы с помощью средства выбора

[ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) — Хороший выбор, когда необходимо разрешить пользователю выбор из списка короткое `string` элементов.

### <a name="the-picker-and-event-handling"></a>Выбор и обработка событий

[ **PickerDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerDemo) образце показано, как использовать XAML для установки `Picker` [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) свойство и добавьте `string` элементы [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) коллекции. Когда пользователь выбирает `Picker`, она отображает элементы в `Items` коллекции в виде зависят от платформы.

[ `SelectedIndexChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Picker.SelectedIndexChanged/) Событие указывает, когда пользователь выбрал элемент. Отсчитываемый от нуля [ `SelectedIndex` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.SelectedIndex/) затем указывает выбранный элемент. Если элемент не выбран, `SelectedIndex` равняется &ndash;1.

Можно также использовать `SelectedIndex` для инициализации выбранного элемента, но он должен быть установлен после `Items` заполнения коллекции. В языке XAML, это означает, что вы воспользуетесь элемент свойства для задания `SelectedIndex`.

### <a name="data-binding-the-picker"></a>Выбор привязки данных

`SelectedIndex` Резервируется привязываемые свойства, но `Items` — нет, поэтому использование привязки данных с `Picker` сложно. Одним из решений является использование `Picker` в сочетании с [ `ObjectToIndexConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ObjectToIndexConverter.cs) подобное показанному в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки. [ **PickerBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/PickerBinding) показано, как это работает.

## <a name="rendering-data-with-listview"></a>Подготовка к просмотру данных с помощью ListView

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Единственный класс, производный от [ `ItemsView<TVisual>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) от которого он наследуется [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) и [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) свойства.

`ItemsSource` относится к типу `IEnumerable` , но он является `null` по умолчанию и необходимо инициализировать явным образом или (обычно) в коллекцию через привязку данных. Элементы в этой коллекции может быть любого типа.

`ListView` Определяет [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/) , либо свойство одного из элементов в `ItemsSource` коллекции или `null` , если элемент не выбран. `ListView` активируется [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) событие, когда элемент выбран.

### <a name="collections-and-selections"></a>Коллекции и выбранные параметры

[ **ListViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewList) образца заливки `ListView` с 17 `Color` значения в `List<Color>` коллекции. Элементы, могут быть выделены, но по умолчанию они отображаются с их все `ToString` представления. Несколько примеров в этой главе показано, как исправить, которые отображают и сделать его полезным, в случае необходимости.

### <a name="the-row-separator"></a>Разделитель строк

В iOS и Android отображает тонкой линии отделяет строки. Можно управлять с [ `SeparatorVisibiliy` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorVisibility/) и [ `SeparatorColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SeparatorColor/) свойства. `SeparatorVisibility` свойство имеет тип [ `SeparatorVisbility` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SeparatorVisibility/), перечисление с двумя членами:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.Default/), значение по умолчанию
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.SeparatorVisibility.None/)

### <a name="data-binding-the-selected-item"></a>Выбранный элемент привязки данных

`SelectedItem` Резервируется привязываемые свойства, поэтому он может быть источника или целевого объекта привязки данных. Значение по умолчанию `BindingMode` — `OneWayToSource`, но обычно он является целевым объектом двухстороннюю привязку данных, особенно в сценариях MVVM. [ **ListViewArray** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewArray) образец демонстрирует этот тип привязки.

### <a name="the-observablecollection-difference"></a>Разница ObservableCollection

[ **ListViewLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewLogger) образцы наборов `ItemsSource` свойство `ListView` для `List<DateTime>` коллекции и затем последовательно добавляет новый `DateTime` в коллекцию Каждый второй использовать таймер.

Тем не менее `ListView` не обновляется автоматически сам поскольку `List<T>` коллекции нет механизм уведомлений для указания при добавлен или удален из коллекции элементов.

Гораздо лучше классом, который используется в таких сценариях — [ `ObservableCollection<T>` ](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) определенные в `System.Collections.ObjectModel` пространства имен. Этот класс реализует [ `INotifyCollectionChanged` ](https://developer.xamarin.com/api/type/System.Collections.Specialized.INotifyCollectionChanged/) интерфейса и, следовательно, активируется [ `CollectionChanged` ](https://developer.xamarin.com/api/event/System.Collections.ObjectModel.ObservableCollection%3CT%3E.CollectionChanged/) событие, когда элементы добавлены или удалены из коллекции или при их замене или перемещен в пределах Коллекция. Когда `ListView` внутренне обнаруживает, что класс, реализующий `INotifyCollectionChanged` ему было присвоено его `ItemsSource` свойство, он присоединяет обработчик для `CollectionChanged` событий и обновляет его отображение при изменении коллекции.

[ **ObservableLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ObservableLogger) образце показано использование `ObservableCollection`.

### <a name="templates-and-cells"></a>Шаблоны и ячеек

По умолчанию `ListView` отображаются элементы в коллекции с помощью каждого элемента `ToString` метод. Оптимальный подход заключается в определении шаблона для отображения элементов.

Чтобы поэкспериментировать с помощью этой функции можно использовать [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки. Этот класс определяет статическое `All` свойство типа `IList<NamedColor>` , содержащий 141 `NamedColor` объекты, соответствующие открытые поля `Color` структуры.

[ **NaiveNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/NaiveNamedColorList) образцы наборов `ItemsSource` из `ListView` к этому `NamedColor.All` свойство, но только имена полной `NamedColor` объектов отображается.

`ListView` требуется шаблон для отображения этих элементов. В коде, можно задать [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemTemplate/) свойством, которое определяется `ItemsView<TVisual>` для [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) с помощью [ `DataTemplate` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) , ссылается на класс, наследуемый от [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) класса. `Cell` состоит из пяти производных продуктов.

- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) &mdash; содержит два `Label` представлений (с концептуальной точки зрения)
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) &mdash; Добавляет `Image` для просмотра `TextCell`
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) &mdash; содержит `Entry` просмотра с `Label`
- [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) &mdash; содержит `Switch` с `Label`
- [`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) &mdash; может быть любым `View` (скорее всего, с помощью дочерних элементов)

Затем вызовите [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) и [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.DataTemplate.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) на `DataTemplate` для привязки значений с `Cell` свойства, или для задания привязки данных на `Cell` Свойства ссылки на свойства элементов в `ItemsSource` коллекции. Это показано в [ **TextCellListCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListCode) образца.

Как каждый элемент отображается с `ListView`небольшой визуальное дерево строится из шаблона и привязки данных, устанавливаются между элементом и свойств элементов в визуальном дереве. Представление этого процесса можно получить, установив обработчики для [ `ItemAppearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemAppearing/) и [ `ItemDisappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemDisappearing/) события `ListView`, или с помощью альтернативы [ `DataTemplate`конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Func%7BSystem.Object%7D/) , использует функцию, которая вызывается всякий раз, необходимо создать элемент визуального дерева.

[ **TextCellListXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/TextCellListXaml) показана идентичных по функциональности программа полностью в XAML. Объект `DataTemplate` тег имеет значение `ItemTemplate` свойство `ListView`, а затем `TextCell` равно `DataTemplate`. Привязки для свойств элементов в коллекции имеют значение непосредственно на [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) и [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) свойства `TextCell`.

### <a name="custom-cells"></a>Пользовательские ячейки

В языке XAML можно задать [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) для `DataTemplate` , а затем определите пользовательского визуального дерева как [ `View` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ViewCell.View/) свойство `ViewCell`. (`View` является свойством содержимого из `ViewCell` поэтому `ViewCell.View` теги не являются обязательными.) [ **CustomNamedColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CustomNamedColorList) этот метод продемонстрирован в примере:

[![Тройной экрана списка цветов с именем пользовательского](images/ch19fg11-small.png "списка цветов с именем пользовательского")](images/ch19fg11-large.png#lightbox "списка цветов с именем пользовательского")

Получение размера для всех платформ может быть непростой задачей. [ `RowHeight` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RowHeight/) Свойство полезно, но в некоторых случаях может потребоваться прибегать к [ `HasUnevenRows` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HasUnevenRows/) свойство, которое является менее эффективным, но принудительно `ListView` для определения размера строки. Для iOS и Android необходимо использовать один из этих свойств для получения правильного изменение размеров.

### <a name="grouping-the-listview-items"></a>Группирование элементов ListView

`ListView` поддерживает группирование элементов и перемещение между группами. `ItemsSource` Свойство должно быть присвоено коллекцию коллекций: объект, `ItemsSource` равно должен реализовать `IEnumerable`, и каждый элемент в коллекции, также должен реализовывать `IEnumerable`. Каждая группа должна содержать два свойства: текстовое описание группы и трехбуквенное обозначение.

[ `NamedColorGroup` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColorGroup.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека создает семь групп `NamedColor` объектов. [ **ColorGroupList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorGroupList) образце показано, как использовать эти группы с [ `IsGroupingEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsGroupingEnabled/) свойство `ListView` значение `true`и [ `GroupDisplayBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) и [ `GroupShortNameBinding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) свойства привязаны к свойствам в каждой группе.

### <a name="custom-group-headers"></a>Заголовки пользовательских групп

Существует возможность создания пользовательских заголовков `ListView` групп, заменив `GroupDisplayBinding` свойство с [ `GroupHeaderTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/) определение шаблона для заголовков.

### <a name="listview-and-interactivity"></a>ListView и взаимодействие

Обычно приложение получает взаимодействие пользователя с `ListView` путем присоединения обработчика для `ItemSelected` или [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) событий, или установив привязку данных на `SelectedItem` свойство. Однако некоторые типы ячейки (`EntryCell` и `SwitchCell`) допускают взаимодействие с пользователем, а также возможно, чтобы создать пользовательские ячейки, которые сами взаимодействия с пользователем. [ **InteractiveListView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/InteractiveListView) создает экземпляры 100 [ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) и позволяет пользователю изменять каждого цвета, с помощью тройка `Slider` элементов. Программа также используется [ `ColorToContrastColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorToContrastColorConverter.cs) в [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit).

## <a name="listview-and-mvvm"></a>ListView и MVVM

`ListView` в сценариях MVVM играет важную роль. Каждый раз, когда `IEnumerable` коллекции в ViewModel, он часто привязан к `ListView`. Кроме того, элементы в коллекции часто реализовать `INotifyPropertyChanged` для привязки свойства в шаблоне.

### <a name="a-collection-of-viewmodels"></a>Коллекция ViewModels

Для его изучения, [ **SchoolOfFineArts** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) библиотека создает несколько классов, на основе [файла XML-данных](http://xamarin.github.io/xamarin-forms-book-samples/SchoolOfFineArt/students.xml) и изображения вымышленной студентов в этом вымышленной школе.

[ `Student` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/Student.cs) Класс является производным от [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/ViewModelBase.cs). [ `StudentBody` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/StudentBody.cs) Класс — это совокупность `Student` объектов и также является производным от `ViewModelBase`. [ `SchoolViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/SchoolOfFineArt/SchoolOfFineArt/SchoolViewModel.cs) Загружает XML-файла и собирает все объекты.

[ **StudentList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/StudentList) программа использует `ImageCell` для отображения студентов и изображений в `ListView`:

[![Снимок экрана тройной список учащихся](images/ch19fg18-small.png "список учащихся")](images/ch19fg18-large.png#lightbox "список учащихся")

[ **ListViewHeader** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ListViewHeader) пример добавляет [ `Header` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.Header/) свойство, но проявится только на Android.

### <a name="selection-and-the-binding-context"></a>Выбор и контекст привязки

[ **SelectedStudentDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/SelectedStudentDetail) программы привязывает `BindingContext` из `StackLayout` для `SelectedItem` свойство `ListView`. Это позволяет отобразить подробные сведения о выбранной студента.

### <a name="context-menus"></a>Контекстные меню

Ячейки можно определить контекстного меню, реализуется в виде конкретную платформу. Чтобы создать это меню, добавьте [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) объектов [ `ContextActions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) свойство `Cell`.

`MenuItem` Определяет пять свойств:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) типа `string`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) типа `FileImageSource`
- [`IsDestructive`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.IsDestructive/) типа `bool`
- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Command/) типа `ICommand`
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.CommandParameter/) типа `object`

`Command` И `CommandParameter` свойства означает, что ViewModel для каждого элемента содержит методы для выполнения команд меню. В сценариях не MVVM `MenuItem` также определяет [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) событий.

[ **CellContextMenu** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/CellContextMenu) демонстрируется этот способ. `Command` Каждого экземпляра `MenuItem` привязано к свойству типа `ICommand` в `Student` класса. Задать `IsDestructive` свойства `true` для `MenuItem` , удаляет или удаление выбранного объекта.

### <a name="varying-the-visuals"></a>Изменение визуальных элементов

Иногда нужна незначительные изменения в визуальных элементах элементы `ListView` на основе свойства. Например, когда Стьюдента баллов среднее падает ниже 2.0, [ **ColorCodedStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ColorCodedStudents) пример отображает имя этого студента красным цветом.
Это обеспечивается за счет использования преобразователя значений привязки [ `ThresholdToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ThresholdToObjectConverter.cs)в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки.

### <a name="refreshing-the-content"></a>Обновление содержимого

`ListView` Поддерживает жест раскрывающийся список для обновления его данных. Необходимо установить программу [ `IsPullToRefresh` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsPullToRefreshEnabled/) свойства `true` Чтобы включить эту возможность. `ListView` Отвечает в выпадающем списке жестов, устанавливая его [ `IsRefreshing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.IsRefreshing/) свойства `true`и формируя [ `Refreshing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.Refreshing/) событий и (в сценарии MVVM) вызова `Execute` метод его [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) свойство.

Код обработки `Refresh` событий или `RefreshCommand` возможно обновление данных, отображаемых в `ListView` и задает `IsRefreshing` к `false`.

[ **RSS-канал** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/RssFeed) образце показано использование [ `RssFeedViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/RssFeed/RssFeed/RssFeed/RssFeedViewModel.cs) , реализующий `RefreshCommand` и `IsRefreshing` свойства для привязки данных.

## <a name="the-tableview-and-its-intents"></a>TableView и его целей

Хотя `ListView` обычно отображается несколько экземпляров того же типа, [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) обычно направлен на предоставление пользовательского интерфейса для нескольких свойств различных типов. Каждый элемент связывается с собственным [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) класс, производный от для отображения свойства или предоставление пользовательского интерфейса к нему.

### <a name="properties-and-hierarchies"></a>Свойства и иерархий

`TableView` определяет только четыре свойства:

- [`Intent`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Intent/) Тип [ `TableIntent` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableIntent/), перечисление
- [`Root`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.Root/) Тип [ `TableRoot` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/), свойство содержимого `TableView`
- [`RowHeight`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.RowHeight/) типа `int`
- [`HasUnevenRows`](https://developer.xamarin.com/api/property/Xamarin.Forms.TableView.HasUnevenRows/) типа `bool`

`TableIntent` Перечисления указывает, как планируется использовать `TableView`:

- [`Data`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Data/)
- [`Form`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Form/)
- [`Settings`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Settings/)
- [`Menu`](https://developer.xamarin.com/api/field/Xamarin.Forms.TableIntent.Menu/)

Эти элементы также предложит применениям `TableView`.

В определении таблицы участвуют несколько классов.

- [`TableSectionBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase/) Представляет абстрактный класс, производный от `BindableObject` и определяет [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TableSectionBase.Title/) свойство

- [`TableSectionBase<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSectionBase%3CT%3E/) Представляет абстрактный класс, производный от `TableSectionBase` и реализует `IList<T>` и `INotifyCollectionChanged`

- [`TableSection`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableSection/) является производным от `TableSectionBase<Cell>`

- [`TableRoot`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableRoot/) является производным от `TableSectionBase<TableSection>`

Иными словами `TableView` имеет `Root` свойства, которое задано для `TableRoot` объект, который представляет собой коллекцию из `TableSection` объектов, каждый из которых представляет собой совокупность `Cell` объектов. Таблица содержит несколько разделов, а каждый раздел содержит несколько ячеек. Сама таблица может иметь заголовок, а каждый раздел может иметь название. Несмотря на то что `TableView` использует `Cell` производные, он не делает использование `DataTemplate`.

### <a name="a-prosaic-form"></a>Прозаическом формы

[ **EntryForm** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/EntryForm) образец определяет [ `PersonalInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) модель представления, экземпляр которого становится `BindingContext` из `TableView`. Каждый `Cell` класс, производный от в его `TableSection` может иметь привязки для свойства `PersonalInformation` класса.

### <a name="custom-cells"></a>Пользовательские ячейки

[ **ConditionalCells** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalCells) расширяет образец **EntryForm**. [ `ProgrammerInformation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter19/EntryForm/EntryForm/EntryForm/PersonalInformation.cs) Класс содержит логическое свойство, которое определяет возможность применения два дополнительных свойства. Эти два дополнительных свойства пользовательской используются программой `PickerCell` на основе [PickerCell.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml) и [PickerCell.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/PickerCell.xaml.cs) в [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки.

Несмотря на то что `IsEnabled` двух `PickerCell` элементы привязываются к свойство логического типа в `ProgrammerInformation`, этот способ не работает, будет предложено следующего примера.

### <a name="conditional-sections"></a>Условные разделы

[ **ConditionalSection** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/ConditionalSection) пример помещает два элемента, которые являются условными на выбор логическое элемента в отдельном `TableSection`. Файл кода удаляет этот раздел из `TableView` или добавит ее обратно на основании свойство логического типа.

### <a name="a-tableview-menu"></a>Меню TableView

Другое использование оператора `TableView` — это меню. [ **Команды MenuCommand** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19/MenuCommands) образце показано меню, которое позволяет перемещать немного `BoxView` по экрану.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 19 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch19-Apr2016.pdf)
- [Образцы Глава 19](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter19)
- [ListView](~/xamarin-forms/user-interface/listview/index.md)
- [TableView](~/xamarin-forms/user-interface/tableview.md)
