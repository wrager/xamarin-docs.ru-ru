---
title: Задание стиля Xamarin.Forms приложений, с помощью каскадных таблиц стилей (CSS)
description: Xamarin.Forms поддерживает стили визуальных элементов с помощью каскадных таблиц стилей (CSS).
ms.prod: xamarin
ms.assetid: C89D57A6-DAB9-4C42-963F-26D67627DDC2
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 76ca67f7ac8a8e27e5f502455d48874c775fc172
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794089"
---
# <a name="styling-xamarinforms-apps-using-cascading-style-sheets-css"></a>Задание стиля Xamarin.Forms приложений, с помощью каскадных таблиц стилей (CSS)

_Xamarin.Forms поддерживает стили визуальных элементов с помощью каскадных таблиц стилей (CSS)._

Xamarin.Forms 3.0 появилась возможность оформить приложение с помощью CSS. Таблица стилей содержит список правил, состоящий из одного или нескольких селекторы и блок объявления. Блок объявления состоит из списка объявлений в фигурные скобки, с каждого объявления, состоящий из свойства, двоеточие и значение. Если существует несколько объявлений в блоке, точкой с запятой вставляется в качестве разделителя. В следующем примере кода показаны некоторые Xamarin.Forms совместимые CSS:

```css
^contentpage {
    background-color: lightgray;
}

#listView {
    background-color: lightgray;
}

stacklayout {
    margin: 20;
}

.mainPageTitle {
    font-style: bold;
    font-size: medium;
}

.mainPageSubtitle {
    margin-top: 15;
}

.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}

listview image {
    height: 60;
    width: 60;
}

stacklayout>image {
    height: 200;
    width: 200;
}
```

В Xamarin.Forms анализируются и вычисляется во время выполнения, а не время компиляции таблицы стилей CSS и повторного анализа, при использовании таблицы стилей.

> [!NOTE]
> В настоящее время все стили, возможно с помощью стилей XAML не может выполняться с помощью CSS. Тем не менее стили XAML можно использовать для дополнения CSS для свойств, которые в настоящее время не поддерживаются в Xamarin.Forms. Дополнительные сведения о стилях XAML см. в разделе [стиля Xamarin.Forms приложения, использующие XAML стили](~/xamarin-forms/user-interface/styles/xaml/index.md).

[MonkeyAppCSS](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/) образец демонстрирует использование CSS для стиля простое приложение и показано на следующих снимках экрана:

