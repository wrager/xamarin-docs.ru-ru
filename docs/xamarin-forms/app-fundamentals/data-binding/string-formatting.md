---
title: "Строка форматирования"
description: "Использование привязки данных для форматирования и отображения объектов в виде строк"
ms.topic: article
ms.prod: xamarin
ms.assetid: 978C85B7-CB58-4483-A131-21B381A865E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 3b54ed876857f0cd04d7a304ff05b9710fbd9e04
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="string-formatting"></a>Строка форматирования

Иногда бывает удобно использовать привязки данных для отображения строковое представление объекта или значение. Например, может потребоваться использовать `Label` для отображения текущего значения `Slider`. В этой привязке данных `Slider` является исходным и целевым объектом является `Text` свойство `Label`.

При отображении строки в коде, наиболее мощный инструмент является статическим [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) метод. Строка форматирования содержит коды для различных типов объектов форматирования, и может содержать другой текст и форматируемого значения. В разделе [типы форматирования в .NET](/dotnet/standard/base-types/formatting-types/) для получения дополнительных сведений о форматировании для строки.

## <a name="the-stringformat-property"></a>Свойство StringFormat

Эта функция позволяет переносится в привязки данных: задать [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) свойство `Binding` (или [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.StringFormat/) свойство `Binding` расширение разметки) для Строка с один заполнитель форматирования стандартных .NET:

```xaml
<Slider x:Name="slider" />
<Label Text="{Binding Source={x:Reference slider},
                      Path=Value,
                      StringFormat='The slider value is {0:F2}'}" />
```

Обратите внимание, что строка форматирования отделяется символов одинарные кавычки (апостроф), чтобы средство синтаксического анализа XAML избежать обработке фигурные скобки как другое расширение разметки XAML. В противном случае, строка без символ одинарной кавычки является та же строка будет использовать для отображения значения с плавающей запятой в вызове `String.Format`. Указание форматирования `F2` приводит значение для отображения с двумя десятичными разрядами.

`StringFormat` Свойство имеет смысл только если целевое свойство имеет тип `string`, и режим привязки является `OneWay` или `TwoWay`. Для двусторонней привязки `StringFormat` применимо только для передачи из источника в целевой объект значения.

Как можно будет увидеть в следующей статье на [путь привязки](binding-path.md), привязки данных может стать довольно сложным и сложные. При отладке эти привязки данных, можно добавить `Label` в XAML-файл с `StringFormat` для отображения некоторых промежуточных результатов. Даже если он используется только для отображения типа объекта, который может быть полезным.

**Форматирования строк** страницы показано несколько способов применения `StringFormat` свойство:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="DataBindingDemos.StringFormattingPage"
             Title="String Formatting">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>

            <Style TargetType="BoxView">
                <Setter Property="Color" Value="Blue" />
                <Setter Property="HeightRequest" Value="2" />
                <Setter Property="Margin" Value="0, 5" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <Slider x:Name="slider" />
        <Label Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='The slider value is {0:F2}'}" />

        <BoxView />

        <TimePicker x:Name="timePicker" />
        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time,
                              StringFormat='The TimeSpan is {0:c}'}" />

        <BoxView />

        <Entry x:Name="entry" />
        <Label Text="{Binding Source={x:Reference entry},
                              Path=Text,
                              StringFormat='The Entry text is &quot;{0}&quot;'}" />

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:DateTime.Now}">
            <Label Text="{Binding}" />
            <Label Text="{Binding Path=Ticks,
                                  StringFormat='{0:N0} ticks since 1/1/1'}" />
            <Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
            <Label Text="{Binding StringFormat='The long date is {0:D}'}" />
        </StackLayout>

        <BoxView />

        <StackLayout BindingContext="{x:Static sys:Math.PI}">
            <Label Text="{Binding}" />
            <Label Text="{Binding StringFormat='PI to 4 decimal points = {0:F4}'}" />
            <Label Text="{Binding StringFormat='PI in scientific notation = {0:E7}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Привязки на `Slider` и `TimePicker` показано использование спецификации формата, определенного для `double` и `TimeSpan` типов данных. `StringFormat` , Отображается текст из `Entry` представление показано указание двойные кавычки в строке форматирования с помощью `&quot;` сущности HTML.

Следующий раздел в файле XAML `StackLayout` с `BindingContext` значение `x:Static` расширение разметки, которое ссылается на статический `DateTime.Now` свойство. Первая привязка не имеет свойств:

```xaml
<Label Text="{Binding}" />
```

Это просто отображает `DateTime` значение `BindingContext` форматирования по умолчанию. Вторая привязка отображает `Ticks` свойство `DateTime`, тогда как другие привязки отображения `DateTime` себя с помощью специального форматирования. Обратите внимание, это `StringFormat`:

```xaml
<Label Text="{Binding StringFormat='The {{0:MMMM}} specifier produces {0:MMMM}'}" />
```

Если необходимо отобразить левой или правой фигурной скобки в строке форматирования, просто используйте пару из них.

Последние наборы раздел `BindingContext` значению `Math.PI` и отображает его с форматирование по умолчанию и два различных типа форматирование чисел.

Вот программу на всех трех платформ.

[![Строка форматирования](string-formatting-images/stringformatting-small.png "строка форматирования")](string-formatting-images/stringformatting-large.png "строка форматирования")

## <a name="viewmodels-and-string-formatting"></a>ViewModels и форматирования строк

При использовании `Label` и `StringFormat` для отображения значения представление, которое также является целевым объектом ViewModel, можно определить привязки в представлении для `Label` или из ViewModel для `Label`. В общем случае второй подход лучше всего подходит, так как он проверяет работоспособность привязки между View и ViewModel.

Этот подход показан в **лучше средства выбора цвета** пример, который использует же ViewModel как **простой селектор цвета** программы, представленной [ **режим привязки** ](binding-mode.md) статьи:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.BetterColorSelectorPage"
             Title="Better Color Selector">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <StackLayout.BindingContext>
            <local:HslColorViewModel Color="Sienna" />
        </StackLayout.BindingContext>

        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <StackLayout Margin="10, 0">
            <Label Text="{Binding Name}" />

            <Slider Value="{Binding Hue}" />
            <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

            <Slider Value="{Binding Saturation}" />
            <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

            <Slider Value="{Binding Luminosity}" />
            <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
        </StackLayout>
    </StackLayout>
</ContentPage>    
```

Теперь имеется три пары `Slider` и `Label` элементы, которые привязаны к тому же исходное свойство в `HslColorViewModel` объекта. Единственным различием является `Label` имеет `StringFormat` свойство для отображения каждого `Slider` значение.

[![Цвет выделения лучше](string-formatting-images/bettercolorselector-small.png "лучше цвет выделения")](string-formatting-images/bettercolorselector-large.png "лучше цвета выделения")

Возможно, вас интересует как можно отобразить значения RGB (красный, зеленый, синий) в традиционных двух цифр в шестнадцатеричном формате. Эти целочисленные значения непосредственно недоступны из `Color` структуры. Одним из решений будет вычислить целочисленных значений компонентов цвета в ViewModel и предоставить их как свойства. Затем можно форматировать их с помощью `X2` спецификацией форматирования.

Другой подход является более общим: можно написать *преобразователь значений привязки* как описано в статье более поздней версии, [ **преобразователи значений привязки**](converters.md).

Однако следующей статье исследуются [ **путь привязки** ](binding-path.md) в более подробно и показывают, как его использовать для ссылки на вложенных свойств и элементов в коллекции.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Данные привязки Глава из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
