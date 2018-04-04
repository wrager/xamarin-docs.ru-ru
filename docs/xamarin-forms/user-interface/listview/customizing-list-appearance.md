---
title: Внешний список
description: Настройте представлениях ListView с помощью заголовки, нижние колонтитулы, групп и переменной высоты ячейки.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: ff36deff996a92fca512158252c64e5c29046be9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="list-appearance"></a>Внешний список

`ListView` содержит параметры для управления представлением общий список, в дополнение к базовым `ViewCell`s. Возможны следующие значения.

- [**Группирование** ](#Grouping) &ndash; группировать элементы в ListView для упрощения навигации и улучшенное организации.
- [**Верхние и нижние колонтитулы** ](#Headers_and_Footers) &ndash; отображения информации в начале и конце представлении, который прокручивается вместе с другими элементами.
- [**В качестве разделителей строк** ](#Row_Separators) &ndash; Отображение или скрытие разделительных линий между элементами.
- [**Переменные строки высоту** ](#Row_Heights) &ndash; по умолчанию все строки имеют одинаковую высоту, но его можно изменить, чтобы разрешить строки с различными высоты для отображения.

<a name="Grouping" />

## <a name="grouping"></a>Группирование
Часто больших объемов данных может оказаться неудобной, при выводе в виде постоянно прокручиваемого списка. Включение группирования можно улучшить взаимодействие с пользователем в этих случаях, лучше организовать содержимое и активация элементов управления от платформы, облегчающие перемещения по данным.

При активации для группирования `ListView`, строку заголовка добавляется для каждой группы.

Чтобы включить группирование:

- Создайте список списков (список групп, каждая группа, которой список элементов).
- Задать `ListView` `ItemsSource` к этому списку.
- Задать `IsGroupingEnabled` значение true.
- Задать [ `GroupDisplayBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupDisplayBinding/) привязку к свойству групп, который используется в качестве заголовка группы.
- [Необязательно] Задать [ `GroupShortNameBinding` ](http://developer.xamarin.com/api/property/Xamarin.Forms.ListView.GroupShortNameBinding/) привязку к свойству групп, который используется в качестве краткого имени для группы. Короткое имя используется для списки переходов (стороне rigt столбец на iOS, ячеек на Windows Phone).

Начните с создания класса для групп:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

В приведенном выше коде `All` — список, который будет предоставлен к нашей ListView как источник привязки. `Title` и `ShortName` являются свойствами, которые будут использоваться для заголовков групп.

На этом этапе `All` — пустой список. Добавьте статический конструктор, чтобы список будет заполнен во время запуска программы:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

В приведенном выше коде можно также вызвать `Add` над элементами `groups`, которые являются экземплярами типа `PageTypeGroup`. Это возможно, так как `PageTypeGroup` наследует от `List<PageModel>`. Ниже приводится пример списка шаблонов списков, указанных выше.

Ниже приведен код XAML для отображения Сгруппированный список.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Эти действия произведут следующий результат:

![](customizing-list-appearance-images/grouping-depth.png "Пример группирования ListView")

Обратите внимание, что у нас есть:

- Задать `GroupShortNameBinding` для `ShortName` свойство, определенное в классе нашей группы
- Задать `GroupDisplayBinding` для `Title` свойство, определенное в классе нашей группы
- Задать `IsGroupingEnabled` значение true
- Изменить `ListView` `ItemsSource` в сгруппированный список

### <a name="customizing-grouping"></a>Настройка группирования
Теперь, когда мы рассмотрели для реализации основных группирования в ListView, давайте посмотрим, как настраивать отображение заголовков групп.

Аналогично тому, как `ListView` имеет `ItemTemplate` для определения отображения строк, `ListView` имеет `GroupHeaderTemplate`. Ниже приведен пример элемента управления ListView выше, с помощью шаблона заголовок особой группы:

![](customizing-list-appearance-images/grouping-depth.png "ListView с настроенные GroupHeaderTemplate")


Вот способ выполнения этой структуры в языке XAML.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Верхние и нижние колонтитулы
Это возможно в ListView для представления верхний и нижний колонтитулы, прокрутите с элементами списка. Верхний и нижний колонтитул может быть строк текста или более сложный макет. Обратите внимание, что это отличается от [статьи группы](#Grouping).

Можно задать `Header` или `Footer` в простую строку значения, или можно задать их для создания более сложных макетов.
Существуют также `HeaderTemplate` и `FooterTemplate` свойства, позволяющие создавать более сложные макеты для верхнего и нижнего колонтитула, поддерживающих привязку данных.

Чтобы создать простой верхний/нижний колонтитул, просто установите свойства верхнего или нижнего колонтитула текст, который будет отображаться. В коде:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

В XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView с верхнего и нижнего колонтитулов")

Чтобы создать настраиваемый верхний и нижний колонтитулы, определите верхний и нижний колонтитулы представления:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView с настраиваемый верхний и нижний колонтитулы")

<a name="Row_Separators" />

## <a name="row-separators"></a>Разделители строк
Разделитель строк отображаются между `ListView` элементы по умолчанию на iOS и Android. Windows Phone не поддерживает разделитель строк, на этой платформы руководствах. Если вы предпочитаете скрыть линии разделителя в iOS и Android, задать `SeparatorVisibility` свойства в ListView. Параметры для `SeparatorVisibility` являются:

* **По умолчанию** -показана строка разделителя в iOS и Android.
* **Нет** -скрывает разделителя на всех платформах.

Видимость по умолчанию:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView с разделителями строк по умолчанию")

NONE:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView без разделителей строк")

Можно также задать цвет разделителя строки через `SeparatorColor` свойство:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView с разделителями строк зеленый")

> [!NOTE]
> Настройка этих свойств в Android после загрузки `ListView` влечет за собой снижение производительности.

<a name="Row_Heights" />

## <a name="row-heights"></a>Высота строки
По умолчанию все строки в ListView имеют одинаковую высоту. ListView имеет два свойства, которые могут использоваться для изменения этого поведения:

- `HasUnevenRows` &ndash; `true`/`false` значение строки имеют различные значения высоты, если значение `true`. По умолчанию — `false`.
- `RowHeight` &ndash; Задает высоту каждой строки, если `HasUnevenRows` — `false`.

Можно задать высоту всех строк, задав `RowHeight` свойство `ListView`.

### <a name="custom-fixed-row-height"></a>Пользовательские фиксированную высоту строки

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView с фиксированную высоту строки")


### <a name="uneven-rows"></a>Неравномерное строк

При желании отдельных строк для получения различной высоты, можно задать `HasUnevenRows` свойства `true`.
Обратите внимание, что высота строк не нужно вручную задать один раз `HasUnevenRows` ему было присвоено `true`, так как значения высоты будет автоматически вычисляется с помощью Xamarin.Forms.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView с различными строками")

### <a name="runtime-resizing-of-rows"></a>Среда выполнения изменение размеров строк

Отдельные `ListView` строк можно программно изменять во время выполнения, при условии, что `HasUnevenRows` свойству `true`. [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) Метод обновляет размер ячейки, даже в том случае, если он отсутствует в настоящее время, как показано в следующем примере кода:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

`OnImageTapped` Выполняется обработчик событий в ответ на [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) в ячейке касание и увеличивает размер `Image` отображаемая в ячейке, чтобы их легко просмотреть.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView с изменением размеров строки среды выполнения")

Обратите внимание, что надежный вероятность снижения производительности, если эта функция является чрезмерным.



## <a name="related-links"></a>Связанные ссылки

- [Группирование (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Пользовательское представление модуля подготовки отчетов (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Динамическое изменение размера строк (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [заметки о выпуске 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [заметки о выпуске 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
