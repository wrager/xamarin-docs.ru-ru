---
title: "Сводка Глава 26. Пользовательские макеты"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: dbddaaf2f4a5ad9d7161013f2ae11466b953e20c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-26-custom-layouts"></a>Сводка Глава 26. Пользовательские макеты

Xamarin.Forms включает несколько классов, производных от [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/):

* `StackLayout`,
* `Grid`,
* `AbsoluteLayout` и
* `RelativeLayout`.

В этой главе описано, как для создания собственных классов, производных от `Layout<View>`.

## <a name="an-overview-of-layout"></a>Общие сведения о макете

Нет нет централизованной системе обработки Xamarin.Forms макета. Каждый элемент отвечает за определение собственный размер должен быть и Подготовка к просмотру сам в определенной области.

### <a name="parents-and-children"></a>Родительские и дочерние элементы

Для позиционирования этих дочерних элементов внутри себя отвечает каждому элементу, который имеет дочерние элементы. Он имеет родителя, который определяет, что размер его дочерние элементы должны быть основаны на размер был доступен и размер дочернего хочет.

### <a name="sizing-and-positioning"></a>Изменения размеров и положения

Макет начинается в верхней части визуального дерева со страницей, а затем выполняется через все ветви. Наиболее важный открытый метод в макете — [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) определяется `VisualElement`. Каждый элемент, который является родительским для других элементов вызовов `Layout` для каждого из его дочерних элементов для предоставления дочерние размер и положение относительно самого в виде [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) значение. Эти `Layout` вызовы распространение визуального дерева.

Вызов `Layout` является обязательным для элемента, на экране, в результате чего следующие свойства только для чтения, задаваемый. Они соответствуют `Rectangle` передается в метод:

- [`Bounds`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) типа `Rectangle`
- [`X`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.X/) типа `double`
- [`Y`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Y/) типа `double`
- [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) типа `double`
- [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) типа `double`

До появления `Layout` вызвать, `Height` и `Width` имеют макетов значения & #x 2013; 1.

Вызов `Layout` также запускает следующих защищенных методов:

- [`SizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.SizeAllocated/p/System.Double/System.Double/), который вызывает
- [`OnSizeAllocated`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeAllocated/p/System.Double/System.Double/), который может быть переопределен.

Наконец возникает следующее событие:

- [`SizeChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/)

`OnSizeAllocated` Метод переопределяется `Page` и `Layout`, которые являются только двумя классами в Xamarin.Forms, может иметь дочерних элементов. Переопределенный метод вызывает функцию

- [`UpdateChildrenLayout`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.UpdateChildrenLayout()/) для `Page` производные и [ `UpdateChildrenLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.UpdateChildrenLayout()/) для `Layout` производные продукты, которые вызывает
- [`LayoutChildren`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) для `Page` производные и [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) для `Layout` производных продуктов.

`LayoutChildren` затем вызывает метод `Layout` для каждого из его дочерних элементов. Если хотя бы один дочерний элемент имеет новый `Bounds` параметра, то возникает следующее событие:

- [`LayoutChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.LayoutChanged/) для `Page` производные и [ `LayoutChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Layout.LayoutChanged/) для `Layout` производных продуктов

### <a name="constraints-and-size-requests"></a>Ограничения и размер запросов

Для `LayoutChildren` интеллектуально вызывать `Layout` на все его дочерние элементы, необходимо знать *предпочтительный* или *требуемой* размер для дочерних элементов. Поэтому вызовы `Layout` для каждого из дочерних элементов обычно стоят вызовы

- [`GetSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/)

После публикации книги, `GetSizeRequest` метод был устарели и заменены