[![MonkeyApp главной страницы с Дизайн CSS](css-images/MonkeyAppMainPage.png "MonkeyApp главной страницы с Дизайн CSS")](css-images/MonkeyAppMainPage-Large.png#lightbox "MonkeyApp главной страницы с Дизайн CSS")

[![Страницы сведений о MonkeyApp с Дизайн CSS](css-images/MonkeyAppDetailPage.png "MonkeyApp страницы сведений с Дизайн CSS")](css-images/MonkeyAppDetailPage-Large.png#lightbox "MonkeyApp страницы сведений с Дизайн CSS")

> [!NOTE]
> В настоящее время невозможно для стиля цвет фона [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) с помощью таблицы стилей. Таким образом, в образце приложения [ `NavigationPage.BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) задано в коде.

## <a name="consuming-a-style-sheet"></a>Использование таблицы стилей

При добавлении таблицы стилей в решение выполняется следующим образом:

1. Добавьте пустой файл CSS для проекта библиотеки .NET Standard.
1. Задайте действие построения CSS-файл для **EmbeddedResource**.

### <a name="loading-a-style-sheet"></a>Загрузка таблицы стилей

Существует несколько подходов, которые могут использоваться для загрузки таблицы стилей.

### <a name="xaml"></a>XAML

Таблица стилей может загружаться и проанализирован с [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) класса перед добавлением [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) страницы:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    ...
</ContentPage>
```

[ `StyleSheet.Source` ](xref:Xamarin.Forms.Xaml.StyleSheetExtension.Source) Свойство указывает таблицу стилей как URI относительно расположения внешнего файла XAML или относительно корневого каталога проекта, если URI начинается с `/`.

> [!WARNING]
> CSS-файл не удастся загрузить, если это не задано действие построения **EmbeddedResource**.

В качестве альтернативы можно загрузки и анализа с помощью таблицы стилей [ `StyleSheet` ](xref:Xamarin.Forms.StyleSheets.StyleSheet) класса путем встраивания в `CDATA` раздела:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet>
            <![CDATA[
            ^contentpage {
                background-color: lightgray;
            }
            ]]>
        </StyleSheet>
    </ContentPage.Resources>
    ...
</ContentPage>
```

### <a name="c"></a>C#

В C# можно загрузить в качестве внедренного ресурса и добавить таблицу стилей [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) страницы:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        this.Resources.Add(StyleSheet.FromAssemblyResource(
            IntrospectionExtensions.GetTypeInfo(typeof(MyPage)).Assembly,
            "MyProject.Assets.styles.css"));
    }
}
```

Первый аргумент `StyleSheet.FromAssemblyResource` метод — сборка, содержащая таблицы стилей, а второй аргумент — `string` , представляющим собой идентификатор ресурса. Идентификатор ресурса можно получить из **свойства** окно при выборе в CSS-файл.

Кроме того, таблицы стилей, можно загрузить из `StringReader` и добавлены [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) страницы:

```csharp
public partial class MyPage : ContentPage
{
    public MyPage()
    {
        InitializeComponent();

        using (var reader = new StringReader("^contentpage { background-color: lightgray; }"))
        {
            this.Resources.Add(StyleSheet.FromReader(reader));
        }
    }
}
```

Аргумент `StyleSheet.FromReader` метод `TextReader` , прочитала таблицы стилей.

## <a name="selecting-elements-and-applying-properties"></a>Выбор элементов и применение свойств

CSS использует селекторы, чтобы определить, какие элементы целевой. Стили с совпадающими селекторы применяются последовательно, в порядке определения. Стили, определенные на конкретном элементе всегда применен последним. Дополнительные сведения о поддерживаемых селекторы см. в разделе [ссылку на селектор](#selector-reference).

Для выбранного элемента в стиле CSS используются свойства. Каждое свойство имеет набор возможных значений, и некоторые свойства могут влиять на любой тип элемента, а другие относятся только к группам элементов. Дополнительные сведения о поддерживаемых свойств см. в разделе [ссылку на свойство](#property-reference).

### <a name="selecting-elements-by-type"></a>Выбор элементов по типу

Можно выбрать элементы в визуальном дереве типом, без учета регистра с `element` выбора:

```css
stacklayout {
    margin: 20;
}
```

Идентифицирует этот селектор любой [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) элементы на страницах использовать таблицы стилей, которые устанавливаются их поля для единая толщина 20.

> [!NOTE]
> `element` Селектор не может определить вложенные классы заданного типа.

### <a name="selecting-elements-by-base-class"></a>При выборе элементов в базовом классе

Можно выбрать элементы в визуальном дереве базовым классом, без учета регистра с `^base` выбора:

```css
^contentpage {
    background-color: lightgray;
}
```

Идентифицирует этот селектор любой [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) элементов, которые используют таблицы стилей и задает фоновый цвет `lightgray`.

> [!NOTE]
> `^base` Селектор относится к Xamarin.Forms и не является частью спецификации CSS.

### <a name="selecting-an-element-by-name"></a>Выбор элемента по имени

С учетом регистра можно выбрать отдельные элементы в визуальном дереве `#id` выбора:

```css
#listView {
    background-color: lightgray;
}
```

Этот селектор идентифицирует элемент которого [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) свойству `listView`. Однако если `StyleId` свойство не задано, селектор будет переключиться на использование `x:Name` элемента. Таким образом, в следующем примере XAML `#listView` определят селектор [ `ListView` ](xref:Xamarin.Forms.ListView) которого `x:Name` атрибута задано значение `listView`и будет задан цвет фона `lightgray`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView x:Name="listView" ...>
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

### <a name="selecting-elements-with-a-specific-class-attribute"></a>При выборе элементов с атрибутом определенного класса

Можно выбрать элементы с атрибутом определенного класса с учетом регистра `.class` выбора:

```css
.detailPageTitle {
    font-style: bold;
    font-size: medium;
    text-align: center;
}

.detailPageSubtitle {
    text-align: center;
    font-style: italic;
}
```

Класс CSS можно назначить элемент XAML, задав [ `StyleClass` ](xref:Xamarin.Forms.VisualElement.StyleClass) свойства элемента в имя класса CSS. Таким образом, в следующем примере XAML стили определяется `.detailPageTitle` класс назначаются первому [ `Label` ](xref:Xamarin.Forms.Label), а стили, определенные по `.detailPageSubtitle` класс назначаются второй `Label`.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            <Label ... StyleClass="detailPageTitle" />
            <Label ... StyleClass="detailPageSubtitle"/>
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

### <a name="selecting-child-elements"></a>При выборе дочерние элементы

Можно выбрать дочерние элементы в визуальном дереве с без учета регистра `element element` выбора:

```css
listview image {
    height: 60;
    width: 60;
}
```

Идентифицирует этот селектор любой [ `Image` ](xref:Xamarin.Forms.Image) элементы, которые являются потомками [ `ListView` ](xref:Xamarin.Forms.ListView) элементов и устанавливает их высота и ширина до 60. Таким образом, в следующем примере XAML `listview image` определят селектор [ `Image` ](xref:Xamarin.Forms.Image) , является дочерним элементом [ `ListView` ](xref:Xamarin.Forms.ListView)и задает его высота и ширина до 60.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <StackLayout>
        <ListView ...>
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <Grid>
                            ...
                            <Image ... />
                            ...
                        </Grid>
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> `element element` Селектор не требуется дочерний элемент, чтобы быть _прямой_ дочерним — дочерний элемент, возможно, другим родительским элементам. Выбор выполняется при условии, что указанного первый элемент является предком.

### <a name="selecting-direct-child-elements"></a>При выборе прямыми дочерними элементами

Прямое с без учета регистра можно выбрать дочерние элементы в визуальном дереве `element>element` выбора:

```css
stacklayout>image {
    height: 200;
    width: 200;
}
```

Идентифицирует этот селектор любой [ `Image` ](xref:Xamarin.Forms.Image) элементы, которые являются прямыми потомками [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) элементов и устанавливает их высота и ширина до 200. Таким образом, в следующем примере XAML `stacklayout>image` определят селектор [ `Image` ](xref:Xamarin.Forms.Image) , является прямым потомком [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)и присваивает его высота и ширина 200.

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <StyleSheet Source="/Assets/styles.css" />
    </ContentPage.Resources>
    <ScrollView>
        <StackLayout>
            ...
            <Image ... />
            ...
        </StackLayout>
    </ScrollView>
</ContentPage>
```

> [!NOTE]
> `element>element` Селектор требует дочерний элемент _прямой_ потомка родителя.

## <a name="selector-reference"></a>Селектор ссылки

Поддерживает следующие селекторов CSS Xamarin.Forms.

|Селектор|Пример|Описание:|
|---|---|---|
|`.class`|`.header`|Выбирает все элементы с `StyleClass` свойство, содержащее «заголовок». Обратите внимание, что этот селектор с учетом регистра.|
|`#id`|`#email`|Выбирает все элементы с `StyleId` значение `email`. Если `StyleId` не задано, возврат к `x:Name`. При использовании XAML, `x:Name` предпочтительнее, чем `StyleId`. Обратите внимание, что этот селектор с учетом регистра.|
|`*`|`*`|Выбирает все элементы.|
|`element`|`label`|Выбирает все элементы типа `Label`, но не вложенной классов. Обратите внимание, что этот селектор без учета регистра.|
|`^base`|`^contentpage`|Выбирает все элементы с `ContentPage` в качестве базового класса, включая `ContentPage` сам. Обратите внимание, что этот селектор не учитывает регистр и не является частью спецификации CSS.|
|`element,element`|`label,button`|Выбирает все `Button` элементы и все `Label` элементы. Обратите внимание, что этот селектор без учета регистра.|
|`element element`|`stacklayout label`|Выбирает все `Label` элементов внутри `StackLayout`. Обратите внимание, что этот селектор без учета регистра.|
|`element>element`|`stacklayout>label`|Выбирает все `Label` элементы с `StackLayout` как прямым родительским элементом. Обратите внимание, что этот селектор без учета регистра.|
|`element+element`|`label+entry`|Выбирает все `Entry` элементы, непосредственно следующие за `Label`. Обратите внимание, что этот селектор без учета регистра.|
|`element~element`|`label~entry`|Выбирает все `Entry` предшествует элементы `Label`. Обратите внимание, что этот селектор без учета регистра.|

Стили с совпадающими селекторы применяются последовательно, в порядке определения. Стили, определенные на конкретном элементе всегда применен последним.

> [!TIP]
> Селекторы могут быть объединены без ограничений, таких как `StackLayout>ContentView>label.email`.

Следующие селекторы в настоящий момент не поддерживаются:

- `[attribute]`
- `@media` и `@supports`.
- `:` и `::`.

> [!NOTE]
> Подробны и спецификой переопределения не поддерживаются.

## <a name="property-reference"></a>Справочник по свойствам

Поддерживаются следующие свойства CSS Xamarin.Forms (в **значения** столбцов, типы являются _курсивом_, если строковые литералы являются `gray`):

|Свойство.|Применение|Значения|Пример|
|---|---|---|---|
|`background-color`|`VisualElement`|_Цвет_ \| `initial` |`background-color: springgreen;`|
|`background-image`|`Page`|_Строка_ \| `initial` |`background-image: bg.png;`|
|`border-color`|`Button`, `Frame`|_Цвет_ \| `initial`|`border-color: #9acd32;`|
|`border-width`|`Button`|_Double_ \| `initial` |`border-width: .5;`|
|`color`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`|_Цвет_ \| `initial` |`color: rgba(255, 0, 0, 0.3);`|
|`direction`|`VisualElement`|`ltr` \| `rtl` \| `inherit` \| `initial` |`direction: rtl;`|
|`font-family`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_Строка_ \| `initial` |`font-family: Consolas;`|
|`font-size`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|_двойные_ \| _namedsize_ \| `initial` |`font-size: 12;`|
|`font-style`|`Button`, `DatePicker`, `Editor`, `Entry`, `Label`, `Picker`, `SearchBar`, `TimePicker`, `Span`|`bold` \| `italic` \| `initial` |`font-style: bold;`|
|`height`|`VisualElement`|_Double_ \| `initial` |`min-height: 250;`|
|`margin`|`View`|_Толщина_ \| `initial` |`margin: 6 12;`|
|`margin-left`|`View`|_Толщина_ \| `initial` |`margin-left: 3;`|
|`margin-top`|`View`|_Толщина_ \| `initial` |`margin-top: 2;`|
|`margin-right`|`View`|_Толщина_ \| `initial` |`margin-right: 1;`|
|`margin-bottom`|`View`|_Толщина_ \| `initial` |`margin-bottom: 6;`|
|`min-height`|`VisualElement`|_Double_ \| `initial` |`min-height: 50;`|
|`min-width`|`VisualElement`|_Double_ \| `initial` |`min-width: 112;`|
|`opacity`|`VisualElement`|_Double_ \| `initial` |`opacity: .3;`|
|`padding`|`Layout`, `Page`|_Толщина_ \| `initial` |`padding: 6 12 12;`|
|`padding-left`|`Layout`, `Page`|_Double_ \| `initial`|`padding-left: 3;`|
|`padding-top`|`Layout`, `Page`| _Double_ \| `initial` |`padding-top: 4;`|
|`padding-right`|`Layout`, `Page`| _Double_ \| `initial` |`padding-right: 2;`|
|`padding-bottom`|`Layout`, `Page`| _Double_ \| `initial` |`padding-bottom: 6;`|
|`text-align`| `Entry`, `EntryCell`, `Label`, `SearchBar`|`left` \| `right` \| `center` \| `start` \| `end` \| `initial`. `left` и `right` следует избегать в средах справа налево.| `text-align: right;`|
|`visibility`|`VisualElement`|`true` \| `visible` \| `false` \| `hidden` \| `collapse` \| `initial `|`visibility: hidden;`|
|`width`|`VisualElement`|_Double_ \| `initial`|`min-width: 320;`|

> [!NOTE]
> `initial` является допустимым значением для всех свойств. Очищается значение (сбрасывает значения по умолчанию), что был выбран из другого стиля.

В настоящее время поддерживаются следующие свойства:

- `all: initial`.
- Свойства макета (поле или сетки).
- Свойства, такие как `font`, и `border`.

Кроме того, не `inherit` наследования значения и поэтому не поддерживается. Поэтому нельзя например, задать `font-size` свойство в макете и ожидать, что все [ `Label` ](xref:Xamarin.Forms.Label) экземпляров в макете наследование значения. Единственное исключение — `direction` свойства, которое имеет значение по умолчанию из `inherit`.

### <a name="color"></a>Цвет

Следующие `color` поддерживаются значения:

- `X11` [цвета](https://en.wikipedia.org/wiki/X11_color_names/), который соответствует CSS цвета, предопределенные цвета UWP и Xamarin.Forms цвета. Обратите внимание, что эти значения цвета без учета регистра.
- шестнадцатеричный цветов: `#rgb`, `#argb`, `#rrggbb`, `#aarrggbb`
- цвета RGB: `rgb(255,0,0)`, `rgb(100%,0%,0%)`. Значения находятся в диапазоне 0 – 255, или 0-100%.
- цвета RGBA: `rgba(255, 0, 0, 0.8)`, `rgba(100%, 0%, 0%, 0.8)`. Значение непрозрачности находится в диапазоне 0.0-1.0.
- цвета HSL: `hsl(120, 100%, 50%)`. H значение находится в диапазоне от 0 до 360, хотя s и l в диапазоне 0-100%.
- цвета hsla: `hsla(120, 100%, 50%, .8)`. Значение непрозрачности находится в диапазоне 0.0-1.0.

### <a name="thickness"></a>Thickness

Одного, двух, трех или четырех `thickness` поддерживаются значений, отделенных друг от друга пробелами:

- Одно значение указывает универсальный толщину.
- Два значения указывают вертикальной и горизонтальной толщину.
- Три значения указывают на начало, затем по горизонтали (слева и справа), а затем толщина нижней.
- Четыре значения указывают сверху, а затем вправо, а затем вниз, а затем толщину левой.

> [!NOTE]
> CSS `thickness` значения отличаются от XAML [ `Thickness` ](/api/type/Xamarin.Forms.Thickness/) значения. Например, в XAML двух значений — `Thickness` указывающее толщину горизонтальной и вертикальной, то время как значение четырех `Thickness` указывает влево, затем сверху, а затем справа, снизу толщину. Кроме того, XAML `Thickness` значения: с разделителями-запятыми.

### <a name="namedsize"></a>NamedSize

Без учета регистра следующие `namedsize` поддерживаются значения:

- `default`
- `micro`
- `small`
- `medium`
- `large`

Точное значение каждого `namedsize` значение, зависящее от платформы и зависящие от представления.

## <a name="css-in-xamarinforms-with-xamarinuniversity"></a>CSS в Xamarin.Forms с Xamarin.University

> [!VIDEO https://youtube.com/embed/va-Vb7vtan8]

**Xamarin.Forms 3.0 CSS, по [университета Xamarin](https://university.xamarin.com/)**

## <a name="related-links"></a>Связанные ссылки

- [MonkeyAppCSS (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/MonkeyAppCSS/)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
