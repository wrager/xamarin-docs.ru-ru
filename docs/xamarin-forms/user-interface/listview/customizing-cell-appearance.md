---
title: "Внешнего вида ячеек"
description: "Варианты представления данных с использованием преимуществ удобством ListView."
ms.topic: article
ms.prod: xamarin
ms.assetid: FD45CB91-1A8F-46FB-B432-6BC20492E456
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/07/2016
ms.openlocfilehash: 551a0de8cd4965815c67a795fb5723d4261a173c
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2018
---
# <a name="cell-appearance"></a>Внешнего вида ячеек

ListView представляется прокручиваемый списки, которые можно настраивать с помощью объекта `ViewCell`s. `ViewCells` можно использовать для отображения текста и изображений, указывающее состояние true или false и получение входных данных пользователя.

Существует два способа получения желаемого эффекта от ячеек ListView.

- **[Настройка встроенных ячеек](#Built_in_Cells)**  &ndash; упрощает реализацию и более высокую производительность за счет подлежит.
- **[Создание пользовательских ячеек](#customcells)**  &ndash; более контроль над конечный результат, но иметь потенциальные проблемы с производительностью, если правильно не реализован.

<a name="Built_in_Cells" />

## <a name="built-in-cells"></a>Встроенные в ячейках
Xamarin.Forms поставляется с встроенной ячеек, которые работают для многих простых приложений:

- **TextCell** &ndash; для отображения текста
- **ImageCell** &ndash; для отображения изображения с текстом.

Две дополнительные ячейки, [ `SwitchCell` ](~/xamarin-forms/user-interface/tableview.md#switchcell) и [ `EntryCell` ](~/xamarin-forms/user-interface/tableview.md#entrycell) доступны, но не часто используются в соответствии с `ListView`. В разделе [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) Дополнительные сведения об этих ячеек.

<a name="TextCell" />

### <a name="textcell"></a>TextCell

[`TextCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) представляет ячейку для отображения текста, при необходимости с вторую строку как текст сведений.

TextCells подготавливаются к просмотру как собственные элементы управления во время выполнения, поэтому производительность — очень хорошо по сравнению с пользовательской `ViewCell`. TextCells являются настраиваемыми, что позволяет задать:

- `Text` &ndash; текст, который отображается в первой строке крупным шрифтом.
- `Detail` &ndash; текст, который отображается под первой строке мелким шрифтом.
- `TextColor` &ndash; цвет текста.
- `DetailColor` &ndash; цвет текста детализации

![](customizing-cell-appearance-images/text-cell-default.png "Пример TextCell по умолчанию")

![](customizing-cell-appearance-images/text-cell-custom.png "Пример настраиваемого TextCell")

<a name="ImageCell" />

### <a name="imagecell"></a>ImageCell

[`ImageCell`](http://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), такие как `TextCell`, можно использовать для отображения текста и текст дополнительный сведений, и он обеспечивает высокую производительность с помощью собственных элементов управления для каждой платформы. `ImageCell` отличается от `TextCell` в том, что он отображает изображение, слева от текста.

`ImageCell` полезно, когда требуется отобразить список данных с помощью внешнего вида, такие как список контактов или фильмов. ImageCells являются настраиваемыми, что позволяет задать:

- `Text` &ndash; текст, который отображается в первой строке крупным шрифтом
- `Detail` &ndash; текст, который отображается под первой строке, меньший размер шрифта
- `TextColor` &ndash; цвет текста
- `DetailColor` &ndash; цвет текста детализации
- `ImageSource` &ndash; изображение, отображаемое рядом с текстом

Обратите внимание, что при разработке для Windows Phone 8.1, `ImageCell` не будет выполнено масштабирование изображений по умолчанию. Кроме того Обратите внимание, что Windows Phone 8.1 только платформа, на которые детализируют данные отображаются в одном цветов и шрифтов как основной текст по умолчанию. Windows Phone 8.0 отображает `ImageCell` как показано ниже:

![](customizing-cell-appearance-images/image-cell-default.png "Пример ImageCell по умолчанию")

![](customizing-cell-appearance-images/image-cell-custom.png "Пример настраиваемого ImageCell")

<a name="customcells" />

## <a name="custom-cells"></a>Пользовательские ячейки
Если встроенные ячеек не обеспечивают требуемый макет, пользовательские ячейки реализован требуемый макет. Например можно представить ячейки с двух меток, которые имеют равный вес. Объект `LabelCell` будет недостаточно поскольку `LabelCell` имеет одной метки, меньший по размеру. Большая часть настроек ячейку добавьте дополнительные данные только для чтения (например, дополнительные ярлыки, изображения или другие сведения об отображении).

Все пользовательские ячейки должны быть производными от [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), тот же базовый класс, что все ячейки встроенные типы использования.

Xamarin.Forms 2 введен новый [поведение кэширования](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy) на `ListView` элемента управления, который может быть задан для повышения быстродействия для некоторых типов пользовательских ячеек.

Ниже приведен пример пользовательского ячейки:

![](customizing-cell-appearance-images/custom-cell.png "Пример пользовательских ячеек")

### <a name="xaml"></a>XAML
XAML для создания макета выше используется следующим образом:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="demoListView.ImageCellPage">
    <ContentPage.Content>
        <ListView  x:Name="listView">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout BackgroundColor="#eee"
                        Orientation="Vertical">
                            <StackLayout Orientation="Horizontal">
                                <Image Source="{Binding image}" />
                                <Label Text="{Binding title}"
                                TextColor="#f35e20" />
                                <Label Text="{Binding subtitle}"
                                HorizontalOptions="EndAndExpand"
                                TextColor="#503026" />
                            </StackLayout>
                        </StackLayout>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Выполняет гораздо выше XAML. Давайте разбить ее:

- Ячейки пользовательского вложен в `DataTemplate`, который находится внутри `ListView.ItemTemplate`. Это тот же процесс, что с помощью любую другую ячейку.
- `ViewCell` — Тип пользовательских ячеек. Дочерний `DataTemplate` элемент должен быть производным от типа или быть `ViewCell`.
- Обратите внимание, что внутри `ViewCell`, макет управляется `StackLayout`. Этот макет позволяет настроить цвет фона. Обратите внимание, что любое свойство `StackLayout` , привязываемых может привязываться внутри пользовательских ячеек, несмотря на то, что, здесь не показан.

### <a name="cnum"></a>C&num;

Указание пользовательских ячеек в C# является более подробным по сравнению эквивалент XAML. Например:

Во-первых, определите пользовательский класс ячейки, с `ViewCell` в качестве базового класса:

```csharp
public class CustomCell : ViewCell
    {
        public CustomCell()
        {
            //instantiate each of our views
            var image = new Image ();
            StackLayout cellWrapper = new StackLayout ();
            StackLayout horizontalLayout = new StackLayout ();
            Label left = new Label ();
            Label right = new Label ();

            //set bindings
            left.SetBinding (Label.TextProperty, "title");
            right.SetBinding (Label.TextProperty, "subtitle");
            image.SetBinding (Image.SourceProperty, "image");

            //Set properties for desired design
            cellWrapper.BackgroundColor = Color.FromHex ("#eee");
            horizontalLayout.Orientation = StackOrientation.Horizontal;
            right.HorizontalOptions = LayoutOptions.EndAndExpand;
            left.TextColor = Color.FromHex ("#f35e20");
            right.TextColor = Color.FromHex ("503026");

            //add views to the view hierarchy
            horizontalLayout.Children.Add (image);
            horizontalLayout.Children.Add (left);
            horizontalLayout.Children.Add (right);
            cellWrapper.Children.Add (horizontalLayout);
            View = cellWrapper;
        }
    }
```

В конструкторе для страницы с `ListView`, задайте ListView `ItemTemplate` для нового `DataTemplate`:

```csharp
public partial class ImageCellPage : ContentPage
    {
        public ImageCellPage ()
        {
            InitializeComponent ();
            listView.ItemTemplate = new DataTemplate (typeof(CustomCell));
        }
    }
```

Обратите внимание, что конструктор `DataTemplate` принимает значение типа. Оператор typeof возвращает тип среды CLR для `CustomCell`.

<a name="binding-context-changes" />

### <a name="binding-context-changes"></a>Изменения контекста привязки

При привязке к ячейки пользовательского типа [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляры элементов управления пользовательского интерфейса, отображающих `BindableProperty` значения следует использовать [ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) переопределение, чтобы задать данные, отображаемые в Каждая ячейка, а не конструктор ячейки, как показано в следующем примере кода:

```csharp
public class CustomCell : ViewCell
{
    Label nameLabel, ageLabel, locationLabel;

    public static readonly BindableProperty NameProperty =
        BindableProperty.Create ("Name", typeof(string), typeof(CustomCell), "Name");
    public static readonly BindableProperty AgeProperty =
        BindableProperty.Create ("Age", typeof(int), typeof(CustomCell), 0);
    public static readonly BindableProperty LocationProperty =
        BindableProperty.Create ("Location", typeof(string), typeof(CustomCell), "Location");

    public string Name {
        get { return(string)GetValue (NameProperty); }
        set { SetValue (NameProperty, value); }
    }

    public int Age {
        get { return(int)GetValue (AgeProperty); }
        set { SetValue (AgeProperty, value); }
    }

    public string Location {
        get { return(string)GetValue (LocationProperty); }
        set { SetValue (LocationProperty, value); }
    }
    ...

    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();

        if (BindingContext != null) {
            nameLabel.Text = Name;
            ageLabel.Text = Age.ToString ();
            locationLabel.Text = Location;
        }
    }
}
```

[ `OnBindingContextChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.OnBindingContextChanged()/) Переопределение будет вызываться при [ `BindingContextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.BindingContextChanged/) вызывается событие, в ответ на значение [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) изменение свойства. Таким образом, когда `BindingContext` изменяет элементы управления пользовательского интерфейса, отображающие [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) значения следует задать свои данные. Обратите внимание, что `BindingContext` должны проверяться `null` значение, как это можно задать с помощью Xamarin.Forms для сборки мусора, что в свою очередь приведет к `OnBindingContextChanged` переопределить вызова.

Кроме того, можно привязать элементы управления пользовательского интерфейса для [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляров, чтобы отобразить их значения, которые не требуется переопределять `OnBindingContextChanged` метод.

> [!NOTE]
> При переопределении метода `OnBindingContextChanged`, убедитесь, что базовый класс `OnBindingContextChanged` метод вызывается, чтобы зарегистрированные делегаты получили `BindingContextChanged` событий.

В языке XAML привязка к данным ячейки пользовательского типа, может осуществляться как показано в следующем примере кода:

```xaml
<ListView x:Name="listView">
    <ListView.ItemTemplate>
        <DataTemplate>
            <local:CustomCell Name="{Binding Name}" Age="{Binding Age}" Location="{Binding Location}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Данный код привязывает `Name`, `Age`, и `Location` свойства связывания в `CustomCell` экземпляра, `Name`, `Age`, и `Location` свойств каждого объекта в коллекции.

В следующем примере кода показан эквивалентный привязки в C#:

```csharp
var customCell = new DataTemplate (typeof(CustomCell));
customCell.SetBinding (CustomCell.NameProperty, "Name");
customCell.SetBinding (CustomCell.AgeProperty, "Age");
customCell.SetBinding (CustomCell.LocationProperty, "Location");

var listView = new ListView {
    ItemsSource = people,
    ItemTemplate = customCell
};
```

В iOS и Android Если [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) перезапускается элементы и ячейки пользовательского использует пользовательское средство отрисовки, пользовательское средство отрисовки необходимо правильно реализовать уведомления об изменении свойства. Если ячейки повторно их значения свойства будут меняться при обновлении контекста привязки, доступные ячейки с `PropertyChanged` вызванных событий. Дополнительные сведения см. в разделе [Настройка ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md). Дополнительные сведения о перезапуске ячейки см. в разделе [стратегии кэширования](~/xamarin-forms/user-interface/listview/performance.md#cachingstrategy).

## <a name="related-links"></a>Связанные ссылки

- [Встроенные в ячейках (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Пользовательские ячейки (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Изменить контекст привязку (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BindingContextChanged)
