---
title: Создание DataTemplateSelector
description: DataTemplateSelector можно использовать для выбора DataTemplate во время выполнения на основе значения свойства с привязкой к данным. Благодаря этому несколько шаблоны DataTemplate, чтобы применить тот же тип объекта, чтобы настроить внешний вид определенных объектов. В этой статье показано, как создавать и использовать DataTemplateSelector.
ms.prod: xamarin
ms.assetid: A4629E8F-2BAF-45CE-A76E-DF225FE8D26C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: d87b9f3cf47e3b3efa42ad53cfba91bac1d07d4f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-datatemplateselector"></a>Создание DataTemplateSelector

_DataTemplateSelector можно использовать для выбора DataTemplate во время выполнения на основе значения свойства с привязкой к данным. Благодаря этому несколько шаблоны DataTemplate, чтобы применить тот же тип объекта, чтобы настроить внешний вид определенных объектов. В этой статье показано, как создавать и использовать DataTemplateSelector._

Селектор шаблона данных позволяет использовать сценарии, такие как [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) привязка к коллекции объектов, где внешний вид каждого объекта в `ListView` можно выбрать во время выполнения, возвращая селектор шаблона данных определенный [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/).

## <a name="creating-a-datatemplateselector"></a>Создание DataTemplateSelector

Селектор шаблона данных реализуется путем создания класса, который наследует от [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). `OnSelectTemplate` Затем переопределить метод для возврата конкретного [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/), как показано в следующем примере кода:

```csharp
public class PersonDataTemplateSelector : DataTemplateSelector
{
  public DataTemplate ValidTemplate { get; set; }
  public DataTemplate InvalidTemplate { get; set; }

  protected override DataTemplate OnSelectTemplate (object item, BindableObject container)
  {
    return ((Person)item).DateOfBirth.Year >= 1980 ? ValidTemplate : InvalidTemplate;
  }
}
```

`OnSelectTemplate` Метод возвращает соответствующий шаблона, основанного на значении `DateOfBirth` свойство. Шаблон для возврата — это значение `ValidTemplate` свойство или `InvalidTemplate` свойства, которые задаются при использовании `PersonDataTemplateSelector`.

Экземпляр класса селектор шаблона данных можно затем назначить свойства элемента управления Xamarin.Forms например [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/). Список допустимых свойств см. в разделе [Создание DataTemplate](~/xamarin-forms/app-fundamentals/templates/data-templates/creating.md).

### <a name="limitations"></a>Ограничения

[`DataTemplateSelector`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) экземпляры имеют следующие ограничения:

- `DataTemplateSelector` Подкласс должны всегда возвращать один и тот же шаблон для тех же данных при запросе несколько раз.
- `DataTemplateSelector` Подкласс не должен возвращать другой `DataTemplateSelector` подкласс.
- `DataTemplateSelector` Подкласс не должен возвращать новые экземпляры `DataTemplate` при каждом вызове. Вместо этого в том же экземпляре должен быть возвращен. Невыполнение этого требования приведет к созданию утечки памяти и отключит виртуализации.
- На Android, может быть не более 20 различные шаблоны данных на `ListView`.

## <a name="consuming-a-datatemplateselector-in-xaml"></a>Использование DataTemplateSelector в XAML

В языке XAML `PersonDataTemplateSelector` могут создаваться, объявив его в качестве ресурса, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Selector;assembly=Selector" x:Class="Selector.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="validPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <DataTemplate x:Key="invalidPersonTemplate">
                <ViewCell>
                   ...
                </ViewCell>
            </DataTemplate>
            <local:PersonDataTemplateSelector x:Key="personDataTemplateSelector"
                ValidTemplate="{StaticResource validPersonTemplate}"
                InvalidTemplate="{StaticResource invalidPersonTemplate}" />
        </ResourceDictionary>
    </ContentPage.Resources>
  ...
</ContentPage>
```

Этот уровень страницы [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определяет два [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) экземпляров и `PersonDataTemplateSelector` экземпляра. `PersonDataTemplateSelector` Экземпляра наборов его `ValidTemplate` и `InvalidTemplate` соответствующих свойств `DataTemplate` экземпляров с помощью `StaticResource` расширения разметки. Обратите внимание, что при ресурсы, определенные на странице [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), они также быть определены на уровне приложения или уровне элемента управления.

`PersonDataTemplateSelector` Экземпляр потребляется путем назначения их [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) свойства, как показано в следующем примере кода:

```xaml
<ListView x:Name="listView" ItemTemplate="{StaticResource personDataTemplateSelector}" />
```

Во время выполнения [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) вызовы `PersonDataTemplateSelector.OnSelectTemplate` метод для каждого элемента в базовой коллекции, для вызова, передав объект данных, что `item` параметра. [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) , Возвращаемый метод применяется к этому объекту.

Следующих снимках экрана отобразится результат [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) применение `PersonDataTemplateSelector` для каждого объекта в базовой коллекции:

![](selector-images/data-template-selector.png "ListView с селектор шаблона данных")

Любой `Person` объекта, имеющего `DateOfBirth` значение свойства больше или равно 1980 отображается зеленым цветом, оставшихся объектов, будет отображаться красным цветом.

## <a name="consuming-a-datatemplateselector-in-cnum"></a>Использование DataTemplateSelector в C&num;

В C# `PersonDataTemplateSelector` создан и назначен [ `ListView.ItemTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/) свойства, как показано в следующем примере кода:

```csharp
public class HomePageCS : ContentPage
{
  DataTemplate validTemplate;
  DataTemplate invalidTemplate;

  public HomePageCS ()
  {
    ...
    SetupDataTemplates ();
    var listView = new ListView {
      ItemsSource = people,
      ItemTemplate = new PersonDataTemplateSelector {
        ValidTemplate = validTemplate,
        InvalidTemplate = invalidTemplate }
    };

    Content = new StackLayout {
      Margin = new Thickness (20),
      Children = {
        ...
        listView
      }
    };
  }
  ...  
}
```

`PersonDataTemplateSelector` Экземпляра наборов его `ValidTemplate` и `InvalidTemplate` соответствующих свойств [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) экземпляры, созданные `SetupDataTemplates` метод. Во время выполнения [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) вызовы `PersonDataTemplateSelector.OnSelectTemplate` метод для каждого элемента в базовой коллекции, для вызова, передав объект данных, что `item` параметра. `DataTemplate` , Возвращаемый метод применяется к этому объекту.

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создавать и использовать [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/). Объект `DataTemplateSelector` можно использовать для выбора [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) во время выполнения на основе значения свойства с привязкой к данным. Это позволяет нескольким `DataTemplate` экземпляры, чтобы применить тот же тип объекта, чтобы настроить внешний вид определенных объектов.


## <a name="related-links"></a>Связанные ссылки

- [Селектор шаблона данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplateselector/)
- [DataTemplateSelector](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)
