---
title: LayoutOptions
description: "Каждому представлению Xamarin.Forms имеет свойства, HorizontalOptions и VerticalOptions, LayoutOptions типа. В этой статье объясняется влияние каждого LayoutOptions значения для выравнивания и расширения представления."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7CAB5631-5153-4DEF-8AD7-C6011CE44307
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2017
ms.openlocfilehash: a2aa143d5aeb801cd753dd99718ca9cf6dd72353
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="layoutoptions"></a>LayoutOptions

_Каждому представлению Xamarin.Forms имеет свойства, HorizontalOptions и VerticalOptions, LayoutOptions типа. В этой статье объясняется влияние каждого LayoutOptions значения для выравнивания и расширения представления._

## <a name="overview"></a>Обзор

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Структура инкапсулирует две настройки компоновки:

- **Выравнивание** — предпочитаемые выравнивания, который определяет его положения и размера в макете родительского представления.
- **Расширения** — используется только [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)и указывает, если представление следует использовать дополнительное пространство, если он доступен.

Эти параметры макета может применяться к [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)относительно родительского, задав [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) или [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойство `View` с одним из общих полей из [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) структуры. Открытые поля выглядят следующим образом:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`Start`, `Center`, `End`, И `Fill` поля используются для определения выравнивания представления макета родительского:

- Для выравнивания по горизонтали [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/) позиций [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) с левой стороны родительского макета, а также для вертикального выравнивания, он помещает `View` в верхней части Макет родительского.
- Для выравнивания по горизонтали и вертикали [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/) выравнивание по горизонтали или вертикали [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).
- Для выравнивания по горизонтали [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/) позиций [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) с правой стороны родительского макета, а также для вертикального выравнивания, он помещает `View` внизу родительский макета.
- Для выравнивания по горизонтали [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) гарантирует, что [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) заполняет ширину родительского макета и вертикальное выравнивание, это гарантирует, что `View` заполняет Высота родительского макета.

`StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, И `FillAndExpand` значения используются для определения выравнивания предпочтения, и ли представление будет занимать больше места, если он доступен в родительском элементе [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

> [!NOTE]
> Значение по умолчанию представления [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойств является [ `LayoutOptions.Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/).

<a name="alignment" />

## <a name="alignment"></a>Выравнивание

Выравнивание элементов управления представления расположение в его родительской структуры при макета родительского содержит неиспользованное пространство (то есть макета родительского больше, чем общий размер всех его дочерних узлов).

Объект [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) только соблюдает `Start`, `Center`, `End`, и `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) поля на дочерние представления, которые находятся в обратном направлении Чтобы `StackLayout` ориентации. Таким образом, дочерние представления в вертикально ориентирован `StackLayout` можно задать их [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) свойств с одним из `Start`, `Center`, `End`, или `Fill` полей. Аналогичным образом дочерние представления в горизонтально ориентирован `StackLayout` можно задать их [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойств с одним из `Start`, `Center`, `End`, или `Fill` полей.

Объект [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) не `Start`, `Center`, `End`, и `Fill` [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) поля на дочерние представления, которые находятся в направлении `StackLayout` ориентации. Таким образом, вертикальный `StackLayout` игнорирует `Start`, `Center`, `End`, или `Fill` поля, если они заданы в [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойств дочерних представлений. Аналогичным образом горизонтальный `StackLayout` игнорирует `Start`, `Center`, `End`, или `Fill` поля, если они заданы в [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) свойств дочерних представлений.

> [!NOTE]
> [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/) Обычно переопределения размер запросы, заданные с помощью [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) и [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) свойства.

В следующем примере кода XAML демонстрирует вертикально ориентирован [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) где каждый дочерний элемент [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) задает его [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) свойство одно из четырех выравнивание поля из [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) структуры:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <Label Text="Start" BackgroundColor="Gray" HorizontalOptions="Start" />
  <Label Text="Center" BackgroundColor="Gray" HorizontalOptions="Center" />
  <Label Text="End" BackgroundColor="Gray" HorizontalOptions="End" />
  <Label Text="Fill" BackgroundColor="Gray" HorizontalOptions="Fill" />
</StackLayout>
```

