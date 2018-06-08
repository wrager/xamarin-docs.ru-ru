---
title: Наследование стилей
description: Стили можно наследовать другие стили, чтобы сократить дублирование и включить повторное использование.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7b489e90daec2659a6d11b2776731582bdf368ff
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848425"
---
# <a name="style-inheritance"></a>Наследование стилей

_Стили можно наследовать другие стили, чтобы сократить дублирование и включить повторное использование._

## <a name="style-inheritance-in-xaml"></a>Наследование стиля в XAML

Наследование стилей выполняется путем задания [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) свойства в существующий [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). В языке XAML, это достигается путем установки `BasedOn` свойства `StaticResource` расширение разметки, которое ссылается на ранее созданный `Style`. В C# это достигается путем установки `BasedOn` свойства `Style` экземпляра.

Стили, которые наследуют от базового стиля могут включать [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) экземпляров для новых свойств или их использовать для переопределения стили из базового стиля. Кроме того стили, которые наследуют от базового стиля должны предназначаться для одного типа или типа, производного от типа целевой базового стиля. Например, если цели базового стиля [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) можно выбрать целевую экземпляров, стили, основанные на базовом стиле `View` экземпляры или типы, производные от `View` класса, такие как [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) и [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров.

В следующем коде показано *явных* стиля наследования в XAML-страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`baseStyle` Цели [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) экземпляров и задает [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства. `baseStyle` Не задано непосредственно в любом элементе управления. Вместо этого `labelStyle` и `buttonStyle` наследоваться от него, параметров дополнительные свойства привязки. `labelStyle` И `buttonStyle` затем применяются к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров и [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляра, установив их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства. Это приводит к появлению показано на следующем снимке экрана:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Неявный стиль может быть производным от явный стиль, но явный стиль не может быть производным от неявный стиль.

### <a name="respecting-the-inheritance-chain"></a>Этом соблюдаются цепочку наследования

Стиль может наследовать только от стили на том же уровне или выше в иерархии представления. Это означает следующее.

- Ресурс на уровне приложения может наследовать только от других ресурсов уровня приложения.
- Ресурс уровня страницы может наследовать от уровня ресурсов приложения и другие ресурсы уровня страницы.
- Ресурс уровня управления могут наследовать от ресурсов на уровне приложения, ресурсы уровня страницы и другие ресурсы уровня управления.

В следующем примере кода демонстрируется эта цепочка наследования:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере `labelStyle` и `buttonStyle` являются ресурсах уровня элемента управления во время `baseStyle` является ресурсом уровня страницы. Однако если `labelStyle` и `buttonStyle` наследовать от `baseStyle`, невозможно для `baseStyle` наследование от `labelStyle` или `buttonStyle`из- за их соответствующих местах в иерархии представления.

## <a name="style-inheritance-in-c35"></a>Наследование стиля в C&#35;

Эквивалентную C# страницу, где [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляров назначенные непосредственно [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) показаны свойства обязательные элементы управления, в следующем примере кода:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

`baseStyle` Цели [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) экземпляров и задает [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства. `baseStyle` Не задано непосредственно в любом элементе управления. Вместо этого `labelStyle` и `buttonStyle` наследоваться от него, параметров дополнительные свойства привязки. `labelStyle` И `buttonStyle` затем применяются к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров и [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляра, установив их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства.

## <a name="summary"></a>Сводка

Стили можно наследовать другие стили, чтобы сократить дублирование и включить повторное использование. Наследование стилей выполняется путем задания [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) свойства в существующий [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Работа со стилями (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [стиль](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Метод задания](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
