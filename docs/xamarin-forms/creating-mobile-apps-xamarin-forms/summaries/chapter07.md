---
title: Сводка Глава 7. XAML и код
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E91F387B-CE90-481C-8D90-CB25519BFD2B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 97f1ad1f818c74a294421f223c4cea0123b83373
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-7-xaml-vs-code"></a>Сводка Глава 7. XAML и код

Xamarin.Forms поддерживает язык разметки на основе XML, называемый язык XAML и XAML (произносится как «замл»). XAML представляет собой альтернативу C# при определении макета пользовательского интерфейса Xamarin.Forms приложения и определения привязок между элементами пользовательского интерфейса и исходные данные.

## <a name="properties-and-attributes"></a>Свойства и атрибуты

Xamarin.Forms классы и структуры становятся XML-элементов в XAML, а свойства этих классов и структур XML-атрибуты. Для создания экземпляра в XAML, класс обычно должен иметь открытый конструктор без параметров. Все свойства, заданные в XAML должен иметь открытый `set` методы доступа.

Для свойств базовых типов данных (`string`, `double`, `bool`, и так далее), средство синтаксического анализа XAML использует стандарт `TryParse` методы преобразования в эти типы параметров атрибутов. Средство синтаксического анализа XAML можно также легко обрабатывать типы перечисления и его можно объединять членов перечисления, если тип перечисления помечается `Flags` атрибута.

Чтобы упростить синтаксического анализа XAML, можно включить более сложных типов (или свойства этих типов) [ `TypeConverterAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverterAttribute/) , определяющий класс, производный от [ `TypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TypeConverter/) которого поддерживает преобразование из строковые значения для этих типов. Например [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) цвет преобразует имена и строки, например «#rrggbb» в `Color` значения.

## <a name="property-element-syntax"></a>Синтаксис элемента свойства

В языке XAML классы и объекты, созданные на их основе выражаются в виде XML-элементов. Эти функции известны как *объекта элементы*. Большинство свойств эти объекты представляют собой XML-атрибуты. Это называется *атрибуты свойства*.

Иногда свойство должно быть присвоено объект, который не может быть выражен как простая строка. В этом случае XAML поддерживает тег *элемент свойства* состоит из имени класса и имя свойства, разделенных точкой. Элемент объекта затем может отображаться заключенный в теги элементов свойства.

## <a name="adding-a-xaml-page-to-your-project"></a>Добавление в проект XAML-страницы

Xamarin.Forms переносимой библиотеки классов может содержать XAML-страницы, при ее создании или XAML-страницы можно добавить в существующий проект. В диалоговом окне, чтобы добавить новый элемент, выберите элемент, который ссылается на страницу XAML или `ContentPage` и XAML. (Не `ContentView`.)

Создаются два файла: файл с расширением C# и XAML-файл с расширением имени файла .xaml. xaml.cs. Файл C#, часто называют *кода* в файле XAML. Файл кода является определение разделяемого класса, производного от `ContentPage`. Во время построения синтаксического анализа XAML и другое определение разделяемого класса создается для того же класса. Этот созданный класс содержит метод с именем `InitializeComponent` , вызывается из конструктора файл кода.

Во время выполнения, в конце `InitializeComponent` вызвать, все элементы XAML-файл был создан и инициализирован, как если бы они созданы в коде C#.

Корневым элементом в XAML-файле является `ContentPage`. Корневой тег содержит по крайней мере два объявления пространств имен XML, один для элементов Xamarin.Forms и других определяющего `x` префикс для элементов и атрибутов, встроенными в реализациях XAML. Также содержит корневой тег `x:Class` атрибут, который указывает пространство имен и имя класса, производного от `ContentPage`. Это соответствует имени пространства имен и класса в файле кода.

Сочетание XAML и кода демонстрируется путем [ **CodePlusXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07) образца.

## <a name="the-xaml-compiler"></a>Компилятор XAML

Xamarin.Forms имеет компилятора XAML, но это необязательный параметр в зависимости от использования [ `XamlCompilationAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/). Если XAML не компилируется, синтаксического анализа XAML во время построения и XAML-файл внедрен в PCL, где он также проанализирован во время выполнения. При компиляции XAML, процесс построения XAML преобразуется в двоичной форме и выполнения обработки является более эффективным.

## <a name="platform-specificity-in-the-xaml-file"></a>Платформа специфичность в XAML-файле

В языке XAML [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) класс может использоваться для выбора разметки зависят от платформы. Это относится к универсальному классу и должен создаваться с `x:TypeArguments` атрибут, соответствующий целевой тип. [ `OnIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnIdiom%3CT%3E/) Класс похожие, но используется гораздо реже.

Использование `OnPlatform` изменился с момента публикации книги. Изначально он использовался в сочетании с свойства с именем `iOS`, `Android`, и `WinPhone`. Теперь используется с дочерним [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) объектов. Задать [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) строковое значение соответствует открытые `const` поля [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) класса. Задать [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Value/) значение не согласуется с `x:TypeArguments` атрибут `OnPlatform` тег.

`OnPlatform` демонстрируется [ **ScaryColorList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/ScaryColorList) образца, поэтому вызов, так как он содержит блоки практически одинаковых XAML. Существование этого повторы разметки предполагает, что методы должны быть доступны для уменьшения его.

## <a name="the-content-property-attributes"></a>Атрибуты свойства содержимого

Некоторые элементы свойств выполняется довольно часто, например `<ContentPage.Content>` тега корневой элемент `ContentPage`, или `<StackLayout.Children>` тег, который содержит дочерние элементы `StackLayout`.

Каждый класс может определить одно свойство с [ `ContentPropertyAttribute` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPropertyAttribute/) в классе. Для этого свойства теги элементов свойства не являются обязательными. `ContentPage` Определяет свойство содержимого в `Content`, и `Layout<T>` (класс, от которого `StackLayout` производный) определяет свойство содержимого в `Children`. Эти теги элементов свойства не являются обязательными.

Свойство элемента `Label` — `Text`.

## <a name="formatted-text"></a>Форматированный текст

[ **TextVariations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/TextVariations) образец содержит несколько примеров параметра `Text` и `FormattedText` свойства `Label`. В языке XAML `Span` объектов отображаются как дочерние элементы `FormattedString` объекта.

 Если значение многострочной строке `Text` свойства, символы конца строки преобразуются в пробелы, но символы конца строки сохраняются в том случае, когда многострочной строке отображается как содержимое `Label` или `Label.Text` теги:

 [![Тройной снимок экрана: текст варианты совместного использования](images/ch07fg03-small.png "варианты текст в формате")](images/ch07fg03-large.png#lightbox "вариации в формате текста")



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 7 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf)
- [Образцы Глава 7](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07)
- [Образец главы 7 F #](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter07/FS/CodePlusXaml)
