---
title: Введение в стили
description: Стили позволяют вида визуальные элементы, которые требуется настроить. Стили определены для конкретного типа и содержит значения для свойств, доступных для этого типа.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 453c4d6edafd6493272f8ca0435fcc86e2f3b2f7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-styles"></a>Введение в стили

_Стили позволяют вида визуальные элементы, которые требуется настроить. Стили определены для конкретного типа и содержит значения для свойств, доступных для этого типа._

Xamarin.Forms приложения часто содержат несколько элементов управления, имеющих идентичным внешним видом. Например, приложение может иметь несколько [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров, имеющих одинаковые параметры шрифта и параметры макета, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показан эквивалентный страницы, созданные на языке C#:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        Icon = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Каждый [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляр имеет идентичные значения свойств внешнего вида текста, отображаемого элементом управления `Label`. Это приводит к появлению показано на следующем снимке экрана:

[![](introduction-images/no-styles.png "Метка внешний вид без стили")](introduction-images/no-styles-large.png#lightbox "метки внешний вид без стили")

Настройка внешнего вида каждого отдельного элемента управления может быть повторяющихся и подвержены ошибкам. Вместо этого стиля могут создаваться, определяет внешний вид, а затем применяются к обязательные элементы управления.

## <a name="creating-a-style"></a>Создание стиля

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Класс группирует коллекцию значений свойств в один объект, который затем можно применить к нескольким экземплярам визуального элемента. Это помогает сократить повторяющихся разметки и позволяет внешний вид приложения проще изменять.

Несмотря на то, что стили были разработаны преимущественно для приложений на основе XAML, можно также создать в C#:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры, созданные на языке XAML обычно определяются в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) , которые назначены [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекцию элементов управления, странице или к [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) сбора приложения.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры, созданные на языке C# обычно определяются в классе страницы или в классе, который может осуществляться глобально.

Выбор места для определения [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) влияние, где он может использоваться:

- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляры, определенные на уровне элемента управления может применяться только для элемента управления и его дочерних элементов.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляров, определенные на уровне страниц может применяться только к странице и его дочерним элементам.
- [`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) в приложении могут применяться экземпляров, определенные на уровне приложения.

Каждый [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляр содержит коллекцию из одного или нескольких [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) объектов, каждый `Setter` наличие [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) и [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/). `Property` Имя привязываемые свойства элемента применяется стиль, а `Value` — значение, которое применяется к свойству.

Каждый [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляр может быть *явных*, или *неявное*:

- *Явных* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляр определяется путем указания [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) и `x:Key` значение, а также задать целевого элемента [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства `x:Key` ссылки. Дополнительные сведения о *явных* стили, в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md).
- *Неявное* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляр определяется путем указания только [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/). `Style` Экземпляр затем автоматически применяются ко всем элементам этого типа. Обратите внимание что подклассы `TargetType` нет автоматически `Style` применения. Дополнительные сведения о *неявное* стили, в разделе [неявных стилей](~/xamarin-forms/user-interface/styles/implicit.md).

При создании [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/), [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) свойство всегда является обязательным. В следующем примере кода показан *явных* стиля (Примечание `x:Key`) создано в XAML:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Чтобы применить `Style`, целевой объект должен иметь [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) , соответствующий [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) значение свойства `Style`, как показано в следующем примере кода XAML:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Стили более низкого уровня в иерархии представления имеют приоритет над указанных выше вверх. Например, установка [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) , которая устанавливает [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) для `Red` приложения уровень, будут переопределяться стиль уровня страницы, который задает `Label.TextColor` для `Green`. Аналогичным образом стиль уровня страницы будут переопределены стилей элемента управления. Кроме того Если `Label.TextColor` не задан непосредственно на свойства элемента управления, это имеет приоритет над любые стили.

Статьи в этом разделе демонстрируют и объясняется, как создать и применить *явных* и *неявное* стили, как создать глобальные стили, стиль наследования, как реагировать на изменения стиля во время выполнения и как использовать встроенные стили, которые включены в Xamarin.Forms.

> [!NOTE]
> **Что такое StyleId?**
>
> Прежде чем Xamarin.Forms 2.2 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) свойство было использовано для идентификации отдельных элементов в приложении для идентификации в тестирование пользовательского интерфейса и в механизмах тему, например Pixate. Тем не менее, накладывает Xamarin.Forms 2.2 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) свойства, которое используется для [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) свойство. Дополнительные сведения см. в разделе [Xamarin.Forms автоматизировать тестирование с помощью Xamarin.UITest и Test Cloud](~/xamarin-forms/deploy-test/uitest-and-test-cloud.md).

## <a name="summary"></a>Сводка

Xamarin.Forms приложения часто содержат несколько элементов управления, имеющих идентичным внешним видом. Настройка внешнего вида каждого отдельного элемента управления может быть повторяющихся и подвержены ошибкам. Вместо этого стили можно создать, настроить внешний вид элемента управления путем группирования и параметры свойств, доступных на типе элемента управления.


## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
