---
title: "Создание пользовательских макетов"
description: "Xamarin.Forms определяет четыре класса макета — StackLayout, AbsoluteLayout, RelativeLayout и сетку, и его дочерние элементы каждого упорядочивает по-разному. Тем не менее иногда бывает необходимо для организации содержимого страницы с использованием макета, не предусмотренная в Xamarin.Forms. В этой статье объясняется, как создать класс пользовательского макета и демонстрируется класса WrapLayout учетом ориентации, упорядочивает свои дочерние элементы горизонтально по странице и затем создает оболочку для отображения дополнительных строк последующие дочерние элементы."
ms.topic: article
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 4c7bf5f2c867faef7d9baf8d511393dbe2d129a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-custom-layout"></a>Создание пользовательских макетов

_Xamarin.Forms определяет четыре класса макета — StackLayout, AbsoluteLayout, RelativeLayout и сетку, и его дочерние элементы каждого упорядочивает по-разному. Тем не менее иногда бывает необходимо для организации содержимого страницы с использованием макета, не предусмотренная в Xamarin.Forms. В этой статье объясняется, как создать класс пользовательского макета и демонстрируется класса WrapLayout учетом ориентации, упорядочивает свои дочерние элементы горизонтально по странице и затем создает оболочку для отображения дополнительных строк последующие дочерние элементы._

## <a name="overview"></a>Обзор

В Xamarin.Forms, все макета классы являются производными от [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) класса и ограничения универсального типа, [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) и его производные типы. В свою очередь `Layout<T>` класс является производным от [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) класса, который предоставляет механизм для размещения и изменения размеров дочерних элементов.

Каждый визуальный элемент отвечает за определение собственный предпочтительный размер, который называется *запрошенный* размер. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), и [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) отвечает за определение расположение и размер своих дочерних элементов или дочерние элементы относительно сами производных типов. Таким образом, макет включает в себя связь родитель потомок, где родительский определяет размер своих дочерних элементов, которое должно быть, но будет пытаться запрошенного размера дочернего элемента.

Для создания пользовательского макета требуется глубокого понимания Xamarin.Forms макета и недействительности циклов. Теперь мы обсудим эти циклы.

## <a name="layout"></a>Макет

Макет начинается в верхней части визуального дерева, страница, и он проходит через все ветви визуального дерева для охвата каждого визуального элемента на странице. Для изменения размеров и положения их дочерние элементы относительно сами отвечают элементы, которые являются родительскими для других элементов.

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Класс определяет [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) методом, позволяющим измерить элемент для операций макета и [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод, который задает Прямоугольная область, элемент будет отображен в. При запуске приложения и отображается первая страница, *цикла разметки* первый состоящий из `Measure` вызовы, а затем `Layout` вызывает, запускает на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) объекта:

1. Во время цикла разметки каждый родительский элемент отвечает за вызов метода `Measure` метод своим дочерним элементам.
1. После дочерние элементы были оценены, каждый родительский элемент отвечает за вызов метода `Layout` метод своим дочерним элементам.

Этого цикла гарантирует, что каждого визуального элемента на странице получение вызовы `Measure` и `Layout` методы. Процесс показан на следующей схеме:

![](custom-images/layout-cycle.png "Цикл Xamarin.Forms макета")

> [!NOTE]
> Обратите внимание, что макет циклов также может возникать на подмножестве визуального дерева при каких-либо изменений влияет на макет. Сюда входят элементы, добавления или удаления из коллекции, например в [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), изменения в [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) свойства элемента или изменения в размере элемента.

Каждый класс Xamarin.Forms, который имеет `Content` или `Children` свойство имеет переопределяемый [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) метод. Пользовательский макет классы, производные от [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) необходимо переопределить этот метод и убедиться, что [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) и [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) методы вызывается для всех его дочерних элементов, для обеспечения требуемой пользовательского макета.

Кроме того, каждый класс, производный от [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) или [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) необходимо переопределить [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) метода, который там, где класс макета Определяет размер, он должен быть путем вызова [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) методов из его дочерних элементов.

