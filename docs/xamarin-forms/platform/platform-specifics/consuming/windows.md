---
title: Особенности веб-платформы Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732805"
---
# <a name="windows-platform-specifics"></a>Особенности веб-платформы Windows

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms._

В универсальной платформы Windows (UWP), Xamarin.Forms содержит следующие платформы особенности:

- Настройка параметров размещения панели инструментов. Дополнительные сведения см. в разделе [изменения расположения инструментов](#toolbar_placement).
- Свертывание [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) панели навигации. Дополнительные сведения см. в разделе [свертывание панель навигации MasterDetailPage](#collapsable_navigation_bar).
- Включение [ `WebView` ](xref:Xamarin.Forms.WebView) для отображения в диалоговом окне сообщения UWP JavaScript. Дополнительные сведения см. в разделе [отображение предупреждений JavaScript](#webview-javascript-alert).
- Включение [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) для взаимодействия с подсистемой проверки орфографии. Дополнительные сведения см. в разделе [Включение SearchBar орфографии](#searchbar-spellcheck).
- Обнаружение порядок чтения из текстового содержимого в [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), и [ `Label` ](xref:Xamarin.Forms.Label) экземпляров. Дополнительные сведения см. в разделе [обнаружение порядок чтения из содержимого](#inputview-readingorder).
- Отключение режима устаревших цвета на поддерживаемой [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Дополнительные сведения см. в разделе [отключение цветовая маркировка прежних версий](#legacy-color-mode).
- Включение поддержки tap жестов в [ `ListView` ](xref:Xamarin.Forms.ListView). Дополнительные сведения см. в разделе [Включение коснитесь поддержки в ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Изменение расположения панели инструментов

Это специфический для платформы используется для изменения положения панели инструментов на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)и используются в языке XAML, задав [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) вложенное свойство в значение [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) перечисления:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

`Page.On<Windows>` Метод указывает, что этой платформой будет запускаться только в Windows. [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) пространства имен, используемый для задания расположение панели инструментов с [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) предоставление перечисления три значения: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), и [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Результатом является применения размещения указанной панели инструментов для [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) экземпляр:

[![](windows-images/toolbar-placement.png "Панель инструментов размещения платформой")](windows-images/toolbar-placement-large.png#lightbox "инструментов размещения платформы")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Свертывание MasterDetailPage панель навигации

Это специфический для платформы используется для свертывания панели навигации в [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)и используются в языке XAML, задав [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) и [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)вложенных свойств:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>` Метод указывает, что этой платформой будет запускаться только в Windows. [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) пространства имен используется для указания стиля свернуть с [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) предоставляет два перечисления значения: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) и [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Метод используется для указания ширины частично свернутой областью навигации.

Результатом является то, что указанный [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) применяется к [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) экземпляра с шириной также указаны:

[![](windows-images/collapsed-navigation-bar.png "Свернуть панель навигации платформой")](windows-images/collapsed-navigation-bar-large.png#lightbox "свернуты платформой панель навигации")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Вывод предупреждений JavaScript

Этой платформы позволяет [ `WebView` ](xref:Xamarin.Forms.WebView) для отображения в диалоговом окне сообщения UWP JavaScript. Он используется в языке XAML, задав [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

`WebView.On<Windows>` Метод указывает, что этой платформой будет запускаться только на универсальной платформе Windows. [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен используется для управления Включение предупреждения JavaScript. Кроме того `WebView.SetIsJavaScriptAlertEnabled` метод может использоваться, чтобы включить или отключить предупреждения JavaScript путем вызова [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) метод для возврата, включены ли они:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Результатом является отображение JavaScript предупреждения в диалоговом окне сообщения UWP:

![Предупреждение WebView JavaScript платформой](windows-images/webview-javascript-alert.png "предупреждение WebView JavaScript платформой")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>Включение SearchBar проверка орфографии

Этой платформы позволяет [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) для взаимодействия с подсистемой проверки орфографии. Он используется в языке XAML, задав [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

`SearchBar.On<Windows>` Метод указывает, что этой платформой будет запускаться только на универсальной платформе Windows. [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен, включает средство проверки орфографии и выключает. Кроме того `SearchBar.SetIsSpellCheckEnabled` метод может использоваться, чтобы включить или отключить средство проверки орфографии, вызвав [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) метод для возврата, включена ли проверка орфографии:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Результатом является, текста, введенного в [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) можно проверять орфографию, с неправильное написание указанный для пользователя:

![SearchBar орфографии проверка платформой](windows-images/searchbar-spellcheck.png "SearchBar орфографии проверки платформы")

> [!NOTE]
> `SearchBar` Класса в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространство имен также содержит [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) и [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) методы, которые можно использовать для включения и отключения Проверка орфографии в `SearchBar`соответственно.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Обнаружение порядок чтения из содержимого

Этой платформы позволяет порядок чтения (слева направо или справа налево) двунаправленного текста в [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), и [ `Label` ](xref:Xamarin.Forms.Label) экземпляров, чтобы быть обнаруженным динамически. Он используется в языке XAML, задав [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (для `Entry` и `Editor` экземпляров) или [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) присоединенному свойству `boolean` значение:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

`Editor.On<Windows>` Метод указывает, что этой платформой будет запускаться только на универсальной платформе Windows. [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен используется для управления обнаружена порядок чтения из содержимого ли [ `InputView` ](xref:Xamarin.Forms.InputView). Кроме того `InputView.SetDetectReadingOrderFromContent` метод может использоваться для переключения между режимами обнаружена порядок чтения из содержимого, вызвав [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) метод для возврата текущего значения:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

В результате [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), и [ `Label` ](xref:Xamarin.Forms.Label) экземпляров может иметь порядок чтения их обнаружил динамического содержимого:

[![Обнаружение порядок чтения из содержимого платформой InputView](windows-images/editor-readingorder.png "InputView обнаружение порядок чтения из содержимого платформой")](windows-images/editor-readingorder-large.png#lightbox "InputView обнаружение порядок чтения из специфический для платформы контент")

> [!NOTE]
> В отличие от параметра [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство, логику для представлений, которые обнаруживают порядок чтения из их содержимого текста не повлияет на способ выравнивания текста в пределах представления. Вместо этого он настраивает порядок, в котором расположены блоки двунаправленного текста.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Отключение режима цвет прежних версий

Некоторые представления Xamarin.Forms признаков устаревших цветового режима. В этом режиме при [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) представления свойству `false`, переопределяет цвета, установленные пользователем с помощью собственного цветов по умолчанию для отключенного состояния представления. Для обеспечения обратной совместимости, этот цвет устаревший режим остается поведение по умолчанию для поддерживаемых представлений.

Этой платформой отключает этот режим устаревшего цвет, так что установленные пользователем для представления цветов остаются даже в том случае, если представление будет отключена. Он используется в языке XAML, задав [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) присоединенному свойству `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<Windows>` Метод указывает, что этой платформой будет запускаться только в Windows. [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен используется для управления ли цвет устаревший режим отключен. Кроме того [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) метод может использоваться для возврата, не отключен ли устаревших цветового режима.

Результатом является что цветовой устаревший режим может быть отключен, так, чтобы пользователь задал для представления цветов оставались даже при отключении представления:

![](windows-images/legacy-color-mode-disabled.png "Цвет устаревший режим отключен")

> [!NOTE]
> При задании [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) в представлении цветовой устаревший режим не оказывает никакого влияния. Дополнительные сведения о визуальных состояний см. в разделе [Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Включение поддержки Tap жестов в ListView

На универсальной платформе Windows по умолчанию Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) использует собственные `ItemClick` событию реагировать на взаимодействие, а не собственного `Tapped` событий. Это обеспечивает функции специальных возможностей, позволяющих Экранный диктор Windows и сочетаний клавиш можно работать с `ListView`. Тем не менее, он также выводит все жесты tap внутри `ListView` вышел из строя.

Эта определяет конкретную платформу ли элементы в [ `ListView` ](xref:Xamarin.Forms.ListView) может отвечать на коснитесь жестов и, следовательно ли собственный `ListView` активируется `ItemClick` или `Tapped` событий. Он используется в языке XAML, задав [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) вложенное свойство в значение [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) перечисления:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

`ListView.On<Windows>` Метод указывает, что этой платформой будет запускаться только на универсальной платформе Windows. [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Метод в [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен, можно указать, является ли элементы в [ `ListView` ](xref:Xamarin.Forms.ListView) может отвечать на коснитесь жестов, с [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) перечисления, предоставляя два возможных значения:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) — Указывает, что `ListView` будет срабатывать собственный `ItemClick` событий для управления взаимодействием и поэтому предоставляет функциональные возможности специальных возможностей. Поэтому Экранный диктор Windows и сочетаний клавиш можно взаимодействовать с `ListView`. Тем не менее, элементы в `ListView` не может отвечать на коснитесь жесты. Это поведение по умолчанию для `ListView` экземпляров на универсальной платформе Windows.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) — Указывает, что `ListView` будет срабатывать собственный `Tapped` событий для обработки взаимодействия. Таким образом, элементы в `ListView` может отвечать на коснитесь жесты. Тем не менее, нет возможности специальных возможностей и поэтому Экранный диктор Windows и сочетаний клавиш не может взаимодействовать с `ListView`.

> [!NOTE]
> `Accessible` И `Inaccessible` режимы выбора являются взаимоисключающими, и необходимо будет выбрать между доступного [ `ListView` ](xref:Xamarin.Forms.ListView) или `ListView` , может отвечать на коснитесь жесты.

Кроме того [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) метод используется для возврата текущего [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Результат является то, что указанный [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) применяется к [ `ListView` ](xref:Xamarin.Forms.ListView), какие элементы управления ли элементы в `ListView` может отвечать на коснитесь жестов и, следовательно ли собственный `ListView` активируется `ItemClick` или `Tapped` событий.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms. Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты.

## <a name="related-links"></a>Связанные ссылки

- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
