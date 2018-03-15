---
title: "Сводка Доп. Прокрутка стека"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 7A39FD4F-15AD-4F94-960E-9FEEB63FFD44
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 5559f9e6a4baf9d3f82701b5e3f341900ba83bae
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-4-scrolling-the-stack"></a>Сводка Доп. Прокрутка стека

В этой главе в основном посвящена введение в концепцию *макета*, это общий термин, классы и методы, которые использует Xamarin.Forms для упорядочения визуального отображения нескольких представлений на странице.

Макет включает в себя несколько классов, которые являются производными от [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) и [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/). В этой главе уделяется [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/).

Также появилась в этой главе будут [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/), [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/), и [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) классы.

## <a name="stacks-of-views"></a>Стеки представлений

[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) является производным от `Layout<View>` и наследует [ `Children` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) свойство типа `IList<View>`. Добавьте несколько элементов представления в эту коллекцию и `StackLayout` отображает их в горизонтальном или вертикальном стека.

Задать [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) свойство `StackLayout` на член [ `StackOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackOrientation/) перечисления, либо [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Vertical/) или [ `Horizontal`](https://developer.xamarin.com/api/field/Xamarin.Forms.StackOrientation.Horizontal/). Значение по умолчанию — `Vertical`.

Задать [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) свойство `StackLayout` для `double` значение, указывающее интервал между дочерние элементы. Значение по умолчанию — 6.

В коде, можно добавить элементы к `Children` коллекцию `StackLayout` в `for` или `foreach` цикл, как показано в [ **ColorLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorLoop) образца, или можно инициализировать `Children` коллекции со списком отдельных представлений, как демонстрируется в [ **ColorList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorList). Дочерние элементы должны быть производными от `View` , но может включать другие `StackLayout` объекты.

## <a name="scrolling-content"></a>Прокрутка содержимого

Если `StackLayout` содержит слишком много дочерних элементов для отображения на странице, можно поместить `StackLayout` в [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) разрешающее прокрутки.

Задать [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) свойство `ScrollView` в представление для прокрутки. Это часто `StackLayout`, но это может быть любое представление.

Задать [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) свойство `ScrollView` на член [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/) свойства [ `Vertical` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Vertical/), [ `Horizontal` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Horizontal/), или [ `Both` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ScrollOrientation.Both/). Значение по умолчанию — `Vertical`. Если содержимое `ScrollView` — `StackLayout`, два ориентации экрана должны быть согласованы.

[ **ReflectedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ReflectedColors) образце показано использование `ScrollView` и `StackLayout` для отображения доступных цветов. Образец также демонстрирует способы использования отражения .NET, чтобы получить все открытые статические свойства и поля `Color` структуры без необходимости явно перечислять.

## <a name="the-expands-option"></a>Параметр разворачивается

Когда `StackLayout` стеки своих дочерних элементов, каждый дочерний элемент занимает определенного ячейку в общей высоты `StackLayout` , зависит от размера дочернего элемента, а также параметры его `HorizontalOptions` и `VerticalOptions` свойства. Эти свойства присваиваются значения типа [ `LayoutOptions` ](http://developer.xamstage.com/api/type/Xamarin.Forms.LayoutOptions/).

`LayoutOptions` Структура определяет два свойства:

- [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) перечисления типа [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/) с четырьмя элементами [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), и [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)
- [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) типа `bool`

Для удобства `LayoutOptions` структура также определяет восемь статического поля только для чтения типа `LayoutOptions` , включают в себя все сочетания свойств два экземпляра:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

Включает в себя следующее обсуждение `StackLayout` с вертикальной ориентацией по умолчанию. Горизонтальный `StackLayout` является аналогом.

Для вертикального ползунка `StackLayout`, `HorizontalOptions` параметр определяет, как дочерний элемент по горизонтали расположено в ширину `StackLayout`. `Alignment` Параметра `Start`, `Center`, или `End` приводит дочерних быть горизонтально без ограничений. Определяет собственную ширину дочерним элементом, а располагается слева, по центру или по правому краю `StackLayout`. `Fill` Параметр, дочерние по горизонтали ограниченных и заполняет ширину `StackLayout`.

Для вертикального ползунка `StackLayout`, каждый дочерний элемент по вертикали произвольные и возвращает вертикальная область памяти в зависимости от высоты дочернего элемента, в этом случае `VerticalOptions` параметр не имеет значения.

Если вертикальную `StackLayout` сам произвольные&mdash;, если его `VerticalOptions` параметр `Start`, `Center`, или `End`, затем высоту `StackLayout` — Общая высота из его дочерних элементов.

Однако если вертикальную `StackLayout` ограничен по вертикали&mdash;при его `VerticalOptions` параметр `Fill` &mdash;затем высоту `StackLayout` будет высоты своего контейнера, который может быть больше, чем сумма Высота его дочерних элементов. Если это так, и имеет хотя бы один дочерний `VerticalOptions` с `Expands` флаг `true`, затем дополнительное пространство в `StackLayout` равномерно распределены среди этих дочерних элементов с `Expands` флаг `true`. Общая высота дочерних элементов будет равен высоту `StackLayout`и `Alignment` частью `VerticalOptions` параметр определяет, как дочерний по вертикали расположено в ее гнездо.

Это показано в [ **VerticalOptionsDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/VerticalOptionsDemo) образца.

## <a name="frame-and-boxview"></a>Рамки и BoxView

Эти два представления прямоугольный часто используются для целей представления.

[ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Отображается прямоугольная рамку вокруг другое представление, которое может быть макета, например `StackLayout`. `Frame` наследует [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) свойство из [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , задайте режим для отображения в `Frame`. `Frame` Является прозрачным по умолчанию. Задайте следующие три свойства для настройки внешнего вида рамки:

- [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/) Свойство, чтобы сделать его видимым. Обычно для задания `OutlineColor` для `Color.Accent` Если вы не знаете базовой цветовой схемы.
- [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/) Свойству можно присвоить значение `true` для отображения черную тень на устройствах iOS.
- Задать [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) свойства `Thickness` содержимое элемента значение, оставьте пробел между и кадром. Значение по умолчанию — 20 единиц по всем сторонам.

`Frame` Имеет значение по умолчанию `HorizontalOptions` и `VerticalOptions` значения `LayoutOptions.Fill`, означающее, что `Frame` заполнит его контейнера. С другими параметрами размер `Frame` соответствии с размером его содержимого.

`Frame` Демонстрируется [ **FramedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FramedText) образца.

[ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Отображает цвет, определенный параметром прямоугольную область его [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) свойство.

Если `BoxView` ограничен (его `HorizontalOptions` и `VerticalOptions` свойства имеют свои параметры по умолчанию `LayoutOptions.Fill`), `BoxView` заполняет пространство, доступное для него. Если `BoxView` — произвольные (с `HorizontalOptions` и `LayoutOptions` параметры `Start`, `Center`, или `End`), он имеет квадрата 40 единиц измерения по умолчанию. Объект `BoxView` в одном измерении ограниченное и неограниченное в другой.

Часто, можно задать [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) и [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) свойства `BoxView` для придания ей определенного размера. Это продемонстрировано [ **SizedBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/SizedBoxView) образца.

Можно использовать несколько экземпляров `StackLayout` для объединения `BoxView` и несколько `Label` экземпляров в `Frame` отображения определенным цветом, а затем поместить каждый из этих представлений в `StackLayout` в `ScrollView` для создания привлекательных Список цветов, отображаемых в [ **ColorBlocks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/ColorBlocks) образца:

[![Тройной снимок экрана: блоки цвет](images/ch04fg11-small.png "список цветов")](images/ch04fg11-large.png#lightbox "список цветов")

## <a name="a-scrollview-in-a-stacklayout"></a>ScrollView в StackLayout?

Размещение `StackLayout` в `ScrollView` , но размещение `ScrollView` в `StackLayout` удобно также иногда. В теории, это не должно быть возможно из-за дочерние элементы по вертикали `StackLayout` — по вертикали без ограничений. Но `ScrollView` должен быть ограничен по вертикали. Он должен быть присвоен высоту, чтобы затем можно определить размер его потомка для прокрутки.

Для этого достаточно для предоставления `ScrollView` потомком `StackLayout` `VerticalOptions` параметра `FillAndExpand`. Это показано в [ **BlackCat** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCat) образца.

**BlackCat** образце также показано, как определить и получить доступ к ресурсам программы, внедренных в переносимой библиотеки классов (PCL). Это также может осуществляться с проектами общего ресурса (SAPs), но этот процесс немного сложнее, как [ **BlackCatSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/BlackCatSap) демонстрирует образец.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 4 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch04-Apr2016.pdf)
- [Образцы раздел 4](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04)
- [Образцы в главе 4 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter04/FS)
- [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md)
- [ScrollView](~/xamarin-forms/user-interface/layouts/scroll-view.md)
