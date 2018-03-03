---
title: "Поля и заполнение"
description: "Поля и свойства заполнения управляющие поведением макета при подготовке к просмотру элемента в пользовательском интерфейсе. В этой статье показано различие между двумя свойствами и способ их настройки."
ms.topic: article
ms.prod: xamarin
ms.assetid: BEB096BB-51DF-410F-B0F1-D235287B0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 7bab512ef11f8e0f553a00f0240d82f860fe2676
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="margin-and-padding"></a>Поля и заполнение

_Поля и свойства заполнения управляющие поведением макета при подготовке к просмотру элемента в пользовательском интерфейсе. В этой статье показано различие между двумя свойствами и способ их настройки._

## <a name="overview"></a>Обзор

Поля и заполнение — макет связанные понятия:

- [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Свойство представляет расстояние между элементом и его соседние элементы и используется для управления позицию отрисовки данного элемента и его соседями позицию отрисовки. `Margin` значения могут быть заданы на [макета](~/xamarin-forms/user-interface/controls/layouts.md) и [представление](~/xamarin-forms/user-interface/controls/views.md) классы.
- [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) Свойство представляет расстояние между элементом и его дочерние элементы и используется для разделения управления из его собственного содержимого. `Padding` значения могут быть заданы на [макета](~/xamarin-forms/user-interface/controls/layouts.md) классы.

Следующая диаграмма иллюстрирует две основные понятия.

[![](margin-and-padding-images/margins-and-padding-sml.png "Поля и заполнение понятия")](margin-and-padding-images/margins-and-padding.png "поля и заполнение основные понятия")

Обратите внимание, что [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) значения являются аддитивными. Таким образом Если два соседних элемента указать на поле шириной в 20 точек, расстояние между элементами будет 40 пикселей. Кроме того, поля и заполнение являются друг к другу при обоих применяются, в том, что расстояние между элементом и любое содержимое будет полей, а также заполнения.

## <a name="specifying-a-thickness"></a>Задание толщины

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) И [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) свойства имеют тип [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/). Возможны три варианта при создании `Thickness` структуры:

- Создание [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) структуры, определяемой универсальный одно значение. Одно значение применяется к слева, сверху, справа и нижней сторонах элемента.
- Создание [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) структуры, определяемой значения по горизонтали и вертикали. Значение горизонтального симметрично применяется к левой и правой сторон элемента, значением по вертикали симметрично, применяемые к верхней и нижней границами элемента.
- Создание [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) структуры, определяемой четыре различных значений, применяемых к слева, сверху, справа и нижней сторонах элемента.

В следующем примере кода XAML показаны все три возможности:

```xaml
<StackLayout Padding="0,20,0,0">
  <Label Text="Xamarin.Forms" Margin="20" />
  <Label Text="Xamarin.iOS" Margin="10, 15" />
  <Label Text="Xamarin.Android" Margin="0, 20, 15, 5" />
</StackLayout>
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var stackLayout = new StackLayout {
  Padding = new Thickness(0,20,0,0),
  Children = {
    new Label { Text = "Xamarin.Forms", Margin = new Thickness (20) },
    new Label { Text = "Xamarin.iOS", Margin = new Thickness (10, 25) },
    new Label { Text = "Xamarin.Android", Margin = new Thickness (0, 20, 15, 5) }
  }
};
```

> [!NOTE]
> **Примечание**: `Thickness` значения могут быть отрицательными, который обычно отсекает или обрезку элементов содержимого.

## <a name="summary"></a>Сводка

В этой статье показано различие между [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) и [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) свойства и способ их настройки. Свойства управляют поведением макета при подготовке к просмотру элемента в пользовательском интерфейсе.


## <a name="related-links"></a>Связанные ссылки

- [Поля](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/)
- [Заполнение](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/)
- [Толщина](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)
