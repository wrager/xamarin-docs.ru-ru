---
title: "Привязываемые свойства"
description: "В Xamarin.Forms привязываемые свойства расширяется функциональные возможности свойств среды выполнения (CLR). Привязываемые свойства — специальный тип свойства, где значение свойства отслеживается системой свойств Xamarin.Forms. В этой статье содержатся вводные привязываемые свойства и показано, как создавать и использовать их."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1EE869D8-6FE1-45CA-A0AD-26EC7D032AD7
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/02/2016
ms.openlocfilehash: ab8c4cfd92a048efb87f7508e53fc024a9c46405
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="bindable-properties"></a>Привязываемые свойства

_В Xamarin.Forms привязываемые свойства расширяется функциональные возможности свойств среды выполнения (CLR). Привязываемые свойства — специальный тип свойства, где значение свойства отслеживается системой свойств Xamarin.Forms. В этой статье содержатся вводные привязываемые свойства и показано, как создавать и использовать их._

## <a name="overview"></a>Обзор

Привязываемые свойства расширяют функциональность свойства CLR, Резервное свойство с [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) типа, вместо Резервное свойство с полем. Привязываемые свойства служит для предоставления системы свойств, которая поддерживает привязку данных, стили, шаблоны и задать значения с помощью родительско дочерних отношений. Кроме того привязываемые свойства можно указать значения по умолчанию, проверку значений свойств и обратные вызовы, которые отслеживают изменения свойств.

Свойства должны быть реализованы как привязываемые свойства для поддержки один или несколько из следующих компонентов:

- Выступает в качестве допустимого *целевой* свойство для привязки данных.
- Задание свойства с помощью [стиль](~/xamarin-forms/user-interface/styles/index.md).
- Предоставление значением свойства по умолчанию, отличный от установленного по умолчанию для типа свойства.
- Проверка значения свойства.
- Мониторинг изменений свойств.

Примеры привязываемые свойства Xamarin.Forms [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/), [ `Button.BorderRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/), и [ `StackLayout.Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/). Каждое свойство привязки имеет соответствующий `public static readonly` свойство типа [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) , отображается в том же классе, и является идентификатором привязываемые свойства. Например, соответствующий идентификатор привязываемые свойства для `Label.Text` свойство [ `Label.TextProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Label.TextProperty/).

<a name="consuming-bindable-property" />

## <a name="creating-and-consuming-a-bindable-property"></a>Создание и использование привязываемые свойства

Процесс создания привязываемые свойства выглядит следующим образом:

1. Создание [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляра с одним из [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) перегруженных версий метода.
1. Определение методов доступа свойства для [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляра.

Обратите внимание, что все [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляры должны создаваться в потоке пользовательского интерфейса. Это означает, что только код, который выполняется в потоке пользовательского интерфейса можно получить или задать значение привязываемые свойства. Тем не менее `BindableProperty` экземпляров может осуществляться из других потоков маршалинг в поток пользовательского интерфейса с [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) метод.

### <a name="creating-a-property"></a>Создание свойства

Для создания `BindableProperty` экземпляр, содержащий класс должен быть производным от [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) класса. Тем не менее `BindableObject` класс высокого уровня в иерархии классов, поэтому большинство классов, используемый для привязки свойства поддержки функциональные возможности пользовательского интерфейса.

Привязываемые свойства могут создаваться путем объявления `public static readonly` свойство типа [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/). Привязываемые свойства должно быть присвоено возвращаемое значение одного из [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) перегруженных версий метода. Объявление должно быть в теле [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) производного класса, но за пределами определения членов.

Как минимум, необходимо указать идентификатор при создании [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/), а также следующие параметры:

- Имя [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/).
- Тип свойства.
- Тип объекта.
- Значение по умолчанию для свойства. Это гарантирует, что данное свойство всегда возвращает значение по умолчанию конкретной, когда оно не определено, может отличаться от значения по умолчанию для типа свойства. Значение по умолчанию будут восстановлены при [ `ClearValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.ClearValue/p/Xamarin.Forms.BindableProperty/) метод вызывается для привязки свойства.

