---
title: "Макет программные ограничения"
description: "В этом руководстве представлен автоматический макет ограничения работы с iOS в коде C#, а не создавать их в iOS конструктор."
ms.topic: article
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 4a3450026eff06555723b16093c7a0daf3d12ae7
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2018
---
# <a name="programmatic-layout-constraints"></a>Макет программные ограничения

_В этом руководстве представлен автоматический макет ограничения работы с iOS в коде C#, а не создавать их в iOS конструктор._

Автоматический макет (также называется «адаптивной макета») — это гибкий Дизайн подход. В отличие от системы переходные макет, где каждый элемент находится жестко запрограммировано на точку на экране, автоматический макет посвящен *связи* -позиции элементов относительно других элементов в рабочей области конструирования. Основа автоматический макет является идея ограничения или правила, которые определяют положение элемента или набора элементов в контексте других элементов на экране. Так как элементы не связаны с определенной позиции на экране, ограничения помогают создать адаптивный макета, удобная для разных размеров и ориентации устройства.

Обычно при работе с автоматический макет в iOS, будет использоваться конструктор iOS графически разместить макета ограничения на элементы пользовательского интерфейса. Однако возможны ситуации, когда требуется создать и применить ограничения в коде C#. Например, при использовании динамически созданных элементов пользовательского интерфейса, добавляются `UIView`.

В этом руководстве будет показано, как создавать и работать с ограничениями, с помощью кода C#, вместо того чтобы создавать их графически в конструкторе iOS.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Создание ограничения программным способом

Как уже говорилось выше, обычно вы будете работать с ограничениями макета Auto в конструкторе iOS. В тех случаях, необходимых для создания ограничения программными средствами у вас есть три варианта:

