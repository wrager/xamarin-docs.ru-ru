---
title: Макеты Xamarin.Forms
description: Xamarin.Forms макеты используются для создания элементов управления пользовательского интерфейса в visual структуры. В этой статье перечислены макеты, включенных в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/21/2018
ms.openlocfilehash: 6e9889bf8ec748ed2034d63acfec9784d074ca44
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243094"
---
# <a name="xamarinforms-layouts"></a>Макеты Xamarin.Forms

_Xamarin.Forms макеты используются для создания элементов управления пользовательского интерфейса в visual структуры._

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) И [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) классы в Xamarin.Forms — это специализированный подтипы представлений, которые действуют как контейнеры для представления и другие макеты. `Layout` Сам класс является производным от [ `View` ](views.md). Объект `Layout` производный класс обычно содержит логику, чтобы задать положение и размеры дочерних элементов в приложениях Xamarin.Forms.

[![Типы макета Xamarin.Forms](layouts-images/layouts-sml.png "типы макета Xamarin.Forms")](layouts-images/layouts.png#lightbox "типы макета Xamarin.Forms")

Классы, производные от `Layout` можно разделить на две категории:

## <a name="layouts-with-single-content"></a>Макеты с содержимым одного

Эти классы являются производными от [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), который определяет [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) и [ `IsClippedToBounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.IsClippedToBounds/) свойства.

<a name="contentView" />

### <a name="contentview"></a>ContentView

|     |     |
| --- | --- |
| [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) содержит один дочерний элемент, который устанавливается с [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) свойство. `Content` Свойство может быть установлено одно `View` производный класс, включая другие `Layout` производных продуктов. `ContentView` в основном используется как элемент структурного и служит базовым классом для [ `Frame` ](#frame).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) | [![Пример ContentView](layouts-images/ContentView.png "пример ContentView")](layouts-images/ContentView-Large.png#lightbox "ContentView пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentViewDemoPage.xaml) |
|     |     |

<a named="frame" />

### <a name="frame"></a>Frame

|     |     |
| --- | --- |
| [ `Frame` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) Класс является производным от [ `ContentView` ](#contentView) и отображает прямоугольную рамку вокруг его потомка. `Frame` имеет значение по умолчанию [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) значение 20, а также определяет [ `OutlineColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.OutlineColor/), [ `CornerRadius` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.CornerRadius/), и [ `HasShadow` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Frame.HasShadow/)свойства.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/) | [![Пример фрейма](layouts-images/Frame.png "фрейма пример")](layouts-images/Frame-Large.png#lightbox "фрейма пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FrameDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FrameDemoPage.xaml) |
|     |     |

<a name="scrollView" />

### <a name="scrollview"></a>ScrollView

|     |     |
| --- | --- |
| [`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) возможна прокрутка его содержимое. Задать [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Content/) свойство к представлению или макет слишком велик для отображения на экране. (Содержимое `ScrollView` очень часто [ `StackLayout` ](#stackLayout).) Задать [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ScrollView.Orientation/) свойство, указывающее, если прокрутка следует вертикальная, по горизонтали или оба.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) / [руководство](~/xamarin-forms/user-interface/layouts/scroll-view.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Пример ScrollView](layouts-images/ScrollView.png "пример ScrollView")](layouts-images/ScrollView-Large.png#lightbox "ScrollView пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ScrollViewDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ScrollViewDemoPage.xaml) |
|     |     |

### <a name="templatedview"></a>TemplatedView

|     |     |
| --- | --- |
| [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) Отображает содержимое с помощью шаблона элемента управления и является базовым классом для [ `ContentView` ](#contentView).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/) / [руководства](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Пример TemplatedView](layouts-images/TemplatedView.png "пример TemplatedView")](layouts-images/TemplatedView.png#lightbox "TemplatedView пример") |
|     |     |

### <a name="contentpresenter"></a>ContentPresenter

|     |     |
| --- | --- |
| [`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) представляет собой диспетчер макета для шаблонного представлений, используемых в [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) для пометки, где отображается содержимое, должна быть предоставлена.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) / [руководства](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Пример ContentPresenter](layouts-images/ContentPresenter.png "примере ContentPresenter")](layouts-images/ContentPresenter.png#lightbox "ContentPresenter пример") |
|     |     |

## <a name="layouts-with-multiple-children"></a>Макеты с несколькими дочерними элементами

Эти классы являются производными от [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/).

<a name="stackLayout" />

### <a name="stacklayout"></a>StackLayout

|     |     |
| --- | --- |
| [`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) размещает дочерние элементы в стеке, либо по горизонтали или по вертикали в зависимости от [ `Orientation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Orientation/) свойство. [ `Spacing` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StackLayout.Spacing/) Свойство определяет расстояние между дочерние элементы и имеет значение по умолчанию 6.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) / [руководство](~/xamarin-forms/user-interface/layouts/stack-layout.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)| [![Пример StackLayout](layouts-images/StackLayout.png "пример StackLayout")](layouts-images/StackLayout-Large.png#lightbox "StackLayout пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StackLayoutDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StackLayoutDemoPage.xaml) |
|     |     |

<a name="grid" />

### <a name="grid"></a>Grid

|     |     |
| --- | --- |
| [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) размещает дочерние элементы в сетке из строк и столбцов. Позиции дочерней таблицы указывается с помощью [вложенные свойства](~/xamarin-forms/xaml/attached-properties.md) [ `Row` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/), [ `Column` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/), [ `RowSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/), и [ `ColumnSpan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) / [руководство](~/xamarin-forms/user-interface/layouts/grid.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Пример сетки](layouts-images/Grid.png "пример сетки")](layouts-images/Grid-Large.png#lightbox "пример сетки")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/GridDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/GridDemoPage.xaml) |
|     |     |

### <a name="absolutelayout"></a>AbsoluteLayout

|     |     |
| --- | --- |
| [`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) размещает дочерние элементы в определенные расположения относительно его родительского элемента. Позиции дочерней таблицы указывается с помощью [вложенные свойства](~/xamarin-forms/xaml/attached-properties.md) [ `LayoutBounds` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutBoundsProperty/) и [ `LayoutFlags` ](https://developer.xamarin.com/api/field/Xamarin.Forms.AbsoluteLayout.LayoutFlagsProperty/). `AbsoluteLayout` Полезна для анимации позиций представлений.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) / [руководство](~/xamarin-forms/user-interface/layouts/absolute-layout.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Пример AbsoluteLayout](layouts-images/AbsoluteLayout.png "пример AbsoluteLayout")](layouts-images/AbsoluteLayout-Large.png#lightbox "AbsoluteLayout пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/AbsoluteLayoutdDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/AbsoluteLayoutDemoPage.xaml.cs) |
|     |     |

### <a name="relativelayout"></a>RelativeLayout

|     |     |
| --- | --- |
| [`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) размещает дочерние элементы относительно `RelativeLayout` себя или своих одноуровневых элементов. Позиции дочерней таблицы указывается с помощью [вложенные свойства](~/xamarin-forms/xaml/attached-properties.md) , которые задаются для объектов типа [ `Constraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/) и [ `BoundsConstraint` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) / [руководство](~/xamarin-forms/user-interface/layouts/relative-layout.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/) | [![Пример RelativeLayout](layouts-images/RelativeLayout.png "пример RelativeLayout")](layouts-images/RelativeLayout-Large.png#lightbox "RelativeLayout пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/RelativeLayoutDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/RelativeLayoutDemoPage.xaml) |
|     |     |

### <a name="flexlayout"></a>FlexLayout

|     |     |
| --- | --- |
| [`FlexLayout`](xref:Xamarin.Forms.FlexLayout) основан на CSS [гибкие поле Макет модуля](http://www.w3.org/TR/css-flexbox-1/), которая часто называется _гибкий макет_ или _flex поле_. `FlexLayout` Определяет шесть привязываемые свойства и пять вложенного привязываемые свойства, позволяющие дочерние элементы с накоплением и оболочку множество вариантов выравнивания и ориентацию.<br /><br />[Документация по API](xref:Xamarin.Forms.FlexLayout) / [руководство](~/xamarin-forms/user-interface/layouts/flex-layout.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/FlexLayoutDemos/) | [![Пример FlexLayout](layouts-images/FlexLayout.png "пример FlexLayout")](layouts-images/FlexLayout-Large.png#lightbox "FlexLayout пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/FlexLayoutDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/FlexLayoutDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Образец Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
