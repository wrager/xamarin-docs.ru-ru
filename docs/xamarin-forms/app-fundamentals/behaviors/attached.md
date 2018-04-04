---
title: Вложенные поведения
description: Вложенные поведения являются статическими и с одного или нескольких вложенных свойств. В этой статье показано, как создавать и использовать вложенные поведения.
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: af891ad1ff1389d5a48c6c47ba1914b8d4dfc20f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="attached-behaviors"></a>Вложенные поведения

_Вложенные поведения являются статическими и с одного или нескольких вложенных свойств. В этой статье показано, как создавать и использовать вложенные поведения._

## <a name="overview"></a>Обзор

Вложенное свойство — это специальный тип привязываемые свойства. Они определены в одном классе, но присоединяется к другим объектам и являются распознается в XAML как атрибуты, которые содержат класс и имя свойства, разделенных точкой.

Можно определить вложенное свойство `propertyChanged` делегат, который выполняется при изменении значения свойства, например, если свойству присвоено значение в элементе управления. Когда `propertyChanged` выполняет делегат, он прошел ссылку на элемент управления, на котором он выполняется присоединен и параметров, которые содержат старое и новое значения для свойства. Этот делегат можно использовать для добавления новых функциональных возможностей для элемента управления, свойства, к которому присоединена, управляя ссылку, которая передается в, как показано ниже:

1. `propertyChanged` Делегат приводит ссылкой на элемент управления, полученные в качестве [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), тип элемента управления, поведение не предназначены для повышения уровня.
1. `propertyChanged` Делегат изменяет свойства элемента управления, вызывает методы элемента управления или регистрирует обработчики событий для события, предоставляемые для реализации поведения основные функциональные возможности элемента управления.

Вложенные поведения проблема состоит в том, что они определены в `static` класса, с `static` свойства и методы. Это усложняет создание вложенные поведения, которые имеют состояние. Кроме того поведение Xamarin.Forms заменили вложенные поведения как рекомендуемым способом создания поведение. Дополнительные сведения о Xamarin.Forms поведения см. в разделе [поведения Xamarin.Forms](~/xamarin-forms/app-fundamentals/behaviors/creating.md) и [для повторного использования поведения](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md).

## <a name="creating-an-attached-behavior"></a>Создание вложенное поведение

Образец приложения показывает `NumericValidationBehavior`, который выделяет значением, введенным пользователем в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управление красным цветом, если он не `double`. Поведение показано в следующем примере кода:

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

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
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior` Класс содержит вложенное свойство с именем `AttachBehavior` с `static` Get и set, управляющим добавлять или удалять поведения элемента управления, к которому будет присоединена. Это присоединенное свойство регистры `OnAttachBehaviorChanged` метод, который выполняется при изменении значения свойства. Этот метод регистрирует или отменяет регистрирует обработчик событий для [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) события, в зависимости от значения `AttachBehavior` вложенное свойство. Основные функциональные возможности поведения обеспечивается `OnEntryTextChanged` метод, который выполняет синтаксический анализ значение, введенное в [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) по пользователю и наборы `TextColor` свойство на красный, если значение не `double`.

## <a name="consuming-an-attached-behavior"></a>Использование вложенное поведение

`NumericValidationBehavior` Класса могут быть использованы добавление `AttachBehavior` присоединенному свойству [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) контролировать, как показано в следующем примере кода XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

Эквивалент [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) в C# показано в следующем примере кода:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

Во время выполнения поведение будет реагировать на взаимодействие с элементом управления, в соответствии с реализация поведения. Следующих снимках экрана демонстрации вложенные поведения на недопустимые входные данные:

[![](attached-images/screenshots-sml.png "Образец приложения с вложенное поведение")](attached-images/screenshots.png#lightbox "образец приложения с вложенное поведение")

> [!NOTE]
> Вложенные поведения записываются для типа элемента управления (или суперкласса, можно применить к многие элементы управления), и должны быть добавлены только к элементу управления совместимы. Попытка назначить поведение элемента управления несовместимые приведет к поведению неизвестный и зависит от реализации поведения.

### <a name="removing-an-attached-behavior-from-a-control"></a>Снятие вложенное поведение элемента управления

`NumericValidationBehavior` Класс может быть удален из элемента управления, задав `AttachBehavior` присоединенному свойству `false`, как показано в следующем примере кода XAML:

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

Эквивалент [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) в C# показано в следующем примере кода:

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

Во время выполнения `OnAttachBehaviorChanged` метод будет выполнен, когда значение `AttachBehavior` вложенному свойству задано `false`. `OnAttachBehaviorChanged` Метод будет затем отменять регистрацию обработчика событий для [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) событие. таким образом, поведение не выполнена, так как пользователь взаимодействует с элементом управления.

## <a name="summary"></a>Сводка

В этой статье показано, как создавать и использовать вложенные поведения. Вложенные поведения являются `static` классы с одного или нескольких вложенных свойств.


## <a name="related-links"></a>Связанные ссылки

- [Вложенные поведения (пример)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
