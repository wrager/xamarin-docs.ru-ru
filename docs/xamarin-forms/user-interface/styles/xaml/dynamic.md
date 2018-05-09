---
title: Динамические стили
description: Стили не реагировать на изменения свойств и остаются неизменными в течение всего приложения. Например после назначения стиля визуальный элемент, если один из экземпляров задания изменены, удалены, или добавить новый экземпляр задания, изменения не будет применяться к визуальный элемент. Тем не менее приложения можно реагировать на изменения стиля динамически во время выполнения с помощью динамического ресурсов.
ms.prod: xamarin
ms.assetid: 13D4FA4B-DF10-42BF-B001-2C49367FC216
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: c484bdc90ec039a8d70209deabbe283cf7100610
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="dynamic-styles"></a>Динамические стили

_Стили не реагировать на изменения свойств и остаются неизменными в течение всего приложения. Например после назначения стиля визуальный элемент, если один из экземпляров задания изменены, удалены, или добавить новый экземпляр задания, изменения не будет применяться к визуальный элемент. Тем не менее приложения можно реагировать на изменения стиля динамически во время выполнения с помощью динамического ресурсов._

`DynamicResource` Расширение разметки аналогичен `StaticResource` расширения разметки в том, что оба используют ключ словаря для извлечения значения из [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Однако если `StaticResource` выполняет один словарю `DynamicResource` хранит ссылку на ключ словаря. Таким образом при замене элемента словаря, связанное с ключом, это изменение применяется для визуального элемента. Это позволяет среды выполнения стиль вносить изменения в приложении.

В следующем примере кода показано *динамическое* стили в XAML-страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesPage" Title="Dynamic" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle"
                   TargetType="SearchBar"
                   BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle"
                   TargetType="SearchBar">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Placeholder="These SearchBar controls"
                       Style="{DynamicResource searchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Экземпляров используйте `DynamicResource` расширения разметки для ссылки на [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) с именем `searchBarStyle`, которая не определена в XAML. Тем не менее поскольку [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства `SearchBar` экземпляры устанавливаются с использованием `DynamicResource`, отсутствует ключ словаря не приведут к созданию исключения.

Вместо этого в файле кода, конструктор создает [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) запись с ключом `searchBarStyle`, как показано в следующем примере кода:

```csharp
public partial class DynamicStylesPage : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPage ()
    {
        InitializeComponent ();
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
    }

    void OnButtonClicked (object sender, EventArgs e)
    {
        if (originalStyle) {
            Resources ["searchBarStyle"] = Resources ["greenSearchBarStyle"];
            originalStyle = false;
        } else {
            Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];
            originalStyle = true;
        }
    }
}
```

Когда `OnButtonClicked` выполняется обработчик событий, `searchBarStyle` будут переключаться между `blueSearchBarStyle` и `greenSearchBarStyle`. Это приводит к появлению показано на следующем снимке экрана:

[![](dynamic-images/dynamic-style-blue.png "Синий пример динамического стиля")](dynamic-images/dynamic-style-blue-large.png#lightbox "синий пример динамического стиля")
[![](dynamic-images/dynamic-style-green.png "зеленый пример динамического стиля") ] (dynamic-images/dynamic-style-green-large.png#lightbox "Зеленый пример динамического стиля")

В следующем примере кода показаны в эквивалентную страницу в C#:

```csharp
public class DynamicStylesPageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesPageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        ...
        var searchBar1 = new SearchBar { Placeholder = "These SearchBar controls" };
        searchBar1.SetDynamicResource (VisualElement.StyleProperty, "searchBarStyle");
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = { searchBar1, searchBar2, searchBar3, searchBar4,    button  }
        };
    }
    ...
}
```

В C# [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) экземпляров используйте [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) способ ссылки `searchBarStyle`. `OnButtonClicked` Код обработчика событий идентичен приведенному XAML и при выполнении `searchBarStyle` будут переключаться между `blueSearchBarStyle` и `greenSearchBarStyle`.

<a name="dynamic-style-inheritance">

## <a name="dynamic-style-inheritance"></a>Наследование динамического стиля

Наследование от динамического стиля style не может сделать с помощью [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) свойство. Вместо этого [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) класс включает [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) свойство, которое может быть присвоено ключ словаря, значение которого может динамически изменять.

В следующем примере кода показано *динамическое* стиля наследования в XAML-страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.DynamicStylesInheritancePage" Title="Dynamic Inheritance" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
            <Style x:Key="blueSearchBarStyle" TargetType="SearchBar" BasedOn="{StaticResource baseStyle}">
              ...
            </Style>
            <Style x:Key="greenSearchBarStyle" TargetType="SearchBar">
              ...
            </Style>
            <Style x:Key="tealSearchBarStyle" TargetType="SearchBar" BaseResourceKey="searchBarStyle">
              ...
            </Style>
            ...
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <SearchBar Text="These SearchBar controls" Style="{StaticResource tealSearchBarStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Экземпляров используйте `StaticResource` расширения разметки для ссылки на [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) с именем `tealSearchBarStyle`. Это `Style` устанавливает некоторые дополнительные свойства и использует [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) свойства для ссылки `searchBarStyle`. `DynamicResource` Расширения разметки не требуется, поскольку `tealSearchBarStyle` не изменится, за исключением `Style` он является производным от. Таким образом `tealSearchBarStyle` сохраняет ссылку `searchBarStyle` и изменяется при изменении базового стиля.

В файле кода, конструктор создает [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) запись с ключом `searchBarStyle`, как показано в предыдущем примере, где показаны динамические стили. Когда `OnButtonClicked` выполняется обработчик событий, `searchBarStyle` будут переключаться между `blueSearchBarStyle` и `greenSearchBarStyle`. Это приводит к появлению показано на следующем снимке экрана:

[![](dynamic-images/dynamic-style-inheritance-blue.png "Пример наследования стиля динамического синий")](dynamic-images/dynamic-style-inheritance-blue-large.png#lightbox "синий пример наследования стиля динамического")
[![](dynamic-images/dynamic-style-inheritance-green.png "зеленый динамического стиля Пример наследования")](dynamic-images/dynamic-style-inheritance-green-large.png#lightbox "пример наследования стиля динамические зеленый")

В следующем примере кода показаны в эквивалентную страницу в C#:

```csharp
public class DynamicStylesInheritancePageCS : ContentPage
{
    bool originalStyle = true;

    public DynamicStylesInheritancePageCS ()
    {
        ...
        var baseStyle = new Style (typeof(View)) {
            ...
        };
        var blueSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var greenSearchBarStyle = new Style (typeof(SearchBar)) {
            ...
        };
        var tealSearchBarStyle = new Style (typeof(SearchBar)) {
            BaseResourceKey = "searchBarStyle",
            ...
        };
        ...
        Resources = new ResourceDictionary ();
        Resources.Add ("blueSearchBarStyle", blueSearchBarStyle);
        Resources.Add ("greenSearchBarStyle", greenSearchBarStyle);
        Resources ["searchBarStyle"] = Resources ["blueSearchBarStyle"];

        Content = new StackLayout {
            Children = {
                new SearchBar { Text = "These SearchBar controls", Style = tealSearchBarStyle },
                ...
            }
        };
    }
    ...
}
```

`tealSearchBarStyle` Назначается непосредственно [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойство [ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) экземпляров. Это `Style` устанавливает некоторые дополнительные свойства и использует [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) свойства для ссылки `searchBarStyle`. [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/) Метод не требуется здесь так как `tealSearchBarStyle` не изменится, за исключением `Style` он является производным от. Таким образом `tealSearchBarStyle` сохраняет ссылку `searchBarStyle` и изменяется при изменении базового стиля.

## <a name="summary"></a>Сводка

Стили не реагировать на изменения свойств и остаются неизменными в течение всего приложения. Тем не менее приложения можно реагировать на изменения стиля динамически во время выполнения с помощью динамического ресурсов. Кроме того *динамическое* стили могут быть производными от с [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/) свойство.



## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Динамические стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/DynamicStyles/)
- [Работа со стилями (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [стиль](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Метод задания](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
