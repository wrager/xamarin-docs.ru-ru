---
title: Триггеры
description: Реагирование на изменения интерфейса пользователя с XAML
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: af5912736e2a2bd7d3347d4aa199faa3fdfe41c7
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846449"
---
# <a name="triggers"></a>Триггеры

Триггеры позволяют указать действия декларативно в XAML, изменить внешний вид элементов управления на основе событий или изменения свойств.

Можно назначить триггер непосредственно в элемент управления, или добавьте его в словарь ресурсов уровня приложения или страницы для применения к нескольким элементам управления.

Существует четыре типа триггера.

* [Триггер свойства](#property) -возникает, когда свойство элемента управления к определенному значению.

* [Триггер данных](#data) - использует данные привязки к триггеру на основе свойств другого элемента управления.

* [Триггер события](#event) -происходит при возникновении события в элементе управления.

* [Триггер несколькими](#multi) -позволяет задать перед выполнением действия сразу нескольких условий триггера.

<a name="property" />

## <a name="property-triggers"></a>Триггеры свойств

Простой триггера может быть представлена исключительно в XAML, добавление `Trigger` коллекцию триггеров элемента к элементу управления.
В этом примере показано триггер, который изменяет `Entry` цвет фона при получении фокуса:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Ниже перечислены важные части объявлении триггера

* **TargetType** -тип элемента управления, к которому применяется триггер.

* **Свойство** -свойства элемента управления, за которым ведется наблюдение.

* **Значение** -значение, если это справедливо для отслеживаемых свойств, вызывает триггер для активации.

* **Метод задания** -коллекцию `Setter` могут быть добавлены элементы, и при соблюдении условия активации. Необходимо указать `Property` и `Value` для задания.

* **Действия EnterActions и ExitActions** (не показано) - записываются в коде и может использоваться в дополнение к (или вместо) `Setter` элементов. Они являются [описанных ниже](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Применение триггера с помощью стиля

Триггеры также могут добавляться в `Style` объявление элемента управления, на странице или приложению `ResourceDictionary`. В этом примере объявляется неявный стиль (т. е. не `Key` задано) это означает, что она будет применяться ко всем `Entry` элементов управления на странице.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Триггеры данных

Триггеры данных привязка данных используется для наблюдения за другого элемента управления, чтобы вызвать `Setter`s к вызову. Вместо `Property` в триггер свойства атрибута, задайте `Binding` атрибут для отслеживания для указанного значения.

В следующем примере используется синтаксис привязки данных `{Binding Source={x:Reference entry}, Path=Text.Length}` это, как мы называем свойства другого элемента управления. Если длина `entry` равен нулю, активации триггера. В этом образце триггер отключает кнопку, если ввод был пустым.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

Совет: при оценке `Path=Text.Length` всегда предоставлять значение по умолчанию для целевого свойства (например) `Text=""`), так как в противном случае он будет `null` и триггер не будет работать, как и ожидается.

Помимо указания `Setter`s, можно также предоставить [ `EnterActions` и `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Триггеры событий

`EventTrigger` Элемента требуется только `Event` свойства, такие как `"Clicked"` в приведенном ниже примере.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Обратите внимание, что не `Setter` элементы, но вместо этого ссылка на класс, определяемый `local:NumericValidationTriggerAction` требующий `xmlns:local` объявления на странице в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Сам класс реализует `TriggerAction` это означает, что он должен предоставить переопределение для `Invoke` метод, вызываемый, когда происходит событие триггера.

Для реализации действия триггера выполнить следующие действия:

* Реализовать универсальный `TriggerAction<T>` класса с параметром универсального типа соответствующего типа элемента управления триггер будут применяться к. Можно использовать является суперклассом, такие как `VisualElement` для записи действий триггера, которые работают со множеством элементов управления или укажите тип элемента управления `Entry`.

* Переопределить `Invoke` - это вызывается при каждом выполнении условий триггера.

* При необходимости предоставляют свойства, которые могут быть установлены в XAML-ФАЙЛЕ, при объявлении триггера (например, `Anchor`, `Scale`, и `Length` в этом примере).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Свойства, предоставляемые действие триггера можно задать в объявлении XAML следующим образом:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Будьте внимательны при совместном использовании триггеров в `ResourceDictionary`, один экземпляр будут совместно использовать элементы управления, поэтому любое состояние, которое настраивается один раз будет применяться к их все.

Обратите внимание, что триггеры событий не поддерживают `EnterActions` и `ExitActions` [описанных ниже](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Несколько триггеров

Объект `MultiTrigger` выглядит аналогично `Trigger` или `DataTrigger` за исключением того, может существовать более одного условия. Прежде чем должны выполняться все условия `Setter`активируются s.

Ниже приведен пример триггера для кнопки, которая связывает два различных набора входных данных (`email` и `phone`):

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` Коллекция может также содержать `PropertyCondition` элементы следующим образом:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>Создание триггера multi «требуются все»

Триггер multi обновляет свой элемент управления только в том случае, если все условия имеют значение true. Тестирование для «все значения длины поля ноль» (например, страницу входа в систему, где все входные данные должны быть завершены) является непростой задачей. так, как требуется, чтобы условие «где Text.Length > 0", но это не могут быть выражены в языке XAML.

Это можно сделать с помощью `IValueConverter`. Код преобразователя ниже преобразований `Text.Length` привязки в `bool` , указывающее, является ли поле пустым:


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Чтобы использовать этот преобразователь в триггере multi, сначала добавить его в словарь ресурсов страницы (вместе с пользовательской `xmlns:local` определения пространства имен):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

Ниже показан код XAML. Обратите внимание, следующие отличия от в первом примере триггер несколькими:

* Эта кнопка имеет `IsEnabled="false"` по умолчанию.
* Условия запуска нескольких использовать преобразователь, чтобы включить `Text.Length` значение в логическое значение.
* Если выполняются все условия `true`, метод задания делает кнопку `IsEnabled` свойства `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Эти снимки экрана Показать различие двух различных триггер приведенных выше примерах. В верхней части окна входной текст только в одном `Entry` , чтобы включить **Сохранить** кнопки.
В нижней части окна **входа** кнопка остается неактивной, пока оба поля содержат данные.


![](triggers-images/multi-requireall.png "Примеры multiTrigger")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>Действия EnterActions и ExitActions

Другой способ реализации изменений при возникновении триггера — путем добавления `EnterActions` и `ExitActions` коллекции и указав `TriggerAction<T>` реализации.

Вы можете предоставить *оба* `EnterActions` и `ExitActions` и `Setter`s в триггере, но имейте в виду, `Setter`немедленно вызывается s (они не ожидают `EnterAction` или `ExitAction` для Завершение). В качестве альтернативы можно выполнить все, что в коде, а не использовать `Setter`s вообще.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Как всегда, когда класс указывается в XAML следует объявить пространство имен, такие как `xmlns:local` как показано ниже:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` Код показан ниже:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Примечание: `EnterActions` и `ExitActions` на игнорируются **триггеров событий**.



## <a name="related-links"></a>Связанные ссылки

- [Пример использования триггеров](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
