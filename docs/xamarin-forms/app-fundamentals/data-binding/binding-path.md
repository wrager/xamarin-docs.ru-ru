---
title: Путь привязки Xamarin.Forms
description: В этой статье объясняется, как использовать Xamarin.Forms привязки данных для доступа к подсвойств и элементы коллекции со свойством путь класса привязки.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: d7c3b1ba991380451b4a82c389c4d46e950bc914
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240477"
---
# <a name="xamarinforms-binding-path"></a>Путь привязки Xamarin.Forms

Во всех предыдущих примерах привязки данных [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) свойство `Binding` класс (или [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) свойство `Binding` расширение разметки) установлен к одному свойству. Фактически можно задать `Path` для *подсвойств* (свойство свойство), или к члену коллекции.

Например, предположим, что страница содержит `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Свойство `TimePicker` имеет тип `TimeSpan`, но это может потребоваться создание привязки данных, который ссылается на `TotalSeconds` , свойство `TimeSpan` значение. Вот привязки данных.

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` Свойство относится к типу `TimeSpan`, который имеет `TotalSeconds` свойства. `Time` И `TotalSeconds` свойства simply связанных с периодом. Элементы в `Path` строка всегда относятся к свойствам, а не типы этих свойств.

Пример и несколько других отображаются в **варианты путь** страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

Во втором `Label`, источник привязки является самой страницы. `Content` Свойство относится к типу `StackLayout`, который имеет `Children` свойство типа `IList<View>`, содержит `Count` свойство, указывающее количество дочерних элементов.

## <a name="paths-with-indexers"></a>Пути с помощью индексаторов

Привязки в третьем `Label` в **варианты путь** страниц ссылки [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) в класс `System.Globalization` пространство имен:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Настройка источника статического `CultureInfo.CurrentCulture` свойство, которое является объектом типа `CultureInfo`. Что класс определяет свойство с именем `DateTimeFormat` типа [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) , содержащий `DayNames` коллекции. Индекс выбирает четвертый элемент.

Четвертый `Label` какого-либо аналогичный, но для языка и региональных параметров, связанных с Франции. `Source` Привязки свойству `CultureInfo` объекта с помощью конструктора:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

В разделе [передачи аргументов конструктора](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) Дополнительные сведения об указании аргументы конструктора в XAML.

Наконец, последний пример аналогичен второй, за исключением того, что оно ссылается на один из дочерних элементов `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Этот дочерний элемент является `Label`, содержит `Text` свойство типа `String`, который имеет `Length` свойства. Первый `Label` отчеты `TimeSpan` в `TimePicker`, поэтому при изменении текста, конечное `Label` также изменения.

Вот программу на всех трех платформ.

[![Варианты путь](binding-path-images/pathvariations-small.png "варианты путь")](binding-path-images/pathvariations-large.png#lightbox "варианты путь")

## <a name="debugging-complex-paths"></a>Отладка сложных контуров

Определения сложного пути могут быть трудности при создании: необходимо знать тип каждого вложенное свойство или тип элементов в коллекции, чтобы правильно добавить вложенное свойство next, но сами типы не отображаются в пути. Один хороший способ — инкрементное построение путь и посмотрите на промежуточных результатов. Для последнего примера, можно начать с нет `Path` определения всех:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Тип источника привязки, или `DataBindingDemos.PathVariationsPage`. Вы знаете `PathVariationsPage` является производным от `ContentPage`, поэтому имеет `Content` свойства:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Тип `Content` свойство теперь получит доступ к быть `Xamarin.Forms.StackLayout`. Добавить `Children` свойства `Path` и тип `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, — это класс, внутренний для Xamarin.Forms, но очевидно, что тип коллекции. Добавьте в этот индекс, а тип — `Xamarin.Forms.Label`. По-прежнему таким образом.

Как Xamarin.Forms обрабатывает путь привязки, он устанавливает `PropertyChanged` обработчик для любого объекта в пути, реализующая `INotifyPropertyChanged` интерфейса. Например, последний привязки реагирует на изменения в первом `Label` из-за `Text` изменения свойств.

Если свойство в путь привязки не реализует `INotifyPropertyChanged`, любые изменения этого свойства будет игнорироваться. Некоторые изменения полностью может сделать недействительным путь привязки, поэтому этот способ следует использовать только в том случае, если строка свойства и подсвойства никогда не становятся недействительными.



## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Данные привязки Глава из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
