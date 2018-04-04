---
title: Вложенные свойства
description: Вложенное свойство — это специальный тип привязываемые свойства, определенные в одном классе, но вложен в другие объекты, распознаваемые в XAML как атрибут, содержащий класс и имя свойства, разделенных точкой. В этой статье содержатся вводные сведения о вложенных свойств и показано, как создавать и использовать их.
ms.prod: xamarin
ms.assetid: 6E9DCDC3-A0E4-46A6-BAA9-4FEB6DF8A5A8
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: 5c903a39e5569c7ffedfff8eb8e6b0bd4071be9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="attached-properties"></a>Вложенные свойства

_Вложенное свойство — это специальный тип привязываемые свойства, определенные в одном классе, но вложен в другие объекты, распознаваемые в XAML как атрибут, содержащий класс и имя свойства, разделенных точкой. В этой статье содержатся вводные сведения о вложенных свойств и показано, как создавать и использовать их._

## <a name="overview"></a>Обзор

Присоединить включить свойства объекта для присвоения значения для свойства, которое не определяет свой собственный класс. Например дочерние элементы можно использовать вложенные свойства для информирования их родительского элемента как они могут быть представлены в пользовательском интерфейсе. [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) Управления позволяет строке и столбце является дочерним для путем задания `Grid.Row` и `Grid.Column` вложенные свойства. `Grid.Row` и `Grid.Column` , вложенные свойства, так как они устанавливаются на элементы, которые являются дочерними элементами `Grid`, а не на `Grid` сам.

Привязываемые свойства должны быть реализованы как вложенные свойства в следующих сценариях:

- При необходимости иметь свойство параметр механизм доступны для классов, отличный от определяющего класса.
- Если класс представляет службу, которую нужно легко интегрируется с другими классами.

Дополнительные сведения о свойствах привязки см. в разделе [привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md).

## <a name="creating-and-consuming-an-attached-property"></a>Создание и использование вложенного свойства

Процесс создания вложенное свойство выглядит следующим образом:

1. Создание [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляра с одним из [ `CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) перегруженных версий метода.
1. Укажите `static` `Get` *PropertyName* и `Set` *PropertyName* методов как методы доступа для вложенных свойств.

### <a name="creating-a-property"></a>Создание свойства

При создании вложенное свойство для использования в других типов, класса, где создается свойство не обязательно должен быть производным от [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/). Тем не менее *целевой* свойства для методов доступа должен быть или производным от [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

Присоединенное свойство могут создаваться путем объявления `public static readonly` свойство типа [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Привязываемые свойства должно быть присвоено возвращаемое значение одного из [ `BindableProperty.CreateAttached` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateAttached/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) перегруженных версий метода. Объявление должно быть в теле класса-владельца, но за пределами определения членов.

Ниже показан пример вложенного свойства.

```csharp
public static readonly BindableProperty HasShadowProperty =
  BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false);
```

При этом создается вложенное свойство с именем `HasShadow`, типа `bool`. Свойство принадлежит `ShadowEffect` класса и имеет значение по умолчанию `false`. Соглашение об именовании для вложенных свойств заключается в том, что идентификатор присоединенного свойства должна соответствовать имя свойства, указанное в `CreateAttached` метод, с добавлением «свойство». Таким образом, в приведенном выше примере идентификатор вложенное свойство является `HasShadowProperty`.

Дополнительные сведения о создании привязываемые свойства, включая параметры, которые могут быть указаны во время создания разделе [создания и использования привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md#consuming-bindable-property).

### <a name="creating-accessors"></a>Создание методов доступа

Статические `Get` *PropertyName* и `Set` *PropertyName* требуются методы как методы доступа для вложенных свойств, в противном случае будет невозможно использовать в системе свойств вложенное свойство. `Get` *PropertyName* доступа должны соответствовать следующую сигнатуру:

```csharp
public static valueType GetPropertyName(BindableObject target)
```

`Get` *PropertyName* метод доступа должен возвращать значение, которое содержится в соответствующем `BindableProperty` поле для вложенного свойства. Это можно сделать путем вызова [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) метод, передавая идентификатор привязываемые свойства, для которого требуется получить значение, а затем приведение результирующее значение в требуемый тип.

`Set` *PropertyName* доступа должны соответствовать следующую сигнатуру:

```csharp
public static void SetPropertyName(BindableObject target, valueType value)
```

`Set` *PropertyName* доступа следует установить значение соответствующего `BindableProperty` поле для вложенного свойства. Это можно сделать путем вызова [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод, передавая идентификатор привязываемые свойства, для которого требуется задать значение и значение для установки.

Для обоих методов доступа *целевой* объект должен быть или производным от [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/).

В следующем примере кода показаны методы доступа для `HasShadow` вложенное свойство:

```csharp
public static bool GetHasShadow (BindableObject view)
{
  return (bool)view.GetValue (HasShadowProperty);
}

public static void SetHasShadow (BindableObject view, bool value)
{
  view.SetValue (HasShadowProperty, value);
}
```

### <a name="consuming-an-attached-property"></a>Использование вложенного свойства

После создания вложенного свойства, он может использоваться из XAML или коде. В языке XAML это достигается путем объявления пространства имен с префиксом с объявления пространства имен, указывающее имя пространства имен Common Language Runtime (CLR) и при необходимости имя сборки. Дополнительные сведения см. в разделе [пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md).

В следующем примере кода показаны пространства имен XAML для пользовательского типа, который содержит вложенное свойство, который определен в сборке, в качестве кода приложения, ссылается на пользовательский тип:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EffectsDemo" ...>
  ...
</ContentPage>
```

Объявление пространства имен используется при установки вложенного свойства на определенный элемент управления, как показано в следующем примере кода XAML:

```xaml
<Label Text="Label Shadow Effect" local:ShadowEffect.HasShadow="true" />
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var label = new Label { Text = "Label Shadow Effect" };
ShadowEffect.SetHasShadow (label, true);
```

### <a name="consuming-an-attached-property-with-a-style"></a>Использование вложенного свойства со стилем

Вложенные свойства можно также добавить к элементу управления стилем. В следующем примере показан код XAML *явных* стиль, который использует `HasShadow` вложенное свойство зависимостей, который может применяться к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) элементов управления:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="true" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Может применяться к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , установив его [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства `Style` экземпляра с помощью `StaticResource`расширения разметки, как показано в следующем примере кода:

```xaml
<Label Text="Label Shadow Effect" Style="{StaticResource ShadowEffectStyle}" />
```

Дополнительные сведения о стилях см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md).

## <a name="advanced-scenarios"></a>Расширенные сценарии

При создании вложенного свойства, существует ряд необязательных параметров, можно настроить для поддержки сценариев, дополнительно вложенное свойство. Это включает обнаружение изменения свойств, проверку значений свойств и приведение значений свойств. Дополнительные сведения см. в разделе [дополнительные сценарии](~/xamarin-forms/xaml/bindable-properties.md#advanced).

## <a name="summary"></a>Сводка

В этой статье сведения о вложенных свойств и было показано, как создавать и использовать их. Вложенное свойство — это специальный тип привязываемые свойства, определенные в одном классе, но подключенных к другим объектам и распознается в XAML как атрибуты, которые содержат класс и имя свойства, разделенных точкой.


## <a name="related-links"></a>Связанные ссылки

- [Привязываемые свойства](~/xamarin-forms/xaml/bindable-properties.md)
- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Эффект тени (пример)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