- [`Measure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/)

`Measure` Метод [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) свойство и включает аргумент типа [ `MeasureFlag` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MeasureFlags/), который имеет два члена:

- [`IncludeMargins`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.IncludeMargins/)
- [`None`](https://developer.xamarin.com/api/field/Xamarin.Forms.MeasureFlags.None/) не требуется включать поля

Для многих элементов `GetSizeRequest` или `Measure` получает собственный размер элемента с его модуль подготовки отчетов. Оба метода принимают параметры ширины и высоты *ограничения*. Например `Label` будет использовать ограничение ширины для определения создание программы-оболочки несколько строк текста.

Оба `GetSizeRequest`и `Measure` возвращаемое значение типа [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/), который имеет два свойства:

- [`Request`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Request/) типа `Size`
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.SizeRequest.Minimum/) типа `Size`

Очень часто эти два значения совпадают и `Minimum` значение обычно можно игнорировать.

`VisualElement` также определяет защищенный метод аналогично `GetSizeRequest` , вызываемый из `GetSizeRequest`:

- [`OnSizeRequest`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnSizeRequest/p/System.Double/System.Double/) Возвращает `SizeRequest` значение

Этот метод является теперь устарели и заменены:

- [`OnMeasure`](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/)

Каждый класс, производный от `Layout` или `Layout<T>` необходимо переопределить `OnSizeRequest` или `OnMeasure`. Это происходит, где макета класс определяет собственный размер, который обычно зависит от размера его дочерние элементы, которые он получает путем вызова `GetSizeRequest` или `Measure` в дочерних элементах. До и после вызова метода `OnSizeRequest` или `OnMeasure`, `GetSizeRequest` или `Measure` позволяет регулировать с учетом следующих свойств:

- [`WidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/)Тип `double`, влияет на `Request` свойство `SizeRequest`
- [`HeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/)Тип `double`, влияет на `Request` свойство `SizeRequest`
- [`MinimumWidthRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumWidthRequest/)Тип `double`, влияет на `Minimum` свойство `SizeRequest`
- [`MinimumHeightRequest`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.MinimumHeightRequest/)Тип `double`, влияет на `Minimum` свойство `SizeRequest`

### <a name="infinite-constraints"></a>Бесконечный ограничения

Ограничение аргументы, передаваемые `GetSizeRequest` (или `Measure`) и `OnSizeRequest` (или `OnMeasure`) может быть бесконечной (т. е. значения `Double.PositiveInfinity`). Тем не менее `SizeRequest` возвращенный эти методы не могут содержать бесконечное измерений.

Бесконечный ограничения указывают, что запрошенный размер должны отражать естественную размер элемента. Вертикальная `StackLayout` вызовы `GetSizeRequest` (или `Measure`) его дочерних элементах с ограничением на бесконечное высоту. Макет горизонтальный стека вызывает `GetSizeRequest` (или `Measure`) его дочерних элементах с ограничением на неограниченной ширины. `AbsoluteLayout` Вызовы `GetSizeRequest` (или `Measure`) его дочерних элементах с бесконечной ограничения ширины и высоты.

### <a name="peeking-inside-the-process"></a>Просмотр внутри процесса

[ **ExploreChildSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/ExploreChildSizes) отображает ограничение и размер запроса сведений для простую структуру.

## <a name="deriving-from-layoutview"></a>Наследование от макета<View>

Пользовательский макет класс является производным от `Layout<View>`. Он имеет два обязанности:

- Переопределить `OnMeasure` для вызова `Measure` макет дочерних элементах. Возвращает запрошенный размер для макета сам
- Переопределить `LayoutChildren` для вызова `Layout` дочерних элементах макета

`for` Или `foreach` цикла в этих переопределениях следует пропустить все дочерние которого `IsVisible` свойству `false`.

Вызов `OnMeasure` не гарантируется. `OnMeasure` не будет вызываться, если родительский макета регулирующих размер структуры (например, макет, который заполняет страницу). По этой причине `LayoutChildren` не следует полагаться на размеры дочерних, полученные во время `OnMeasure` вызова. Очень часто `LayoutChildren` должен сам вызов `Measure` на макет дочерних элементов, или же можно применить какой-либо размер кэширование логику (описаны ниже).

### <a name="an-easy-example"></a>Простой пример

[ **VerticalStackDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/VerticalStackDemo) пример содержит упрощенную [ `VerticalStack` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter26/VerticalStackDemo/VerticalStackDemo/VerticalStackDemo/VerticalStack.cs) класс и демонстрация его использования.

### <a name="vertical-and-horizontal-positioning-simplified"></a>Вертикальное и горизонтальное расположение упрощенное письмо

