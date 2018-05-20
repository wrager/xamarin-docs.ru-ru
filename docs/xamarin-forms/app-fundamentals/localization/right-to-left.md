---
title: Локализация справа налево
description: Локализация справа налево добавляет поддержку направлением потока справа налево Xamarin.Forms приложениям.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: ff9814291d5a28ec9e0bbb3c2a6fc6cce5d8ee25
ms.sourcegitcommit: 4db5f5c93f79f273d8fc462de2f405458b62fc02
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/19/2018
---
# <a name="right-to-left-localization"></a>Локализация справа налево

_Локализация справа налево добавляет поддержку направлением потока справа налево Xamarin.Forms приложениям._

> [!NOTE]
> Справа налево локализации требует использования API 17 и выше на Android и iOS 9 или более поздней версии.

Направление потока имеет направление, в котором проверяются элементы пользовательского интерфейса на странице глаза. Некоторые языки, например арабском или иврите, требует, что элементы пользовательского интерфейса располагаются в направлении справа налево. Это можно сделать, задав [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство. Это свойство Возвращает или задает направление, в которую проходят элементы пользовательского интерфейса в родительском элементе, определяющем их размещение и должно быть присвоено одно из [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) значения перечисления:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Установка [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойства [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) на элементе обычно задает способ выравнивания вправо, порядок чтения справа налево и макет элемента управления из потока справа налево:

[![TodoItemPage на арабском языке с направлением потока справа налево](rtl-images/TodoItemPage-Arabic.png "TodoItemPage на арабском языке с направлением потока справа налево")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage на арабском языке с направлением потока справа налево")

> [!TIP]
> Следует задавать только [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство начального макета. Изменение этого значения во время выполнения приведет дорогих макета процесс, который повлияет на производительность.

Значение по умолчанию [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) значение свойства для элемента без родительских [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), а значение по умолчанию `FlowDirection` для элемента с родительским [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом, элемент наследует `FlowDirection` значение свойства от своего родительского элемента в визуальном дереве и любой элемент можно переопределить значение, он получает от своего родительского объекта.

> [!TIP]
> При локализации приложения для языков справа налево, задать [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство в макете страницы или корневой. В результате все элементы, содержащиеся в страницу или корневой макет, соответствующим образом реагировать на направление потока.

## <a name="respecting-device-flow-direction"></a>Направление потока уважение устройства

Этом соблюдаются направление устройства на основе выбранного языка и региона вариант явного разработчика и не происходит автоматически. Он может быть достигнуто путем установки [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство на странице, или макет корневой для `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) значение:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Все дочерние элементы страницы или корневой макет, будут по умолчанию унаследованы [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) значение.

## <a name="platform-setup"></a>Программа установки платформы

Программа установки для конкретной платформы необходимо включить язык и региональные стандарты справа налево.

### <a name="ios"></a>iOS

Требуется языковой стандарт справа налево необходимо добавить в качестве поддерживаемого языка с элементами массива для `CFBundleLocalizations` ключа в **Info.plist**. В следующем примере показано были добавлены в массив для арабского языка `CFBundleLocalizations` ключ:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Языки поддерживаются Info.plist](rtl-images/ios-locales.png "Info.plist поддерживаемые языки")

Дополнительные сведения см. в разделе [основные сведения о локализации в iOS](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Впоследствии можно будет проверить локализации справа налево, изменив язык и регион, в симуляторе устройства или справа налево языковой стандарт, который был указан в **Info.plist**.

> [!WARNING]
> Обратите внимание, что при изменении языка и региона языкового справа налево в iOS, любое [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) представления будет вызывать исключение, если не включать ресурсы, необходимые для языкового стандарта. Например, при тестировании приложения на арабском языке, который имеет `DatePicker`, убедитесь, что **mideast** выбран в **интернационализации** раздел **сборка iOS** области.

### <a name="android"></a>Android

Приложения **AndroidManifest.xml** файла должны быть обновлены, чтобы `<uses-sdk>` наборов узлов `android:minSdkVersion` атрибут 17 и `<application>` наборов узлов `android:supportsRtl` атрибут `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Справа налево локализации впоследствии можно будет проверить, изменив устройстве или эмуляторе с использованием языка справа налево, или путем включения **направление макета Force RTL** в **параметры > Параметры разработчика**.

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Необходимые языковые ресурсы должны быть указаны в `<Resources>` узел **Package.appxmanifest** файла. В следующем примере показано арабский, были добавлены `<Resources>` узла:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Кроме того UWP необходимо, явно определить язык и региональные параметры приложения по умолчанию в библиотеке .NET Standard. Это можно сделать, задав `NeutralResourcesLanguage` атрибут `AssemblyInfo.cs`, или в другом классе, язык и региональные параметры по умолчанию:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Впоследствии можно будет проверить локализации справа налево, изменив язык и регион на устройстве на соответствующий языковой стандарт справа налево.

## <a name="limitations"></a>Ограничения

Локализация Xamarin.Forms справа налево в настоящее время имеет ряд ограничений:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Размещение кнопки панели инструментов элемента расположение и анимации перехода управляется устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Проведите направление не отражение.
- [`Image`](xref:Xamarin.Forms.Image) не выполняет отражение визуального содержимого.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) и [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) управляет ориентации устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`WebView`](xref:Xamarin.Forms.WebView) содержимое не имеют отношения к [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- Объект `TextDirection` свойства должны добавляться, для управления выравниванием текста.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) Управляет ориентации устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) Выравнивание текста управляется устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Ввод и выравнивание не будут отменены.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Управляет ориентации устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Размещение управляется устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) Выравнивание текста управляется устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство не наследуются [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) дочерних элементов.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Выравнивание текста управляется устройства языкового стандарта, а не [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) свойство.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Право левой языковой поддержки с Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Поддерживает Xamarin.Forms 3.0 справа налево, по [университета Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Связанные ссылки

- [Пример TodoLocalizedRTL приложения](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
