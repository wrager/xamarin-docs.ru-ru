---
title: Xamarin.Forms Basic привязок
description: В этой статье описывается использование Xamarin.Forms привязки данных, связывает пару свойства между двумя объектами, по крайней мере один из которых обычно является объект пользовательского интерфейса. Эти два объекта, называются целевой объект и источник.
ms.prod: xamarin
ms.assetid: 96553DF7-12EA-4FB2-AE85-3D1D59382B40
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: f932b7dfbcccb8f1c6ccb726f5e48c2df6e93c6c
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241693"
---
# <a name="xamarinforms-basic-bindings"></a>Xamarin.Forms Basic привязок

Привязка данных Xamarin.Forms связывает пару свойства между двумя объектами, по крайней мере один из которых обычно является объект пользовательского интерфейса. Эти два объекта, называются *целевой* и *источника*:

- *Целевой* является объекта (и свойства), на которой задана привязка данных.
- *Источника* — объект (и свойства), ссылается привязки данных.

Это различие иногда может быть немного сбить с толку: В самом простом случае данные передаются из источника в целевой объект, это означает, что целевое свойство имеет значение из значения свойства source. Однако в некоторых случаях можно в качестве альтернативы потока данных от целевого объекта в источнике или в обоих направлениях. Чтобы избежать путаницы, имейте в виду, что целевой объект всегда является объект, на которой задана привязка к данным, даже если он предоставление данных, а не получение данных.

## <a name="bindings-with-a-binding-context"></a>Привязки с контекстом привязки

Несмотря на то, что привязки к данным обычно указаны полностью в XAML, полезно в разделе привязки данных в коде. **Привязки базовый код** страница содержит файл XAML с `Label` и `Slider`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicCodeBindingPage"
             Title="Basic Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="48"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Slider` Имеет значение для диапазона от 0 до 360. Цель данной программы — Поворот `Label` , управляя `Slider`.

Без привязки данных, необходимо установить `ValueChanged` событие `Slider` обработчика событий, который обращается к `Value` свойство `Slider` и присваивает это значение `Rotation` свойство `Label`. Привязка данных позволяет автоматизировать задания; обработчик событий и код внутри его больше не требуется.

Можно задать привязку на экземпляр класса, производного от [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/), включающее `Element`, `VisualElement`, `View`, и `View` производных продуктов.  Привязка всегда устанавливается в целевом объекте. Привязка связывает объект источника. Чтобы задать привязки данных, используйте следующие два члена целевого класса:

- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Свойство задает исходный объект.
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Метод задает свойство источника и целевого свойства.

В этом примере `Label` является целевого объекта привязки и `Slider` является источником привязки. Изменения в `Slider` источника влияют на смену `Label` цели. Потоки данных из источника в целевой объект.

`SetBinding` Метода, определенного `BindableObject` в качестве аргумента типа [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) откуда [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) класс является производным, но существуют другие `SetBinding` методы определяется [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) класса. Файл кода в **привязки базовый код** образец использует более простой [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) метод расширения из этого класса.

```csharp
public partial class BasicCodeBindingPage : ContentPage
{
    public BasicCodeBindingPage()
    {
        InitializeComponent();

        label.BindingContext = slider;
        label.SetBinding(Label.RotationProperty, "Value");
    }
}
```

`Label` Объект является целевого объекта привязки, то есть объекта, на которой это свойство имеет значение и в котором выполняется вызов метода. `BindingContext` Свойство указывает источник привязки, который является `Slider`.

`SetBinding` Метод вызывается для целевого объекта привязки, но указывает целевое свойство и свойство source. Свойство target указывается как `BindableProperty` объект: `Label.RotationProperty`. Свойство источника указан как строка и указывает `Value` свойство `Slider`.

`SetBinding` Метод раскрывает одно из наиболее важных правил привязки данных:

*Целевое свойство должно поддерживаться привязываемые свойства.*

Это правило предполагает, что целевой объект должен быть экземпляром класса, производного от `BindableObject`. В разделе [ **привязываемые свойства** ](~/xamarin-forms/xaml/bindable-properties.md) статье Обзор связываемых объектов и свойства для привязки.

Нет таких правил для свойства источника, которое указывается в виде строки. Внутри отражение используется для доступа к Фактическое свойство. В этом случае, однако `Value` также реализуется привязываемые свойства.

Код может быть немного упрощен: `RotationProperty` привязываемые свойства определяется `VisualElement`и наследуется `Label` и `ContentPage` , поэтому имя класса, которые не обязательно в `SetBinding` вызова:

```csharp
label.SetBinding(RotationProperty, "Value");
```

Тем не менее включая имя класса является хорошим напоминание целевого объекта.

Во время работы `Slider`, `Label` поворачивает соответствующим образом:

