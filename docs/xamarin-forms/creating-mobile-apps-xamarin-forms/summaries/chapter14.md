---
title: "Сводка Глава 14. Абсолютный макета"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 88882A48-3226-42D1-96ED-241250B64A84
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: a3980c63c31f4fdf0297fdc9b05da3590f0cac54
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-14-absolute-layout"></a>Сводка Глава 14. Абсолютный макета

Как `StackLayout`, [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) является производным от `Layout<View>` и наследует `Children` свойство. `AbsoluteLayout` реализует систему макета, которая требует программисту указывать положения его дочерних элементов и, возможно, их размер. Позиция задается верхнего левого угла дочернего элемента относительно верхнего левого угла `AbsoluteLayout` в аппаратно независимых единицах. `AbsoluteLayout` также реализует пропорционально позиционирования и возможность изменения размера.

`AbsoluteLayout` следует рассматривать как система специальная макет для использования только в том случае, если программист может применить размер на дочерние элементы (например, `BoxView` элементы) или когда размер элемента не влияет на расположение других дочерних объектов. `HorizontalOptions` И `VerticalOptions` свойства не оказывают влияния на дочерние элементы `AbsoluteLayout`.

В этой главе появился важной характеристикой *присоединенного свойства для привязки* , которые позволяют свойства, определенные в одном классе (в этом случае `AbsoluteLayout`) подключен к другой класс (потомком `AbsoluteLayout`).

## <a name="absolutelayout-in-code"></a>AbsoluteLayout в коде

Можно добавить дочерний элемент `Children` коллекцию `AbsoluteLayout` с использованием стандарта [ `Add` ](https://developer.xamarin.com/api/member/System.Collections.Generic.ICollection%3CT%3E.Add/p/T/) метода, но `AbsoluteLayout` также предоставляет расширенную [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Rectangle/Xamarin.Forms.AbsoluteLayoutFlags/) метод, который позволяет указать [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/). Другой [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout+IAbsoluteList%3CT%3E.Add/p/Xamarin.Forms.View/Xamarin.Forms.Point/) метод требует только [ `Point` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Point/), в этом случае дочерний произвольные и изменяет свой размер.

Можно создать `Rectangle` значение с [конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/System.Double/System.Double/System.Double/System.Double/) , требуется четыре значения &mdash; первых двух, указывающее положение верхнего левого угла дочернего элемента относительно его родительского элемента, а вторые два, указывающее, размер дочернего элемента. Или можно использовать [конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Rectangle.Rectangle/p/Xamarin.Forms.Point/Xamarin.Forms.Size/) , требующего `Point` и [ `Size` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/) значение.

Эти `Add` демонстрируются методы в [ **AbsoluteDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteDemo), какие позиции `BoxView` элементов с помощью `Rectangle` значения и `Label` элемента с помощью только что `Point` значение.

[ **ChessboardFixed** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardFixed) примере используются 32 `BoxView` элементах, чтобы создать шаблон доске. Программа выдает `BoxView` элементы жестко запрограммированный размер квадрата единицы 35. `AbsoluteLayout` Имеет его `HorizontalOptions` и `VerticalOptions` значение `LayoutOptions.Center`, чего `AbsoluteLayout` иметь общий размер квадрата единицы 280.

## <a name="attached-bindable-properties"></a>Вложенные свойства связывания

Также можно задать положение и, возможно, размер дочернего элемента `AbsoluteLayout` после его добавления в `Children` коллекции с помощью статического метода [ `AbsoluteLayout.SetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutBounds/p/Xamarin.Forms.BindableObject/Xamarin.Forms.Rectangle/). Первым аргументом является дочерним; Во-вторых, `Rectangle` объекта. Можно указать, что дочерний изменяет свой размер по горизонтали или по вертикали, задав значения высоты и ширины [ `AbsoluteLayout.AutoSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.AbsoluteLayout.AutoSize/) константой.

[ **ChessboardDynamic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardDynamic) пример помещает `AbsoluteLayout` в `ContentView` с `SizeChanged` обработчик для вызова `AbsoluteLayout.SetLayoutBounds` на все дочерние элементы, чтобы сделать их как можно большего размера.  

Вложенное свойство привязки, `AbsoluteLayout` определяет является статического поля только для чтения типа `BindableProperty` с именем [ `AbsoluteLayout.LayoutBoundsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/). Статический `AbsoluteLayout.SetLayoutBounds` метод реализуется путем вызова `SetValue` на дочерний элемент с `AbsoluteLayout.LayoutBoundsProperty`. Дочерние содержит словарь, в которой хранятся привязываемых вложенное свойство и его значение. Во время структурирования `AbsoluteLayout` это значение можно получить, вызвав [ `AbsoluteLayout.GetLayoutBounds` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutBounds/p/Xamarin.Forms.BindableObject/), который реализуется с помощью `GetValue` вызова.

## <a name="proportional-sizing-and-positioning"></a>Пропорциональное изменения размера и размещения

`AbsoluteLayout` реализует пропорциональное изменения размера и размещения компонентов. Этот класс определяет второй вложенное свойство привязки, [ `LayoutFlagsProperty` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/), со статическими методами связанные [ `AbsoluteLayout.SetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.SetLayoutFlags/p/Xamarin.Forms.BindableObject/Xamarin.Forms.AbsoluteLayoutFlags/) и [ `AbsoluteLayout.GetLayoutFlags` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AbsoluteLayout.GetLayoutFlags/p/Xamarin.Forms.BindableObject/).

Аргумент `AbsoluteLayout.SetLayoutFlags` и возвращаемое значение `AbsoluteLayout.GetLayoutFlags` является значением типа [ `AbsoluteLayoutFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/), перечисление со следующими членами:

- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.None/) (равно 0)
- [`XProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.XProportional/) (1)
- [`YProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.YProportional/) (2)
- [`PositionProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.PositionProportional/) (3)
- [`WidthProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.WidthProportional/) (4)
- [`HeightProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.HeightProportional/) (8)
- [`SizeProportional`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.SizeProportional/) (12)
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayoutFlags.All/) (\xFFFFFFFF)

