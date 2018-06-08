---
title: Вступление
description: Шаблоны Xamarin.Forms данных предоставляют возможность определения представления данных в элементе управления, поддерживаемых. В этой статье содержатся вводные данные шаблонов, проверки, поэтому они необходимы.
ms.prod: xamarin
ms.assetid: 4ED4ACF4-BE4A-44ED-8EAF-C03947B8663B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: beb1633919a1d4d5bc2f5a8b0452b535a5cc8f3f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847121"
---
# <a name="introduction"></a>Вступление

_Шаблоны Xamarin.Forms данных предоставляют возможность определения представления данных в элементе управления, поддерживаемых. В этой статье содержатся вводные данные шаблонов, проверки, поэтому они необходимы._

Рассмотрим [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) , отображающий коллекцию `Person` объектов. В следующем примере кода показано определение `Person` класса:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Location { get; set; }
}
```

`Person` Класс определяет `Name`, `Age`, и `Location` свойства, которые можно задать при `Person` создан объект. [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Используется для отображения коллекцию `Person` объекты, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataTemplates"
             ...>
    <StackLayout Margin="20">
        ...
        <ListView Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    <local:Person Name="John" Age="37" Location="USA" />
                    <local:Person Name="Tom" Age="42" Location="UK" />
                    <local:Person Name="Lucas" Age="29" Location="Germany" />
                    <local:Person Name="Tariq" Age="39" Location="UK" />
                    <local:Person Name="Jane" Age="30" Location="USA" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

Элементы добавляются [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) в XAML путем инициализации [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) свойства из массива `Person` экземпляров.

> [!NOTE]
> Обратите внимание, что `x:Array` элемента требуется `Type` атрибут, указывающий тип элементов в массиве.

Эквивалентную страницу C# показано в следующем примере кода, который инициализирует [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView%3CTVisual%3E.ItemsSource/) свойства `List` из `Person` экземпляров:

```csharp
public WithoutDataTemplatePageCS()
{
    ...
    var people = new List<Person>
    {
        new Person { Name = "Steve", Age = 21, Location = "USA" },
        new Person { Name = "John", Age = 37, Location = "USA" },
        new Person { Name = "Tom", Age = 42, Location = "UK" },
        new Person { Name = "Lucas", Age = 29, Location = "Germany" },
        new Person { Name = "Tariq", Age = 39, Location = "UK" },
        new Person { Name = "Jane", Age = 30, Location = "USA" }
    };

    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children = {
            ...
            new ListView { ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
        }
    };
}
```

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Вызовы `ToString` при отображении объектов в коллекции. Так как не `Person.ToString` переопределить, `ToString` возвращает имя типа для каждого объекта, как показано на следующем снимке экрана:

![](introduction-images/no-data-template.png "ListView без шаблонов данных")

`Person` Объекта можно переопределить `ToString` метод для отображения значимых данных, как показано в следующем примере кода:

```csharp
public class Person
{
  ...
  public override string ToString ()
  {
    return Name;
  }
}
```

В результате [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) отображение `Person.Name` значение свойства для каждого объекта в коллекции, как показано на следующем снимке экрана:

![](introduction-images/override-tostring.png "ListView с помощью шаблона данных")

`Person.ToString` Переопределения может вернуть отформатированную строку, состоящую из `Name`, `Age`, и `Location` свойства. Однако этот подход обеспечивает отчасти контролировать внешний вид каждого элемента данных. Для большей гибкости [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) могут быть созданы, определяет внешний вид данных.

## <a name="creating-a-datatemplate"></a>Создание DataTemplate

Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) используется для определения внешнего вида данных и обычно использует привязку данных для отображения данных. Его обычным сценарием использования — при отображении данных из коллекции объектов в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Например, если `ListView` привязан к коллекции `Person` объектов, `ListView.ItemTemplate` свойству будет присвоено `DataTemplate` , определяет внешний вид каждого `Person` объекта в `ListView`. `DataTemplate` Будет содержать элементы, привязанные к значениям свойств каждого `Person` объекта. Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) может использоваться в качестве значения для следующих свойств:

- [`ListView.HeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.HeaderTemplate/)
- [`ListView.FooterTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.FooterTemplate/)
- [`ListView.GroupHeaderTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupHeaderTemplate/)
- [`ItemsView.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/), который наследуется [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/).
- [`MultiPage.ItemTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), который наследуется [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), и [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/).

> [!NOTE]
> Обратите внимание, что, несмотря на то что [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) используются [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) объектов, он не использует [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Это так, как привязки данных всегда устанавливаются непосредственно на `Cell` объектов.

Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , помещаются как является прямым потомком свойства, перечисленные выше, называется *встроенного шаблона*. Кроме того `DataTemplate` могут определяться как ресурс уровня управления, странице или приложения. Выбор места для определения [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) влияние, где он может использоваться:

- Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) определен на элементе управления уровень могут применяться только к элементу управления.
- Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) определены на странице уровня могут применяться к несколько допустимых элементов управления на странице.
- Объект [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) определены на уровне приложения может быть применен к допустимым элементов управления в приложении.

Шаблоны данных более низкого уровня в иерархии представления приоритет, чем те, которые определены в выше вверх, если они имеют `x:Key` атрибуты. Например шаблон приложения уровня данных, будут переопределяться шаблон уровня страницы данных и шаблон страницам данных, будут переопределяться шаблон данных на уровне элемента управления или встроенного шаблона данных.


## <a name="related-links"></a>Связанные ссылки

- [Вид ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Шаблоны данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