Ниже приведен пример привязки свойства, с идентификатором и значения четыре обязательных параметров:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null);
```

При этом создается [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляр с именем `EventName`, типа `string`. Свойство принадлежит `EventToCommandBehavior` класса и имеет значение по умолчанию `null`. Соглашение об именовании для свойства связывания заключается в том, что идентификатор привязываемые свойства должна соответствовать имя свойства, указанное в `Create` метод, с добавлением «свойство». Таким образом, в приведенном выше примере указан идентификатор привязываемые свойства `EventNameProperty`.

Кроме того при создании [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляра следующие параметры могут быть заданы:

- Режим привязки. Используется для указания направления, в котором будет распространено изменениях значения свойства. В режиме привязки по умолчанию, изменения будут распространяться из *источника* для *целевой*.
- Проверка делегат, который вызывается, если задать значение свойства. Дополнительные сведения см. в разделе [обратные вызовы проверки](#validation).
- Делегат, который будет вызываться при изменении значения свойства изменено свойство. Дополнительные сведения см. в разделе [обнаружение изменения свойств](#propertychanges).
- Изменение делегата, который будет вызываться при изменении значения свойства будут свойство. Этот делегат имеет ту же сигнатуру, что и делегат изменения свойства.
- Делегат значение принудительного, который будет вызываться при изменении значения свойства. Дополнительные сведения см. в разделе [Coerce обратных вызовов значение](#coerce).
- Объект `Func` , используемый для инициализации значения свойства по умолчанию. Дополнительные сведения см. в разделе [создание значения по умолчанию с Func](#defaultfunc).

### <a name="creating-accessors"></a>Создание методов доступа

К свойствам должны использовать синтаксис свойства для доступа к привязываемые свойства. `Get` Метод доступа должен возвращать значение, которое содержится в соответствующее свойство, связываемое. Это можно сделать путем вызова [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/) метод, передавая идентификатор привязываемые свойства, для которого требуется получить значение, а затем приведение результата в требуемый тип. `Set` Доступа следует установить значение соответствующего свойства привязки. Это можно сделать путем вызова [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) метод, передавая идентификатор привязываемые свойства, для которого требуется задать значение и значение для установки.

В следующем примере кода показаны методы доступа для `EventName` привязываемые свойства:

```csharp
public string EventName {
  get { return (string)GetValue (EventNameProperty); }
  set { SetValue (EventNameProperty, value); }
}
```

### <a name="consuming-a-bindable-property"></a>Использование привязываемые свойства

После создания привязываемые свойства могут быть востребованы из XAML-кода. В языке XAML это достигается путем объявления пространства имен с префиксом с объявления пространства имен, указывающее имя пространства имен CLR и при необходимости, имя сборки. Дополнительные сведения см. в разделе [пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md).

В следующем примере кода показаны пространства имен XAML для пользовательского типа, который содержит привязываемые свойства, которое определяется в сборке, в качестве кода приложения, ссылается на пользовательский тип:

```xaml
<ContentPage ... xmlns:local="clr-namespace:EventToCommandBehavior" ...>
  ...
</ContentPage>
```

Объявление пространства имен используется при задании `EventName` привязываемые свойства, как демонстрируется в следующем примере кода XAML:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var listView = new ListView ();
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  ...
});
```

<a name="advanced" />

## <a name="advanced-scenarios"></a>Расширенные сценарии