Можно объединять с помощью C# оператор побитового или.

Эти флаги установлены, определенные свойства `Rectangle` структура границы макета, используемая в положение и размер дочернего интерпретируются пропорционально.

Когда `WidthProportional` флаг установлен, `Width` значение 1 означает, что дочерние имеет ту же ширину, что `AbsoluteLayout`. Подобный подход используется для высоты.

Пропорциональное позиционирования учитывает размер. Когда `XProportional` флаг установлен, `X` свойство `Rectangle` пропорционально границы макета. Значение 0 означает, что дочерние левого края расположено на левой границе `AbsoluteLayout`, но положение 1 означает, что правый край ребенка расположен на правом краю `AbsoluteLayout`, не за правый край `AbsoluteLayout` виде может expec t. `X` Свойство 0,5 выравнивание по горизонтали в дочерних `AbsoluteLayout`.

[ **ChessboardProportional** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardProportional) образце показано использование пропорциональное изменения размера и положения.

## <a name="working-with-proportional-coordinates"></a>Работа с пропорциональным координаты

Иногда проще всего представить пропорционально позиционирования иначе, чем реализации в `AbsoluteLayout`. Можно работать с пропорциональным координаты где `X` помещает свойство 1 левой границей дочернего элемента (а не правого края) от правого края `AbsoluteLayout`.

Эта альтернативного размещения схема может быть вызван «координаты долей дочернего». Можно преобразовать координаты долей дочернего для границы макета, необходимые для `AbsoluteLayout` со следующими формулами:

layoutBounds.X = (fractionalChildCoordinate.X или (1 - layoutBounds.Width))

layoutBounds.Y = (fractionalChildCoordinate.Y или (1 - layoutBounds.Height))

[ **ProportionalCoordinateCalc** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/PropCoordCalc) примере демонстрируется это.

## <a name="absolutelayout-and-xaml"></a>AbsoluteLayout и XAML

Можно использовать `AbsoluteLayout` в XAML и задайте вложенные привязываемые свойства с дочерними объектами `AbsoluteLayout` с использованием значений атрибута `AbsoluteLayout.LayoutBounds` и `AbsoluteLayout.LayoutFlags`. Это показано в [ **AbsoluteXamlDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/AbsoluteXamlDemo) и [ **ChessboardXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/ChessboardXaml) образцов. Последний программа содержит 32 `BoxView` элементы только использует неявный `Style` , включающего `AbsoluteLayout.LayoutFlags` свойство для сохранения разметки до минимум.

Атрибут в языке XAML, который состоит из имени класса, точки и имени свойства — *всегда* привязываемых вложенное свойство.

## <a name="overlays"></a>Перекрытия

Можно использовать `AbsoluteLayout` для создания *наложения*, который охватывает страницы с другими элементами управления возможно защита пользователь взаимодействовать с помощью обычных элементов управления на странице. 

[ **SimpleOverlay** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/SimpleOverlay) образец иллюстрирует этот прием и также [ `ProgressBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/), которая отображает область, к которой программа завершила задача.

## <a name="some-fun"></a>Некоторые интересные

[ **DotMatrixClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/DotMatrixClock) пример отображает текущее время с отображением имитацию 5 x 7 матричного. Каждая точка обозначает `BoxView` (отсутствуют 228 из них) изменять размеры и расположение на `AbsoluteLayout`.

[![Тройной экрана часов матричного](images/ch14fg08-small.png "часов матричного")](images/ch14fg08-large.png#lightbox "матричного часов")

[ **BouncingText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14/BouncingText) программы анимирует два `Label` объектов к экрану скачками по горизонтали и вертикали.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 14 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch14-Apr2016.pdf)
- [Образцы Глава 14](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter14)
- [AbsoluteLayout](~/xamarin-forms/user-interface/layouts/absolute-layout.md)
