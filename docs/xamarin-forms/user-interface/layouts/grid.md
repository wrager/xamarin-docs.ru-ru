---
title: Grid
description: В сетке показаны представления.
ms.prod: xamarin
ms.assetid: 762B1802-D185-494C-B643-74EED55882FE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 6ff36f511c5194017afd34601fc9ea2f89b1e2d4
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848113"
---
# <a name="grid"></a>Grid

[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) поддерживает упорядочение по строкам и столбцам представлений. Строки и столбцы можно задать пропорционально размеры или абсолютные размеры. `Grid` Макет не следует путать с традиционных таблиц и не предназначен для представления табличных данных. `Grid` не поддерживает концепцию строк, столбцов или форматирование ячеек. В отличие от таблиц HTML `Grid` предназначен исключительно для расположения содержимого.

[![](grid-images/layouts-sml.png "Макеты Xamarin.Forms")](grid-images/layouts.png#lightbox "Xamarin.Forms макетов")

В этой статье рассматривается:

- **[Назначение](#Purpose)**  &ndash; распространенные варианты применения `Grid`.
- **[Использование](#Usage)**  &ndash; использовании `Grid` для достижения нужного макета.
  - **[Строки и столбцы](#Rows_and_Columns)**  &ndash; указания строк и столбцов для `Grid`.
  - **[Размещение представлений](#Placing_Views)**  &ndash; добавить представления сетки на определенных строках и столбцах.
  - **[Интервал между](#Spacing)**  &ndash; Настройка пробелов между строками и столбцами.
  - **[Диапазоны](#Spans)**  &ndash; Настройка элементов, охватывающих несколько строк или столбцов.

![](grid-images/grid.png "Сетка просмотра")

## <a name="purpose"></a>Цель

`Grid` можно использовать для упорядочения представления в сетку. Это бывает полезно в нескольких случаях.

- Упорядочивание кнопок в приложении "Калькулятор"
- Упорядочивания кнопок и вариантов в виде сетки, таких как iOS или Android домашних экранов
- Упорядочение представления, чтобы они были одинакового размера, в одном измерении (как в некоторых панелей инструментов)

## <a name="usage"></a>Использование

В отличие от традиционных таблиц `Grid` не выводит число и размеры строк и столбцов на основе содержимого. Вместо этого `Grid` имеет `RowDefinitions` и `ColumnDefinitions` коллекции. Они содержат определения количества строк и столбцов будут располагаться. Представления добавляются `Grid` с указанной строки и столбца индексов, которая идентифицирует какие строки и столбца, в представлении должен быть помещен в.

<a name="Rows_and_Columns" />

### <a name="rows-and-columns"></a>Строки и столбцы

Строк и столбцов сведения хранятся в `Grid` `RowDefinitions`  &  `ColumnDefinitions` свойства, которые являются коллекциями каждого из [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) и [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/)объектов, соответственно. `RowDefinition` имеется единственное свойство `Height`, и `ColumnDefinition` имеется единственное свойство `Width`. Ниже приведены параметры высоты и ширины.

- **Auto** &ndash; автоматически размеров в зависимости от содержимого в строке или столбце. Указанный в виде [ `GridUnitType.Auto` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/) в C#, или как `Auto` в XAML.
- **Proportional(*)** &ndash; размеров столбцов и строк в виде пропорциональных долей оставшегося свободного места. Указанный как значение и `GridUnitType.Star` в C# и как `#*` в XAML, с `#` выполняется требуемое значение. Указание одной строки или столбца с `*` приведет к его для заполнения всего доступного пространства.
- **Абсолютный** &ndash; размеров столбцов и строк с конкретными, фиксированный значения высоты и ширины. Указанный как значение и `GridUnitType.Absolute` в C# и как `#` в XAML, с `#` выполняется требуемое значение.

> [!NOTE]
> Значения ширины столбцов заданы как "*" по умолчанию в Xamarin.Forms, который гарантирует, что столбец будет заполнено все доступное пространство.

Рассмотрим приложение, которое требуется три строки и два столбца. В нижней строке должен быть ровно 200 пкс высоту и верхней строки должен быть дважды ту же высоту как посередине. Левый столбец должен быть настолько широки, в соответствии с содержимым и правый столбец для заполнения оставшегося пространства.

В XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="2*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="200" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

В C#:

```csharp
var grid = new Grid();
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(2, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
grid.RowDefinitions.Add (new RowDefinition { Height = new GridLength(200)});
grid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (200) });
```

<a name="Placing_Views" />

### <a name="placing-views-in-a-grid"></a>Размещение представления в виде сетки

Чтобы поместить представлений в `Grid` необходимо добавлять их в виде дочерних элементов в сетке, а затем укажите, какие строк и столбцов они принадлежат.

В языке XAML, используйте `Grid.Row` и `Grid.Column` для каждого отдельного представления для указания размещения. Обратите внимание, что `Grid.Row` и `Grid.Column` укажите расположение, на основе нуля списков строк и столбцов. Это означает, что в сетку 4 x 4 верхняя левая ячейка — (0,0), а в нижнюю правую ячейку, (3,3).

`Grid` Показано ниже содержит четыре ячейки:

![](grid-images/label-grid.png "Сетка с четыре представления")

В XAML:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Label Text="Top Left" Grid.Row="0" Grid.Column="0" />
  <Label Text="Top Right" Grid.Row="0" Grid.Column="1" />
  <Label Text="Bottom Left" Grid.Row="1" Grid.Column="0" />
  <Label Text="Bottom Right" Grid.Row="1" Grid.Column="1" />
</Grid>
```

В C#:

```csharp
var grid = new Grid();

grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.RowDefinitions.Add(new RowDefinition { Height = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});
grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(1, GridUnitType.Star)});

var topLeft = new Label { Text = "Top Left" };
var topRight = new Label { Text = "Top Right" };
var bottomLeft = new Label { Text = "Bottom Left" };
var bottomRight = new Label { Text = "Bottom Right" };

grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);
```

Приведенный выше код создает сетку с четырьмя меток, два столбца и две строки. Обратите внимание, что каждая метка будет иметь тот же размер, и что строк будет расширен, чтобы использовать все доступное пространство.

В приведенном выше примере представления добавляются к [ `Grid.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.Children/) коллекции с помощью [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/) перегрузку, которая задает левую и верхнюю аргументов. При использовании [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) перегрузку, которая указывает влево, правой, верхней и нижней аргументов, тогда как влево и top аргументов будет всегда относятся к ячейкам [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), вправо и может показаться нижней аргументы ссылок на ячейки, которые находятся за пределами `Grid`. Это объясняется правый аргумент всегда должно быть больше левого аргумента и аргумента нижней всегда должно быть больше верхней аргумент. В следующем примере показано использование обеих эквивалентный код `Add` перегрузки:

```csharp
// left, top
grid.Children.Add(topLeft, 0, 0);
grid.Children.Add(topRight, 1, 0);
grid.Children.Add(bottomLeft, 0, 1);
grid.Children.Add(bottomRight, 1, 1);

// left, right, top, bottom
grid.Children.Add(topLeft, 0, 1, 0, 1);
grid.Children.Add(topRight, 1, 2, 0, 1);
grid.Children.Add(bottomLeft, 0, 1, 1, 2);
grid.Children.Add(bottomRight, 1, 2, 1, 2);
```

### <a name="spacing"></a>Интервал

`Grid` содержит свойства для расстояния между строками и столбцами.  Следующие свойства доступны для настройки `Grid`:

- **ColumnSpacing** &ndash; объем пространства между столбцами.
- **RowSpacing** &ndash; объем пространства между строками.

Следующий код XAML определяет `Grid` с двумя столбцами, одну строку и 5 пкс расстояние между столбцами:

```xaml
<Grid ColumnSpacing="5">
  <Grid.ColumnDefinitions>
    <ColumnDefinitions Width="*" />
    <ColumnDefinitions Width="*" />
  </Grid.ColumnDefinitions>
</Grid>
```

В C#:

```csharp
var grid = new Grid { ColumnSpacing = 5 };
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
grid.ColumnDefnitions.Add(new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star)});
```

### <a name="spans"></a>Диапазоны

Часто при работе с сеткой, отсутствует элемент, который должен занимать более одной строки или столбца. Рассмотрим простое приложение калькулятора.

![](grid-images/calculator.png "Calulator приложения")

Обратите внимание, что кнопки 0 занимает два столбца, так же, как на встроенные калькуляторы для каждой платформы. Это достигается с помощью `ColumnSpan` свойство, которое указывает, сколько столбцов элемент должен занимать. XAML кнопки:

```xaml
<Button Text = "0" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" />
```

И в C#:

```csharp
Button zeroButton = new Button { Text = "0" };
controlGrid.Children.Add (zeroButton, 0, 4);
Grid.SetColumnSpan (zeroButton, 2);
```

Обратите внимание, что в коде статических методов `Grid` класса используются для выполнения позиционирования изменения, включая изменения `ColumnSpan` и `RowSpan`. Также Обратите внимание, что в других свойств, которые могут быть установлены в любое время, свойства, заданные с помощью статических методов уже должен быть в сетке, перед их изменения.

Завершенный XAML выше приложение калькулятора выглядит следующим образом:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.CalculatorGridXAML"
Title = "Calculator - XAML"
BackgroundColor="#404040">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="plainButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#eee"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="darkerButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#ddd"/>
                <Setter Property="TextColor" Value="Black" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
            <Style x:Key="orangeButton" TargetType="Button">
                <Setter Property="BackgroundColor" Value="#E8AD00"/>
                <Setter Property="TextColor" Value="White" />
                <Setter Property="BorderRadius" Value="0"/>
                <Setter Property="FontSize" Value="40" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <Grid x:Name="controlGrid" RowSpacing="1" ColumnSpacing="1">
            <Grid.RowDefinitions>
                <RowDefinition Height="150" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
            <Label Text="0" Grid.Row="0" HorizontalTextAlignment="End" VerticalTextAlignment="End" TextColor="White"
        FontSize="60" Grid.ColumnSpan="4" />
            <Button Text = "C" Grid.Row="1" Grid.Column="0"
        Style="{StaticResource darkerButton}" />
            <Button Text = "+/-" Grid.Row="1" Grid.Column="1"
        Style="{StaticResource darkerButton}" />
            <Button Text = "%" Grid.Row="1" Grid.Column="2"
        Style="{StaticResource darkerButton}" />
            <Button Text = "div" Grid.Row="1" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "7" Grid.Row="2" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "8" Grid.Row="2" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "9" Grid.Row="2" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "X" Grid.Row="2" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "4" Grid.Row="3" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "5" Grid.Row="3" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "6" Grid.Row="3" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "-" Grid.Row="3" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "1" Grid.Row="4" Grid.Column="0"
        Style="{StaticResource plainButton}" />
            <Button Text = "2" Grid.Row="4" Grid.Column="1"
        Style="{StaticResource plainButton}" />
            <Button Text = "3" Grid.Row="4" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "+" Grid.Row="4" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
            <Button Text = "0" Grid.ColumnSpan="2"
        Grid.Row="5" Grid.Column="0" Style="{StaticResource plainButton}" />
            <Button Text = "." Grid.Row="5" Grid.Column="2"
        Style="{StaticResource plainButton}" />
            <Button Text = "=" Grid.Row="5" Grid.Column="3"
        Style="{StaticResource orangeButton}" />
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

Обратите внимание, как метки в верхней части сетки и ноль кнопки occuping больше, чем один столбец. Несмотря на то, что похожая структура можно достичь с помощью вложенной таблицы `ColumnSpan`  &  `RowSpan` подход более прост.

Реализация C#:

```csharp
public CalculatorGridCode ()
{
  Title = "Calculator - C#";
  BackgroundColor = Color.FromHex ("#404040");

  var plainButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#eee") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var darkerButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#ddd") },
      new Setter { Property = Button.TextColorProperty, Value = Color.Black },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };
  var orangeButton = new Style (typeof(Button)) {
    Setters = {
      new Setter { Property = Button.BackgroundColorProperty, Value = Color.FromHex ("#E8AD00") },
      new Setter { Property = Button.TextColorProperty, Value = Color.White },
      new Setter { Property = Button.BorderRadiusProperty, Value = 0 },
      new Setter { Property = Button.FontSizeProperty, Value = 40 }
    }
  };

  var controlGrid = new Grid { RowSpacing = 1, ColumnSpacing = 1 };
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (150) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
  controlGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });

  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
  controlGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });

  var label = new Label {
    Text = "0",
    HorizontalTextAlignment = TextAlignment.End,
    VerticalTextAlignment = TextAlignment.End,
    TextColor = Color.White,
    FontSize = 60
  };
  controlGrid.Children.Add (label, 0, 0);

  Grid.SetColumnSpan (label, 4);

  controlGrid.Children.Add (new Button { Text = "C", Style = darkerButton }, 0, 1);
  controlGrid.Children.Add (new Button { Text = "+/-", Style = darkerButton }, 1, 1);
  controlGrid.Children.Add (new Button { Text = "%", Style = darkerButton }, 2, 1);
  controlGrid.Children.Add (new Button { Text = "div", Style = orangeButton }, 3, 1);
  controlGrid.Children.Add (new Button { Text = "7", Style = plainButton }, 0, 2);
  controlGrid.Children.Add (new Button { Text = "8", Style = plainButton }, 1, 2);
  controlGrid.Children.Add (new Button { Text = "9", Style = plainButton }, 2, 2);
  controlGrid.Children.Add (new Button { Text = "X", Style = orangeButton }, 3, 2);
  controlGrid.Children.Add (new Button { Text = "4", Style = plainButton }, 0, 3);
  controlGrid.Children.Add (new Button { Text = "5", Style = plainButton }, 1, 3);
  controlGrid.Children.Add (new Button { Text = "6", Style = plainButton }, 2, 3);
  controlGrid.Children.Add (new Button { Text = "-", Style = orangeButton }, 3, 3);
  controlGrid.Children.Add (new Button { Text = "1", Style = plainButton }, 0, 4);
  controlGrid.Children.Add (new Button { Text = "2", Style = plainButton }, 1, 4);
  controlGrid.Children.Add (new Button { Text = "3", Style = plainButton }, 2, 4);
  controlGrid.Children.Add (new Button { Text = "+", Style = orangeButton }, 3, 4);
  controlGrid.Children.Add (new Button { Text = ".", Style = plainButton }, 2, 5);
  controlGrid.Children.Add (new Button { Text = "=", Style = orangeButton }, 3, 5);

  var zeroButton = new Button { Text = "0", Style = plainButton };
  controlGrid.Children.Add (zeroButton, 0, 5);
  Grid.SetColumnSpan (zeroButton, 2);

  Content = controlGrid;
}
```


## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms, Глава 17](https://developer.xamarin.com/r/xamarin-forms/book/chapter17.pdf)
- [Сетка](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)
- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
