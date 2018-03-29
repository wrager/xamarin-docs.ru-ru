---
title: Словари ресурсов
description: Ресурсы XAML — определения объектов, которые могут использоваться более одного раза. ResourceDictionary позволяет ресурсы, определенные в одном месте, и повторно используются в приложении Xamarin.Forms. В этой статье объясняется, как создавать и использовать ResourceDictionary и способ слияния словари ресурсов.
ms.topic: article
ms.prod: xamarin
ms.assetid: DF103686-4A92-40FA-9CF1-A9376293B13C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/17/2017
ms.openlocfilehash: aa3ae9fed67b6cd7521e5c59edcb54f05cc6b7c5
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2018
---
# <a name="resource-dictionaries"></a>Словари ресурсов

_Ресурсы XAML — определения объектов, которые могут использоваться более одного раза. ResourceDictionary позволяет ресурсы, определенные в одном месте, и повторно используются в приложении Xamarin.Forms. В этой статье объясняется, как создавать и использовать ResourceDictionary и способ слияния словари ресурсов._

## <a name="overview"></a>Обзор

Объект [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) является репозиторием для ресурсов, используемых приложением Xamarin.Forms. Стандартные ресурсы, которые хранятся в `ResourceDictionary` включают [стили](~/xamarin-forms/user-interface/styles/index.md), [шаблоны элементов управления](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md), [шаблоны данных](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md), цвета и преобразователи типов.

В языке XAML, ресурсы определяются в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) получить и применить к элементам с помощью `StaticResource` расширения разметки. В C# ресурсы определяются в `ResourceDictionary` получить и применяется к элементам с помощью индексатора, на основе строки. Однако нет большой пользы, с помощью `ResourceDictionary` в C#, как ресурсы можно легко быть непосредственно присвоены свойства визуальных элементов, без необходимости сначала извлечь их из `ResourceDictionary`.

## <a name="creating-and-consuming-a-resourcedictionary"></a>Создание и использование ResourceDictionary

