---
title: Функции плавности в Xamarin.Forms
description: Xamarin.Forms включает замедление класс, позволяющий указать функцию передачи, который определяет способ ускорить анимации, или замедлять работу, как они выполняются. В этой статье показано, как использовать предварительно определенные функции плавности и создание функции плавности.
ms.prod: xamarin
ms.assetid: E6F124C7-A161-4C1F-AF40-52F0935E54DE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 9398a1b9cf4e5f6fd18f2213a7cf55e9cbb93ef0
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243146"
---
# <a name="easing-functions-in-xamarinforms"></a>Функции плавности в Xamarin.Forms

_Xamarin.Forms включает замедление класс, позволяющий указать функцию передачи, который определяет способ ускорить анимации, или замедлять работу, как они выполняются. В этой статье показано, как использовать предварительно определенные функции плавности и создание функции плавности._


[ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) Класс определяет количество функции плавности, которые могут быть использованы анимации:

- [ `BounceIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) Функции реалистичной анимации входа будет передаваться анимации в начале.
- [ `BounceOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/) Функции реалистичной анимации входа будет передаваться анимации в конце.
- [ `CubicIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/) Функция плавности медленно ускоряется анимации.
- [ `CubicInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/) Функции реалистичной анимации входа анимации в начале ускоряется и замедляется анимации в конце.
- [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/) Функция плавности быстро замедляется анимации.
- [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Функции реалистичной анимации входа использует постоянной скоростью и имеет значение по умолчанию функции реалистичной анимации входа.
- [ `SinIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/) Функция плавности плавно ускоряется анимации.
- [ `SinInOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/) Функция плавности плавно ускоряется анимации в начале и плавно замедляется анимации в конце.
- [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/) Функция плавности плавно замедляется анимации.
- [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Функции реалистичной анимации входа задает очень быстро ускорения окончания анимации.
- [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Функции реалистичной анимации входа задает быстро замедлением окончания анимации.

`In` И `Out` суффиксы укажите, является ли заметно в начале анимации в конце, и оба эффекта, предоставляемые функции для реалистичной анимации.

Кроме того можно создать пользовательские функции реалистичной анимации. Дополнительные сведения см. в разделе [пользовательские функции плавности](#customeasing).

## <a name="consuming-an-easing-function"></a>Использование функции плавности

Методы расширения анимации в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класс разрешить функции плавности определен как окончательный параметра метода, то, как показано в следующем примере кода:

```csharp
await image.TranslateTo(0, 200, 2000, Easing.BounceIn);
await image.ScaleTo(2, 2000, Easing.CubicIn);
await image.RotateTo(360, 2000, Easing.SinInOut);
await image.ScaleTo(1, 2000, Easing.CubicOut);
await image.TranslateTo(0, -200, 2000, Easing.BounceOut);
```

Задавая функции плавности анимации, скорость анимации становится нелинейного и создает эффект, предоставляемые функции для реалистичной анимации. Пропуск функции плавности при создании анимации вызывает анимации должно использоваться значение по умолчанию [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) функции, которая создает линейная скорость замедления.

Дополнительные сведения об использовании методов расширения анимации в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) см. в описании [простой анимации](~/xamarin-forms/user-interface/animation/simple.md). Функции плавности может также использоваться [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) класса. Дополнительные сведения см. в разделе [Настройка анимации](~/xamarin-forms/user-interface/animation/custom.md).

<a name="customeasing" />

## <a name="custom-easing-functions"></a>Настройка функции плавности

Существует три основных подхода для создания пользовательской функции реалистичной анимации.

1. Создайте метод, который принимает `double` аргумента и возвращает `double` результат.
1. Создайте таблицу `Func<double, double>`.
1. Укажите в качестве аргумента для функции для реалистичной анимации [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) конструктор.

Во всех трех случаях функции плавности должен возвращать 0 для аргумента 0 и 1 для аргумента 1. Тем не менее любое значение можно получить от значения аргумента от 0 до 1. Оба этих подхода теперь обсуждаются в свою очередь.

### <a name="custom-easing-method"></a>Пользовательский метод реалистичной анимации

Функции плавности могут быть определены как метод, который принимает `double` аргумента и возвращает `double` привести, как показано в следующем примере кода:

```csharp
await image.TranslateTo(0, 200, 2000, CustomEase);

double CustomEase (double t)
{
  return t == 0 || t == 1 ? t : (int)(5 * t) / 5.0;
}
```

`CustomEase` Метод усекает значение входящего значения 0, 0,2, 0,4, 0,6, 0,8 и 1. Таким образом [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляр преобразуется в дискретные перемещается, а не плавно.

### <a name="custom-easing-func"></a>Настраиваемый плавности Func

Также можно определить пользовательскую функцию реалистичной анимации как `Func<double, double>`, как показано в следующем примере кода:

```csharp
Func<double, double> CustomEase = t => 9 * t * t * t - 13.5 * t * t + 5.5 * t;
await image.TranslateTo(0, 200, 2000, CustomEase));
```

`CustomEase` `Func` Представляет функцию реалистичной анимации, которая начинается быстрых, замедляет обращает курса и перестановка курс снова для ускорения быстро в последней. Следовательно, тогда как общее перемещение [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляр вниз, он также временно обращает курс на полпути через анимацию.

### <a name="custom-easing-constructor"></a>Пользовательский конструктор реалистичной анимации

Также можно определить в качестве аргумента для функции плавности [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) конструктора, как показано в следующем примере кода:

```csharp
await image.TranslateTo (0, 200, 2000, new Easing (t => 1 - Math.Cos (10 * Math.PI * t) * Math.Exp (-5 * t)));
```

Функции плавности указывается как аргумент функции лямбда-выражения для [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) конструктора, а также использует `Math.Cos` метод для создания медленно эффект, притормаживается по `Math.Exp` метод. Таким образом [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляр преобразуется, чтобы он отображался может упасть его окончательного место хранения.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать предварительно определенные функции плавности и способ создания функции плавности. Включает Xamarin.Forms [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/) класс, который позволяет указать функцию передачи, которая определяет способ ускорить анимации или замедлять работу, как они работают.



## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Функции плавности (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/easing/)
- [Замедление](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
