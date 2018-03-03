---
title: "Передача аргументов в XAML"
description: "В этой статье демонстрируется использование XAML атрибуты, которые могут использоваться для передачи аргументов в конструкторы не по умолчанию, для вызова методов, фабрики, а также указать тип универсального аргумента."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: a30dd9b33466ac6907322f8c6b586c012452a44f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="passing-arguments-in-xaml"></a>Передача аргументов в XAML

_В этой статье демонстрируется использование XAML атрибуты, которые могут использоваться для передачи аргументов в конструкторы не по умолчанию, для вызова методов, фабрики, а также указать тип универсального аргумента._

## <a name="overview"></a>Обзор

Часто она необходима для создания экземпляров объектов с помощью конструкторов, которые требуют аргументов или путем вызова метода статических создания. Это можно сделать в XAML с помощью `x:Arguments` и `x:FactoryMethod` атрибуты:

- `x:Arguments` Атрибута можно задать аргументы конструктора для конструктора не по умолчанию или объявления метода объекта фабрики. Дополнительные сведения см. в разделе [передачи аргументов конструктора](#constructor_arguments).
- `x:FactoryMethod` Атрибут используется для указания фабричный метод, который может использоваться для инициализации объекта. Дополнительные сведения см. в разделе [фабричные методы вызова](#factory_methods).

Кроме того `x:TypeArguments` атрибут может использоваться для указания аргументов универсального типа в конструктор универсального типа. Дополнительные сведения см. в разделе [указания аргумента универсального типа](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Передача аргументов конструктора

Аргументы могут быть переданы с помощью конструктора не по умолчанию `x:Arguments` атрибута. Каждый аргумент конструктора должны быть заключены в XML-элемент, который представляет тип аргумента. Xamarin.Forms поддерживает следующие элементы для основных типов:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

В следующем примере кода показано использование `x:Arguments` атрибута с тремя [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) конструкторы:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Количество элементов в пределах `x:Arguments` тег и типы из этих элементов должен соответствовать одному из [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) конструкторы. `Color` [Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) с одним параметром требует серого значение от 0 (черный) до 1 (белый цвет). `Color` [Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) с тремя параметрами требует красного, зеленого и синего значение в диапазоне от 0 до 1. `Color` [Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) с четырьмя параметрами добавляет альфа-канал в качестве четвертого параметра.

Результат вызова каждой Показать следующих снимках экрана [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) конструктор с заданным аргументом значениями:

![](passing-arguments-images/passing-arguments.png "BoxView.Color, указанный с помощью x: Arguments")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Вызов методов фабрики

В языке XAML, определяя метод можно вызывать методы фабрики имя `x:FactoryMethod` атрибута и его аргументов, с помощью `x:Arguments` атрибута. Метод фабрики `public static` метод, который возвращает объекты или значения того же типа, как класс или структура, определяющая методы.

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Структура определяет количество фабричные методы и в следующем примере кода показано, вызывающий три из них:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                      
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

Количество элементов в пределах `x:Arguments` тег и типы из этих элементов должен соответствовать аргументы вызываемого метода фабрики. [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) Фабричный метод требует четыре [ `Int32` ](https://developer.xamarin.com/api/type/System.Int32/) параметров, представляющих значений красного, зеленого, синего и альфа-канала, в диапазоне от 0 до 255, соответственно. [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Фабричный метод требует четыре [ `Double` ](https://developer.xamarin.com/api/type/System.Double/) параметры, которые представляют цветового тона, насыщенности, яркость и альфа-значения, в диапазоне от 0 до 1, соответственно. [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Требует фабричный метод [ `String` ](https://developer.xamarin.com/api/type/System.String/) , представляющий шестнадцатеричное число (A) цвета RGB.

Результат вызова каждой Показать следующих снимках экрана [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) фабричный метод со значениями аргументов:

![](passing-arguments-images/factory-methods.png "X: FactoryMethod, x: Arguments и BoxView.Color")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Указание аргумента универсального типа

Аргументы универсального типа для универсального типа конструктора можно задать с помощью `x:TypeArguments` атрибута, как показано в следующем примере кода:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="WinPhone, Windows" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Класс — это универсальный класс и должен создаваться с `x:TypeArguments` атрибут, соответствующий целевой тип. В [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) класса [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) атрибут может принимать один `string` значение или несколько разделенных запятыми `string` значения. В этом примере [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) свойству конкретную платформу [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Сводка

В этой статье демонстрируется использование атрибутов XAML, которые могут использоваться для передачи аргументов в конструкторы не по умолчанию, для вызова методов, фабрики, а также указать тип универсального аргумента.


## <a name="related-links"></a>Связанные ссылки

- [Пространства имен языка XAML](~/xamarin-forms/xaml/namespaces.md)
- [Передача аргументов конструктора (пример)](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Вызов методов фабрики (пример)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