Ресурсы могут быть определены в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) , присоединенный к [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекции страницы или элемента управления или для [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) коллекции приложения. Выбор места для определения [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) влияние, где он может использоваться:

- Ресурсы в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определены в элементе управления уровень может применяться только к элементу управления и его дочерним элементам.
- Ресурсы в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определены на странице уровня может применяться только к странице и его дочерним элементам.
- Ресурсы в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определенный уровень приложения может применяться в приложении.

В следующем примере кода XAML показан ресурсы, определенные в уровне приложения [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Application ...>
    <Application.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Yellow</Color>
            <Color x:Key="HeadingTextColor">Black</Color>
            <Color x:Key="NormalTextColor">Blue</Color>
            <Style x:Key="LabelPageHeadingStyle" TargetType="Label">
                <Setter Property="FontAttributes" Value="Bold" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="TextColor" Value="{StaticResource HeadingTextColor}" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Это [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) определяет три [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) ресурсы и [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) ресурсов. Дополнительные сведения о создании XAML `App` см. в описании [класса приложения](~/xamarin-forms/app-fundamentals/application-class.md).

Каждый ресурс имеет ключ, который задается с помощью `x:Key` атрибут, который предоставляет описательные ключ в `ResourceDictionary`. Этот ключ используется для извлечения ресурсов из [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) по `StaticResource` расширения разметки, как показано в следующем примере кода XAML, показывающий уровня дополнительные ресурсы, определенные в элементе управления `ResourceDictionary`:

```xaml
<StackLayout Margin="0,20,0,0">
  <StackLayout.Resources>
    <ResourceDictionary>
      <Style x:Key="LabelNormalStyle" TargetType="Label">
        <Setter Property="TextColor" Value="{StaticResource NormalTextColor}" />
      </Style>
      <Style x:Key="MediumBoldText" TargetType="Button">
        <Setter Property="FontSize" Value="Medium" />
        <Setter Property="FontAttributes" Value="Bold" />
      </Style>
    </ResourceDictionary>
  </StackLayout.Resources>
  <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
    <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
           Margin="10,20,10,0"
           Style="{StaticResource LabelNormalStyle}" />
    <Button Text="Navigate"
            Clicked="OnNavigateButtonClicked"
            TextColor="{StaticResource NormalTextColor}"
            Margin="0,20,0,0"
            HorizontalOptions="Center"
            Style="{StaticResource MediumBoldText}" />
</StackLayout>
```

Первый [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляр получает и использует `LabelPageHeadingStyle` ресурсов, которые определены на уровне приложения [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), со вторым `Label` экземпляра Получение и использование `LabelNormalStyle` ресурсов, которые определены на уровне элемента управления `ResourceDictionary`. Аналогичным образом [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) экземпляр получает и использует `NormalTextColor` ресурсов, которые определены на уровне приложения `ResourceDictionary`и `MediumBoldText` ресурсов, которые определены на уровне элемента управления `ResourceDictionary`. Это приводит к появлению показано на следующем снимке экрана:

[![](resource-dictionaries-images/screenshots-sml.png "Потребление ресурсов ResourceDictionary")](resource-dictionaries-images/screenshots.png#lightbox "потребление ресурсов ResourceDictionary")

> [!NOTE]
> Ресурсы, относящиеся к одной странице не должно быть включено в приложения уровне словаря ресурсов, таким образом ресурсы, затем можно проанализировать при запуске приложения, а не требуется для страницы. Дополнительные сведения см. в разделе [уменьшить размер словаря ресурсов приложения](~/xamarin-forms/deploy-test/performance.md).

## <a name="overriding-resources"></a>Переопределение ресурсы

Когда [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ресурсы используют `x:Key` значений атрибутов, ресурсы, определенные ниже в иерархии представления имеют приоритет над указанных выше вверх. Например, установка `PageBackgroundColor` ресурс `Blue` приложения уровень, будут переопределяться уровне страниц `PageBackgroundColor` ресурс `Yellow`. Аналогичным образом, уровень страницы `PageBackgroundColor` ресурса будет переопределено уровень управления `PageBackgroundColor` ресурсов. Этот приоритет демонстрируется в следующем примере кода XAML:

```xaml
<ContentPage ... BackgroundColor="{StaticResource PageBackgroundColor}">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Color x:Key="PageBackgroundColor">Blue</Color>
            <Color x:Key="NormalTextColor">Yellow</Color>
        </ResourceDictionary>
    </ContentPage.Resources>
    <StackLayout Margin="0,20,0,0">
        ...
        <Label Text="ResourceDictionary Demo" Style="{StaticResource LabelPageHeadingStyle}" />
        <Label Text="This app demonstrates consuming resources that have been defined in resource dictionaries."
               Margin="10,20,10,0"
               Style="{StaticResource LabelNormalStyle}" />
        <Button Text="Navigate"
                Clicked="OnNavigateButtonClicked"
                TextColor="{StaticResource NormalTextColor}"
                Margin="0,20,0,0"
                HorizontalOptions="Center"
                Style="{StaticResource MediumBoldText}" />
    </StackLayout>
</ContentPage>
```

Исходный `PageBackgroundColor` и `NormalTextColor` экземпляров, определенные на уровне приложения, будут переопределяться `PageBackgroundColor` и `NormalTextColor` экземпляров, определенные на уровне страницы. Таким образом, цвет фона страницы становится синим, а текст на странице yellow, как показано на следующих снимках экрана:

[![](resource-dictionaries-images/overridding-screenshots-sml.png "Переопределение ресурсы ResourceDictionary")](resource-dictionaries-images/overridding-screenshots.png#lightbox "переопределение ResourceDictionary ресурсы")

Тем не менее, обратите внимание, что в фоновом режиме окна [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) по-прежнему желтый, так как [ `BarBackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/) свойству присвоено значение `PageBackgroundColor` ресурсов, определенных в приложении уровень [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).

## <a name="merged-resource-dictionaries"></a>Объединенные словари ресурсов

Объединенные словари ресурсов объединения одного или нескольких [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) экземпляры в другой. Это делается путем присвоения свойству `ResourceDictionary.MergedDictionaries` свойства словари ресурсов, которые будут объединены в приложение, страницы или уровень управления `ResourceDictionary`.

> [!IMPORTANT]
> [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) Тип также имеет [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) свойство, которое можно использовать для слияния одной `ResourceDictionary` в другой, с `ResourceDictionary` указанный как значение `MergedWith`при объединении в текущее свойство `ResourceDictionary` экземпляра. При слиянии через `MergedWith` свойство, все ресурсы в текущем `ResourceDictionary` , совместно использующие `x:Key` значения с ресурсами в атрибутов `ResourceDictionary` для слияния, будут заменены. Однако `MergedWith` свойства будет считаться устаревшей в следующей версии Xamarin.Forms. Таким образом, рекомендуется использовать `MergedDictionaries` свойство для слияния `ResourceDictionary` экземпляров.

В следующем примере показан код XAML [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) с именем `MyResourceDictionary` , содержащий одного ресурса:

```xaml
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                    x:Class="ResourceDictionaryDemo.MyResourceDictionary">
    <DataTemplate x:Key="PersonDataTemplate">
        <ViewCell>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.5*" />
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.3*" />
                </Grid.ColumnDefinitions>
                <Label Text="{Binding Name}" TextColor="{StaticResource NormalTextColor}" FontAttributes="Bold" />
                <Label Grid.Column="1" Text="{Binding Age}" TextColor="{StaticResource NormalTextColor}" />
                <Label Grid.Column="2" Text="{Binding Location}" TextColor="{StaticResource NormalTextColor}" HorizontalTextAlignment="End" />
            </Grid>
        </ViewCell>
    </DataTemplate>
</ResourceDictionary>
```

`MyResourceDictionary` могут быть объединены в любое приложение, страницы или уровень управления `ResourceDictionary`. В следующем примере кода XAML отображает их в уровне страниц `ResourceDictionary` с помощью `MergedDictionaries` свойство:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <local:MyResourceDictionary />
                <!-- Add more resource dictionaries here -->
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

При слиянии [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ресурсы используют одинаковые `x:Key` значений атрибутов, Xamarin.Forms используется следующий приоритет ресурса:

1. Ресурсы локального словарь ресурсов.
1. Ресурсы, содержащиеся в словаре ресурсов, которая была объединена через [ `MergedWith` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ResourceDictionary.MergedWith/) свойство.
1. Ресурсы, содержащиеся в словарях ресурсов, которые были объединены через `MergedDictionaries` коллекции в порядке их перечисления в `MergedDictionaries` свойство.

> [!NOTE]
> Поиск в словарях ресурсов может быть вычислительных задач, если приложение содержит несколько словарей больших ресурсов. Таким образом Убедитесь, что каждой страницы в приложении только использует словари ресурсов, которые подходят для страницы, чтобы избежать ненужного поиска.

## <a name="summary"></a>Сводка

В этой статье описано, как создать и использовать [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)и способ слияния словари ресурсов. Объект `ResourceDictionary` позволяет определить в одном месте и повторно используются в приложении Xamarin.Forms ресурсы.


## <a name="related-links"></a>Связанные ссылки

- [Словари ресурсов (пример)](https://developer.xamarin.com/samples/xamarin-forms/xaml/resourcedictionaries/)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
