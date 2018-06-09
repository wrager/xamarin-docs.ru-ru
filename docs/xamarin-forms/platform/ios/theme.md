---
title: Добавление форматирование конкретных операций ввода-вывода
description: В этой статье объясняется, как настроить внешний вид конкретных операций ввода-вывода не с помощью Xamarin.Forms пользовательское средство отрисовки.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 74a3cdc340cb09e8adf15ed0dd09315c985d18b5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243536"
---
# <a name="adding-ios-specific-formatting"></a>Добавление форматирование конкретных операций ввода-вывода

Один способ для задания конкретного iOS форматирования — создать [пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) для элемента управления и стили специфический для платформы набор цветов для каждой платформы.

Другие параметры для управления способом внешний вид приложения iOS Xamarin.Forms:

* Настройка отображения параметров в [ **Info.plist**](#info-plist)
* Настройка стилей элемента управления через [ `UIAppearance` API](#uiappearance)

Эти возможности описаны ниже.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Настройка Info.plist

**Info.plist** файла позволяет настроить некоторые аспекты приложения iOS типы, такие как способ (и необходимость) отображается в строке состояния.

Например [Todo образец](https://developer.xamarin.com/samples/xamarin-forms/Todo/) используется следующий код, чтобы задать цвет и цвет текста на панели навигации на всех платформах:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Результат показан в приведенном ниже фрагменте кода экрана. Обратите внимание, что элементы строки состояния черный (не может быть установлен в Xamarin.Forms, так как она является компонентом платформы).

![](theme-images/status-default-sml.png "iOS темы")

В идеальном случае в строке состояния, также будут белый — что-то можно выполнить непосредственно в проекте iOS. Добавьте следующие строки для **Info.plist** для принудительного строке состояния белым цветом:

![](theme-images/info-plist.png "операций ввода-вывода записи Info.plist")

или изменение соответствующего **Info.plist** непосредственно включаемых файлов:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Теперь при запуске приложения на панели навигации имеет зеленый цвет, и его текст отображается белый цвет (из-за форматирование Xamarin.Forms) *и* текст строки состояния, также очень белый благодаря конфигурации iOS:

![](theme-images/status-white-sml.png "iOS темы")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) можно использовать для задания свойств visual многие элементы управления iOS *без* создавать [пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

При добавлении одной строки кода, чтобы **AppDelegate.cs** `FinishedLaunching` метод задать стиль всех элементов управления данного типа, используя их `Appearance` свойство. В следующем коде содержатся два примера - глобально стиля на вкладке панели и переключите управления:

**AppDelegate.cs** в проекте iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

По умолчанию значок панели выбранная вкладка в [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) бы синий:

![](theme-images/tabbar-default.png "По умолчанию значок панели вкладки в TabbedPage iOS")

Чтобы изменить это поведение, установите `UITabBar.Appearance` свойство:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

В результате выбранная вкладка с:

![](theme-images/tabbar-custom.png "Зеленый значок панели вкладки в TabbedPage iOS")

С помощью этого API позволяет настраивать внешний вид Xamarin.Forms `TabbedPage` в iOS с помощью очень небольшого объема кода. Ссылаться на [настройки вкладки рецепт](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) Дополнительные сведения об использовании пользовательского средства визуализации для задания определенного шрифта для вкладки.

### <a name="uiswitch"></a>UISwitch

`Switch` Управления еще один пример, который можно легко со стилем:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Эти снимки экрана два Показать значение по умолчанию `UISwitch` управления слева и настроенную версию (параметр `Appearance`) в правой [Todo образец](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "По умолчанию цвет UISwitch") ![ ] (theme-images/switch-custom.png "настроить цвет UISwitch")

### <a name="other-controls"></a>Другие элементы управления

Многие элементы управления пользовательского интерфейса iOS может иметь их цвета по умолчанию и другие атрибуты, заданные с помощью [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Связанные ссылки

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Настройка вкладок](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
