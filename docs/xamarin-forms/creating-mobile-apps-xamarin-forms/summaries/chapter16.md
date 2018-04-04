---
title: Сводка Глава 16. привязка данных,
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: ED997DB0-C229-4868-A5FB-928703B377D6
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0f200e0c482402813ac7051255dd7c27da93d6dc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-16-data-binding"></a>Сводка Глава 16. привязка данных,

Программистов часто оказываются написание обработчиков событий, которые позволяют обнаружить при изменении свойства одного объекта и использовать, чтобы изменить значение свойства другого объекта. Этот процесс можно автоматизировать при помощи метод *привязки данных*. Данные привязки обычно определяется на языке XAML и становятся частью определения пользовательского интерфейса.

Очень часто эти привязки данных подключения объекты пользовательского интерфейса в базовых данных. Это метод, который является более изучены в [ **Глава 18. MVVM**](chapter18.md). Тем не менее привязки данных можно также подключиться два или более элементов пользовательского интерфейса. В большинстве примеров раннего связывания данных в этой главе демонстрации этого метода.

## <a name="binding-basics"></a>Основные принципы привязки

Некоторые свойства, методы и классы участвуют в привязке данных.

- [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/) Класс является производным от [ `BindingBase` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingBase/) и инкапсулирует многие характеристики привязки данных
- [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Определяется свойство [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) класса
- [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Также определяется метод [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) класса
- [ `BindableObjectExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObjectExtensions/) Класс определяет три дополнительных `SetBinding` методы

Следующие два класса поддержки расширения разметки XAML для привязки:

- [`BindingExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.BindingExtension/) поддерживает `Binding` расширения разметки
- [`ReferenceExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.ReferenceExtension/) поддерживает `x:Reference` расширения разметки

В процессе привязки данных участвуют два интерфейса:

- [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) в `System.ComponentModel` пространство имен — для реализации уведомлений при изменении свойства
- [`IValueConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) используется для определения небольших классов, преобразовывать значения из одного типа в другой привязки данных

Привязка данных соединяет два свойства и тот же объект или (обычно) двумя разными объектами. Эти свойства называются *источника* и *целевой*. Как правило изменения в исходном свойстве приводит к изменению в целевое свойство, но иногда обратным. В любом случае:

- *целевой* свойства должны быть основаны на [`BindableProperty`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)
- *источника* свойства обычно является членом класса, реализующего [`INotifyPropertyChanged`](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)

Класс, реализующий `INotifyPropertyChanged` активируется [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/) событие при изменении значения свойства. `BindableObject` реализует `INotifyPropertyChanged` и автоматически запускает `PropertyChanged` событие, когда свойство `BindableProperty` изменяет значения, но можно написать собственные классы, реализующие `INotifyPropertyChanged` не на основе `BindableObject`.

## <a name="code-and-xaml"></a>Код и XAML

[ **OpacityBindingCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingCode) образце демонстрируется задание привязки данных в коде:

- Источником является `Value` свойство `Slider`
- Целевой объект — `Opacity` свойство `Label`

Два объекта связаны `BindingContext` из `Label` объект `Slider` объекта. Эти два свойства подключены путем вызова [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObjectExtensions.SetBinding/p/Xamarin.Forms.BindableObject/Xamarin.Forms.BindableProperty/System.String/) метод расширения в `Label` ссылки на `OpacityProperty` привязываемые свойства и `Value` свойство `Slider` выраженное Строка.

Управление `Slider` вызывает `Label` для исчезновения и появления.

[ **OpacityBindingXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/OpacityBindingXaml) является той же программе, с использованием привязки данных, заданное в XAML. `BindingContext` Из `Label` равно `x:Reference` ссылающиеся на расширения разметки `Slider`и `Opacity` свойство `Label` равно `Binding` расширения разметки с его [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) ссылки на свойство `Value` свойство `Slider`.

## <a name="source-and-bindingcontext"></a>Источник и BindingContext

[ **BindingSourceCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceCode) пример альтернативный подход в коде. Объект `Binding` объекта создается путем задания [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) свойства `Slider` объекта и [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) свойство «Значение». [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) Метод `BindableObject` затем вызывается на `Label` объекта.

[ `Binding` Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Binding.Binding/p/System.String/Xamarin.Forms.BindingMode/Xamarin.Forms.IValueConverter/System.Object/System.String/System.Object/) можно также было определение `Binding` объекта.

[ **BindingSourceXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingSourceXaml) сопоставимых метод приведен пример на языке XAML. `Opacity` Свойство `Label` равно `Binding` расширения разметки с [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) значение `Value` свойство и [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Source/) значение внедренные `x:Reference` расширения разметки.

Таким образом для ссылки на объект источника привязки двумя способами.

- Через `BindingContext` свойства целевого объекта
- Через `Source` свойство `Binding` сам объект

Если указаны оба аргумента, второй имеет приоритет. Преимущество `BindingContext` — это распространяется по визуальному дереву. Это *очень* под рукой, один и тот же объект источника связаны несколько свойств целевого объекта.

[ **WebViewDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WebViewDemo) программе демонстрируется этот способ с [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) элемента. Два `Button` элементы для перемещения назад и вперед наследуют `BindingContext` из их родительского элемента, который ссылается на `WebView`. `IsEnabled` Свойства две кнопки затем иметь простой `Binding` расширения разметки, предназначенных для кнопки `IsEnabled` свойства на основе параметров [ `CanGoBack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoBack/) и [ `CanGoForward` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.CanGoForward/) только для чтения свойства `WebView`.

## <a name="the-binding-mode"></a>Режим привязки

Задать [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) свойство `Binding` на член [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) перечисления:

- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) Чтобы изменения в исходном свойстве влияют на целевой объект
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) Чтобы изменения в свойстве target влияет на источник
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) Чтобы изменения в исходной и целевой влияют друг на друга
- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) для использования [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) , заданный при целевой `BindableProperty` был создан. Если ничего не было указано, значение по умолчанию — `OneWay` для обычного свойства для привязки и `OneWayToSource` для привязываемые свойства только для чтения.

Свойства, которые могут быть целевыми для привязки данных в сценариях MVVM обычно имеют `DefaultBindingMode` из `TwoWay`. Эти особые значения приведены ниже.

- `Value` Свойство `Slider` и `Stepper`
- `IsToggled` Свойство `Switch`
- `Text` Свойство `Entry`, `Editor`, и `SearchBar`
- `Date` Свойство `DatePicker`
- `Time` Свойство `TimePicker`

[ **BindingModes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingModes) образец демонстрирует режимах четыре привязки для привязки данных, где целевым объектом является `FontSize` свойство `Label` и источником является `Value` Свойство `Slider`. Это позволит каждой `Slider` для управления размером шрифта соответствующего `Label`. Но `Slider` элементы не инициализирован, поскольку `DefaultBindingMode` из `FontSize` свойство `OneWay`.

[ **ReverseBinding** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ReverseBinding) пример задает привязки на `Value` свойство `Slider` ссылки на `FontSize` каждого экземпляра `Label`. Это кажется вперед, но лучше работает в инициализации `Slider` элементы из-за `Value` свойство `Slider` имеет `DefaultBindingMode` из `TwoWay`.

[![Тройной экрана обратного привязки](images/ch16fg06-small.png "отменить привязку")](images/ch16fg06-large.png#lightbox "отменить привязку")

Это аналогично определение привязки в MVVM и используйте этот тип привязки, часто.

## <a name="string-formatting"></a>Строка форматирования

Если целевое свойство имеет тип `string`, можно использовать [ `StringFormat` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.StringFormat/) свойством, которое определяется `BindingBase` для преобразования источника в `string`. Задать `StringFormat` свойств для форматирования строки, который нужно использовать вместе со статическими .NET [ `String.Format` ](https://developer.xamarin.com/api/member/System.String.Format/p/System.String/System.Object/) формат для отображения объекта. При использовании данной строки форматирования внутри расширения разметки, заключите его в одинарные кавычки, фигурные скобки не будет ошибочно для расширения разметки embedded.

[ **ShowViewValues** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ShowViewValues) образце показано, как использовать `StringFormat` в XAML.

[ **WhatSizeBindings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/WhatSizeBindings) примере демонстрируется отображение размер страницы с привязками к `Width` и `Height` свойства `ContentPage`.

## <a name="why-is-it-called-path"></a>Почему он называется «Путь»?

[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) Свойство `Binding` поэтому вызов, так как он может быть ряд свойств и индексаторов, разделенных точками. [ **BindingPathDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/BindingPathDemos) образце показано несколько примеров.

## <a name="binding-value-converters"></a>Преобразователи значений привязки

Если исходного и целевого свойства привязки являются разными типами, можно будет преобразовать между типами, используя преобразователь привязки. Это класс, реализующий [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) интерфейса и содержит два метода: [ `Convert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.Convert/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) для преобразования источника в целевой объект и [ `ConvertBack` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IValueConverter.ConvertBack/p/System.Object/System.Type/System.Object/System.Globalization.CultureInfo/) для преобразования целевой объект в источнике.

[ `IntToBoolConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IntToBoolConverter.cs) Класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки приведен пример преобразования `int` для `bool`. Демонстрируется путем [ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/ButtonEnabler) образец, который позволяет `Button` Если хотя бы один символ было введено в `Entry`.

[ `BoolToStringConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToStringConverter.cs) Класса преобразует `bool` для `string` и определяет два свойства, чтобы указать, какой текст должны быть возвращены для `false` и `true` значения.
[ `BoolToColorConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToColorConverter.cs) Аналогично. [ **SwitchText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/SwitchText) образце показано использование этих двух преобразователи для отображения разных текстов в разные цвета на основе `Switch` параметр.

Универсальный [ `BoolToObjectConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BoolToObjectConverter.cs) можно заменить `BoolToStringConverter` и `BoolToColorConverter` и служить обобщенный `bool`-к-преобразователь любого типа объекта.

## <a name="bindings-and-custom-views"></a>Привязки и пользовательские представления

Можно упростить пользовательские элементы управления с помощью привязки данных. [ `NewCheckBox.cs` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml.cs) Файл кода определяет `Text`, `TextColor`, `FontSize`, `FontAttributes`, и `IsChecked` свойства, но не имеет логики для визуальных элементов управления.
Вместо этого [ `NewCheckBox.cs.xaml` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NewCheckBox.xaml) файл содержит все разметки для элемента управления визуальных элементов через привязки данных на `Label` элементов на основе свойств, определенные в файле кода.

[ **NewCheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16/NewCheckBoxDemo) образец демонстрирует `NewCheckBox` пользовательского элемента управления.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 16 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch16-Apr2016.pdf)
- [Глава 16 образцы](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter16)
