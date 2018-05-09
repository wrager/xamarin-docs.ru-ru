---
title: Явные стили
description: Явный стиль —, выборочно применять к элементам управления, задав их свойства стиля.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 53f87fe9dfbf8284055d28fd87bab7bad02c1fd8
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="explicit-styles"></a>Явные стили

_Явный стиль —, выборочно применять к элементам управления, задав их свойства стиля._

## <a name="creating-an-explicit-style-in-xaml"></a>Создание явный стиль в XAML

Для объявления [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) на уровне страницы, [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) необходимо добавить страницу и выберите один или несколько `Style` объявления могут быть включены в `ResourceDictionary`. A `Style` становится *явных* , предоставляя его объявление `x:Key` атрибут, который предоставляет описательные ключ в `ResourceDictionary`. *Явные* этого необходимо применить стили для конкретных визуальные элементы, установив их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства.

В следующем примере кода показан *явных* стили, объявленного в языке XAML на странице `ResourceDictionary` и применяется к странице [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Определяет три *явных* стили, примененные к странице [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров. Каждый `Style` используется для отображения текста в другой цвет, а для параметра шрифта параметры размера и горизонтальные и вертикальные макета. Каждый `Style` применяется к другому `Label` , установив его [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства с помощью `StaticResource` расширения разметки. Это приводит к появлению показано на следующем снимке экрана:

[![](explicit-images/explicit-styles.png "Пример явной стилей")](explicit-images/explicit-styles-large.png#lightbox "Пример явной стилей")

Кроме того, конечный [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) имеет [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) применяемый к нему, но также переопределяет [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) свойство для разных `Color`значение.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Создание уровня явный стиль в элемент управления

Помимо создания *явных* стили на уровне страниц, они могут также создаваться на уровне элемента управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *явных* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляров назначаются [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекцию [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) элемента управления. Стили применима для элемента управления и его дочерних элементов.

Дополнительные сведения о создании стилей в приложении [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Создание явный стиль в C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры можно добавить на страницу [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекции для C#, создав новую [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)и затем добавив `Style` экземпляров `ResourceDictionary`, как показано в Следующий пример кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red  }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label { Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явных* стили, примененные к странице [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров. Каждый *явных* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) добавляется [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) с помощью [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) метод, указание `key` строки для ссылки на `Style` экземпляра. Каждый `Style` применяется к другому `Label` , задав их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства.

Однако нет никаких преимуществ с помощью [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) здесь. Вместо этого [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры можно назначить непосредственно [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства требуется визуальных элементов и `ResourceDictionary` можно удалить, как показано в следующем пример кода:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Конструктор определяет три *явных* стили, примененные к странице [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров. Каждый `Style` используется для отображения текста в другой цвет, а для параметра шрифта параметры размера и горизонтальные и вертикальные макета. Каждый `Style` применяется к другому `Label` , установив его [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства. Кроме того, конечный `Label` имеет `Style` применяемый к нему, но также переопределяет `TextColor` свойства к другому `Color` значение.

## <a name="summary"></a>Сводка

Объект [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) становится *явных* , предоставляя его объявления `x:Key` атрибута, а затем выборочно применять его к элементам управления, установив их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства.



## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Работа со стилями (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [стиль](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Метод задания](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
