---
title: "Сводка Глава 17. Захвата сетки"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 09f63dd418ea1fb523c028edb02c28c22bfdccd1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-17-mastering-the-grid"></a>Сводка Глава 17. Захвата сетки

[ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) — Это механизм набор мощных, упорядочивает свои дочерние элементы по строкам и столбцам ячеек. В отличие от HTML-код, аналогичный `table` элемент, `Grid` является исключительно для целей макета, а не в презентации.

## <a name="the-basic-grid"></a>Базовой сетки

`Grid` является производным от [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), который определяет [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) свойство, `Grid` наследует. Можно заполнить этой коллекции в XAML или коде.

### <a name="the-grid-in-xaml"></a>В сетку в XAML

Определение `Grid` в XAML обычно начинается с заполнение [ `RowDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) и [ `ColumnDefinitions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) коллекции `Grid` с [ `RowDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) и [ `ColumnDefinition` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) объектов. Это, как установить число строк и столбцов `Grid`и их свойств.

`RowDefinition` имеет [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) свойство и `ColumnDefinition` имеет [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) свойства, оба типа [ `GridLength` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), структуру.

В языке XAML [ `GridLengthTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) преобразует простые текстовые строки в `GridLength` значения. В фоновом [ `GridLength` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) создает `GridLength` значение на основе номера и значения типа [ `GridUnitType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), перечисление с три члена:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) & #x 2014; Ширина или высота задается в аппаратно независимых единицах (число в языке XAML)
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) & #x 2014; Высота или ширина будет определяться автоматически на основе содержимого ячейки («Авто» в языке XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) & #x 2014; остался высота или ширина распределяется пропорционально (число с «\*», который называется *звезда*, в языке XAML)

Каждый дочерний `Grid` также должен быть назначен строк и столбцов (явно или неявно). Охватывает строки и столбца диапазоны являются необязательными. Они все указываются с помощью присоединенного свойства для привязки & #x 2014; свойства, которые определены в `Grid` но устанавливаться для дочерних элементов `Grid`. `Grid` Определяет четыре статические вложенные свойства привязки:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) & #x 2014; Отсчитываемый от нуля строку; по умолчанию равно 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) & #x 2014; Отсчитываемый от нуля столбца; по умолчанию равно 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) & #x 2014; число строк, охватывающий дочерний; по умолчанию — 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) & #x 2014; число столбцов, охватывающий дочерний; по умолчанию — 1

В коде программы можно использовать восемь статические методы для задания и получения этих значений:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) и [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) и [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) и [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) и [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

Для задания этих значений в XAML используется следующие атрибуты:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

[ **SimpleGridDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) образец демонстрирует создание и инициализация `Grid` в XAML.

`Grid` Наследует [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) свойство из `Layout` и определяет два дополнительных свойства, предоставляющие интервал между строками и столбцами:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) имеет значение по умолчанию 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) имеет значение по умолчанию 6

`RowDefinitions` И `ColumnDefinitions` коллекции не является обязательным. Если отсутствует, `Grid` создает строки и столбцы для `Grid` дочерних элементов и присваивает им все значения по умолчанию `GridLength` из "\*" (звездочка).

### <a name="the-grid-in-code"></a>Сетка в коде

[ **GridCodeDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) образце показано, как создать и заполнить `Grid` в коде. Вложенные свойства можно задать для каждого дочернего непосредственно или косвенно путем вызова дополнительных `Add` методов, таких как [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) определяется [Grid.IGridList<T> ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) интерфейс.

### <a name="the-grid-bar-chart"></a>Линейчатая диаграмма сетки

[ **GridBarChart** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) образце показано, как добавить несколько `BoxView` элементы `Grid` с помощью массового [ `AddHorizontal` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) метод. По умолчанию эти `BoxView` элементы имеют равные ширину. Высота каждого `BoxView` можно управлять для сходства линейчатую диаграмму.

`Grid` В **GridBarChart** пример общих папок `AbsoluteLayout` родительскому с изначально невидимыми `Frame`. Программа также задает `TapGestureRecognizer` на каждом `BoxView` использовать `Frame` для отображения сведений о строке полученные.

### <a name="alignment-in-the-grid"></a>Выравнивание в сетке

[ **GridAlignment** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) образце демонстрируется использование `VerticalOptions` и `HorizontalOptions` свойства выравнивание дочерних элементов в `Grid` ячейки.

[ **SpacingButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) одинаково образец пробелы `Button` элементов по центру `Grid` ячеек.

### <a name="cell-dividers-and-borders"></a>Разделители ячеек и границы

`Grid` Не включает функцию, которая рисует разделителей ячейки или границы. Тем не менее можно создать свой собственный.

[ **GridCellDividers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) демонстрируется определение дополнительных строк и столбцов специально для тонкая `BoxView` элементы, чтобы имитировать разделительные линии.

[ **GridCellBorders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) программа не создает дополнительные ячейки, но вместо этого выравнивает `BoxView` элементов в каждой ячейке, чтобы имитировать границы ячейки.

## <a name="almost-real-life-grid-examples"></a>Почти практические примеры сетки

[ **KeypadGrid** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) примере используются `Grid` для отображения клавиатуры:

[![Снимок экрана тройной клавишной панели сетки](images/ch17fg12-small.png "клавиатуре сетки")](images/ch17fg12-large.png#lightbox "клавиатуре сетки")

### <a name="responding-to-orientation-changes"></a>При изменении ориентации

`Grid` Может помочь структура программы реагировать на изменение ориентации. [ **GridRgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) образец демонстрирует технику, которая перемещает элемент между второй строке телефона Книжная ориентированного и второго столбца телефона альбомной ориентацией.

Инициализирует программу `Slider` элементов в диапазоне от 0 до 255 и привязки данных используется для отображения значения ползунки в шестнадцатеричном формате. Поскольку `Slider` значения представлены плавающими точка и строка форматирования для шестнадцатеричного работает только с целыми числами, .NET [ `DoubleToIntConvert` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки помогает.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 17 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Образцы Глава 17](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Сетка](~/xamarin-forms/user-interface/layouts/grid.md)