> [!NOTE]
> Определить элементы, их размер в зависимости от *ограничения*, который показывает, сколько места доступно для элемента внутри родительского элемента. Ограничения, передаваемый [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) и [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) методы может находиться в диапазоне от 0 до `Double.PositiveInfinity`. Элемент представляет собой *ограниченного*, или *полностью ограниченное*, когда он получает вызов для его [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) метод с аргументами бессрочной - ограничен элемента определенный размер. Элемент представляет собой *произвольные*, или *частично ограниченного*, когда он получает вызов для его `Measure` метод с хотя бы один аргумент равен `Double.PositiveInfinity` — бесконечное ограничение может быть считать, указывающее, автоматическое изменение размера.

## <a name="invalidation"></a>Недействительность

Недействительность — это процесс, с помощью которого изменения в элемент на странице запускает новый цикл макета. Элементы считаются недопустимыми, если они больше не имеют неподходящий размер или положение. Например если [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) свойство [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) изменений, `Button` считается недействительным, так как он больше не будут иметь правильный размер. Изменение размеров `Button` у окажет влияние изменений в макет через оставшуюся часть страницы.

Элементы недействительным сами путем вызова [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) метод, обычно когда свойство элемента изменяется, может привести к новый размер элемента. Этот метод запускает [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) события, который обрабатывает родительского элемента, чтобы запустить новый цикл макета.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Класс задает обработчик для [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) событий на всех дочерних действиях, добавить его `Content` свойство или `Children` коллекции и отсоединяет обработчик при дочерние удаляется. Таким образом каждый элемент в визуальном дереве, имеющего дочерние оповещения, всякий раз, когда один из его дочерних элементов изменяется размер. На следующей схеме показана, как изменение размера элемента в визуальное дерево может привести к изменения, которые ripple вверх по дереву:

![](custom-images/invalidation.png "Недействительность визуального дерева")

