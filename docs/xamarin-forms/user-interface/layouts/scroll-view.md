---
title: ScrollView
description: "Чтобы предоставить макеты, которые не может поместиться на одной экрана и иметь содержимое освободить место для клавиатуры, используйте ScrollView."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 648125ca8bd2c7c8a015b4c29195dc75c0bbf0a0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="scrollview"></a>ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) содержит макеты и позволяет им прокрутки вне экрана. `ScrollView` также используется для разрешения представления для автоматического перемещения видимой области экрана при отображении клавиатуры.

[![](scroll-view-images/layouts-sml.png "Макеты Xamarin.Forms")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms макетов")

В этой статье рассматриваются:

- **[Назначение](#Purpose)**  &ndash; цели `ScrollView` и при использовании.
- **[Использование](#Usage)**  &ndash; использовании `ScrollView` на практике.
- **[Свойства](#Properties)**  &ndash; открытых свойств, которые можно читать и изменять.
- **[Методы](#Methods)**  &ndash; открытые методы, которые могут быть вызваны для прокручивания представления текста.
- **[События](#Events)**  &ndash; события, которые можно использовать для ожидания изменения состояния представления.

## <a name="purpose"></a>Цель

`ScrollView` может использоваться, чтобы обеспечить отображение представлений больше подходят для небольших телефоны. Например макет, который работает на iPhone 6s может обрезаться на iPhone 4s. С помощью `ScrollView` позволит усеченные части макета для отображения на экране меньшего размера.

## <a name="usage"></a>Использование

> [!NOTE]
> `ScrollView`s не должен быть вложенным. Кроме того `ScrollView`s не должен быть вложенным в других элементах управления, предоставляющих прокрутки, таких как `ListView` и `WebView`.

`ScrollView` предоставляет `Content` свойство, которое может быть задано в одном представлении или макета. Рассмотрим следующий пример макета с очень большой boxView, за которым следует `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

В C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,   HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Прежде чем пользователь выполняет прокрутку вниз, только `BoxView` отображается:

![](scroll-view-images/scroll-start.png "BoxView в ScrollView")

Обратите внимание, что когда пользователь начинает вводить текст в `Entry`, закрепить на экране отобразится представление:

![](scroll-view-images/scroll-end.png "Запись в ScrollView")

## <a name="properties"></a>Свойства

ScrollView имеет следующие свойства:

- **Содержимого** &ndash; Возвращает или задает представление для отображения в `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; только для чтения, возвращает размер содержимого, что компонент ширины и высоты. Это может быть привязано
- **[Ориентация](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; это [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), который представляет собой перечисление, которое может быть присвоено `Horizontal`, `Vertical`, или `Both`.
- **ScrollX** &ndash; только для чтения, возвращает текущую позицию прокрутки по оси X.
- **ScrollY** &ndash; только для чтения, возвращает текущую позицию прокрутки по оси Y.

## <a name="methods"></a>Методы

`ScrollView` предоставляет `ScrollToAsync` метод, который может использоваться для прокручивания представления текста с помощью координат или укажите способ представления, должны быть доступны.

При использовании координаты, указать `x` и `y` координаты, а также логическое значение, указывающее, должен быть анимированы прокрутки:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

При прокрутке к определенному элементу `ScrollToPosition` перечисления указывает, где в представлении отображается элемент:

- **Центр** &ndash; прокручивает элемент в центр видимой части представления.
- **Конец** &ndash; прокручивает элемент в конец видимой части представления.
- **MakeVisible** &ndash; прокручивает элемент, чтобы он был виден в представлении.
- **Запуск** &ndash; прокручивает элемент в начало видимой части представления.

`IsAnimated` Свойство указывает, каким образом будет прокручивать представление. При задано значение true, гладкой анимации будет использоваться, вместо того чтобы мгновенно Перемещение содержимого в представление.

## <a name="events"></a>События

`ScrollView` предоставляет только одно событие `Scrolled`. `Scrolled` возникает, когда представление завершил прокрутки. Обработчик событий для `Scrolled` принимает `ScrolledEventArgs`, который имеет `ScrollX` и `ScrollY` свойства. Следующий код демонстрирует, как обновить метку с текущей позиции прокрутки `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Обратите внимание, что позиции прокрутки может быть отрицательным, из-за влияния показатель, если прокрутка в конце списка.


## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
