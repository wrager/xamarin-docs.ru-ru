---
title: Xamarin.Forms поведения
description: Xamarin.Forms поведения создаются путем наследования от поведение или поведение<T> класса. В этой статье показано, как создавать и использовать Xamarin.Forms поведения.
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3a86e7713620eff90db995941eb35df7bc393a76
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848295"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms поведения

_Xamarin.Forms поведения создаются путем наследования от поведение или поведение<T> класса. В этой статье показано, как создавать и использовать Xamarin.Forms поведения._

## <a name="overview"></a>Обзор

Процесс создания Xamarin.Forms поведение выглядит следующим образом:

1. Создайте класс, наследующий от [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) или [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) класса, где `T` тип элемента управления, к которому должно применяться поведение.
1. Переопределить [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) метод для выполнения дополнительных настроек.
1. Переопределить [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) метод, чтобы выполнить необходимую очистку.
1. Реализуйте основные функциональные возможности поведения.

Это приводит к структуре, показано в следующем примере кода:

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Метод вызывается сразу после поведение элемента управления. Этот метод получает ссылку на элемент управления, к которому он подключен и может использоваться для регистрации обработчиков событий или выполнять другие настройки, необходимые для поддержки функциональных возможностей поведения. Например может подписаться на событие в элементе управления. Функциональных возможностей поведения, то может быть реализован в обработчике событий для события.

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Метод возникает, когда поведение удаляется из элемента управления. Этот метод получает ссылку на элемент управления, к которому он подключен и позволяет выполнить необходимую очистку. Например вы может отменить подписку на событие элемента управления для предотвращения утечек памяти.

Поведение может быть обработано путем присоединения ее к [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) коллекцию соответствующий элемент управления.

## <a name="creating-a-xamarinforms-behavior"></a>Создание поведения Xamarin.Forms

Образец приложения показывает `NumericValidationBehavior`, который выделяет значением, введенным пользователем в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управление красным цветом, если он не `double`. Поведение показано в следующем примере кода:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Является производным от [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) класса, где `T` — [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Метод регистрирует обработчик событий для [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) события, с [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) метод отмены регистрации `TextChanged`события памяти во избежание утечки. Основные функциональные возможности поведения обеспечивается `OnEntryTextChanged` метод, который выполняет синтаксический анализ значения, введенного пользователем в `Entry`и задает [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) свойство на красный, если значение не `double`.

> [!NOTE]
> Xamarin.Forms не устанавливает `BindingContext` поведения, так как поведения можно совместно и применить к нескольким элементам управления через стили.

## <a name="consuming-a-xamarinforms-behavior"></a>Использование поведения Xamarin.Forms

Каждому элементу управления Xamarin.Forms [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) коллекции, к которой можно добавить один или несколько вариантов поведения, как показано в следующем примере кода XAML:

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

Эквивалент [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) в C# показано в следующем примере кода:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

Во время выполнения поведение будет реагировать на взаимодействие с элементом управления, в соответствии с реализация поведения. Следующих снимках экрана демонстрации поведения на недопустимые входные данные:

[![](creating-images/screenshots-sml.png "Образец приложения с поведением Xamarin.Forms")](creating-images/screenshots.png#lightbox "образец приложения с поведением Xamarin.Forms")

> [!NOTE]
> Поведение записываются для типа элемента управления (или суперкласса, можно применить к многие элементы управления) и должны быть добавлены только к элементу управления совместимы. Предпринимается попытка назначить поведение элемента управления несовместимые приведет к созданию исключения.

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>Использование Xamarin.Forms поведения со стилем

Поведение также могут использоваться стилем явным или неявным. Однако создание стиль, который задает [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) свойство элемента управления невозможна, так как свойство доступно только для чтения. Решение — добавить вложенное свойство в класс поведение, управляющее Добавление и удаление поведение. Процесс выглядит следующим образом:

1. Добавление класса поведения, который будет использоваться для управления, добавлять или удалять поведения, элементом управления, к которому будет поведение вложенное свойство. Убедитесь, что присоединенное свойство регистрирует `propertyChanged` делегат, который выполняется при изменении значения свойства.
1. Создание `static` Get и set для вложенного свойства.
1. Реализовать логику в `propertyChanged` делегат для добавления и удаления поведение.

В следующем примере кода показано вложенное свойство, определяющее, добавление и удаление `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior` Класс содержит вложенное свойство с именем `AttachBehavior` с `static` Get и set, управляющим добавлять или удалять поведения элемента управления, к которому будет присоединена. Это присоединенное свойство регистры `OnAttachBehaviorChanged` метод, который выполняется при изменении значения свойства. Этот метод добавляет или удаляет способ управления, в зависимости от значения `AttachBehavior` вложенное свойство.

В следующем примере кода показан *явных* стиля для `NumericValidationBehavior` , использующий `AttachBehavior` присоединенное свойство и который может применяться к [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) элементов управления:

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Может применяться к [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления, установив его [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства `Style` экземпляра с помощью `StaticResource` расширения разметки, как показано в следующем примере кода:

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

Дополнительные сведения о стилях см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md).

> [!NOTE]
> При добавлении свойства связывания на поведение, которое задается или запросы на языке XAML, при создании поведения, имеют состояние они не могут совместно использоваться между элементами управления в `Style` в `ResourceDictionary`.

### <a name="removing-a-behavior-from-a-control"></a>Снятие поведение элемента управления

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Метод возникает, когда поведение удаляется из элемента управления, а также позволяет выполнить необходимую очистку например отмены подписки на событие, чтобы предотвратить утечку памяти. Тем не менее, поведение не удаляются неявно из элементов управления Если элемент управления [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) коллекция была изменена с `Remove` или `Clear` метод. В следующем примере кода демонстрируется удаление определенное поведение элемента управления `Behaviors` коллекции:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

Кроме того, элемента управления [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) очистить коллекцию, как показано в следующем примере кода:

```csharp
entry.Behaviors.Clear();
```

Кроме того Обратите внимание, что поведения не удаляются неявно из элементов управления при страниц извлекаются из стека навигации. Вместо этого они должны быть явно удалены до переход за пределы области страницы.

## <a name="summary"></a>Сводка

В этой статье показано, как создавать и использовать Xamarin.Forms поведения. Xamarin.Forms поведения создаются путем наследования от [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) или [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) класса.


## <a name="related-links"></a>Связанные ссылки

- [Поведение Xamarin.Forms (пример)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Поведение Xamarin.Forms применен стиль (пример)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [Поведение](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Поведение<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