[![Привязка кода Basice](basic-bindings-images/basiccodebinding-small.png "привязки базовый код")](basic-bindings-images/basiccodebinding-large.png#lightbox "привязки базовый код")

**Простые привязки Xaml** страница идентична **простые привязки кода** за исключением того, он определяет привязки данных в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BasicXamlBindingPage"
             Title="Basic XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               BindingContext="{x:Reference Name=slider}"
               Rotation="{Binding Path=Value}" />

        <Slider x:Name="slider"
                Maximum="360"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Так же, как показано в коде, привязка данных имеет значение в целевом объекте, который является `Label`. Участвуют два расширения разметки XAML. Это мгновенно распознаваемым методом разделители фигурную скобку.

- `x:Reference` Расширения разметки требуется для ссылки на исходный объект, который является `Slider` с именем `slider`.
- `Binding` Ссылки расширения разметки `Rotation` свойство `Label` для `Value` свойство `Slider`.

См. в статье [расширения разметки XAML](~/xamarin-forms/xaml/markup-extensions/index.md) Дополнительные сведения о расширениях разметки XAML. `x:Reference` Поддерживается расширение разметки [ `ReferenceExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) класса; `Binding` поддерживается [ `BindingExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) класса. Как XML указывает префиксы пространства имен, `x:Reference` является частью спецификации XAML 2009 г., пока `Binding` является частью Xamarin.Forms. Обратите внимание, что отображаются без кавычек в фигурные скобки.

Можно легко забыть `x:Reference` расширения разметки при задании `BindingContext`. Чаще всего ошибочно установить свойство непосредственно имя источника привязки к следующим образом:

```xaml
BindingContext="slider"
```

Но это не так. Задает, разметку `BindingContext` свойства `string` объекта, символы которых орфографии «ползунок»!

Обратите внимание, что указано свойство источника с [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) свойство `BindingExtension`, который соответствует [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) свойство [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) класса.

Разметка, показанная на **Basic XAML привязки** страницы можно упростить, представив: расширения разметки XAML, такие как `x:Reference` и `Binding` может иметь *свойства содержимого* атрибуты определено, что для XAML расширения разметки означает, что имя свойства не нужно отображать. `Name` Свойство является свойством содержимого `x:Reference`и `Path` свойство является свойством содержимого `Binding`, это означает, что они может быть исключен из выражений:

```xaml
<Label Text="TEXT"
       FontSize="80"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       BindingContext="{x:Reference slider}"
       Rotation="{Binding Value}" />
```

## <a name="bindings-without-a-binding-context"></a>Привязки без контекста привязки

`BindingContext` Свойство является важным компонентом привязок данных, но это не всегда необходимо. Исходный объект, вместо этого может быть указано в `SetBinding` вызова или `Binding` расширения разметки.

Это показано в **привязки кода альтернативой** образца. Файл XAML аналогичен **привязки базовый код** образец разницей, что `Slider` определено для элемента управления `Scale` свойство `Label`. По этой причине `Slider` имеет значение для диапазона &ndash;2 до 2:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeCodeBindingPage"
             Title="Alternative Code Binding">
    <StackLayout Padding="10, 0">
        <Label x:Name="label"
               Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

В файле кода устанавливается привязка, с помощью [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) метода, определенного `BindableObject`. Аргумент является [конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) для [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) класса:

```csharp
public partial class AlternativeCodeBindingPage : ContentPage
{
    public AlternativeCodeBindingPage()
    {
        InitializeComponent();

        label.SetBinding(Label.ScaleProperty, new Binding("Value", source: slider));
    }
}
```

`Binding` Конструктор имеет 6 параметров, поэтому `source` с именованный аргумент указан параметр. Аргумент является `slider` объекта.

Выполнение программы может быть немного неожиданным:

[![Альтернативный образец программного кода привязки](basic-bindings-images/alternativecodebinding-small.png "альтернативный образец программного кода привязки")](basic-bindings-images/alternativecodebinding-large.png#lightbox "привязки альтернативный образец программного кода")

В левой части экрана iOS показано экрана при первой загрузке страницы. Где является `Label`?

Проблема заключается в том `Slider` начальное значение 0. В результате `Scale` свойство `Label` требуется также задать значение 0, переопределив значения по умолчанию 1. В результате `Label` , изначально невидимыми. Как показано на снимке экрана Android и универсальной платформы Windows (UWP), можно управлять `Slider` вносить `Label` снова отображаются, но его исходный исчезновением толку.

Вы узнаете о в [следующей статье](binding-mode.md) как избежать этой проблемы, инициализация `Slider` из значения по умолчанию `Scale` свойство.

**Привязкой XAML альтернативой** страницы показана привязка эквивалентные полностью в XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.AlternativeXamlBindingPage"
             Title="Alternative XAML Binding">
    <StackLayout Padding="10, 0">
        <Label Text="TEXT"
               FontSize="40"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="{Binding Source={x:Reference slider},
                               Path=Value}" />

        <Slider x:Name="slider"
                Minimum="-2"
                Maximum="2"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Теперь `Binding` расширение разметки имеет два свойства, заданные, `Source` и `Path`, разделив их запятой. Они могут находиться в той же строке, если вы предпочитаете:

```xaml
Scale="{Binding Source={x:Reference slider}, Path=Value}" />
```

`Source` Свойству к внедренному `x:Reference` расширение разметки, которое в противном случае имеет тот же синтаксис, как параметр `BindingContext`. Обратите внимание, что без кавычек отображаются в фигурные скобки, и что эти два свойства должны быть разделены запятыми.

Свойство содержимого `Binding` расширение разметки — `Path`, но `Path=` часть расширение разметки можно устранить, только если это первое свойство в выражении. Чтобы исключить `Path=` часть, требуется поменять местами два свойства:

```xaml
Scale="{Binding Value, Source={x:Reference slider}}" />
```

Несмотря на то, что расширения разметки XAML, обычно разделяются в фигурные скобки, они также может быть выражен как элементы объектов:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Source="{x:Reference slider}"
                 Path="Value" />
    </Label.Scale>
</Label>
```

Теперь `Source` и `Path` свойства являются обычные атрибуты XAML: значения отображаться в кавычках и атрибуты не разделяются запятыми. `x:Reference` Расширения разметки также может стать элемента объекта:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand">
    <Label.Scale>
        <Binding Path="Value">
            <Binding.Source>
                <x:Reference Name="slider" />
            </Binding.Source>
        </Binding>
    </Label.Scale>
</Label>
```

Этот синтаксис не стандартным, но иногда это необходимо при использовании сложных объектов.

В примерах выше задается `BindingContext` свойство и `Source` свойство `Binding` для `x:Reference` расширения разметки для ссылки на другое представление, на странице. Эти свойства имеют тип `Object`, и их можно задать для любого объекта, который содержит свойства, которые подходят для источников привязки.

В статьях вперед, вы обнаружите, что можно установить `BindingContext` или `Source` свойства `x:Static` расширения разметки для ссылки на значение статического свойства или поля, или `StaticResource` расширения разметки для ссылки на объект, сохраненный в словарь ресурсов, или непосредственно в объект, который является обычно (но не всегда) экземпляр ViewModel.

`BindingContext` Свойство также может быть присвоено `Binding` объекта, чтобы `Source` и `Path` свойства `Binding` определить контекст привязки.

## <a name="binding-context-inheritance"></a>Наследование контекста привязки

В этой статье вы видели, можно указать исходный объект с использованием `BindingContext` свойство или `Source` свойство `Binding` объекта. Если заданы обе `Source` свойство `Binding` имеет приоритет над `BindingContext`.

`BindingContext` Свойство имеет очень важной характеристикой:

*Параметр `BindingContext` свойство наследуется по визуальному дереву.*

Как вы увидите, это может быть очень удобно для упрощения выражения привязки, а в некоторых случаях &mdash; особенно в сценариях Model-View-ViewModel (MVVM) &mdash; очень важно.

**Наследование контекста привязки** образец является простой демонстрационный наследования контекста привязки:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.BindingContextInheritancePage"
             Title="BindingContext Inheritance">
    <StackLayout Padding="10">

        <StackLayout VerticalOptions="FillAndExpand"
                     BindingContext="{x:Reference slider}">

            <Label Text="TEXT"
                   FontSize="80"
                   HorizontalOptions="Center"
                   VerticalOptions="EndAndExpand"
                   Rotation="{Binding Value}" />

            <BoxView Color="#800000FF"
                     WidthRequest="180"
                     HeightRequest="40"
                     HorizontalOptions="Center"
                     VerticalOptions="StartAndExpand"
                     Rotation="{Binding Value}" />
        </StackLayout>

        <Slider x:Name="slider"
                Maximum="360" />

    </StackLayout>
</ContentPage>
```

`BindingContext` Свойство `StackLayout` равно `slider` объекта. Этот контекст привязки наследуются оба `Label` и `BoxView`, оба о том, что их `Rotation` значение свойства `Value` свойства `Slider`:

[![Привязки наследования контекста](basic-bindings-images/bindingcontextinheritance-small.png "привязки наследования контекста")](basic-bindings-images/bindingcontextinheritance-large.png#lightbox "привязки наследования контекста")

В [следующей статье](binding-mode.md), будет отображен как *режим привязки* можно изменить поток данных между исходной и целевой объекты.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Данные привязки Глава из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