Ниже приведен эквивалентный код C#:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
    new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
    new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
    new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
  }
};
```

Код вызывает макета, показанные на следующих снимках экрана.

[![](layout-options-images/alignment.png "Параметры выравнивания макета")](layout-options-images/alignment-large.png#lightbox "параметры выравнивания макета")

<a name="expansion" />

## <a name="expansion"></a>Расширения

Расширения управляет ли представление будет занимать больше места, если он доступен в [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Если `StackLayout` содержит неиспользованное пространство (то есть `StackLayout` больше, чем общий размер всех его дочерних элементов), неиспользуемое пространство поровну разделяется все дочерние представления, запросить расширения, установив их [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)или [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) поля, которое использует `AndExpand` суффикс. Обратите внимание, что при все пробелы в `StackLayout` — используется, дополнительные возможности не действуют.

Объект [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) можно развернуть только дочерние представления в направлении его ориентацию. Таким образом, вертикальный `StackLayout` можно развернуть дочерние представления, которые установлены их [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойств с одним из `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, или `FillAndExpand` полей, если `StackLayout` содержит неиспользованное пространство. Аналогичным образом горизонтальный `StackLayout` можно развернуть дочерние представления, которые установлены их [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) свойств с одним из `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, или `FillAndExpand` полей, если `StackLayout` содержит неиспользованное пространство.

Объект [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) нельзя развернуть дочерние представления в направлении, противоположном его ориентацию. Поэтому в вертикально ориентирован `StackLayout`, параметр [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) свойство дочернего представления, чтобы [ `StartAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/) действует так же, как задать свойству значение [ `Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/).

> [!NOTE]
> Обратите внимание, что включение расширения не изменяйте размер представления, если он использует [ `LayoutOptions.FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/).

В следующем примере кода XAML демонстрирует вертикально ориентирован [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) где каждый дочерний элемент [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) задает его [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойство одно из полей расширения из [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) структуры:

```xaml
<StackLayout Margin="0,20,0,0">
  ...
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Start" BackgroundColor="Gray" VerticalOptions="StartAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Center" BackgroundColor="Gray" VerticalOptions="CenterAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="End" BackgroundColor="Gray" VerticalOptions="EndAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
  <Label Text="Fill" BackgroundColor="Gray" VerticalOptions="FillAndExpand" />
  <BoxView BackgroundColor="Red" HeightRequest="1" />
</StackLayout>
```

Ниже приведен эквивалентный код C#:

```csharp
Content = new StackLayout
{
  Margin = new Thickness(0, 20, 0, 0),
  Children = {
    ...
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
    new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
    new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
  }
};
```

Код вызывает макета, показанные на следующих снимках экрана.

[![](layout-options-images/expansion.png "Параметры расширения разметки")](layout-options-images/expansion-large.png#lightbox "параметры расширения разметки")

Каждый [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) занимает столько пространства в [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/). Однако только конечный `Label`, который устанавливает его [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) свойства [ `FillAndExpand` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/) другого размера. Кроме того каждый `Label` разделенных маленький красный значок [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/), позволяющий пространство `Label` занимает легко просматривать.

## <a name="summary"></a>Сводка

В этой статье описано влияние, каждая из которых [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) имеет значение структуры на выравнивание и расширения представления, относительно его родительского элемента. `Start`, `Center`, `End`, И `Fill` поля используются для определения представления выравнивания в макете родительского и `StartAndExpand`, `CenterAndExpand`, `EndAndExpand`, и `FillAndExpand` поля используются для определения Выравнивание предпочтения и определить ли представление будет занимать больше места, если он доступен в [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).



## <a name="related-links"></a>Связанные ссылки

- [LayoutOptions (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/layoutoptions/)
- [LayoutOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)