При создании [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляра, существует ряд необязательных параметров, которые могут быть установлены для реализации сценариев дополнительного привязываемые свойства. В этом разделе рассматриваются эти сценарии.

<a name="propertychanges" />

### <a name="detecting-property-changes"></a>Обнаружение изменений свойств

Объект `static` метод обратного вызова изменения свойства может быть зарегистрирован привязываемые свойства путем указания `propertyChanged` параметр [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) метод. Указанный метод обратного вызова будет вызываться при изменении значения свойства привязки.

В следующем примере кода показан способ `EventName` регистры привязываемые свойства `OnEventNameChanged` метода в качестве метода обратного вызова изменения свойства:

```csharp
public static readonly BindableProperty EventNameProperty =
  BindableProperty.Create (
    "EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
...

static void OnEventNameChanged (BindableObject bindable, object oldValue, object newValue)
{
  // Property changed implementation goes here
}
```

В методе обратного вызова изменения свойства [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) параметр используется для обозначения того, какой экземпляр класс-владелец обнаружил изменения и значения двух `object` параметров представляют старое и новое значения свойство привязки.

<a name="validation" />

### <a name="validation-callbacks"></a>Обратные вызовы проверки

Объект `static` метод обратного вызова проверки может быть зарегистрирован привязываемые свойства путем указания `validateValue` параметр [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) метод. Указанный метод обратного вызова будет вызываться, если задано значение привязываемые свойства.

В следующем примере кода показан способ `Angle` регистры привязываемые свойства `IsValidValue` метода в качестве метода обратного вызова проверки:

```csharp
public static readonly BindableProperty AngleProperty =
  BindableProperty.Create ("Angle", typeof(double), typeof(HomePage), 0.0, validateValue: IsValidValue);
...

static bool IsValidValue (BindableObject view, object value)
{
  double result;
  bool isDouble = double.TryParse (value.ToString (), out result);
  return (result >= 0 && result <= 360);
}
```

Обратные вызовы проверки предоставляются со значением и должен возвращать `true` Если допустимым является значение для свойства, в противном случае `false`. Будет ли вызываться исключение, если обратный вызов проверки возвращает `false`, что должны обрабатываться разработчиком. Типичное использование метода обратного вызова проверки ограничение значения целых при привязываемых свойству. Например `IsValidValue` метод проверяет, что значение свойства `double` в диапазоне от 0 до 360.

<a name="coerce" />

### <a name="coerce-value-callbacks"></a>Приведенных значений

Объект `static` приведение значения метод обратного вызова может быть зарегистрирован привязываемые свойства путем указания `coerceValue` параметр [ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/) метод. Указанный метод обратного вызова будет вызываться при изменении значения свойства привязки.

Приведение значения обратные вызовы используются для принудительного повторного вычисления, привязываемые свойства при изменении значения свойства. Например значение принудительного обратного вызова можно использовать чтобы значение одного свойства привязки не больше, чем значение другого привязываемые свойства.

В следующем примере кода показан способ `Angle` регистры привязываемые свойства `CoerceAngle` метода в качестве метода обратного вызова значение принудительного:

```csharp
public static readonly BindableProperty AngleProperty = BindableProperty.Create (
  "Angle", typeof(double), typeof(HomePage), 0.0, coerceValue: CoerceAngle);
public static readonly BindableProperty MaximumAngleProperty = BindableProperty.Create (
  "MaximumAngle", typeof(double), typeof(HomePage), 360.0);
...

static object CoerceAngle (BindableObject bindable, object value)
{
  var homePage = bindable as HomePage;
  double input = (double)value;

  if (input > homePage.MaximumAngle) {
    input = homePage.MaximumAngle;
  }

  return input;
}
```

`CoerceAngle` Метод проверяет значение `MaximumAngle` свойство и если `Angle` он превышает значение свойства, он приводит значение к `MaximumAngle` значение свойства.

<a name="defaultfunc" />

### <a name="creating-a-default-value-with-a-func"></a>Создание значения по умолчанию с Func

Объект `Func` можно использовать для инициализации значения по умолчанию привязываемые свойства, как показано в следующем примере кода:

```csharp
public static readonly BindableProperty SizeProperty =
  BindableProperty.Create ("Size", typeof(double), typeof(HomePage), 0.0,
  defaultValueCreator: bindable => Device.GetNamedSize (NamedSize.Large, (Label)bindable));
```

`defaultValueCreator` Параметра равным `Func` , вызывающий [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) метод для возврата `double` , представляющий именованный размер шрифта, используемый в [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) на собственной платформы.

## <a name="summary"></a>Сводка

В этой статье сведения о привязываемые свойства и было показано, как создавать и использовать их. Привязываемые свойства — специальный тип свойства, где значение свойства отслеживается системой свойств Xamarin.Forms.


## <a name="related-links"></a>Связанные ссылки

- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [События на поведение команды (пример)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Обратный вызов проверки (пример)](https://developer.xamarin.com/samples/xamarin-forms/xaml/validationcallback/)
- [Приведение значения обратного вызова (пример)](https://developer.xamarin.com/samples/xamarin-forms/xaml/coercevaluecallback/)
- [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- [BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)
