---
title: Установка значений специальных возможностей на элементы пользовательского интерфейса
description: Xamarin.Forms позволяет задать на элементах пользовательского интерфейса с помощью вложенные свойства в классе AutomationProperties, который в свою очередь собственные специальные значения набора специальные значения. В этой статье объясняется, как использовать класс AutomationProperties, чтобы средство чтения с экрана могут согласовать об элементах на странице.
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: cf9071684061b584e1cb75cfd50b33212f42bf79
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="setting-accessibility-values-on-user-interface-elements"></a>Установка значений специальных возможностей на элементы пользовательского интерфейса

_Xamarin.Forms позволяет задать на элементах пользовательского интерфейса с помощью вложенные свойства в классе AutomationProperties, который в свою очередь собственные специальные значения набора специальные значения. В этой статье объясняется, как использовать класс AutomationProperties, чтобы средство чтения с экрана могут согласовать об элементах на странице._

## <a name="overview"></a>Обзор

Xamarin.Forms позволяет специальные значения, которые необходимо установить на элементы пользовательского интерфейса посредством следующих вложенных свойств:

- `AutomationProperties.IsInAccessibleTree` — Указывает, доступен ли элемент в приложении со специальными возможностями. Дополнительные сведения см. в разделе [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` — Краткое описание элемента, который служит в качестве speakable идентификатор элемента. Дополнительные сведения см. в разделе [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` — Подробное описание элемента, который может рассматриваться как текст всплывающей подсказки, связанной с элементом. Дополнительные сведения см. в разделе [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` — позволяет другой элемент определить сведения о специальных возможностях для текущего элемента. Дополнительные сведения см. в разделе [AutomationProperties.LabeledBy](#labeledby).

Они присоединены собственные специальные значения свойства набора, чтобы средство чтения с экрана могут согласовать об элементе. Дополнительные сведения о вложенных свойствах см. в разделе [присоединенного свойства](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> С помощью `AutomationProperties` присоединенных свойств может повлиять на выполнение теста пользовательского интерфейса на Android. [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` И `AutomationProperties.HelpText` свойства будет установить оба собственный `ContentDescription` свойства, с `AutomationProperties.Name` и `AutomationProperties.HelpText` значения свойств переназначает `AutomationId`значение (если оба `AutomationProperties.Name` и `AutomationProperties.HelpText` настраиваются, будут сцеплены значения). Это означает, что все тесты, поиск `AutomationId` завершится ошибкой, если `AutomationProperties.Name` или `AutomationProperties.HelpText` также задать в элементе. В этом сценарии следует изменять тесты пользовательского интерфейса для поиска значения `AutomationProperties.Name` или `AutomationProperties.HelpText`, или это объединение.

Каждая платформа имеет различные озвучивания расположит специальных значений:

- iOS имеет VoiceOver. Дополнительные сведения см. в разделе [специальные возможности тестирования на устройство с VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) на developer.apple.com.
- Android имеет TalkBack. Дополнительные сведения см. в разделе [специальных возможностей тестирования приложения](https://developer.android.com/training/accessibility/testing.html#talkback) на developer.android.com.
- В Windows есть Экранный диктор. Дополнительные сведения см. в разделе [проверить сценарии основное приложение с помощью Экранный диктор](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

Однако точное поведение чтения с экрана зависит от программного обеспечения и настройка пользователя его. Например большинство чтения с экрана текст, связанный с элементом управления при получении фокуса, позволяя пользователям ориентацию при перемещении по элементам управления на странице. Некоторые средства чтения с экрана также считывать все приложение пользовательского интерфейса при откроется страница, которая позволяет пользователю получать все доступные информационные содержимое страницы перед попыткой перемещаться по нему.

Средства чтения с экрана также считывать значения для людей с ограниченными возможностями. В примере приложения:

- Считывает voiceOver `Placeholder` значение `Entry`, а затем инструкции по использованию элемента управления.
- Считывает talkBack `Placeholder` значение `Entry`, за которым следует `AutomationProperties.HelpText` значения с инструкциями по использованию элемента управления.
- Экранный диктор будет считывать `AutomationProperties.LabeledBy` значение `Entry`, а затем инструкции по использованию элемента управления.

Кроме того, определять их приоритеты Экранный диктор `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, а затем `AutomationProperties.HelpText`. В Android могут сочетаться TalkBack `AutomationProperties.Name` и `AutomationProperties.HelpText` значения. Таким образом рекомендуется, проверки доступности тщательную выполняется для каждой платформы, для обеспечения оптимальной производительности.

<a name="isinaccessibletree" />

## <a name="automationpropertiesisinaccessibletree"></a>AutomationProperties.IsInAccessibleTree

`AutomationProperties.IsInAccessibleTree` Вложенное свойство `boolean` , определяющий, если элемент доступен и поэтому видимым для программ чтения с экрана. Она должна быть задана `true` использовании других специальных вложенные свойства. Это можно сделать в XAML следующим образом:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Кроме того это можно сделать в C# следующим образом:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Обратите внимание, что [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод может также использоваться для задания `AutomationProperties.IsInAccessibleTree` вложенное свойство. `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## <a name="automationpropertiesname"></a>AutomationProperties.Name

`AutomationProperties.Name` Вложенное свойство должно иметь значение короткие, описательные текстовой строки, средство чтения с экрана для объявления элемента. Это свойство должно задаваться для элементов, имеющих это значит, что важно для понимания содержания и взаимодействии с пользовательским интерфейсом. Это можно сделать в XAML следующим образом:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Кроме того это можно сделать в C# следующим образом:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Обратите внимание, что [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод может также использоваться для задания `AutomationProperties.Name` вложенное свойство. `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## <a name="automationpropertieshelptext"></a>AutomationProperties.HelpText

`AutomationProperties.HelpText` Вложенное свойство должно быть присвоено текст, описывающий элемент пользовательского интерфейса, и можно считать как текст всплывающей подсказки связать с элементом. Это можно сделать в XAML следующим образом:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Кроме того это можно сделать в C# следующим образом:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Обратите внимание, что [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод может также использоваться для задания `AutomationProperties.HelpText` вложенное свойство. `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

На некоторых платформах для редактирования элементов управления, таких как [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), `HelpText` иногда свойство можно пропустить и символы текста заполнителя. Например, «введите здесь имя» является хорошим кандидатом для [ `Entry.Placeholder` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) свойство, которое помещает текст в элементе управления до фактического входные данные пользователя.

<a name="labeledby" />

## <a name="automationpropertieslabeledby"></a>AutomationProperties.LabeledBy

`AutomationProperties.LabeledBy` Вложенное свойство позволяет определить сведения о специальных возможностях для текущего элемента другого элемента. Например [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) рядом с [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) может использоваться для описания того, что `Entry` представляет. Это можно сделать в XAML следующим образом:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Кроме того это можно сделать в C# следующим образом:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Обратите внимание, что [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод может также использоваться для задания `AutomationProperties.IsInAccessibleTree` вложенное свойство. `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## <a name="summary"></a>Сводка

В этой статье описано, как задать значений специальных возможностей для пользователя элементов интерфейса в приложении Xamarin.Forms с помощью вложенных свойств из `AutomationProperties` класса. Они присоединены собственные специальные значения свойства набора, чтобы средство чтения с экрана могут согласовать об элементах на странице.


## <a name="related-links"></a>Связанные ссылки

- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
- [Специальные возможности (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
