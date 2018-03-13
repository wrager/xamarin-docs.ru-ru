---
title: "Макеты"
description: "Создание макета представления на экране."
ms.topic: article
ms.prod: xamarin
ms.assetid: 65030DA3-C7C1-4A02-B478-811073C39139
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2017
ms.openlocfilehash: 0ede9bbb47f398a82d6eae5d827122f469ad6ea4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="layouts"></a>Макеты

Xamarin.Forms имеет несколько макетов и возможностей для организации содержимого на экране. 

> [!VIDEO https://youtube.com/embed/4HlLjTZQzjM]

**Xamarin.Forms макеты, по [университета Xamarin](https://university.xamarin.com/)**

Каждый элемент управления макета описан ниже, а также сведения о том, как обрабатывать изменения ориентации экрана.

* **[StackLayout](stack-layout.md)**  &ndash; используется для размещения представления линейно, горизонтально или вертикально. Представления в StackLayout можно выравнивать по центру, левой или правой части макета.
* **[AbsoluteLayout](absolute-layout.md)**  &ndash; позволяет расположить представления, задав координаты & размер с точки зрения абсолютные значения или соотношений. Можно использовать AbsoluteLayout слоя представления, а также привязывать их к слева, справа или center.
* **[RelativeLayout](relative-layout.md)**  &ndash; используется для размещения представлений, задав ограничения относительно их родительского измерения и положение.
* **[Сетка](grid.md)**  &ndash; используется для размещения представления в виде сетки. С точки зрения абсолютные значения или соотношений, можно указать строки и столбцы.
* **[ScrollView](scroll-view.md)**  &ndash; используется для предоставления прокрутку, если представление не может поместиться полностью в пределах границ экрана.
* **[LayoutOptions](layout-options.md)**  &ndash; определяют выравнивание и расширение для представления, относительно его родительского элемента.
* **[Входной прозрачности](#input_transparency)**  &ndash; определяет, получает ли элемент входных данных.
* **[Поля и заполнение](margin-and-padding.md)**  &ndash; показано, как управлять поведением макета при подготовке к просмотру элемента в пользовательском интерфейсе.
* **[Ориентации устройства](device-orientation.md)**  &ndash; способы обработки изменение ориентации устройства.
* **[Разметка рабочего стола и планшетных устройств](tablet.md)**  &ndash; показано, как оптимизировать для больших экранов на каждой платформе.
* **[Создание пользовательских макетов](custom.md)**  &ndash; объясняет, как создать класс настраиваемого макета.
* **[Сжатие макета](layout-compression.md)**  &ndash; Удаляет указанный макет из визуального дерева в целях повышения производительности отрисовки страницы.

Элементов управления платформы также может использоваться непосредственно в макете Xamarin.Forms [ **собственного внедрение** ](~/xamarin-forms/platform/native-views/index.md) (новые возможности Xamarin.Forms 2.2), и вы можете [ **создавать пользовательские макеты** ](custom.md) для удовлетворения конкретных требований.

Приведенный ниже рисунок визуализирует элементы управления макета:

[![](images/layouts-sml.png "Макеты Xamarin.Forms")](images/layouts.png#lightbox "Xamarin.Forms макетов")

## <a name="choosing-the-right-layout"></a>Выбор макета вправо

Макеты, выбранного в приложении можно справки или вызвать снижение, при создании приложения Xamarin.Forms привлекательным и готовым к использованию. Занимает некоторое время, которые необходимо учитывать, как работает каждый макет может помочь написать более понятные и масштабируемое код пользовательского интерфейса. Экран может иметь сочетание разные макеты для достижения определенного конструктора.

### <a name="stacklayoutstack-layoutmd"></a>[StackLayout](stack-layout.md)

`StackLayout` Используется для отображения представления вдоль горизонтальной или вертикальной линии. Положение и размер в макете определяется на основе представления `HeightRequest`, `WidthRequest`, `HorizontalOptions` и `VerticalOptions`. `StackLayout` часто используется в качестве базового макета, упорядочение другие раскладки на экране.

Например, если `StackLayout` может быть хорошим выбором, рассмотрим приложение, которое требуется отобразить кнопку и метку, с меткой по левому краю и кнопку по правому краю.

```xaml
<StackLayout Orientation="Horizontal">
  <Label HorizontalOptions="StartAndExpand" Text="Label" />
  <Button HorizontalOptions="End" Text="Button" />
</StackLayout>
```

### <a name="absolutelayoutabsolute-layoutmd"></a>[AbsoluteLayout](absolute-layout.md)

`AbsoluteLayout` Используется для отображения представлений, а также размер и положение указывается либо как явные значения или значения к размеру макета. В отличие от `StackLayout` и `Grid`, `AbsoluteLayout` позволяет дочерние представления перекрываются друг с другом. В отличие от `RelativeLayout`, `AbsoluteLayout` не позволяет размещать элементы вне экрана.

Например, если `AbsoluteLayout` может быть хорошим выбором, рассмотрим приложение, которое требуется для представления коллекции объектов, как стеки. Это часто встречается при представлении альбомам, фото или композиции. Следующий код предоставляет внешний вид стопке, с элементами, повернуто некоторые содержимое накопление:

В XAML:

```xaml
<AbsoluteLayout Padding="15">
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="30"
    Source="bottom.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100" Rotation="60"
    Source="middle.png" />
  <Image AbsoluteLayout.LayoutFlags="PositionProportional" AbsoluteLayout.LayoutBounds="0.5, 0, 100, 100"
    Source="cover.png" />
</AbsoluteLayout>
```

Отметьте следующие аспекты выполнения приведенного выше кода:

- Каждый `Image` отображается в том же месте (в середине пространство по горизонтали)
- `Padding` Считается `AbsoluteLayout`, в отличие от `RelativeLayout`, который не учитывается.
- `AbsoluteLayout.LayoutFlags` Задает интерпретацию границы макета. В этом случае `PositionProportional`, означает, что координаты соотношение размера макета, пока размер будет интерпретироваться как размер, явно.
- `AbsoluteLayout.Layoutbounds` задает горизонтальное положение, вертикальное положение, ширину и высоту в указанном порядке.

### <a name="relativelayoutrelative-layoutmd"></a>[RelativeLayout](relative-layout.md)

`RelativeLayout` Используется для отображения представлений, а также размер и положение, задавать в качестве значений относительно значения макета или другое представление. Относительные значения не обязательно должны соответствовать ему соответствующее значение для представления связанных. Например, можно задать представление `Width` свойство было пропорционально другое представление `X` свойство.

RelativeLayout можно использовать для создания пользовательских интерфейсов, которые масштабируются пропорционально через размеры устройств. Приведенный ниже код XAML реализует конструктора с полями в верхних углах с flagpole с флагом в центре.

```xaml
<RelativeLayout HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand">
  <BoxView Color="Blue" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = 0}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Red" HeightRequest="50" WidthRequest="50"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .9}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = 0}" />
  <BoxView Color="Gray" WidthRequest="15" x:Name="pole"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.75}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToParent, Property=Width, Factor = .45}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor = .25}" />
  <BoxView Color="Green"
    RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height, Factor=.10, Constant=10}"
    RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Width, Factor=.2, Constant=20}"
    RelativeLayout.XConstraint= "{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=X, Constant=15}"
    RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToView, ElementName=pole, Property=Y, Constant=0}" />
</RelativeLayout>
```

Отметьте следующие аспекты выполнения приведенного выше кода:

- Положение и размеры указываются в качестве ограничений.
- Flagpole имеет имя, чтобы флаг (зеленой рамке) можно задать положение относительно flagpole.
- Выражения ограничения имеют `Factor` и `Constant` свойства, которые можно использовать для определения расположения и размеров, кратные (или доли секунды) свойств других объектов, а также константы. Константы могут быть отрицательными.

### <a name="gridgridmd"></a>[Сетка](grid.md)

`Grid` Используется для отображения элементов строк и столбцов. Обратите внимание, что сетка таблицы, поэтому он не поддерживает концепцию ячеек, строк верхний и нижний колонтитулы или границы между строк и столбцов. В общем случае сетка не подходит для отображения табличных данных. Использующие рассмотрим [ListView](~/xamarin-forms/user-interface/listview/index.md) или [TableView](~/xamarin-forms/user-interface/tableview.md).

Например, если `Grid` вправо макет для использования, рассмотрите возможность ввода чисел для калькулятора. Числового ввода для калькулятора может состоять из четырех строк и трех столбцов, каждый из которых кнопки. Следующий код реализует этот проект:

```xaml
<Grid>
  <Grid.RowDefinitions>
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
    <RowDefinition Height="*" />
  </Grid.RowDefinitions>

  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
    <ColumnDefinition Width="*" />
  </Grid.ColumnDefinitions>
  <Button Text="1" Grid.Row="0" Grid.Column="0" />
  <Button Text="2" Grid.Row="0" Grid.Column="1" />
  <Button Text="3" Grid.Row="0" Grid.Column="2" />
  <Button Text="4" Grid.Row="1" Grid.Column="0" />
  <Button Text="5" Grid.Row="1" Grid.Column="1" />
  <Button Text="6" Grid.Row="1" Grid.Column="2" />
  <Button Text="7" Grid.Row="2" Grid.Column="0" />
  <Button Text="8" Grid.Row="2" Grid.Column="1" />
  <Button Text="9" Grid.Row="2" Grid.Column="2" />
  <Button Text="0" Grid.Row="3" Grid.Column="1" />
  <Button Text="&lt;-" Grid.Row="3" Grid.Column="2" />
</Grid>
```

Отметьте следующие аспекты выполнения приведенного выше кода:

- Таблицы и столбцы указаны явно, не выведены из содержимого.
- `Height` и `Width` можно присваивать значения, узнайте, что означает, что сетка будет задавать эти значения для заполнения всего доступного пространства.
- Каждая кнопка позиция задана `Grid.Row`  &  `Grid.Column` свойства.

### <a name="layoutoptionslayout-optionsmd"></a>[LayoutOptions](layout-options.md)

[ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/) Структуру может использоваться для определения выравнивания и расширения для представления, относительно его родительского элемента.

### <a name="margin-and-paddingmargin-and-paddingmd"></a>[Поля и заполнение](margin-and-padding.md)

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) И [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) свойства управляют поведением макета при подготовке к просмотру элемента в пользовательском интерфейсе.

<a name="input_transparency" />

### <a name="input-transparency"></a>Входной прозрачности

Каждый элемент имеет [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) свойство, используемое для определения, является ли элемент получает входные данные. Значением по умолчанию является `false`, гарантируя, что элемент получает входные данные.

Если это свойство имеет значение для класса контейнера, например макет класса, его значение передается дочерних элементов. Таким образом, задание [ `InputTransparent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.InputTransparent/) свойства `true` в макете класса приведет к элементы макета, не получает входные данные.

### <a name="device-orientationdevice-orientationmd"></a>[Ориентация устройства](device-orientation.md)

Xamarin.Forms и его встроенные макеты способны обработка изменений в ориентации устройства. Рассмотрим, какие ориентации экрана приложения будет поддерживать, а также как вам предстоит использовать соответствующее поле в альбомной и портретной режимах.

### <a name="layout-for-tablet-and-desktop-appstabletmd"></a>[Макет для приложений, планшетных и настольных компьютеров](tablet.md)

iOS, Android и Windows все поддерживать большие размеры экрана на платформах планшетных устройствах (а также настольных и переносных компьютеров для Windows). Xamarin.Forms позволяет оптимизировать приложения для больших экранов, определение типа устройства и либо подгонки макета страницы или используя совершенно разных страницы полностью для больших экранов.

### <a name="creating-a-custom-layoutcustommd"></a>[Создание пользовательского макета](custom.md)

Xamarin.Forms определяет четыре класса макет - [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), [ `AbsoluteLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/), [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/), и [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/), и его дочерние элементы каждого упорядочивает по-разному. Тем не менее, иногда ее необходимо для организации содержимого страницы с использованием макета, не предоставляемые Xamarin.Forms. В этой статье объясняется, как создать класс пользовательского макета и демонстрирует чувствительных ориентации `WrapLayout` класс, который упорядочивает свои дочерние элементы горизонтально по странице и затем создает оболочку для отображения дополнительных строк последующие дочерние элементы.

### <a name="layout-compressionlayout-compressionmd"></a>[Сжатие макета](layout-compression.md)

Макет сжатия удаляет указанный макеты из визуального дерева с целью повышения производительности отрисовки страницы. В этом случае рост производительности зависит от сложности страницы, версии используемой операционной системы и устройства, на котором выполняется приложение. Однако наиболее заметное повышение производительности будет наблюдаться на старых устройствах.

## <a name="making-your-choice"></a>Делая выбор

Имейте в виду, что в большинстве случаев несколько вариантов макета может использоваться для реализации требуемой макета. Если имеются несколько допустимых значений, необходимо определить, какой подход будут самый простой способ для конкретной ситуации.
Большинство схем невозможно реализовать с лишь один макет, поэтому вложенные структуры, как требуется для создания более сложных конструкций.


## <a name="related-links"></a>Связанные ссылки

- [Рекомендации Apple пользовательского интерфейса](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG)
- [Веб-сайт разработки Android](https://developer.android.com/design/index.html)
- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
