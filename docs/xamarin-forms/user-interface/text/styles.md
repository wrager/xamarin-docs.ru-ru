---
title: "Стили"
description: "Стиль текста в Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 764fb77bedf9e00427348e95b4ecf029ae94741f
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="styles"></a>Стили

_Стиль текста в Xamarin.Forms_


Стили можно настроить внешний вид меток, записи и редакторов. Стили может быть определен один раз и используются несколько представлений, но стиль может использоваться только с представлениями одного типа.
Стили можно предоставить `Key` и применяются выборочно с помощью конкретного элемента управления `Style` свойство.

В этой статье рассматриваются следующие темы:

- **[Встроенные стили](#Built-In_Styles)**  &ndash; использовать встроенные стили для стиля текстового представления в приложении.
- **[Пользовательские стили](#Custom_Styles)**  &ndash; определить пользовательские стили при встроенные возможности оказывается недостаточно.
- **[Применение стилей](#Applying_Styles)**  &ndash; применять пользовательские и встроенные стили к представлениям.
- **[Специальные возможности](#Accessibility)**  &ndash; убедитесь, что текст учитывает параметры специальных возможностей.

<a name="Built-In_Styles" />

## <a name="built-in-styles"></a>Встроенные стили

Xamarin.Forms включает несколько [встроенные](http://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) стили для общих сценариев:

- `BodyStyle`
- `CaptionStyle`
- `ListItemDetailTextStyle`
- `ListItemTextStyle`
- `SubtitleStyle`
- `TitleStyle`

Для применения одного из встроенных стилей, используйте `DynamicResource` расширение разметки, чтобы задать стиль:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

В C#, встроенные стили выбираются из `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

![](styles-images/builtinstyles.png "Пример стилей устройства")

<a name="Custom_Styles" />

## <a name="custom-styles"></a>Пользовательские стили

Стили состоят из методов задания и задания состоят из свойств и значений свойств будет присвоено.

В C# пользовательский стиль метки красным цветом, размером 30 будут определены следующим образом:

```csharp
var LabelStyle = new Style (typeof(Label)) {
    Setters = {
        new Setter {Property = Label.TextColorProperty, Value = Color.Red},
        new Setter {Property = Label.FontSizeProperty, Value = 30}
    }
};

var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

В XAML:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style x:Key="LabelStyle" TargetType="Label">
            <Setter Property="TextColor" Value="Red"/>
            <Setter Property="FontSize" Value="30"/>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>

<ContentPage.Content>
    <StackLayout>
        <Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
    </StackLayout>
</ContentPage.Content>
```

Обратите внимание, что ресурсы (включая все стили) определяются в `ContentPage.Resources`, который является одноуровневым знакомую `ContentPage.Content` элемента.

![](styles-images/customstyle.png "Пример пользовательские стили")

<a name="Applying_Styles" />

## <a name="applying-styles"></a>Применение стилей

После создания стиля, оно может применяться в сопоставлении представления его `TargetType`.

В языке XAML, пользовательские стили применяются к представлениям, указав их `Style` свойство с `StaticResource` расширения разметки, нужный стиль ссылки:

```xaml
<Label Text="Check out my style." Style="{StaticResource LabelStyle}" />
```

В C# стили можно либо применяется непосредственно к представлению или добавлены и получать с страницы `ResourceDictionary`. Чтобы добавить напрямую:

```csharp
var label = new Label { Text = "Check out my style.", Style = LabelStyle };
```

Чтобы добавить и получить на странице `ResourceDictionary`:

```csharp
this.Resources.Add ("LabelStyle", LabelStyle);
label.Style = (Style)Resources["LabelStyle"];
```

Встроенные стили применяются по-разному, так как они должны отвечать на параметры специальных возможностей. Для применения встроенных стилей в XAML, `DynamicResource` используется расширение разметки:

```xaml
<Label Text="I'm a Title" Style="{DynamicResource TitleStyle}"/>
```

В C#, встроенные стили выбираются из `Device.Styles`:

```csharp
label.Style = Device.Styles.TitleStyle;
```

## <a name="accessibility"></a>Специальные возможности

Встроенные стили существует для упрощения учитывают параметры расширенного доступа. При использовании любой из встроенных стилей, размеры шрифтов автоматически увеличится, если пользователь задает их параметры расширенного доступа соответствующим образом.

Рассмотрим следующий пример той же страницы представления, для которого установлен с помощью встроенных стилей параметры специальных возможностей включенных и отключенных:

Отключено:

![](styles-images/pre-access.png "Стили устройства с отключенной специальных возможностей")

Включено:

![](styles-images/post-access.png "Стили устройства включена специальных возможностей")

Для обеспечения доступности, убедитесь, что использования встроенных стилей как основу для любого стиля, связанных с текстом в приложении, и постоянно помощи стилей. В разделе [стили](~/xamarin-forms/user-interface/styles/index.md) Дополнительные сведения о расширении и работы со стилями в целом.


## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms, Глава 12](https://developer.xamarin.com/r/xamarin-forms/book/chapter12.pdf)
- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Style](http://developer.xamarin.comhttps://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
