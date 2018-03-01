---
title: "Источники данных ListView"
description: "Узнайте, как для заполнения ListView с данными."
ms.topic: article
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 78659aff0b6b4e6401bec96a55935c141d1199f1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="listview-data-sources"></a>Источники данных ListView

ListView используется для отображения списков данных. Мы рассмотрим заполнение ListView с данными и как можно привязать к выбранного элемента.

- **[Установка ItemsSource](#ItemsSource)**  &ndash; используется простой список или массив.
- **[Привязка данных](#Data_Binding)**  &ndash; устанавливает связь между моделью и ListView. Привязка идеально подходит для шаблона MVVM.

## <a name="itemssource"></a>ItemsSource
ListView заполняется данными с помощью `ItemsSource` свойство, которое может принять любое коллекция, реализующая `IEnumerable`. Самый простой способ заполнения `ListView` предполагает использование массива строк:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView Отображение списка строк")

Описанный подход будет заполнять `ListView` со списком строк. По умолчанию `ListView` вызовет `ToString` и отображения результата в `TextCell` для каждой строки. Чтобы настроить способ отображения данных, в разделе [внешнего вида ячеек](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Поскольку `ItemsSource` было отправлено в массив, не будет обновляться при изменении базового списка или массива. Если вы хотите ListView для автоматического обновления как элементы добавлены, удалены и изменения в базовом списке, необходимо использовать `ObservableCollection`. [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) определен в `System.Collections.ObjectModel` и похож `List`, за исключением того, что он может уведомить `ListView` любых изменений:

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Привязка данных
Привязка данных является «объединяющей» привязывает к свойствам объекта среды CLR, например, класс в вашей ViewModel свойства объект пользовательского интерфейса. Привязка данных полезно, так как он упрощает разработку пользовательских интерфейсов, заменив много скучно стандартный код.

Привязка данных осуществляется посредством синхронизации объектов при изменении их связанных значений. Вместо того, чтобы обработчики событий для каждый раз при изменении значения элемента управления, устанавливать привязки и позволяют выполнять привязку в вашей ViewModel.

Дополнительные сведения о привязке данных см. в разделе [основные сведения о привязке данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) которого входит четыре [Xamarin.Forms XAML основы статьи серии](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Привязка ячеек
Свойства ячеек (и дочерних ячеек) могут быть привязаны к свойствам объектов в `ItemsSource`. Например ListView может использоваться для представления списка сотрудников с изображениями.

Класс сотрудника:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` создается и задать в качестве `ListView` `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

Список заполняется данными:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

В следующем фрагменте показан `ListView` привязан к список сотрудников:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Обратите внимание, что привязка выполнена установки в коде для простоты, несмотря на то, что он может были привязаны в XAML.

Определяет предыдущих бит XAML `ContentPage` , содержащий `ListView`. Источник данных `ListView` задана с помощью `ItemsSource` атрибута. Макет каждой строки в `ItemsSource`определен внутри `ListView.ItemTemplate` элемента.

Это результат:

![](data-and-databinding-images/bound-data.png "ListView с использованием привязки данных")

### <a name="binding-selecteditem"></a>Привязка SelectedItem

Часто необходимо привязать к выбранному элементу `ListView`, а не использовать обработчик событий для реагирования на изменения. Для этого в языке XAML, привязать `SelectedItem` свойство:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

При условии, что `listView` `ItemsSource` приведен список строк, `SomeLabel` будет иметь свойства text, привязанный к `SelectedItem`.



## <a name="related-links"></a>Связанные ссылки

- [Двусторонней привязки (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [заметки о выпуске 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [заметки о выпуске 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