* [Макет привязки](#Layout-Anchors) -данный API обеспечивает доступ к свойствам привязки (например `TopAnchor`, `BottomAnchor` или `HeightAnchor`) ограниченные элементы пользовательского интерфейса.
* [Ограничения макета](#Layout-Constraints) -можно создать напрямую, используя ограничения `NSLayoutConstraint` класса.
* [Визуального форматирования языка](#Visual-Format-Language) -предоставляет ASCII-графики, как и метод для определения ограничения.

В следующих разделах будут рассмотрены подробное описание каждого параметра.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Макет привязки

С помощью `NSLayoutAnchor` класс, у вас есть fluent интерфейс для создания ограничения на основе свойств привязки ограниченные элементы пользовательского интерфейса. Например, контроллер представление макета верхнего и нижнего проводит предоставляет `TopAnchor`, `BottomAnchor` и `HeightAnchor` свойства привязки, пока представление отображает свойства edge, центр, размер и базовых показателей.

> [!IMPORTANT]
> **Примечание:** помимо стандартного набора свойств привязки, также включают представления iOS `LayoutMarginsGuides` и `ReadableContentGuide` свойства. Эти свойства предоставляют `UILayoutGuide` объекты для работы с полями представления и для чтения содержимого руководства соответственно.

Макет привязки предусмотрено несколько методов для создания ограничений в удобной для чтения компактном формате:

- **ConstraintEqualTo** -определяет связь где `first attribute = second attribute + [constant]` с при необходимости заданный `constant` значение смещения.
- **ConstraintGreaterThanOrEqualTo** -определяет связь где `first attribute >= second attribute + [constant]` с при необходимости заданный `constant` значение смещения.
- **ConstraintLessThanOrEqualTo** -определяет связь где `first attribute <= second attribute + [constant]` с при необходимости заданный `constant` значение смещения.

Пример:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Типичным ограничением макета могут быть выражены просто как линейной выражение. Рассмотрим следующий пример:

[![](programmatic-layout-constraints-images/graph01.png "Ограничение макета, выраженное как выражение линейной")](programmatic-layout-constraints-images/graph01.png#lightbox)

Который будет преобразован в следующая строка кода C# с помощью привязки макета:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Где части кода C# соответствуют данной части формулы следующим образом:

|Уравнение|Код|
|---|---|
|Элемент 1|PurpleView|
|Атрибут 1|LeadingAnchor|
|Relationship|ConstraintEqualTo|
|Множитель|По умолчанию используется значение 1.0 таким образом не указан|
|Элемент 2|OrangeView|
|Атрибут 2|TrailingAnchor|
|Константа|10.0|

Помимо только параметры, необходимые для решения уравнении ограничение указанным макетом, каждый из методов привязки макета обеспечивать безопасность типов параметров, передаваемых в них. Поэтому горизонтальной ограничение связывает таких как `LeadingAnchor` или `TrailingAnchor` может использоваться только с другими горизонтальной привязки типов и множители предоставляются только для ограничений по размеру.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Ограничения макета

Автоматический макет ограничения можно добавить вручную, создав непосредственно `NSLayoutConstraint` в коде C#. В отличие от макета привязки, необходимо указать значение для каждого параметра, даже если он не будет действовать в процессе определения ограничения. В результате приходится формирующего значительное трудно читать стандартный код. Пример:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Где `NSLayoutAttribute` перечисление определяет значение для поля представления и соответствуют `LayoutMarginsGuide` свойств, таких как `Left`, `Right`, `Top` и `Bottom` и `NSLayoutRelation` перечисление определяет связи, будет создана между заданными атрибутами как `Equal`, `LessThanOrEqual` или `GreaterThanOrEqual`.

В отличие от с помощью API привязки макета `NSLayoutConstraint` методы создания не выделять важные аспекты ограничения и нет не компилировать время проверки, выполняемые для ограничения. В результате можно легко создать недопустимое ограничение, возникает исключение во время выполнения.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Язык Visual формата

Visual языка можно задать ограничения с помощью ASCII-графики как строки, которые визуально представляют ограничения, в процессе создания. Это имеет следующие преимущества и недостатки:

- Язык Visual формата обеспечивает создание только допустимые ограничения.
 - Автоматический макет генерирует ограничения в консоль с помощью языка Visual, поэтому сообщения отладки будут напоминают код, используемый для создания ограничения.
 - Visual языка позволяет создавать несколько ограничений в то же время очень compact выражением.
 - Поскольку нет проверки на стороне компиляции визуального языка формат строк, проблемы могут быть обнаружены только во время выполнения.
 - Поскольку Visual языка особое внимание уделяется визуализации по полноте, некоторые типы ограничений не может быть создан с ней (например, коэффициенты).

При использовании визуального формата языка для создания ограничения, выполните следующие действия:

1. Создание `NSDictionary` , содержащий объекты представлений и направляющие и строковый ключ, который будет использоваться при определении форматы.
2. При необходимости создайте `NSDictionary` , определяющее набор ключей и значений (`NSNumber`) используется в качестве постоянное значение для ограничения.
3. Создайте строку формата для макета одного столбца или строки элементов.
4. Вызовите `FromVisualFormat` метод `NSLayoutConstraint` класса для создания ограничения.
5. Вызовите `ActivateConstraints` метод `NSLayoutConstraint` класса, чтобы активировать и применить ограничения.

Например чтобы создать начальные и конечные ограничения в Visual языка, можно использовать следующие:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Поскольку Visual языка всегда создает нулевой точки ограничения для полей родительского представления при использовании интервал по умолчанию, этот код выводит одинаковые результаты для приведенных выше примерах.

Для более сложных конструкций пользовательского интерфейса, такие как несколько дочерних представлений в одной строке языка Visual задает интервал по горизонтали и по вертикали. Как показано в приведенном выше примере, где он задает `AlignAllTop` `NSLayoutFormatOptions` Выравнивает все представления в строку или столбец для их верхние края.

Разделе Apple [Visual приложение формат языка](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) некоторые примеры указания общих ограничений и грамматики Visual строку формата.

<a name="Summary" />

## <a name="summary"></a>Сводка

Создание и работа с ограничениями автоматический макет в C#, в отличие от создания их графически в конструкторе iOS, представленных в этом руководстве. Во-первых, хотя рассматривались с помощью привязки макета (`NSLayoutAnchor`) для обработки автоматический макет. Затем он было показано, как работать с ограничениями макета (`NSLayoutConstraint`). Наконец оно представлено в языке Visual формат для автоматический макет.

## <a name="related-links"></a>Связанные ссылки

- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)
- [Проектирование пошагового руководства элементы управления iOS](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [Автоматический макет с помощью конструктора Xamarin для iOS](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple — программными средствами создания ограничения](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple — приложение визуальный формат языка](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
