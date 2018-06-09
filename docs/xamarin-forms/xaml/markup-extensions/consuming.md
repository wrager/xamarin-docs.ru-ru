---
title: Использование расширения разметки XAML
description: В этой статье описывается использование расширения разметки Xamarin.Forms XAML для улучшения мощность и гибкость языка XAML, позволяя атрибуты элемента для задания из различных источников.
ms.prod: xamarin
ms.assetid: CE686893-609C-4EC3-9225-6C68D2A9F79C
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: 278677d45f997ac446c2a20967dc3501179bf8da
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245941"
---
# <a name="consuming-xaml-markup-extensions"></a>Использование расширения разметки XAML

Расширения разметки XAML для повышения мощность и гибкость языка XAML, позволяя атрибуты элемента для задания из различных источников. Несколько расширений разметки XAML являются частью спецификации XAML 2009 г. Эти сведения отображаются в XAML-файлов с обычной `x` префикс пространства имен и они часто ссылаются с этого префикса. Они описаны в следующих разделах:

- [`x:Static`](#static) &ndash; ссылаться на статических свойств, полей или членов перечисления.
- [`x:Reference`](#reference) &ndash; Ссылка с именем элементов на странице.
- [`x:Type`](#type) &ndash; значение атрибута `System.Type` объекта.
- [`x:Array`](#array) &ndash; Создание массива объектов определенного типа.
- [`x:Null`](#null) &ndash; значение атрибута `null` значение.

Три других расширений разметки XAML Исторически поддерживаются в других реализациях XAML и поддерживаются также Xamarin.Forms. Более подробно они описаны в других статьях:

- `StaticResource` &ndash; ссылок на объекты из словаря ресурсов, как описано в статье [ **словари ресурсов**](~/xamarin-forms/xaml/resource-dictionaries.md).
- `DynamicResource` &ndash; реагирование на изменения в объекты в словаре ресурсов, как описано в статье [ **динамические стили**](~/xamarin-forms/user-interface/styles/dynamic.md).
- `Binding` &ndash; установить связь между свойствами двух объектов, как описано в статье [ **привязки данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- `TemplateBinding` &ndash; выполняет привязку данных из шаблона элемента управления, как описано в статье [**привязки из шаблона элемента управления**] / направляющие/xamarin-forms/приложения основы/шаблоны/привязка элементов управления шаблоны/шаблона и)

[ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) Макета используется пользовательское расширение разметки [ `ConstraintExpression` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ConstraintExpression/). Это расширение разметки, описанное в статье [ **RelativeLayout**](~/xamarin-forms/user-interface/layouts/relative-layout.md).

<a name="static" />

## <a name="xstatic-markup-extension"></a>Расширение разметки x:Static

`x:Static` Поддерживается расширение разметки [ `StaticExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) класса. Класс имеет одно свойство с именем [ `Member` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.StaticExtension.Member/) типа `string` присвоено имя открытого константа, статическое свойство, статическое поле или член перечисления.

Простой способ использования `x:Static` является сначала определить класс с некоторыми константы или статические переменные, такие как этом очень мала `AppConstants` класса в [ **MarkupExtensions** ](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/) программы:

```csharp
static class AppConstants
{
    public static double NormalFontSize = 18;
}
```

**X: Static Демонстрация** странице демонстрируется несколько способов использования `x:Static` расширения разметки. Создает экземпляр подход наиболее подробный `StaticExtension` класс между `Label.FontSize` теги элементов свойства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.StaticDemoPage"
             Title="x:Static Demo">
    <StackLayout Margin="10, 0">
        <Label Text="Label No. 1">
            <Label.FontSize>
                <x:StaticExtension Member="local:AppConstants.NormalFontSize" />
            </Label.FontSize>
        </Label>

        ···

    </StackLayout>
</ContentPage>
```

Средство синтаксического анализа XAML также позволяет `StaticExtension` класс, чтобы сократить до `x:Static`:

```xaml
<Label Text="Label No. 2">
    <Label.FontSize>
        <x:Static Member="local:AppConstants.NormalFontSize" />
    </Label.FontSize>
</Label>
```

Это может быть еще более упрощено, но изменение вводит некоторые новый синтаксис: он состоит из помещения `StaticExtension` классов и членов, установив в фигурные скобки. Результирующее выражение имеет значение непосредственно в `FontSize` атрибута:

```xaml
<Label Text="Label No. 3"
       FontSize="{x:StaticExtension Member=local:AppConstants.NormalFontSize}" />
```

Обратите внимание, что *не* кавычки в фигурные скобки. `Member` Свойство `StaticExtension` больше не XML-атрибут. Вместо этого он является частью выражения для расширения разметки.

Точно так же, как можно сократить `x:StaticExtension` для `x:Static` при использовании как объектный элемент, можно также записать его в выражение в фигурных скобках:

```xaml
<Label Text="Label No. 4"
       FontSize="{x:Static Member=local:AppConstants.NormalFontSize}" />
```

`StaticExtension` Класс имеет `ContentProperty` атрибут ссылки на свойство `Member`, что означает пометку этому свойству значение свойства класса по умолчанию для содержимого. Для расширения разметки XAML, выраженное в фигурные скобки, то можно исключить `Member=` часть выражения:

```xaml
<Label Text="Label No. 5"
       FontSize="{x:Static local:AppConstants.NormalFontSize}" />
```

Это наиболее распространенная форма `x:Static` расширения разметки.

**Статических Демонстрация** страница содержит два примера. Корневой тег в файле XAML содержит объявление пространства имен XML для .NET `System` пространство имен:

```xaml
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Это позволяет `Label` размер шрифта, задаваемое для статического поля `Math.PI`. Приводит сравнительно небольшой текст таким образом `Scale` свойству `Math.E`:

```xaml
<Label Text="&#x03C0; &#x00D7; E sized text"
       FontSize="{x:Static sys:Math.PI}"
       Scale="{x:Static sys:Math.E}"
       HorizontalOptions="Center" />
```

Последний пример отображает `Device.RuntimePlatform` значение. `Environment.NewLine` Статическое свойство используется для вставки символа новой строки между двумя `Span` объектов:

```xaml
<Label HorizontalTextAlignment="Center"
       FontSize="{x:Static local:AppConstants.NormalFontSize}">
    <Label.FormattedText>
        <FormattedString>
            <Span Text="Runtime Platform: " />
            <Span Text="{x:Static sys:Environment.NewLine}" />
            <Span Text="{x:Static Device.RuntimePlatform}" />
        </FormattedString>
    </Label.FormattedText>
</Label>
```

Ниже приведен пример, запущенного на всех трех платформ.

[![Демонстрация x: Static](consuming-images/staticdemo-small.png "Демонстрация x: Static")](consuming-images/staticdemo-large.png#lightbox "Демонстрация x: Static")

<a name="reference" />

## <a name="xreference-markup-extension"></a>расширение разметки x:Reference

`x:Reference` Поддерживается расширение разметки [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) класса. Класс имеет одно свойство с именем [ `Name` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.ReferenceExtension.Name/) типа `string` присвоено имя элемента на странице, задано имя с `x:Name`. Это `Name` свойство является свойством содержимого `ReferenceExtension`, поэтому `Name=` является не обязательным, если `x:Reference` отображается в фигурных скобках.

`x:Reference` Расширения разметки используется исключительно с привязками данных, которые описаны более подробно в статье [ **привязки данных**](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**X: Reference Демонстрация** страницы показано два способа использования `x:Reference` с привязками данных, первый, где он используется для задания `Source` свойство `Binding` объекта и второй, где он используется для задания `BindingContext` свойство для привязки данных на два:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ReferenceDemoPage"
             x:Name="page"
             Title="x:Reference Demo">

    <StackLayout Margin="10, 0">

        <Label Text="{Binding Source={x:Reference page},
                              StringFormat='The type of this page is {0}'}"
               FontSize="18"
               VerticalOptions="CenterAndExpand"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="Center" />

        <Label BindingContext="{x:Reference slider}"
               Text="{Binding Value, StringFormat='{0:F0}&#x00B0; rotation'}"
               Rotation="{Binding Value}"
               FontSize="24"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

    </StackLayout>
</ContentPage>
```

Оба `x:Reference` выражения используйте сокращенную версию `ReferenceExtension` имя класса, а также помогает избежать `Name=` часть выражения. В первом примере `x:Reference` расширения разметки, внедренных в `Binding` расширения разметки. Обратите внимание, что `Source` и `StringFormat` параметров разделяются запятыми. Вот программу на всех трех платформ.

[![Демонстрация x: Reference](consuming-images/referencedemo-small.png "Демонстрация x: Reference")](consuming-images/referencedemo-large.png#lightbox "Демонстрация x: Reference")

<a name="type" />

## <a name="xtype-markup-extension"></a>Расширение разметки x:Type

`x:Type` Расширение разметки эквивалентен XAML C# [ `typeof` ](/dotnet/csharp/language-reference/keywords/typeof/) ключевое слово. Этот режим поддерживается [ `TypeExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TypeExtension/) класса, который определяет одно свойство с именем [ `TypeName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.TypeExtension.TypeName/) типа `string` присвоено имя класса или структуры. `x:Type` Возвращает расширение разметки [ `System.Type` ](https://developer.xamarin.com/api/type/System.Type/) объект этого класса или структуры. `TypeName` свойство содержимого `TypeExtension`, поэтому `TypeName=` является не обязательным, если `x:Type` отображается с фигурными скобками.

В Xamarin.Forms, имеют несколько свойств, имеющие аргументы типа `Type`. Примеры включают [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) свойство `Style`и [x: TypeArguments](~/xamarin-forms/xaml/passing-arguments.md#generic_type_arguments) атрибут, используемый для указания аргументов в универсальных классах. Тем не менее, средство синтаксического анализа XAML выполняет `typeof` операции автоматически и `x:Type` расширения разметки не используется в таких случаях.

Централизованно где `x:Type` *—* требуется с `x:Array` расширения разметки, который описан в [разделу](#array).

`x:Type` Расширения разметки также полезен, при создании меню, где каждый элемент меню соответствует объекта определенного типа. Можно связать `Type` с каждым пунктом меню и затем создать экземпляр объекта, при выборе пункта меню.

Это как меню навигации в `MainPage` в **расширения разметки** программа works. **MainPage.xaml** файл содержит `TableView` с каждым `TextCell` соответствующий определенной страницы в программе:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.MainPage"
             Title="Markup Extensions"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection>
                <TextCell Text="x:Static Demo"
                          Detail="Access constants or statics"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:StaticDemoPage}" />

                <TextCell Text="x:Reference Demo"
                          Detail="Reference named elements on the page"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ReferenceDemoPage}" />

                <TextCell Text="x:Type Demo"
                          Detail="Associate a Button with a Type"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:TypeDemoPage}" />

                <TextCell Text="x:Array Demo"
                          Detail="Use an array to fill a ListView"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:ArrayDemoPage}" />

                ···                          

        </TableRoot>
    </TableView>
</ContentPage>
```

Вот на главной странице открытия **расширения разметки**:

[![Главной страницы](consuming-images/mainpage-small.png "Main страницы")](consuming-images/mainpage-large.png#lightbox "главной страницы")

Каждый `CommandParameter` свойству `x:Type` расширение разметки, которое ссылается на один из других страниц. `Command` Свойство привязано к свойству с именем `NavigateCommand`. Это свойство определено в `MainPage` файл кода программной части:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(async (Type pageType) =>
        {
            Page page = (Page)Activator.CreateInstance(pageType);
            await Navigation.PushAsync(page);
        });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

`NavigateCommand` Свойство `Command` объект, реализующий команду execute с аргументом типа `Type` &mdash; значение `CommandParameter`. Этот метод использует `Activator.CreateInstance` для создания экземпляра на странице и затем переходит к нему. Конструктор завершается, задав `BindingContext` страницы к самому себе, что позволяет `Binding` на `Command` для работы. В разделе [ **привязки данных** ](~/xamarin-forms/app-fundamentals/data-binding/index.md) статьи и особенно [ **Commanding** ](~/xamarin-forms/app-fundamentals/data-binding/commanding.md) Дополнительные сведения об этом типе код.

**X: Type Демонстрация** странице используется та же методика создать Xamarin.Forms элементы и добавлять их в `StackLayout`. В файле XAML, изначально состоит из трех `Button` элементы с их `Command` значение свойства `Binding` и `CommandParameter` свойства, заданные для типов из трех представлений Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.TypeDemoPage"
             Title="x:Type Demo">

    <StackLayout x:Name="stackLayout"
                 Padding="10, 0">

        <Button Text="Create a Slider"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Slider}" />

        <Button Text="Create a Stepper"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Stepper}" />

        <Button Text="Create a Switch"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Command="{Binding CreateCommand}"
                CommandParameter="{x:Type Switch}" />
    </StackLayout>
</ContentPage>
```

Определяет файл кода и инициализирует `CreateCommand` свойство:

```csharp
public partial class TypeDemoPage : ContentPage
{
    public TypeDemoPage()
    {
        InitializeComponent();

        CreateCommand = new Command<Type>((Type viewType) =>
        {
            View view = (View)Activator.CreateInstance(viewType);
            view.VerticalOptions = LayoutOptions.CenterAndExpand;
            stackLayout.Children.Add(view);
        });

        BindingContext = this;
    }

    public ICommand CreateCommand { private set; get; }
}
```

Метод, который является выполняются при `Button` нажатии создает новый экземпляр аргумента, устанавливает его `VerticalOptions` свойство и добавляет его в `StackLayout`. Три `Button` элементы совместно использовать страницу с динамически созданным представления:

[![Демонстрация x: Type](consuming-images/typedemo-small.png "Демонстрация x: Type")](consuming-images/typedemo-large.png#lightbox "Демонстрация x: Type")

<a name="array" />

## <a name="xarray-markup-extension"></a>Расширение разметки x:Array

`x:Array` Расширение разметки можно определить массив в разметке. Этот режим поддерживается [ `ArrayExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ArrayExtension/) класс, который определяет два свойства:

- `Type` Тип `Type`, который указывает тип элементов в массиве.
- `Items` Тип `IList`, которое является коллекцией сами элементы. Это свойство содержимого `ArrayExtension`.

`x:Array` Расширение разметки, сам никогда не отображается в фигурных скобках. Вместо этого `x:Array` открывающий и закрывающий теги выделения в списке элементов. Задать `Type` свойства `x:Type` расширения разметки.

**X: Array Демонстрация** страницы показано, как использовать `x:Array` добавления элементов в `ListView` , установив `ItemsSource` свойства массива:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.ArrayDemoPage"
             Title="x:Array Demo Page">
    <ListView Margin="10">
        <ListView.ItemsSource>
            <x:Array Type="{x:Type Color}">
                <Color>Aqua</Color>
                <Color>Black</Color>
                <Color>Blue</Color>
                <Color>Fuchsia</Color>
                <Color>Gray</Color>
                <Color>Green</Color>
                <Color>Lime</Color>
                <Color>Maroon</Color>
                <Color>Navy</Color>
                <Color>Olive</Color>
                <Color>Pink</Color>
                <Color>Purple</Color>
                <Color>Red</Color>
                <Color>Silver</Color>
                <Color>Teal</Color>
                <Color>White</Color>
                <Color>Yellow</Color>
            </x:Array>
        </ListView.ItemsSource>

        <ListView.ItemTemplate>
            <DataTemplate>
                <ViewCell>
                    <BoxView Color="{Binding}"
                             Margin="3" />    
                </ViewCell>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</ContentPage>        
```

`ViewCell` Создается простой `BoxView` для каждой записи цвет:

[![Демонстрация x: Array](consuming-images/arraydemo-small.png "Демонстрация x: Array")](consuming-images/arraydemo-large.png#lightbox "Демонстрация x: Array")

Существует несколько способов, чтобы указать отдельные `Color` элементов в этом массиве. Можно использовать `x:Static` расширения разметки:

```xaml
<x:Static Member="Color.Blue" />
```

Можно также использовать `StaticResource` для извлечения цвета из словаря ресурсов:

```xaml
<StaticResource Key="myColor" />
```

В конце этой статьи вы увидите пользовательского расширения разметки XAML, также создает новое значение цвета:

```xaml
<local:HslColor H="0.5" S="1.0" L="0.5" />
```

При определении массивы общих типов, такие как строки или числа, используйте теги, перечисленные в [ **передачи аргументов конструктора** ](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) статьи для разделения значений.

<a name="null" />

## <a name="xnull-markup-extension"></a>Расширение разметки x:NULL

`x:Null` Поддерживается расширение разметки [ `NullExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.NullExtension/) класса. Он не имеет свойств и эквивалентно просто XAML C# [ `null` ](/dotnet/csharp/language-reference/keywords/null/) ключевое слово.

`x:Null` Редко требуется расширение разметки, а редко используемые, но если вы найдете потребность, оказывается, что он существует.

**X: Null Демонстрация** страницы демонстрируется сценарий, когда `x:Null` может быть удобным. Предположим, определены неявный `Style` для `Label` , включающего `Setter` , которая устанавливает `FontFamily` свойства зависят от платформы имя семейства:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MarkupExtensions.NullDemoPage"
             Title="x:Null Demo">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="48" />
                <Setter Property="FontFamily">
                    <Setter.Value>
                        <OnPlatform x:TypeArguments="x:String">
                            <On Platform="iOS" Value="Times New Roman" />
                            <On Platform="Android" Value="serif" />
                            <On Platform="UWP" Value="Times New Roman" />
                        </OnPlatform>
                    </Setter.Value>
                </Setter>
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <ContentPage.Content>
        <StackLayout Padding="10, 0">
            <Label Text="Text 1" />
            <Label Text="Text 2" />

            <Label Text="Text 3"
                   FontFamily="{x:Null}" />

            <Label Text="Text 4" />
            <Label Text="Text 5" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>   
```

Затем вы обнаружите, что для одного из `Label` элементов, необходимо, чтобы все значения свойств в неявный `Style` за исключением `FontFamily`, который будет использоваться значение по умолчанию в. Можно определить другую `Style` для этой цели, но более простой подход заключается в задании `FontFamily` свойства конкретного `Label` для `x:Null`, как показано в центре `Label`.

Вот программу на трех платформ.

[![x: Null Демонстрация](consuming-images/nulldemo-small.png "Демонстрация x: Null")](consuming-images/nulldemo-large.png#lightbox "Демонстрация x: Null")

Обратите внимание, четыре из `Label` элементы имеют шрифт serif, но центр `Label` имеет стандартный шрифт sans serif.

## <a name="define-your-own-markup-extensions"></a>Определить собственные расширения разметки

Если появляется необходимость использования расширения разметки XAML, которые не доступны в Xamarin.Forms, вы можете [создавать собственные](creating.md).


## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Глава расширений разметки XAML из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Привязка данных](~/xamarin-forms/app-fundamentals/data-binding/index.md)
