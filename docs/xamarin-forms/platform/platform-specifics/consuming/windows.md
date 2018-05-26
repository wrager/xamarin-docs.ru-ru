---
title: Особенности веб-платформы Windows
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/25/2018
---
# <a name="windows-platform-specifics"></a>Особенности веб-платформы Windows

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты. В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms._

В универсальной платформы Windows (UWP), Xamarin.Forms содержит следующие платформы особенности:

- Настройка параметров размещения панели инструментов. Дополнительные сведения см. в разделе [изменения расположения инструментов](#toolbar_placement).
- Свертывание [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) панели навигации. Дополнительные сведения см. в разделе [свертывание панель навигации MasterDetailPage](#collapsable_navigation_bar).
- Включение [ `WebView` ](xref:Xamarin.Forms.WebView) для отображения в диалоговом окне сообщения UWP JavaScript. Дополнительные сведения см. в разделе [отображение предупреждений JavaScript](#webview-javascript-alert).

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

## <a name="summary"></a>Сводка

В этой статье показано, как использовать платформу особенности Windows, которые встроены в Xamarin.Forms. Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты.

## <a name="related-links"></a>Связанные ссылки

- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
