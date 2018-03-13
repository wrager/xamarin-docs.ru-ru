---
title: "Неявные стили"
description: "Неявный стиль то, которое используется для всех элементов управления из того же TargetType, без необходимости каждый элемент управления для ссылки на стиль."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: b96b306c882eb30aaf8c81604afb9b6a547d715b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="implicit-styles"></a>Неявные стили

_Неявный стиль то, которое используется для всех элементов управления из того же TargetType, без необходимости каждый элемент управления для ссылки на стиль._

## <a name="creating-an-implicit-style-in-xaml"></a>Создание неявный стиль в XAML

Для объявления [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) на уровне страницы, [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) необходимо добавить страницу и выберите один или несколько `Style` объявления могут быть включены в `ResourceDictionary`. Объект `Style` становится *неявное* , не указывая `x:Key` атрибута. Стиль затем применяются к визуальные элементы, которые соответствуют `TargetType` точно, но не к элементам, которые являются производными от `TargetType` значение.

В следующем примере кода показан *неявное* стиля, объявленного в языке XAML на странице `ResourceDictionary`и применяется к странице [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Определяет одно *неявное* стиль, применяемый к странице [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) экземпляров. `Style` Используется для отображения синий текст на желтом фоне, а для параметра другие параметры внешнего вида. `Style` Добавляется к странице [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) без указания `x:Key` атрибута. Таким образом `Style` применяется ко всем `Entry` экземпляров неявным образом, как они соответствуют [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) свойство `Style` точно. Тем не менее `Style` не применяется к `CustomEntry` экземпляра, то есть подклассов `Entry`. Это приводит к появлению показано на следующем снимке экрана:

[![](implicit-images/implicit-styles.png "Пример неявных стилей")](implicit-images/implicit-styles-large.png#lightbox "примере неявных стилей")

Кроме того, четвертый [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) переопределяет [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) и [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) свойства неявный стиль для разных `Color`значения.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Создание уровня неявный стиль в элемент управления

Помимо создания *неявное* стили на уровне страниц, они могут также создаваться на уровне элемента управления, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В этом примере *неявное* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) назначается [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекцию [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)элемента управления. *Неявное* затем можно применить стиль для элемента управления и его дочерних элементов.

Дополнительные сведения о создании стилей в приложении [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), в разделе [глобальные стили](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Создание неявный стиль в C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры можно добавить на страницу [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекции для C#, создав новую [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)и затем добавив `Style` экземпляров `ResourceDictionary`, как показано в Следующий пример кода:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Конструктор определяет одно *неявное* стиль, применяемый к странице [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) экземпляров. `Style` Используется для отображения синий текст на желтом фоне, а для параметра другие параметры внешнего вида. `Style` Добавляется к странице [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) без указания `key` строки. Таким образом `Style` применяется ко всем `Entry` экземпляров неявным образом, как они соответствуют [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) свойство `Style` точно. Тем не менее `Style` не применяется к `CustomEntry` экземпляра, то есть подклассов `Entry`.

## <a name="summary"></a>Сводка

*Неявное* это один из используемых все визуальные элементы в одной и той же [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), без необходимости каждый элемент управления для ссылки на стиль. Объект `Style` становится *неявное* , не указывая `x:Key` атрибута. Вместо этого `x:Key` атрибут автоматически становится значение [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) свойство.



## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Работа со стилями (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
