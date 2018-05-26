---
title: Особенности платформы
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 0d536c2c9c98ba65f80bf95810bf45d395ee785d
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/25/2018
---
# <a name="platform-specifics"></a>Особенности платформы

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффекты._

Следующие функциональные возможности платформой встроено в Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth и MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume и Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)
|[Page.PrefersStatusBarHidden и Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|

Процесс для использования платформой помощью XAML или fluent API кода выглядит следующим образом:

1. Добавить `xmlns` объявление или `using` директивы [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) пространства имен.
1. Добавить `xmlns` объявление или `using` директив для имен, которое содержит функции, определяемые платформой:
    1. На iOS, это [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) пространства имен.
    1. На Android, это [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) пространства имен. Для Android AppCompat это [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) пространства имен.
    1. Это универсальная платформа Windows [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) пространства имен.
1. Применить конкретную платформу из XAML или кода с помощью `On<T>` fluent API. Значение `T` может быть [ `iOS` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [ `Android` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), или [ `Windows` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) типов из [ `Xamarin.Forms.PlatformConfiguration` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) пространства имен.

> [!NOTE]
> Обратите внимание, что попытка использовать конкретную платформу на платформе, где он недоступен не влияет ошибку. Вместо этого код будет выполняться без применения конкретную платформу.

Особенности платформы востребован `On<T>` кода fluent API из [ `IPlatformElementConfiguration` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) объектов. Это позволяет несколько особенностей платформы для вызова того же объекта с каскадным метод.

Дополнительные сведения об особенностях платформы см. в разделе [использование платформы особенности](~/xamarin-forms/platform/platform-specifics/consuming/index.md) и [особенности платформы в создании](~/xamarin-forms/platform/platform-specifics/creating.md).


## <a name="related-links"></a>Связанные ссылки

- [Использование особенностей платформы](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