Тем не менее `Layout` класс пытается ограничить влияние изменения в размере ребенка на макет страницы. Если макет ограниченного размера, затем изменения размера дочернего не влияет на превышает макета родительского визуального дерева. Однако обычно изменения в размере макета влияет как макет упорядочивает свои дочерние элементы. Таким образом, любые изменения в размере макет будет запущена цикла разметки для макета и макета будет получать вызовы для его [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) и [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) методы.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Класс также определяет [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) метода, имеющего же цель [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) метод. `InvalidateLayout` Метод должен вызываться всякий раз, когда вносится изменение, влияет на способ макета помещает и размеров его дочерних элементов. Например `Layout` класс вызывает `InvalidateLayout` метод всякий раз, когда дочерний элемент добавлен или удален из макета.

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Может быть переопределен для реализации кэша, чтобы свести к минимуму повторяющиеся вызовы [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) методы макет дочерних элементов. Переопределение `InvalidateLayout` метод предоставит уведомления, когда добавлены или удалены из разметки дочерних элементов. Аналогичным образом [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) метод может быть переопределен для предоставления уведомления при изменении размера одного из дочерних элементов макета. Для обоих переопределения методов пользовательских макетов выдавал Очистка кэша. Дополнительные сведения см. в разделе [вычисление и кэширование данных](#caching).

## <a name="creating-a-custom-layout"></a>Создание пользовательских макетов

Создание пользовательских макетов выполняется следующим образом:

1. Создайте класс, производный от класса `Layout<View>`. Дополнительные сведения см. в разделе [Создание WrapLayout](#creating).
1. [*необязательно*] добавить свойства, поддерживаемый свойства для привязки для всех параметров, которые необходимо настроить макет класса. Дополнительные сведения см. в разделе [Добавление свойства поддерживаемый привязываемые свойства](#adding_properties).
1. Переопределить [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) метод, вызываемый [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) метод на дочерние элементы макета и возвращают запрошенный размер для макета. Дополнительные сведения см. в разделе [переопределение метода OnMeasure](#onmeasure).
1. Переопределить [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) метод, вызываемый [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод макет дочерних элементах. Ошибка вызова [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) никогда не получает правильные размер и положение дочерних приведет к метод для каждого дочернего элемента в макете, и поэтому дочернего, не станет видимым на странице. Дополнительные сведения см. в разделе [переопределение метода LayoutChildren](#layoutchildren).

  > [!NOTE]
>  При перечислении дочерних элементов в [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) и [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) переопределения, пропустить все дочерние которого [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) свойству `false`. Это позволит гарантировать, что пользовательский макет не остается места для невидимой дочерних элементов.

1. [*необязательно*] переопределить [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) метод, чтобы получать уведомления при добавлении дочерних элементов или удалены из разметки. Дополнительные сведения см. в разделе [переопределение метода InvalidateLayout](#invalidatelayout).
1. [*необязательно*] переопределить [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) метод, чтобы получать уведомления при изменении размера одного из дочерних элементов макета. Дополнительные сведения см. в разделе [переопределение метода OnChildMeasureInvalidated](#onchildmeasureinvalidated).

> [!NOTE]
> Обратите внимание, что [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) переопределение не будет вызван, если размер макета регулируется родительским, а не его дочерние элементы. Тем не менее, если один или оба ограничения имеют бесконечный или класс макета имеет не по умолчанию будет вызываться переопределение [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) или [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) значения свойств. По этой причине [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) переопределение не следует полагаться на размеры дочерних, полученные во время [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) вызова метода. Вместо этого `LayoutChildren` необходимо вызвать [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) дочерние элементы макета, перед вызовом метода [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод. Можно также получить размер дочерних элементов в `OnMeasure` переопределения могут кэшироваться, чтобы впоследствии избежать `Measure` вызовы в `LayoutChildren` переопределение, но макет класса необходимо знать, когда потребуется снова получить размеры. Дополнительные сведения см. в разделе [вычисление и кэширование данных макет](#caching).

Макет класса могут быть впоследствии использованы путем добавления его в [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)и добавление дочерних элементов макета. Дополнительные сведения см. в разделе [использование WrapLayout](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Создание WrapLayout

Образец приложения показывает чувствительных ориентации `WrapLayout` класс, который упорядочивает свои дочерние элементы горизонтально по странице и затем создает оболочку для отображения дополнительных строк последующие дочерние элементы.

`WrapLayout` Класс выделяет такого же объема пространства для каждого дочернего элемента, известный как *размеру ячейки*, основываясь на максимальный размер дочерних элементов. Дочерние элементы, меньше, чем размер ячейки могут быть расположены в ячейке на основе их [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) значения свойств.

`WrapLayout` В следующем примере кода показано определение класса:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Вычисление и кэширования данных макета

`LayoutData` Структура сохраняет данные о коллекции дочерних элементов в число свойств:

- `VisibleChildCount` — число дочерних объектов, видимых в макете.
- `CellSize` — Максимальный размер всех дочерних элементов, размером макета.
- `Rows` — количество строк.
- `Columns` — количество столбцов.

`layoutDataCache` Поле используется для хранения нескольких `LayoutData` значения. При запуске приложения, два `LayoutData` объектов будет кэшироваться в `layoutDataCache` словарь для текущей ориентации — один для аргументов ограничения `OnMeasure` переопределения и один для `width` и `height` аргументов Чтобы `LayoutChildren` переопределения. При повороте устройства в альбомной ориентации `OnMeasure` переопределить и `LayoutChildren` переопределение будет снова вызван, что приведет к другой двух `LayoutData` объектов кэширования в словарь. Тем не менее, при возвращении устройства в книжной ориентации, последующие вычисления не необходимы, поскольку `layoutDataCache` уже содержит необходимые данные.

В следующем примере кода показан `GetLayoutData` метод, который рассчитывает свойства `LayoutData` структурированных на основе определенного размера:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData` Метод выполняет следующие операции:

- Определяет, является ли вычисляемый `LayoutData` значение уже находится в кэше и возвращает его, если он доступен.
- В противном случае он выполняет перечисление всех дочерних элементов, вызов `Measure` метод для каждого дочернего элемента с неограниченной ширины и высоты и определяет размер максимального дочерних.
- При условии, что имеется по крайней мере один дочерний элемент видимым, он вычисляет количество строк и столбцов, необходимых, а затем вычисляет размер ячейки для дочерними, основанными на размеры `WrapLayout`. Обратите внимание, размер ячейки обычно немного шире, чем дочерних максимальный размер, однако, это также может быть меньше, если `WrapLayout` не достаточно широким для широкой дочерний или установить достаточно для самого высокого дочернего элемента.
- Она сохраняет новый `LayoutData` значение в кэше.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Добавление свойств, поддерживаемое привязываемые свойства

`WrapLayout` Класс определяет `ColumnSpacing` и `RowSpacing` свойства, значения которых используются для разделения строк и столбцов в макете, и где резервное копирование с привязываемыми свойствами. В следующем примере кода показаны привязываемые свойства:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Вызывает обработчик изменения свойства каждого привязываемые свойства `InvalidateLayout` передать переопределяющий метод, чтобы запустить новый макет `WrapLayout`. Дополнительные сведения см. в разделе [переопределение метода InvalidateLayout](#invalidatelayout) и [переопределение метода OnChildMeasureInvalidated](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>Переопределение метода OnMeasure

`OnMeasure` Переопределение показано в следующем примере кода:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Вызывает переопределение `GetLayoutData` метода и конструкции `SizeRequest` объекта из возвращенных данных при также принимая во внимание `RowSpacing` и `ColumnSpacing` значения свойств. Дополнительные сведения о `GetLayoutData` метода, в разделе [вычисление и кэширование данных](#caching).

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) И [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) методы никогда не следует запрашивать бесконечный измерения, возвращая [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) значение свойству присвоено значение `Double.PositiveInfinity`. Тем не менее по крайней мере один из аргументов ограничения для `OnMeasure` может быть `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>Переопределение метода LayoutChildren

`LayoutChildren` Переопределение показано в следующем примере кода:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Переопределение начинается с вызова `GetLayoutData` метод и затем перечисляет все дочерние элементы для их размера и положения в пределах каждого дочернего элемента ячейки. Это достигается путем вызова [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) метод, который используется для размещения дочерних внутри прямоугольника, в зависимости от его [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) и [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)значения свойств. Это эквивалентно вызова к дочернему [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) метод.

> [!NOTE]
> Обратите внимание, что прямоугольник, переданный в `LayoutChildIntoBoundingRegion` метод включает всю область, в котором может находиться дочернего.

Дополнительные сведения о `GetLayoutData` метода, в разделе [вычисление и кэширование данных](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>Переопределение метода InvalidateLayout

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Переопределение вызывается в том случае, когда дочерние элементы добавлении или удалении из макета или когда один из `WrapLayout` изменения свойств значения, как показано в следующем примере кода:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Переопределение делает недействительной макета и удаляет все сведения о кэшированных макета.

> [!NOTE]
> Чтобы остановить [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) вызов класса [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) переопределение метода всякий раз, когда дочерний элемент добавлен или удален из макета, [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) и [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) методов и возвращаемых `false`. Макет класса затем можно реализовать пользовательский процесс, при удалении или добавлении дочерних элементов.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>Переопределение метода OnChildMeasureInvalidated

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Переопределение вызывается, когда один из дочерних элементов макета изменяется размер, показано в следующем примере кода:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Переопределение делает недействительными макет дочерних и отменяет все сведения о кэшированных макете.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>Использование WrapLayout

`WrapLayout` Класс может использоваться, поместив его на [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) производному типу, как показано в следующем примере кода XAML:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Ниже приведен эквивалентный код C#:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Дочерние элементы можно затем добавить `WrapLayout` при необходимости. В следующем примере кода показан [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элементы, добавляемые `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Если на страницу, содержащую `WrapLayout` отображается образец приложения асинхронно получает доступ к удаленной JSON-файла, содержащего список фотографий, создает [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) фото для каждого элемента и добавляет его в `WrapLayout`. Это приводит к появлению показано на следующем снимке экрана:

![](custom-images/portait-screenshots.png "Образец приложения книжной ориентации, снимки экрана")

В следующих снимках экрана показано `WrapLayout` после поворачивается на альбомную ориентацию страницы:

![](custom-images/landscape-ios.png "Образец iOS снимок экрана приложения Альбомная")
![](custom-images/landscape-android.png "Android приложения альбомная снимок экрана") 
 ![ ] (custom-images/landscape-uwp.png " Снимок экрана альбомная приложений UWP")

Число столбцов в каждой строке зависит от размера фотографий, ширину экрана и количество точек в аппаратно независимая единица. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Элементы асинхронно загружать фотографии и, следовательно, `WrapLayout` класса получит часто его [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) метод при каждом `Image` элемент получает новый размер в зависимости от загрузки фото.

## <a name="summary"></a>Сводка

В этой статье описано, как написать пользовательский макет класса и продемонстрировали чувствительных ориентации `WrapLayout` класс, который упорядочивает свои дочерние элементы горизонтально по странице и затем создает оболочку для отображения дополнительных строк последующие дочерние элементы.


## <a name="related-links"></a>Связанные ссылки

- [WrapLayout (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Пользовательские макеты](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Создание пользовательских макетов Xamarin.Forms (видео)](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Макет<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Макет](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
