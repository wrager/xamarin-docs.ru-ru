---
title: TableView
description: Представляет меню прокрутки "," Параметры "и" формами ввода.
ms.prod: xamarin
ms.assetid: D1619D19-A74F-40DF-8E53-B1B7DFF7A3FB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 312472fdfae65bc62b76f4295a13760236dededc
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847664"
---
# <a name="tableview"></a>TableView

[TableView](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) — это представление для отображения прокручиваемой списки данных или возможных вариантов там, где имеются строки, которые не используют тот же шаблон. В отличие от [ListView](~/xamarin-forms/user-interface/listview/index.md), TableView не поддерживает концепцию `ItemsSource`, поэтому элементы должны быть добавлены как дочерние элементы вручную.

Это руководство состоит из следующих разделов:

- **[Варианты использования](#Use_Cases)**  &ndash; использование TableView вместо ListView или пользовательское представление.
- **[Структура TableView](#TableView_Structure)**  &ndash; иерархию представления, которые требуется в течение TableView.
- **[Внешний вид TableView](#TableView_Appearance)**  &ndash; параметры настройки для TableView.
- **[Встроенные ячеек](#Built-In_Cells)**  &ndash; ячейки встроенные параметры, включая [EntryCell](#EntryCell) и [SwitchCell](#SwitchCell).
- **[Пользовательские ячейки](#Custom_Cells)**  &ndash; том, как делать собственные пользовательские ячейки.

![](tableview-images/tableview-all-sml.png "Пример TableView")

<a name="Use_Cases" />

## <a name="use-cases"></a>Варианты использования

TableView полезен, когда:

- представления списка параметров,
- Сбор данных в форме, или
- Отображение данных, которые представляются по-разному из строки строки (например, номера, процентные показатели и изображения).

TableView обрабатывает прокрутку и размещение строк в разделах привлекательным, требуется для указанных выше сценариев. `TableView` Управления использует каждой платформы базовый эквивалентное представление, если она доступна, создание собственного внешний вид для каждой платформы.

<a name="TableView_Structure" />

## <a name="tableview-structure"></a>Структура TableView

Элементы в `TableView` упорядочены по разделам. В корне `TableView` — `TableRoot`, который является родительским для одного или нескольких `TableSections`:

```csharp
Content = new TableView {
    Root = new TableRoot {
        new TableSection...
    },
    Intent = TableIntent.Settings
};
```

Каждый `TableSection` состоит из заголовка и один или несколько ViewCells. Здесь мы видим `TableSection` `Title` свойство *«Кольцо»* в конструкторе:

```csharp
var section = new TableSection ("Ring") { //TableSection constructor takes title as an optional parameter
    new SwitchCell {Text = "New Voice Mail"},
    new SwitchCell {Text = "New Mail", On = true}
};
```

Чтобы добиться тот же макет, как описано выше в XAML.

```xaml
<TableView Intent="Settings">
    <TableRoot>
        <TableSection Title="Ring">
            <SwitchCell Text="New Voice Mail" />
      <SwitchCell Text="New Mail" On="true" />
        </TableSection>
    </TableRoot>
</TableView>
```

<a name="TableView_Appearance" />

## <a name="tableview-appearance"></a>Внешний вид TableView

Предоставляет TableView `Intent` свойство, которое представляет собой перечисление следующие параметры:

- **Данные** &ndash; для использования при отображении записей данных. Обратите внимание, что [ListView](~/xamarin-forms/user-interface/listview/index.md) может быть лучшим вариантом для прокрутки списков данных.
- **Форма** &ndash; для использования при TableView выступает в качестве формы.
- **Меню** &ndash; для использования при представлении меню выбранных объектов.
- **Параметры** &ndash; для использования при отображении список параметров конфигурации.

`TableIntent` Выберите может повлиять на способ `TableView` появляется на каждой платформе. Даже если не очистить различия, рекомендуется выбрать `TableIntent` , наиболее точно соответствующий, как планируется использовать в таблице.

<a name="Built-In_Cells" />

## <a name="built-in-cells"></a>Встроенные ячеек

Xamarin.Forms поставляется со встроенной ячеек для сбора и отображения сведений. Несмотря на то, что ListView и TableView можно использовать все же ячейки, ниже приведены наиболее важные для сценария TableView.

- **SwitchCell** &ndash; для представления и записи состояния true или false, вместе с текстовой метки.
- **EntryCell** &ndash; для представления и извлечение текста.

В разделе [внешнего вида ячеек ListView](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md) подробное описание [TextCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) и [ImageCell](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell).

<a name="switchcell" />

### <a name="switchcell"></a>SwitchCell
[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/) Представляет элемент управления, используемый для представления и отслеживания Вкл/Выкл или `true` / `false` состояния.

SwitchCells имеет одну строку текста для редактирования и свойством Вкл. / выкл. Оба эти свойства можно привязать.

- `Text` &ndash; текст, отображаемый рядом с параметром.
- `On` &ndash; параметр вывод как on или off.

Обратите внимание, что `SwitchCell` предоставляет `OnChanged` событий, что позволяет ответить на изменения в состоянии ячейки.

![](tableview-images/switch-cell.png "Пример SwitchCell")

<a name="entrycell" />

### <a name="entrycell"></a>EntryCell
[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) полезно, когда требуется отобразить текстовые данные, пользователь может изменить. `EntryCell`s имеют следующие свойства, которые могут быть настроены.

- `Keyboard` &ndash; Клавиатуры для отображения во время редактирования. Существуют параметры для таких вещей, как числовые значения, электронной почты, номера телефонов и т. д. [В разделе документы API](http://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/).
- `Label` &ndash; Текст метки для отображения справа от текстового поля ввода.
- `LabelColor` &ndash; Цвет текста метки.
- `Placeholder` &ndash; Текст, отображаемый в поле ввода, если он пуст или равен null. Этот текст исчезает, когда начинается запись текста.
- `Text` &ndash; Текст в поле ввода.
- `HorizontalTextAlignment` &ndash; Горизонтальное выравнивание текста. Могут быть центра влево или вправо выровнены. [В разделе документы API](http://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/).

Обратите внимание, что `EntryCell` предоставляет `Completed` событие, которое возникает, когда пользователь нажимает «Готово» на клавиатуре во время редактирования текста.

![](tableview-images/entry-cell.png "Пример EntryCell")

<a name="Custom_Cells" />

## <a name="custom-cells"></a>Пользовательские ячейки
Если встроенные ячеек оказывается недостаточно, пользовательские ячейки можно использовать для представления и записывать данные в удобном для вашего приложения. Например можно представить ползунок, чтобы разрешить пользователю выбирать прозрачности изображения.

Все пользовательские ячейки должны быть производными от [ `ViewCell` ](http://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/), тот же базовый класс, что все ячейки встроенные типы использования.

Ниже приведен пример пользовательского ячейки:

![](tableview-images/custom-cell.png "Пример пользовательских ячеек")

### <a name="xaml"></a>XAML
XAML для создания макета выше используется следующим образом:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="DemoTableView.TablePage" Title="TableView">
    <ContentPage.Content>
        <TableView Intent="Settings">
            <TableRoot>
                <TableSection Title="Getting Started">
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <Image Source="bulb.png" />
                            <Label Text="left"
                              TextColor="#f35e20" />
                            <Label Text="right"
                              HorizontalOptions="EndAndExpand"
                              TextColor="#503026" />
                        </StackLayout>
                    </ViewCell>
                </TableSection>
            </TableRoot>
        </TableView>
    </ContentPage.Content>
</ContentPage>

```

Выполняет гораздо выше XAML. Давайте разбить ее:

- Корневой элемент в списке `TableView` — `TableRoot`.
- Отсутствует `TableSection` непосредственно под `TableRoot`.
- `ViewCell` Определен непосредственно под раздел таблицы. В отличие от `ListView`, `TableView` не требуется, настраиваемый (или все), определенных в `ItemTemplate`.
- StackLayout используется для управления макета пользовательских ячеек. Здесь можно использовать любой макет.

### <a name="cnum"></a>C&num;

Поскольку `TableView` работает с статических данных или данные, необходимо вручную изменить, не имеет концепции шаблона элемента. Вместо этого пользовательские ячейки можно вручную создать и поместить в таблицу. Обратите внимание, что способ создания пользовательской ячейки, наследует от `ViewCell`, а затем добавление его в `TableView` как вы бы встроенных ячейки, также поддерживается.
Ниже приведен код c# для выполнения макета выше:

```csharp
var table = new TableView();
table.Intent = TableIntent.Settings;
var layout = new StackLayout() { Orientation = StackOrientation.Horizontal };
layout.Children.Add (new Image() {Source = "bulb.png"});
layout.Children.Add (new Label() {
    Text = "left",
    TextColor = Color.FromHex("#f35e20"),
    VerticalOptions = LayoutOptions.Center
});
layout.Children.Add (new Label () {
    Text = "right",
    TextColor = Color.FromHex ("#503026"),
    VerticalOptions = LayoutOptions.Center,
    HorizontalOptions = LayoutOptions.EndAndExpand
});
table.Root = new TableRoot () {
    new TableSection("Getting Started") {
        new ViewCell() {View = layout}
    }
};

Content = table;
```

В C# выше выполняет много. Давайте разбить ее:

- Корневой элемент в списке `TableView` — `TableRoot`.
- Отсутствует `TableSection` непосредственно под `TableRoot`.
- `ViewCell` Определен непосредственно под раздел таблицы. В отличие от `ListView`, `TableView` не требуется, настраиваемый (или все), определенных в `ItemTemplate`.
- StackLayout используется для управления макета пользовательских ячеек. Здесь можно использовать любой макет.

Обратите внимание, что никогда не определен класс для пользовательских ячеек. Вместо этого `ViewCell`его представление является свойство для конкретного экземпляра `ViewCell`.



## <a name="related-links"></a>Связанные ссылки

- [TableView (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView)
