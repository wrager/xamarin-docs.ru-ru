---
title: "Справочник по элементам управления DataPages"
ms.topic: article
ms.prod: xamarin
ms.assetid: 891615D0-E8BD-4ACC-A7F0-4C3725FBCC31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: fbbc7e8c93f8ed562381d9203060c862960f44b9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="datapages-controls-reference"></a>Справочник по элементам управления DataPages

![](~/media/shared/preview.png "Этот API в настоящее время находится в предварительной версии")

> [!IMPORTANT]
> Требуется DataPages [Xamarin.Forms темы](~/xamarin-forms/user-interface/themes/index.md) ссылку для отображения.


Xamarin.Forms DataPages Nuget включает несколько элементов управления, которые могут воспользоваться преимуществами привязки источника данных.

Чтобы использовать эти элементы управления в XAML, убедитесь пространство имен был включен см. пример `xmlns:pages` объявление ниже:

```xaml
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:pages="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
    x:Class="DataPagesDemo.Detail">
```

Приведенные ниже примеры включают `DynamicResource` ссылки, которые пришлось бы существует в словаре ресурсов проекта для работы. Также приводится пример создания [пользовательского элемента управления](#custom)

## <a name="built-in-controls"></a>Встроенные элементы управления

* [HeroImage](#heroimage)
* [ListItem](#listitem)

<a name="heroimage" />

### <a name="heroimage"></a>HeroImage

`HeroImage` Элемент управления имеет четыре свойства:

* Text
* Подробно
* Вложенный класс ImageSource
* Аспект

```xaml
<pages:HeroImage
    ImageSource="{ DynamicResource HeroImageImage }"
    Text="Keith Ballinger"
    Detail="Xamarin"
/>
```

**Android**

![](controls-images/heroimage-light-android.png "Элемент управления HeroImage на Android") ![ ] (controls-images/heroimage-dark-android.png "управления HeroImage на Android")

**iOS**

![](controls-images/heroimage-light-ios.png "Элемент управления HeroImage на iOS") ![ ] (controls-images/heroimage-dark-ios.png "HeroImage управления в iOS")


<a name="listitem" />

### <a name="listitem"></a>ListItem

`ListItem` Макет элемента управления аналогичен машинным кодом iOS и Android списка или таблицы строк, однако он может также использоваться как обычное представление. В примере кода ниже показан базирующееся в `StackLayout`, но также может использоваться в элементах управления с привязкой к данным scolling списка.

Существует пять свойств.

* Заголовок
* Подробно
* Вложенный класс ImageSource
* PlaceholdImageSource
* Аспект

```xaml
<StackLayout Spacing="0">
    <pages:ListItemControl
        Detail="Xamarin"
        ImageSource="{ DynamicResource UserImage }"
        Title="Miguel de Icaza"
        PlaceholdImageSource="{ DynamicResource IconImage }"
    />
```

Показать эти снимки экрана `ListItem` в iOS и Android платформ, при использовании светлой и темной темы:

**Android**

![](controls-images/listitem-light-android.png "Элемента управления ListItem в Android") ![ ] (controls-images/listitem-dark-android.png "элемента управления ListItem в Android")

**iOS**

![](controls-images/listitem-light-ios.png "Элемента управления ListItem в iOS") ![ ] (controls-images/listitem-dark-ios.png "элемента управления ListItem в iOS")


## <a name="custom-control-example"></a>Пример пользовательского элемента управления

Эта пользовательская цель `CardView` элемент управления является выглядеть собственного CardView Android.

Он будет содержать три свойства:

* Text
* Подробно
* Вложенный класс ImageSource

Цель — пользовательский элемент управления, который будет выглядеть, как в приведенном ниже коде (Обратите внимание, что пользовательский `xmlns:local` требуется который ссылается текущая сборка):

```xaml
<local:CardView
  ImageSource="{ DynamicResource CardViewImage }"
  Text="CardView Text"
  Detail="CardView Detail"
/>
```

Оно должно иметь вид снимки экрана ниже, с помощью цвета, соответствующие встроенные светлой и темной темах:

**Android**

![](controls-images/cardview-light-android.png "CardView пользовательского элемента управления в Android") ![ ] (controls-images/cardview-dark-android.png "CardView пользовательского элемента управления в Android")

**iOS**

![](controls-images/cardview-light-ios.png "CardView пользовательского элемента управления в iOS") ![ ] (controls-images/cardview-dark-ios.png "CardView пользовательского элемента управления в iOS")

<a name="custom" />

### <a name="building-the-custom-cardview"></a>Построение пользовательского CardView

1. [Подкласс DataView](#1)
2. [Определить шрифт, макет и поля](#2)
3. [Создание стилей для дочернего элемента управления.](#3)
4. [Создать шаблон макета элемента управления](#4)
5. [Добавление ресурсов тематических](#5)
6. [Задайте элемент ControlTemplate для класса CardView](#6)
7. [Добавьте элемент управления на страницу](#7)

<a name="1" />

#### <a name="1-dataview-subclass"></a>1. Подкласс DataView

Подкласс из `DataView` определяет привязываемые свойства для элемента управления.

```csharp
public class CardView : DataView
{
    public static readonly BindableProperty TextProperty =
        BindableProperty.Create ("Text", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Text
    {
        get { return (string)GetValue (TextProperty); }
        set { SetValue (TextProperty, value); }
    }

    public static readonly BindableProperty DetailProperty =
        BindableProperty.Create ("Detail", typeof (string), typeof (CardView), null, BindingMode.TwoWay);

    public string Detail
    {
        get { return (string)GetValue (DetailProperty); }
        set { SetValue (DetailProperty, value); }
    }

    public static readonly BindableProperty ImageSourceProperty =
        BindableProperty.Create ("ImageSource", typeof (ImageSource), typeof (CardView), null, BindingMode.TwoWay);

    public ImageSource ImageSource
    {
        get { return (ImageSource)GetValue (ImageSourceProperty); }
        set { SetValue (ImageSourceProperty, value); }
    }

    public CardView()
    {
    }
}
```

<a name="2" />

#### <a name="2-define-font-layout-and-margins"></a>2. Определить шрифт, макет и поля

Конструктор элементов управления будет понять, эти значения как часть разработки пользовательского интерфейса для пользовательского элемента управления. Там, где требуются спецификации платформой, `OnPlatform` используется элемент.

Обратите внимание, что некоторые значения относятся `StaticResource`s — они будут определены [шаг 5](#5).

```xml
<!-- CARDVIEW FONT SIZES -->
<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewTextFontSize">
        <On Platform="iOS, Android" Value="15" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewDetailFontSize">
        <On Platform="iOS, Android" Value="13" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewTextTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewTextTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewTextTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewTextlMargin">
        <On Platform="iOS" Value="12,10,12,4" />
        <On Platform="Android" Value="20,0,20,5" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewDetailTextColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewDetailTextColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewDetailTextColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="Thickness" x:Key="CardViewDetailMargin">
        <On Platform="iOS" Value="12,0,10,12" />
        <On Platform="Android" Value="20,0,20,20" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewBackgroundColor">
        <On Platform="iOS" Value="{StaticResource iOSCardViewBackgroundColor}" />
        <On Platform="Android" Value="{StaticResource AndroidCardViewBackgroundColor}" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewShadowSize">
        <On Platform="iOS" Value="2" />
        <On Platform="Android" Value="5" />
</OnPlatform>

<OnPlatform x:TypeArguments="x:Double" x:Key="CardViewCornerRadius">
        <On Platform="iOS" Value="0" />
        <On Platform="Android" Value="4" />
</OnPlatform>

<OnPlatform x:TypeArguments="Color" x:Key="CardViewShadowColor">
        <On Platform="iOS, Android" Value="#CDCDD1" />
</OnPlatform>
```

<a name="3" />

#### <a name="3-create-styles-for-the-controls-children"></a>3. Создание стилей для дочернего элемента управления.

Ссылаться на все элементы, определенные созданы дочерние элементы, которые будут использоваться в пользовательском элементе управления:

```xml
<!-- EXPLICIT STYLES (will be Classes) -->
<Style TargetType="Label" x:Key="CardViewTextStyle">
    <Setter Property="FontSize" Value="{ StaticResource CardViewTextFontSize }" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewTextTextColor }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewTextlMargin }" />
    <Setter Property="HorizontalTextAlignment" Value="Start" />
</Style>

<Style TargetType="Label" x:Key="CardViewDetailStyle">
    <Setter Property="HorizontalTextAlignment" Value="Start" />
    <Setter Property="TextColor" Value="{ StaticResource CardViewDetailTextColor }" />
    <Setter Property="FontSize" Value="{ StaticResource CardViewDetailFontSize }" />
    <Setter Property="HorizontalOptions" Value="Start" />
    <Setter Property="Margin" Value="{ StaticResource CardViewDetailMargin }" />
</Style>

<Style TargetType="Image" x:Key="CardViewImageImageStyle">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="Center" />
    <Setter Property="WidthRequest" Value="220"/>
    <Setter Property="HeightRequest" Value="165"/>
</Style>
```

<a name="4" />

#### <a name="4-create-the-control-layout-template"></a>4. Создать шаблон макета элемента управления

Визуальная разработка пользовательского элемента управления был объявлен явным образом в шаблоне элемента управления с использованием ресурсов, определенный выше:

```xml
<!--- CARDVIEW -->
<ControlTemplate x:Key="CardViewControlControlTemplate">
  <StackLayout
    Spacing="0"
    BackgroundColor="{ TemplateBinding BackgroundColor }"
  >

    <!-- CARDVIEW IMAGE -->
    <Image
      Source="{ TemplateBinding ImageSource }"
      HorizontalOptions="FillAndExpand"
      VerticalOptions="StartAndExpand"
      Aspect="AspectFill"
      Style="{ StaticResource CardViewImageImageStyle }"
    />

    <!-- CARDVIEW TEXT -->
    <Label
      Text="{ TemplateBinding Text }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewTextStyle }"
    />


    <!-- CARDVIEW DETAIL -->
    <Label
      Text="{ TemplateBinding Detail }"
      LineBreakMode="WordWrap"
      VerticalOptions="End"
      Style="{ StaticResource CardViewDetailStyle }" />

  </StackLayout>

</ControlTemplate>
```

<a name="5" />

#### <a name="5-add-the-theme-specific-resources"></a>5. Добавление ресурсов тематических

Так как пользовательский элемент управления, добавьте ресурсы, соответствующие темы, которые вы используете словарь ресурсов:

##### <a name="light-theme-colors"></a>Цвета "светлой" теме

```xaml
<Color x:Key="iOSCardViewBackgroundColor">#FFFFFF</Color>
<Color x:Key="AndroidCardViewBackgroundColor">#FFFFFF</Color>

<Color x:Key="AndroidCardViewTextTextColor">#030303</Color>
<Color x:Key="iOSCardViewTextTextColor">#030303</Color>

<Color x:Key="AndroidCardViewDetailTextColor">#8F8E94</Color>
<Color x:Key="iOSCardViewDetailTextColor">#8F8E94</Color>
```

##### <a name="dark-theme-colors"></a>Цвета темной темы

```xaml
<!-- CARD VIEW COLORS -->
            <Color x:Key="iOSCardViewBackgroundColor">#404040</Color>
            <Color x:Key="AndroidCardViewBackgroundColor">#404040</Color>

            <Color x:Key="AndroidCardViewTextTextColor">#FFFFFF</Color>
            <Color x:Key="iOSCardViewTextTextColor">#FFFFFF</Color>

            <Color x:Key="AndroidCardViewDetailTextColor">#B5B4B9</Color>
            <Color x:Key="iOSCardViewDetailTextColor">#B5B4B9</Color>
```

<a name="6" />

#### <a name="6-set-the-controltemplate-for-the-cardview-class"></a>6. Задайте элемент ControlTemplate для класса CardView

Наконец, убедитесь в классе C#, созданных в [шаг 1](#1) использует шаблон элемента управления, определенных в [шаг 4](#4) с помощью `Style` `Setter` элемент

```xml
<Style TargetType="local:CardView">
    <Setter Property="ControlTemplate" Value="{ StaticResource CardViewControlControlTemplate }" />
  ... some custom styling omitted
  <Setter Property="BackgroundColor" Value="{ StaticResource CardViewBackgroundColor }" />
</Style>
```

<a name="7" />

#### <a name="7-add-the-control-to-a-page"></a>7. Добавьте элемент управления на страницу

`CardView` Управления теперь можно добавить на страницу. В следующем примере показано он размещен в `StackLayout`:

```xaml
<StackLayout Spacing="0">
  <local:CardView
    Margin="12,6"
    ImageSource="{ DynamicResource CardViewImage }"
    Text="CardView Text"
    Detail="CardView Detail"
  />
</StackLayout>

```