Одно из заданий, `VerticalStack` необходимо выполнить во время `LayoutChildren` переопределения. Этот метод использует значение дочернего элемента `HorizontalOptions` свойство для определения способа изменить расположение дочерних в его слота в `VerticalStack`. Вместо этого можно вызвать статический метод [ `Layout.LayoutChildIntoBoundingRect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/). Этот метод вызывает метод `Measure` на дочерней и использует его `HorizontalOptions` и `VerticalOptions` свойства, чтобы изменить расположение дочерних заданного прямоугольника.

### <a name="invalidation"></a>Недействительность

Часто изменение свойства элемента влияет на способ отображения этого элемента в макет. Представление должно быть недействительным, чтобы запустить новый макет.

`VisualElement` Определяет метод, защищенный [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/), обычно называемый обработчиком изменения свойств любого привязываемые свойства которого изменение влияет на размер элемента. `InvalidateMeasure` Вызывается метод [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) событий.

`Layout` Класс определяет аналогичные защищенный метод с именем [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/), который `Layout` производный класс должен вызывать при любом изменении, влияет на способ помещает и размеров его дочерних элементов.

### <a name="some-rules-for-coding-layouts"></a>Некоторые правила кодирования макетов

1. Свойства, определенные `Layout<T>` производные должно поддерживаться свойства для привязки и должны вызывать обработчики изменения свойства `InvalidateLayout`.

2. Объект `Layout<T>` производный класс, определяющий вложенное привязываемые свойства должны переопределять [ `OnAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnAdded/p/T/) для добавления обработчика изменения свойства с его дочерними элементами и [ `OnRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout%3CT%3E.OnRemoved/p/T/) , удаление обработчик. Обработчик должен проверять для изменения этих присоединенного свойства для привязки и отвечать путем вызова `InvalidateLayout`.

3. Объект `Layout<T>` необходимо переопределить производный класс, реализующий кэш размеры дочерних `InvalidateLayout` и [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) и очистки кэша при вызове этих методов.

### <a name="a-layout-with-properties"></a>Макет со свойствами

[ `WrapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/WrapLayout.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) предполагается, что все его дочерние элементы размеры и была перенесена дочерние элементы из одной строки (или столбца) Далее. Он определяет `Orientation` свойства, такого как `StackLayout`, и `ColumnSpacing` и `RowSpacing` свойства, такие как `Grid`, и он кэширует размеры дочерних.

[ **PhotoWrap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoWrap) пример помещает `WrapLayout` в `ScrollView` для отображения фотографий.

### <a name="no-unconstrained-dimensions-allowed"></a>Произвольные измерения не допускается.

[ `UniformGridLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/UniformGridLayout.cs) В [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки предназначена для отображения его дочерних элементов внутри себя. Следовательно он не может работать с произвольными измерений и вызывает исключение, если обнаружен один.

[ **PhotoGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/PhotoGrid) образец демонстрирует `UniformGridLayout`:

[![Снимок экрана тройной фотографии в табличном](images/ch26fg08-small.png "универсальный макет сетки")](images/ch26fg08-large.png "универсальный макет сетки")

### <a name="overlapping-children"></a>Перекрывающиеся дочерние элементы

Объект `Layout<T>` производный класс может перекрывать его дочерних элементов. Тем не менее, дочерние элементы отображаются в порядке их в `Children` коллекции, а не порядок, в котором их `Layout` вызываются методы.

`Layout` Класс определяет два метода, которые позволяют переместить дочерние коллекции:

- [`LowerChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LowerChild/p/Xamarin.Forms.View/) Чтобы переместить дочерний элемент в начало коллекции
- [`RaiseChild`](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.RaiseChild/p/Xamarin.Forms.View/) Чтобы переместить дочерний элемент в конец коллекции

Для перекрывающихся дочерних элементов дочерние элементы в конец коллекции визуально выводились поверх дочерние элементы в начало коллекции.

[ `OverlapLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/OverlapLayout.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека определяет вложенное свойство для указания порядка отрисовки, что один из его дочерние элементы, отображаемый поверх остальных. [ **StudentCardFile** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/StudentCardFile) образец демонстрирует это:

[![Тройной экрана сетки файла карты студента](images/ch26fg10-small.png "перекрывающиеся дочерние элементы макета")](images/ch26fg10-large.png "перекрывающиеся дочерние элементы макета")

### <a name="more-attached-bindable-properties"></a>Несколько присоединенного свойства для привязки

[ `CartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CartesianLayout.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека определяет вложенное привязываемые свойства для указания двух `Point` значения и значение толщины и управляет `BoxView` элементы для сходства строк.

[ **UnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/UnitCube) образец использует эти данные для отрисовки трехмерного куба.

### <a name="layout-and-layoutto"></a>Макет и LayoutTo

Объект `Layout<T>` производный класс может вызывать `LayoutTo` вместо `Layout` анимировать макета. [ `AnimatedCartesianLayout` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnimatedCartesianLayout.cs) Класса делает это и [ **AnimatedUnitCube** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26/AnimatedUnitCube) образец демонстрирует его.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 26 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch26-Apr2016.pdf)
- [Глава 26 образцы](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter26)
- [Создание пользовательских макетов](~/xamarin-forms/user-interface/layouts/custom.md)
