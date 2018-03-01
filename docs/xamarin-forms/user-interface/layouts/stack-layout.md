---
title: StackLayout
description: "Используйте StackLayout для представления коллекции представлений через одно измерение."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6A91EA70-268C-462C-AAAF-F8DA011403F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: b2d89fd6f9030864931395db00bd6f6321b7fbf9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="stacklayout"></a>StackLayout

`StackLayout` организует представления в строку одномерного («стек»), горизонтально или вертикально. Представления в `StackLayout` можно изменять, зависит от используемого пространства в макете, используя параметры макета. Размещение определяется порядок представления были добавлены в макет и параметры макета представления.

[ ![](stack-layout-images/layouts-sml.png "Макеты Xamarin.Forms")](stack-layout-images/layouts.png "Xamarin.Forms макетов")

## <a name="purpose"></a>Цель

`StackLayout` намного проще других представлений. Можно создать простой линейный интерфейсы, просто добавив представлений для `StackLayout`и более сложные интерфейсы, созданные с вложением.

## <a name="usage--behavior"></a>Использование & поведение

### <a name="spacing"></a>Интервал

По умолчанию `StackLayout` добавит поле 6px между представлениями. Это может управлять или значение без полей, задав `Spacing` свойство StackLayout. Ниже показано, как задать интервал, а также влияние различные интервалы:

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout Spacing="10" x:Name="layout">
      <Button Text="StackLayout" VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView Color="Yellow" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView Color="Green" VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" Color="Blue" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { Text = "StackLayout", VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var yellowBox = new BoxView { Color = Color.Yellow, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var greenBox = new BoxView { Color = Color.Green, VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var blueBox = new BoxView { Color = Color.Blue, VerticalOptions = LayoutOptions.FillAndExpand,
      HorizontalOptions = LayoutOptions.FillAndExpand, HeightRequest = 75 };

    layout.Children.Add(button);
    layout.Children.Add(yellowBox);
    layout.Children.Add(greenBox);
    layout.Children.Add(blueBox);
    layout.Spacing = 10;
    Content = layout;
  }
}
```

Интервал между = 0:

![](stack-layout-images/spacing-zero.png "StackLayout с интервалом = 0")

Интервал 10:

![](stack-layout-images/spacing-ten.png "StackLayout с интервалом = 10")

### <a name="sizing"></a>Изменение размера

Размер представления в StackLayout зависит от высоты и ширины запросов и параметры макета. `StackLayout` будет обеспечивать заполнения. Следующие `LayoutOption`s вызовет представлений занимает меньше места можно получить из макета:

- **CenterAndExpand** &ndash; центрирует представление макета и занимают меньше места макет будет предоставлять при развертывании.
- **EndAndExpand** &ndash; размещает представления в конце макета (нижней или границ справа) и занимают меньше места макет будет предоставлять при развертывании.
- **FillAndExpand** &ndash; помещает представление таким образом, чтобы его без заполнения, будет занимать меньше места макет будет предоставлять.
- **StartAndExpand** &ndash; размещает представления в начале макета и занимает столько места, сколько даст родительского.

Дополнительные сведения см. в разделе [расширения](~/xamarin-forms/user-interface/layouts/layout-options.md#expansion).

### <a name="positioning"></a>Размещение

Представления в StackLayout могут размещаться и размера с помощью `LayoutOptions`. Каждое представление может быть задан `VerticalOptions` и `HorizontalOptions`, определение того, как представления будет перемещать сами относительно макета. Следующие предварительно определенные `LayoutOptions` доступны:

- **Центр** &ndash; центрирует Просмотр в макете.
- **Конец** &ndash; помещает представления в конце макета (нижней или границ справа).
- **Заливка** &ndash; помещает представления, чтобы он включал без заполнения.
- **Запуск** &ndash; помещает в начале макет представления.

В следующем коде показано задание параметров макета:

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.StackLayoutDemo"
Title="StackLayout Demo">
  <ContentPage.Content>
    <StackLayout x:Name="layout">
      <Button VerticalOptions="Start"
        HorizontalOptions="FillAndExpand" />
      <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView VerticalOptions="FillAndExpand"
        HorizontalOptions="FillAndExpand" />
            <BoxView HeightRequest="75" VerticalOptions="End"
        HorizontalOptions="FillAndExpand" />
    </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В C#:

```csharp
public class StackLayoutCode : ContentPage
{
  public StackLayoutCode ()
  {
    var layout = new StackLayout ();
    var button = new Button { VerticalOptions = LayoutOptions.Start,
     HorizontalOptions = LayoutOptions.FillAndExpand };
    var oneBox = new BoxView { VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var twoBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };
    var threeBox = new BoxView {  VerticalOptions = LayoutOptions.FillAndExpand, HorizontalOptions = LayoutOptions.FillAndExpand };

    layout.Children.Add(button);
    layout.Children.Add(oneBox);
    layout.Children.Add(twoBox);
    layout.Children.Add(threeBox);
    Content = layout;
  }
}
```

Дополнительные сведения см. в разделе [выравнивание](~/xamarin-forms/user-interface/layouts/layout-options.md#alignment).

## <a name="exploring-a-complex-layout"></a>Изучение создания сложных макетов

Каждый из макетов иметь сильные и слабые стороны при компоновке определенного. В этой серии статей макета пример приложения был создан с тем же макетом страницы, реализовано с помощью трех различных макетов.

Рассмотрим следующий код XAML.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.StackLayoutPage"
BackgroundColor="Maroon"
Title="StackLayouts">
    <ContentPage.Content>
    <ScrollView>
      <StackLayout Spacing="0" Padding="0" BackgroundColor="Maroon">
        <BoxView HorizontalOptions="FillAndExpand" HeightRequest="100"
          VerticalOptions="Start" Color="Gray" />
        <Button BorderRadius="30" HeightRequest="60" WidthRequest="60"
          BackgroundColor="Red" HorizontalOptions="Center" VerticalOptions="Start" />
        <StackLayout HeightRequest="100" VerticalOptions="Start" HorizontalOptions="FillAndExpand" Spacing="20" BackgroundColor="Maroon">
          <Label Text="User Name" FontSize="28" HorizontalOptions="Center"
            VerticalOptions="Center" FontAttributes="Bold" />
          <Entry Text="Bio + Hashtags" TextColor="White"
            BackgroundColor="Maroon" HorizontalOptions="FillAndExpand" VerticalOptions="CenterAndExpand" />
        </StackLayout>
        <StackLayout Orientation="Horizontal" HeightRequest="50" BackgroundColor="White" Padding="5">
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="Start">
            <BoxView BackgroundColor="Black" WidthRequest="40" HeightRequest="40"  HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Accent Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
          <StackLayout Spacing="0" BackgroundColor="White" Orientation="Horizontal" HorizontalOptions="EndAndExpand">
            <BoxView BackgroundColor="Maroon" WidthRequest="40" HeightRequest="40" HorizontalOptions="Start" VerticalOptions="Center" />
            <Label FontSize="14" TextColor="Black" Text="Primary Color" HorizontalOptions="StartAndExpand" VerticalOptions="Center" />
          </StackLayout>
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Age:" TextColor="White" HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="35" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Interests:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100" />
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin.Forms" TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
          <Label FontSize="14" Text="Ask me about:" TextColor="White"
          HorizontalOptions="Start" VerticalOptions="Center" WidthRequest="100"/>
          <Entry HorizontalOptions="FillAndExpand" Text="Xamarin, C#, .NET, Mono..." TextColor="White" BackgroundColor="Maroon" />
        </StackLayout>
      </StackLayout>
    </ScrollView>
    </ContentPage.Content>
</ContentPage>

```

Приведенный выше код результатов в следующем формате:

![](stack-layout-images/stack.png "Сложные StackLayout")

Обратите внимание, что связано с различиями в подготовку к просмотру кнопок с Windows Phone, некоторые из кругов были заменены boxviews на снимке экрана Windows Phone.

Обратите внимание, что `StackLayouts`вложены s, так как в некоторых случаях вложенности макеты может оказаться удобнее, чем представления все элементы в тот же макет. Также Обратите внимание, что, поскольку `StackLayout` не поддерживает перекрывающиеся элементы не имеют некоторые изысканных возможностей макета найти страницу на страницах для других макетов.



## <a name="related-links"></a>Связанные ссылки

- [LayoutOptions](~/xamarin-forms/user-interface/layouts/layout-options.md)
- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
