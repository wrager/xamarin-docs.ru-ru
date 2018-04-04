---
title: Специфический для платформы iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности операций ввода-вывода, которые встроены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: 7826962cd3bf9595a63841e3f2d9fb377d1a0574
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="ios-platform-specifics"></a>Специфический для платформы iOS

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности операций ввода-вывода, которые встроены в Xamarin.Forms._

На iOS Xamarin.Forms содержит следующие платформы особенности:

- Размытие поддержка для любых [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Дополнительные сведения см. в разделе [применение Размытие](#blur).
- Управление отображением заголовка страницы крупным заголовком на панели навигации страницы. Дополнительные сведения см. в разделе [отображения крупных названий](#large_title).
- Обеспечение содержимое страницы располагается на область экрана, что все устройства iOS. Дополнительные сведения см. в разделе [включения безопасном структуры макета области](#safe_area_layout).
- Панель навигации полупрозрачным. Дополнительные сведения см. в разделе [внесения полупрозрачные панель навигации](#translucent_navigation_bar).
- Управление цветом ли текст строки состояния на [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) настраивается для сопоставления яркость на панели навигации. Дополнительные сведения см. в разделе [Настройка цветового режима текст строки состояния](#status_bar_color_mode).
- Обеспечение, введена текст помещается в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) , корректируя размер шрифта. Дополнительные сведения см. в разделе [корректировки значения Font-Size записи](#adjust_font_size).
- Управление, когда происходит выбор элементов в [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Дополнительные сведения см. в разделе [управление Выбор элемента](#picker_update_mode).
- Настройка видимости панели состояния на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Дополнительные сведения см. в разделе [установки видимость строки состояния на странице](#set_status_bar_visibility).
- Управление ли [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) жест касания обрабатывает или передает его на его содержимое. Дополнительные сведения см. в разделе [задержки содержимого штрихи в ScrollView](#delay_content_touches).

<a name="blur" />

## <a name="applying-blur"></a>Применение размытия

Используется для Размытие содержимого располагаются ниже его этой платформой и используются в языке XAML, задав [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) вложенное свойство в значение [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
    <Image Source="monkeyface.png" />
    <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

`BoxView.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен используется для применения эффекта размытия с [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) предоставляет четыре перечисления значения: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), и [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Результатом является то, что указанный [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) применяется к [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) экземпляра какие Размытие [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) располагаются ниже его:

![](ios-images/blur-effect.png "Чтобы минимизировать эффект от платформы")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Отображения крупных названий

Этой платформой используется для отображения заголовка страницы в виде крупным заголовком на панели навигации для устройств, использующих iOS 11 или выше. Большой Заголовок выравнивается по левому краю и использует большего размера шрифта и переходит на стандартный заголовок при пользователь начинает прокрутка содержимого, чтобы использовалась площадь экрана эффективно. Однако в альбомной ориентации заголовок будет возвращать относительно центральной части панели навигации, чтобы оптимизировать макет содержимого. Он используется в языке XAML, задав `NavigationPage.PrefersLargeTitles` присоединенному свойству `boolean` значение:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Также он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `NavigationPage.SetPrefersLargeTitle` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространство имен, определяет, включены ли крупных названий.

Условии, что включена крупных названий [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), все страницы в стеке навигации будут отображены в крупных названий. Это поведение можно переопределить на страницах, задав `Page.LargeTitleDisplay` вложенное свойство в значение `LargeTitleDisplayMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Кроме того поведение страницы может быть переопределено с помощью плавного API в C#:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `Page.SetLargeTitleDisplay` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен, управляет поведением большого заголовка на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), с `LargeTitleDisplayMode` перечисления, предоставляя три возможные значения:

- `Always` — Принудительное панели навигации и шрифта размер для использования большого формата.
- `Automatic` — использовать тот же стиль (большие или малые) с предыдущим элементом в стеке навигации.
- `Never` — принудительно использовать обычный, небольшой формата панели навигации.

Кроме того `SetLargeTitleDisplay` метод может использоваться для включения значений перечисления, вызвав `LargeTitleDisplay` метод, который возвращает текущий `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Результатом является то, что указанный `LargeTitleDisplayMode` применяется к [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), которой управляет поведением большого заголовка:

![](ios-images/large-title.png "Чтобы минимизировать эффект от платформы")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Включение зарезервированная область макета

Чтобы убедиться, что содержимое страницы расположен на область экрана, безопасного для всех устройств, использующих iOS 11 и более поздней версии используется этой платформой. В частности, полезно убедитесь, что содержимое не обрезается устройства со скругленными углами, индикатор домашней или корпусе датчика на iPhone X. Он используется в языке XAML, задав `Page.UseSafeArea` присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

`Page.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `Page.SetUseSafeArea` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространство имен, определяет, включено ли в руководстве зарезервированная область макета.

Результатом является то, что содержимое страницы может располагаться на область экрана, который является безопасным для всех iPhone:

[![](ios-images/safe-area-layout.png "Руководство по зарезервированная область макета")](ios-images/safe-area-layout-large.png#lightbox "руководство зарезервированная область макета")

> [!NOTE]
> Безопасные области, определяемой Apple, используемый для задания в Xamarin.Forms [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) свойство и переопределяет все предыдущие значения этого свойства, заданные.

Зарезервированная область может настраиваться путем извлечения его [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) значение с `Page.SafeAreaInsets` метод [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен. Затем могут редактироваться как необходимые и повторно назначены `Padding` свойства в конструкторе страницы или [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) переопределения:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Делая полупрозрачные панель навигации

Используется для изменения прозрачности панели навигации этой платформой и используются в языке XAML, задав [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) присоединенному свойству `boolean` значение:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

`NavigationPage.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен, позволяет сделать прозрачным на панели навигации. Кроме того [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) класса в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространство имен также содержит [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) метод, который восстанавливает состояние по умолчанию на панели навигации и [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) метод, который можно использовать для переключения прозрачности панель навигации, вызвав [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) метод:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Результатом является то, что прозрачность на панели навигации можно изменить:

![](ios-images/translucent-navigation-bar.png "Специфический для платформы полупрозрачные панель навигации")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Настройка строки текстовый режим цвета состояния

Эта определяет конкретную платформу ли текст строки состояния цвета на [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) настраивается для сопоставления яркость на панели навигации. Он используется в языке XAML, задав [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) вложенное свойство в значение [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) перечисления:

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

`NavigationPage.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен, элементы управления ли текст строки состояния цвета на [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) настраивается для сопоставления яркость панели навигации с [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) перечисления, предоставляя два возможных значения:

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) — Указывает строки цвет текста состояния не переводится.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) — Указывает совпадение строки цвет текста состояния яркость на панели навигации.

Кроме того [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) метод может использоваться для получения текущего значения [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) перечисления, которое применяется к [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Результат —, состояние цвет текста для столбца [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) можно настраивать, чтобы соответствовать яркость на панели навигации. В этом примере состояние изменения цвета текста строки как пользователь переключается между [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) и [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) страниц [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Строка состояния текст цвет режим платформой")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Изменение размера шрифта элемента

Это специфический для платформы используется для масштабирования размер шрифта для [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) чтобы убедиться, что введено текст помещается в элемент управления. Он используется в языке XAML, задав [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) присоединенному свойству `boolean` значение:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

`Entry.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен используется для масштабирования размер шрифта текста введено, чтобы убедиться, что она помещается в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Кроме того [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) класса в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространство имен также содержит [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) метод, который отключает этой платформой, и [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) метод, который можно использовать для переключения размер шрифта, масштабирование путем вызова [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) метод:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Результатом является, размер шрифта для [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) масштабируется, чтобы убедиться, что введено текст помещается в элемент управления:

![](ios-images/entry-font-size.png "Измените запись шрифта размер определяемых платформой")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Выбор элемента управления

Этой платформой определяет, когда происходит выбор элементов в [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), предоставляя пользователю возможность указать, что выбор элемента происходит при просмотре элементов в элементе управления или только в том случае, когда **сделать** нажата кнопка. Он используется в языке XAML, задав `Picker.UpdateMode` вложенное свойство в значение `UpdateMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

`Picker.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `Picker.SetUpdateMode` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен используется для управления, когда происходит выбор элементов, с `UpdateMode` перечисления, предоставляя два возможных значения:

- `Immediately` — Выбор элемента происходит при просмотре элементов в [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Это поведение по умолчанию в Xamarin.Forms.
- `WhenFinished` — Выбор элемента происходят, когда пользователь нажал **сделать** кнопку в [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Кроме того `SetUpdateMode` метод может использоваться для включения значений перечисления, вызвав `UpdateMode` метод, который возвращает текущий `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Результатом является то, что указанный `UpdateMode` применяется к [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), который управляет, когда происходит выбор элементов:

[![](ios-images/picker-updatemode.png "Выбор UpdateMode специфический для платформы")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Параметр в строке состояния видимости на странице

Этой платформой используется для задания видимость строки состояния на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), а также возможность управлять как строка состояния или выходе из `Page`. Он используется в языке XAML, задав `Page.PrefersStatusBarHidden` вложенное свойство в значение `StatusBarHiddenMode` перечисления и при необходимости `Page.PreferredStatusBarUpdateAnimation` вложенное свойство в значение `UIStatusBarAnimation` перечисления:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

`Page.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `Page.SetPrefersStatusBarHidden` Метод в `Xamarin.Forms.PlatformConfiguration.iOSSpecific` пространства имен, используемый для задания видимость строки состояния на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) , указав одно из `StatusBarHiddenMode` значения перечисления: `Default`, `True` , или `False`. `StatusBarHiddenMode.True` И `StatusBarHiddenMode.False` значения задать видимость полосы состояния вне зависимости от ориентации устройства и `StatusBarHiddenMode.Default` значение скрывает строку состояния в среде по вертикали compact.

Результатом является, видимость строки состояния на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) можно задать:

![](ios-images/hide-status-bar.png "Строка состояния видимости специфический для платформы")

> [!NOTE]
> На [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), указанный `StatusBarHiddenMode` значение перечисления также будет обновлять строку состояния на всех дочерних страниц. На всех остальных [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-производные типы, указанный `StatusBarHiddenMode` значение перечисления только обновит строки состояния на текущей странице.

`Page.SetPreferredStatusBarUpdateAnimation` Метод используется для установки как в строке состояния или выходе из [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) , указав одно из `UIStatusBarAnimation` значения перечисления: `None`, `Fade`, или `Slide`. Если `Fade` или `Slide` указано значение перечисления, 0,25 второй анимации выполняются как строка состояния или выходе из `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Задержка изменения содержимого в ScrollView

Неявные таймер запускается в том случае, когда жест касания начинается в [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) в iOS и `ScrollView` решает, на основании действий пользователя в течение интервала таймера, он должен обрабатывать жест или передать его содержимого. По умолчанию iOS `ScrollView` содержимого штрихи задержки, но это может вызвать проблемы в некоторых случаях при работе с `ScrollView` содержимого не выиграть жест, когда это должно происходить. Таким образом, эта определяет конкретную платформу ли `ScrollView` жест касания обрабатывает или передает его на его содержимое. Он используется в языке XAML, задав `ScrollView.ShouldDelayContentTouches` присоединенному свойству `boolean` значение:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Метод указывает, что этой платформой будет запускаться только в iOS. `ScrollView.SetShouldDelayContentTouches` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен, можно указать, является ли [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) жест касания обрабатывает или передает его на его содержимое. Кроме того `SetShouldDelayContentTouches` метод может использоваться для переключения задержки содержимого штрихи путем вызова `ShouldDelayContentTouches` метод для возврата, отложена ли изменения содержимого:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

В результате [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) можно отключить задержки, поэтому получение содержимого штрихи, в этом сценарии [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) получает жест вместо [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) страница [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "Задержка ScrollView содержимое касается определяемых платформой")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>Сводка

В этой статье показано, как использовать платформу особенности операций ввода-вывода, которые встроены в Xamarin.Forms. Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты.


## <a name="related-links"></a>Связанные ссылки

- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
