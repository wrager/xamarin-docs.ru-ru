---
title: Глобальные стили
description: Стили можно сделать доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей всех страниц или элементов управления.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 219973e26c5ee25accec57f1bebbd1753391e6de
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847908"
---
# <a name="global-styles"></a>Глобальные стили

_Стили можно сделать доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей всех страниц или элементов управления._

## <a name="creating-a-global-style-in-xaml"></a>Создание глобальных стиля в XAML

По умолчанию используют все Xamarin.Forms приложения, созданные из шаблона **приложения** класса для реализации [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) подкласс. Для объявления [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) на уровне приложения, в приложении [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) с помощью XAML, значение по умолчанию **приложения** класса должны быть заменены XAML **Приложения** класса и связанный с выделенным кодом. Дополнительные сведения см. в разделе [работа с класс приложения](~/xamarin-forms/app-fundamentals/application-class.md).

В следующем примере кода показан [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) , объявленные на уровне приложения:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Это [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определяет одно *явных* стиля, `buttonStyle`, который будет использоваться, чтобы задать внешний вид [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров. Тем не менее, могут быть глобальные стили *явных* или *неявное*.

В следующем примере кода показано применение страницу XAML `buttonStyle` на страницу [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Это приводит к появлению показано на следующем снимке экрана:

[![](application-images/application-styles-1.png "Пример глобального стилей")](application-images/application-styles-1-large.png#lightbox "примере глобальные стили")

Дополнительные сведения о создании стилей на странице [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md) и [неявных стилей](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Переопределение стилей

Стили более низкого уровня в иерархии представления имеют приоритет над указанных выше вверх. Например, установка [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) , которая устанавливает [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) для `Red` приложения уровень, будут переопределяться стиль уровня страницы, который задает `Button.TextColor` для `Green`. Аналогичным образом стиль уровня страницы будут переопределены стилей элемента управления. Кроме того Если `Button.TextColor` не задан непосредственно на свойства элемента управления, это будет иметь приоритет над любые стили. В следующем примере кода демонстрируется это имеет приоритет:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Исходный `buttonStyle`, определенного на уровне приложения, переопределяется `buttonStyle` экземпляр, определенный на уровне страниц. Кроме того, стиль уровня страницы переопределяется уровень управления `buttonStyle`. Таким образом [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров отображается синий цвет, как показано на следующем снимке экрана:

[![](application-images/application-styles-2.png "Переопределение стилей пример")](application-images/application-styles-2-large.png#lightbox "переопределение пример стилей")

## <a name="creating-a-global-style-in-c35"></a>Создание глобальных стиля в C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры можно добавить в приложение [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекции для C#, создав новую [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)и затем добавив `Style` экземпляров `ResourceDictionary`, как показано в следующем примере кода:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Конструктор определяет одно *явных* стиль для применения к [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляры во всем приложении. *Явные* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры добавляются [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) с помощью [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) метод, указывая `key`строки для ссылки на `Style` экземпляра. `Style` Экземпляр затем можно применить ко всем элементам управления в приложении правильного типа. Тем не менее, могут быть глобальные стили *явных* или *неявное*.

Показано в следующем примере кода C# страницы применение `buttonStyle` на страницу [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` Применяется к [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляров, задав их [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства и управляет внешним видом `Button` экземпляров.

## <a name="summary"></a>Сводка

Стили можно сделать доступными глобально, добавив их в приложение [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Это помогает избежать дублирования стилей всех страниц или элементов управления.



## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Работа со стилями (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [стиль](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Метод задания](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
