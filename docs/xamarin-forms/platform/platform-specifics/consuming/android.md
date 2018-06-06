---
title: Android платформы подробные сведения
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать Android платформы особенности, которые встроены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 05f1fc6158e9a20892ab4a4b49b33e4eac6bc5e5
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34733065"
---
# <a name="android-platform-specifics"></a>Android платформы подробные сведения

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать Android платформы особенности, которые встроены в Xamarin.Forms._

В Android Xamarin.Forms содержит следующие платформы особенности:

- Установка режима работы экранную клавиатуру. Дополнительные сведения см. в разделе [режим ввода мягкие клавиатуры](#soft_input_mode).
- Включение быстрого прокрутки в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Дополнительные сведения см. в разделе [Включение быстрой прокрутки в ListView](#fastscroll).
- Включение считывания между страницами в [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Дополнительные сведения см. в разделе [Включение считывания между страницами в TabbedPage](#enable_swipe_paging).
- Управление Z-порядка визуальных элементов, чтобы определить порядок отображения. Дополнительные сведения см. в разделе [управление повышение визуальные элементы](#elevation).
- Отключение [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) и [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) страницы события жизненного цикла на приостановить и возобновить соответственно, для приложений, использующих совместимости приложений. Дополнительные сведения см. в разделе [отключение Disappearing и появления событий жизненного цикла страницы](#disable_lifecycle_events).
- Управление ли [ `WebView` ](xref:Xamarin.Forms.WebView) может отображать смешанное содержимое. Дополнительные сведения см. в разделе [Включение смешанного содержимого в WebView](#webview-mixed-content).
- Установка метода ввода параметров редактора для программируемой клавиатуры для [ `Entry` ](xref:Xamarin.Forms.Entry). Дополнительные сведения см. в разделе [параметры редактора метода ввода для параметра записи](#entry-imeoptions).
- Отключение режима устаревших цвета на поддерживаемой [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. в разделе [отключение цветовая маркировка прежних версий](#legacy-color-mode).
- С помощью заполнения по умолчанию и значения Android кнопок. Дополнительные сведения см. в разделе [с помощью кнопок Android](#button-padding-shadow).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Режим ввода Экранная клавиатура

Позволяет задать режим работы для области ввода Экранная клавиатура этой платформой и используются в языке XAML, задав [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) вложенное свойство в значение [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) перечисление:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

`Application.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) пространства имен, используемый для задания Экранная клавиатура область ввода режим, с [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) предоставляет два значения перечисления: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) и [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). `Pan` Использует значение [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) корректировки параметр, который не размера окна, когда элемент управления вводом в фокусе. Вместо этого содержимое окна сдвигаемых, чтобы текущий фокус не закрыты Экранная клавиатура. `Resize` Использует значение [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) корректировки параметр, который изменяет размер окна, если элемент управления вводом в фокусе, чтобы освободить место для Экранная клавиатура.

Результатом является то, что Экранная клавиатура входом область, которую можно задать режим работы, когда элемент управления вводом в фокусе:

[![](android-images/pan-resize.png "Экранная клавиатура, специфический для платформы режим работы")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Включение быстрого прокрутки в ListView

Позволяет включить быстрый прокрутку данных в этой платформой [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Он используется в языке XAML, задав `ListView.IsFastScrollEnabled` присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. `ListView.SetIsFastScrollEnabled` Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) пространства имен используется для включения быстрой прокрутке данные в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Кроме того `SetIsFastScrollEnabled` метод может использоваться для переключения быстрого прокрутки путем вызова `IsFastScrollEnabled` метод для возврата, включен ли быстрый прокрутка:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Результатом является это быстрое прокручивание через данные [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) можно включить, который изменяет размер бегунка прокрутки:

[![](android-images/fastscroll.png "ListView FastScroll платформы")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Включение считывания между страницами в TabbedPage

Это специфический для платформы используется для включения считывания с горизонтальной пальцем жест между страницами в [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Он используется в языке XAML, задав [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) присоединенному свойству `boolean` значение:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

`TabbedPage.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) пространства имен используется для включения считывания между страницами в [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Кроме того `TabbedPage` класса в `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` пространство имен также содержит [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) метод, позволяющий этой платформой, и [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) метод, который отключает этой платформой. [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) Вложенное свойство зависимостей, и [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) метод, используется для установки количества страниц, которые должны сохраняться в состоянии простоя, слева от текущей страницы.

Результатом является разбиение на страницы, проведите по страницам, отображаемого элементом [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) включен:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Управление повышение визуальных элементов

Этой платформой, используемых для управления повышения уровня или Z-порядка визуальных элементов в приложениях, предназначенных для API 21 или больше. Повышение визуальный элемент определяет его рисования порядке, в котором визуальные элементы с более высоким значением Z occluding визуальные элементы с более низкими значениями Z. Он используется в языке XAML, задав `VisualElement.Elevation` присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

`Button.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. `VisualElement.SetElevation` Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) пространства имен, используемый для задания повышение визуальный элемент значения NULL `float`. Кроме того `VisualElement.GetElevation` метод может использоваться для получения значения высоты визуального элемента.

Получается, что повышение визуальные элементы можно управлять, чтобы визуальные элементы с более высоким значением Z скрывать визуальные элементы с более низкими значениями Z. Таким образом, в этом примере второй [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) отображается над [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) , так как он имеет более высокое значение повышения прав:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Отключение события жизненного цикла страницы исчезнувшей и отображения объектов

Это специфический для платформы используется для отключения [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) и [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) событий страницы в приложении, приостанавливать и возобновлять соответственно, для приложений, использующих совместимости приложений. Кроме того, он включает возможность управления отображением Экранная клавиатура начинать, если оно отображалось на Пауза, при условии, что режим работы Экранная клавиатура имеет значение [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Обратите внимание, что эти события включены по умолчанию для сохранения существующих поведения для приложения, зависящие от события. При выключении эти события соответствует цикла событий pre AppCompat цикла событий совместимости приложений.

Это специфический для платформы можно использовать в XAML, задав [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), и [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) присоединенных свойств для `boolean` значения:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

`Application.Current.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) пространство имен, используется для включения или отключения срабатывание [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) событий страницы, когда приложение переходит в фоновом режиме. [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Метод используется для включения или отключения срабатывание [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) событий страницы, когда приложение выходит из фона. [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Используется метод управления ли экранная клавиатура отображается на resume, если оно отображалось на паузы, предоставлено значение режима работы Экранная клавиатура [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

В результате [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) и [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) события страницы в приостановить приложение не будет инициировать и возобновить соответственно, а также было Экранная клавиатура, если отображается, когда приложение был приостановлен, он также отображается при возобновлении приложения:

[![](android-images/keyboard-on-resume.png "Жизненный цикл события платформы")](android-images/keyboard-on-resume-large.png#lightbox "жизненного цикла события платформы")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Включение смешанного содержимого в WebView

Это управляет платформой ли [ `WebView` ](xref:Xamarin.Forms.WebView) можно Отображение разнородного содержимого в приложениях, предназначенных API 21 или выше. Смешанный содержимое является начальной загрузке через HTTPS-соединение, но подключения по протоколу HTTP, который загружает ресурсы (например, изображения, аудио, видео, таблицы стилей, сценарии). Он используется в языке XAML, задав [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) вложенное свойство в значение [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

`WebView.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен используется для управления ли смешанное содержимое может отображаться, с [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Перечисление, предоставляя три возможных значения:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) позволит источник HTTPS для загрузки содержимого из источника HTTP.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) не разрешит источник HTTPS для загрузки содержимого из источника HTTP.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) — Указывает, что [ `WebView` ](xref:Xamarin.Forms.WebView) попытается несовместима с данным подходом последние веб-браузере устройства. Часть содержимого HTTP может быть разрешен загружается источник HTTPS и других типов содержимого будет заблокирован. Типы содержимого, которые заблокированы или разрешены могут изменяться при каждом выпуске операционной системы.

Результатом является то, что указанный [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) применяется значение [ `WebView` ](xref:Xamarin.Forms.WebView), который управляет отображением смешанное содержимое:

[![WebView смешанный платформой обработки содержимого](android-images/webview-mixedcontent.png "WebView смешанный платформой обработки содержимого")](android-images/webview-mixedcontent-large.png#lightbox "WebView смешанный платформой обработки содержимого")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Параметры редактора метода ввода для параметра записи

Этой платформой задает метод ввода параметров редактора (IME) для программируемой клавиатуры для [ `Entry` ](xref:Xamarin.Forms.Entry). Это включает настройку кнопки действия пользователя в нижнем углу Экранная клавиатура и взаимодействие с `Entry`. Он используется в языке XAML, задав [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) вложенное свойство в значение [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) перечисления:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

`Entry.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен используется для задания параметра действие метода ввода для программируемой клавиатуры для [ `Entry` ](xref:Xamarin.Forms.Entry), с [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) перечисления, предоставляя следующие значения:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) — Указывает, что ключ не определенное действие не требуется и что базового элемента управления выведет самостоятельно при можно. Это будет `Next` или `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) — Указывает, что ключ не действия будут доступны.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) — Указывает, что ключ действия будет выполнять операцию «go», выполнив пользователю целевой текст они введены.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) — Указывает, что ключ действия выполняет операцию «поиск», принимающий пользователю результаты поиска в тексте введено.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) — Указывает, что ключ действия будет выполнять операцию «отправить», доставка текст на целевой объект.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) — Указывает, что ключ действия выполнит операцию «Далее» получение пользователя к следующему полю, принимается текста.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) — Указывает, что ключ действия выполнит операцию «Готово» закрывает экранную клавиатуру.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) — Указывает, что ключ действия выполнит операцию «раньше» пользователь с переводом в предыдущем поле, которое будет принимать текст.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) — маску для выбора параметров действия.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) — Указывает, что средство проверки орфографии будет ни сведения от пользователя, а также предложить исправления, основанные на то, что данные ранее, введенные пользователем.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) — Указывает, что пользовательский Интерфейс не должны во весь экран.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) — Указывает, что пользовательский Интерфейс не будет отображаться для извлеченного текста.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) — Указывает, что пользовательский Интерфейс не будет отображаться для пользовательских действий.

Результатом является то, что указанный [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) значение применяется к Экранная клавиатура для [ `Entry` ](xref:Xamarin.Forms.Entry), который задает параметры редактора метода ввода:

[![Платформой редактора метода ввода записи](android-images/entry-imeoptions.png "платформой редактора метода ввода записи")](android-images/entry-imeoptions-large.png#lightbox "платформой редактора метода ввода записи")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Отключение режима цвет прежних версий

Некоторые представления Xamarin.Forms признаков устаревших цветового режима. В этом режиме при [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) представления свойству `false`, переопределяет цвета, установленные пользователем с помощью собственного цветов по умолчанию для отключенного состояния представления. Для обеспечения обратной совместимости, этот цвет устаревший режим остается поведение по умолчанию для поддерживаемых представлений.

Этой платформой отключает этот режим устаревшего цвет, так что установленные пользователем для представления цветов остаются даже в том случае, если представление будет отключена. Он используется в языке XAML, задав [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) присоединенному свойству `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен используется для управления ли цвет устаревший режим отключен. Кроме того [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) метод может использоваться для возврата, не отключен ли устаревших цветового режима.

Результатом является что цветовой устаревший режим может быть отключен, так, чтобы пользователь задал для представления цветов оставались даже при отключении представления:

![](android-images/legacy-color-mode-disabled.png "Цвет устаревший режим отключен")

> [!NOTE]
> При задании [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) в представлении цветовой устаревший режим не оказывает никакого влияния. Дополнительные сведения о визуальных состояний см. в разделе [Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>С помощью кнопок Android

Этой платформой определяет, используется ли Xamarin.Forms кнопок заполнения по умолчанию и значения Android кнопок. Он используется в языке XAML, задав [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) и [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) присоединенных свойств для `boolean` значения:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

`Button.On<Android>` Метод указывает, что этой платформой будет запускаться только в Android. [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) И[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) методы в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен используются для управления ли кнопки Xamarin.Forms использовать значение по умолчанию Заполнение и значения Android кнопок. Кроме того [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) и [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) методы можно использовать для возврата, использует ли кнопки по умолчанию, заполнение значение и значение по умолчанию тень, соответственно.

Результатом является использование кнопок Xamarin.Forms заполнения по умолчанию и значения Android кнопок:

![](android-images/button-padding-and-shadow.png "Цвет устаревший режим отключен")

Обратите внимание, что на снимке экрана выше [ `Button` ](xref:Xamarin.Forms.Button) с одинаковыми определениями, за исключением того, что правой `Button` использует значение заполнения по умолчанию и значения Android кнопок.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать Android платформы особенности, которые встроены в Xamarin.Forms. Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты.

## <a name="related-links"></a>Связанные ссылки

- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
