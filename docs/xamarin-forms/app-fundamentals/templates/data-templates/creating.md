---
title: Создание Xamarin.Forms DataTemplate
description: Шаблоны данных можно создавать встроенные, ResourceDictionary или из пользовательского типа или соответствующий тип ячейки Xamarin.Forms. В этой статье исследуются каждого метода.
ms.prod: xamarin
ms.assetid: CFF4AB5E-9069-461C-84D8-F9F6C38510AB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 8aa0ad693fd1a7f086492f93f18c1e33871dee0e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240516"
---
# <a name="creating-a-xamarinforms-datatemplate"></a>Создание Xamarin.Forms DataTemplate

_Шаблоны данных можно создавать встроенные, ResourceDictionary или из пользовательского типа или соответствующий тип ячейки Xamarin.Forms. В этой статье исследуются каждого метода._

Стандартный сценарий использования [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) отображаются данные из коллекции объектов в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Внешний вид данных для каждой ячейки в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) можно управлять, задав [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) свойства [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Существует несколько способов, которые могут использоваться для выполнения этой задачи.

- [Создание встроенных DataTemplate](#inline).
- [Создание DataTemplate с типом](#type).
- [Создание DataTemplate как ресурс](#resource).

Независимо от используемого метода, результатом является, внешний вид каждой ячейки в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) определяется [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), как показано на следующем снимке экрана:

![](creating-images/data-template-appearance.png "ListView с DataTemplate")

<a name="inline" />

## <a name="creating-an-inline-datatemplate"></a>Создание встроенных DataTemplate

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Может быть установлено на встроенный [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/). Встроенного шаблона, который является тот, который указан в качестве ближайшего дочернего свойства соответствующего элемента управления, следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Элементы, указанные в `DataTemplate` определяют внешний вид каждой ячейки, как показано в следующем примере кода XAML:

```xaml
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
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Label Text="{Binding Name}" FontAttributes="Bold" />
                    <Label Grid.Column="1" Text="{Binding Age}" />
                    <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Дочерние встроенный [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) должно быть, или быть производным от, введите [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/). Макет внутри `ViewCell` управляется здесь [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Содержит три [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляры этой привязки их [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства для каждого из соответствующих свойств `Person` объекта в коллекции.

Эквивалентный код на языке C# показан в следующем примере:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
    public WithDataTemplatePageCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        var personDataTemplate = new DataTemplate(() =>
        {
            var grid = new Grid();
            ...
            var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
            var ageLabel = new Label();
            var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

            nameLabel.SetBinding(Label.TextProperty, "Name");
            ageLabel.SetBinding(Label.TextProperty, "Age");
            locationLabel.SetBinding(Label.TextProperty, "Location");

            grid.Children.Add(nameLabel);
            grid.Children.Add(ageLabel, 1, 0);
            grid.Children.Add(locationLabel, 2, 0);

            return new ViewCell { View = grid };
        });

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemsSource = people, ItemTemplate = personDataTemplate, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

В C#, встроенной функции [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) создается с использованием перегрузки конструктора, которая указывает `Func` аргумент.

<a name="type" />

## <a name="creating-a-datatemplate-with-a-type"></a>Создание DataTemplate с типом

[ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) Свойство также может быть присвоено [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , созданный из типа ячейки. Преимущество такого подхода является, внешний вид, определяемый тип ячейки могут быть использованы несколько шаблонов данных в приложении. Следующий код XAML показан пример такого подхода.

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
                    ...
                </x:Array>
            </ListView.ItemsSource>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <local:PersonCell />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Здесь [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) свойству [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , созданных из пользовательского типа, который определяет внешний вид для ячеек. Пользовательский тип должен быть производным от типа [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), как показано в следующем примере кода:

```xaml
<ViewCell xmlns="http://xamarin.com/schemas/2014/forms"
          xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
          x:Class="DataTemplates.PersonCell">
     <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.2*" />
            <ColumnDefinition Width="0.3*" />
        </Grid.ColumnDefinitions>
        <Label Text="{Binding Name}" FontAttributes="Bold" />
        <Label Grid.Column="1" Text="{Binding Age}" />
        <Label Grid.Column="2" Text="{Binding Location}" HorizontalTextAlignment="End" />
    </Grid>
</ViewCell>
```

В пределах [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), макет управляется здесь [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). `Grid` Содержит три [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляры этой привязки их [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства для каждого из соответствующих свойств `Person` объекта в коллекции.

В следующем примере показан эквивалентный код C#:

```csharp
public class WithDataTemplatePageFromTypeCS : ContentPage
{
    public WithDataTemplatePageFromTypeCS()
    {
        ...
        var people = new List<Person>
        {
            new Person { Name = "Steve", Age = 21, Location = "USA" },
            ...
        };

        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = {
                ...
                new ListView { ItemTemplate = new DataTemplate(typeof(PersonCellCS)), ItemsSource = people, Margin = new Thickness(0, 20, 0, 0) }
            }
        };
    }
}
```

В C# [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) создается с использованием перегрузки конструктора, которая указывает тип ячейки в качестве аргумента. Тип ячейки должен быть производным от типа [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), как показано в следующем примере кода:

```csharp
public class PersonCellCS : ViewCell
{
    public PersonCellCS()
    {
        var grid = new Grid();
        ...
        var nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        var ageLabel = new Label();
        var locationLabel = new Label { HorizontalTextAlignment = TextAlignment.End };

        nameLabel.SetBinding(Label.TextProperty, "Name");
        ageLabel.SetBinding(Label.TextProperty, "Age");
        locationLabel.SetBinding(Label.TextProperty, "Location");

        grid.Children.Add(nameLabel);
        grid.Children.Add(ageLabel, 1, 0);
        grid.Children.Add(locationLabel, 2, 0);

        View = grid;
    }
}
```

> [!NOTE]
> Обратите внимание, что Xamarin.Forms также включает типов ячеек, которые могут использоваться для отображения данных в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) ячеек. Дополнительные сведения см. в разделе [внешнего вида ячеек](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

<a name="resource" />

## <a name="creating-a-datatemplate-as-a-resource"></a>Создание DataTemplate как ресурс

Также можно создавать шаблоны данных для повторного использования в виде объектов [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Это достигается путем предоставления каждое объявление уникальный `x:Key` атрибут, который предоставляет ему описательное ключ в `ResourceDictionary`, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="personTemplate">
                 <ViewCell>
                    <Grid>
                        ...
                    </Grid>
                </ViewCell>
            </DataTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="20">
        ...
        <ListView ItemTemplate="{StaticResource personTemplate}" Margin="0,20,0,0">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:Person}">
                    <local:Person Name="Steve" Age="21" Location="USA" />
                    ...
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </StackLayout>
</ContentPage>
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Назначается [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) свойства с помощью `StaticResource` расширения разметки. Обратите внимание, что, хотя `DataTemplate` определено на странице [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), также можно задать на уровне элемента управления или приложения.

В следующем примере кода показан эквивалентный страницы в C#:

```csharp
public class WithDataTemplatePageCS : ContentPage
{
  public WithDataTemplatePageCS ()
  {
    ...
    var personDataTemplate = new DataTemplate (() => {
      var grid = new Grid ();
      ...
      return new ViewCell { View = grid };
    });

    Resources = new ResourceDictionary ();
    Resources.Add ("personTemplate", personDataTemplate);

    Content = new StackLayout {
      Margin = new Thickness(20),
      Children = {
        ...
        new ListView { ItemTemplate = (DataTemplate)Resources ["personTemplate"], ItemsSource = people };
      }
    };
  }
}
```

[ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) Добавляется [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) с помощью [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) метод, который указывает `Key` строку, которая используется для Справочник по `DataTemplate` при его получении.

## <a name="summary"></a>Сводка

В этой статье показано создание шаблонов данных встроенным образом, из пользовательского типа, так и [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Встроенного шаблона следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Кроме того шаблон данных могут использоваться повторно путем определения его как пользовательский тип, или как ресурс на уровне страницы или уровня приложения уровня управления.


## <a name="related-links"></a>Связанные ссылки

- [Вид ячейки](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)
- [Шаблоны данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
- [DataTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)
