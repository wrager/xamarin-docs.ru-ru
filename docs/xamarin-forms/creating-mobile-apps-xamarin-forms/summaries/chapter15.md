---
title: "Сводка по 15 главы. Интерактивный интерфейс"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: e6c61f9a6ba66db2b9a5c7b217c7da952607e709
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-15-the-interactive-interface"></a>Сводка по 15 главы. Интерактивный интерфейс

В этой главе изучает восьми `View` производные продукты, которые допускают взаимодействие с пользователем.

## <a name="view-overview"></a>Просмотр общих сведений

Xamarin.Forms содержит 20 допускающих создание экземпляров классов, производных от `View` , но не `Layout`. Шесть из них были описаны в предыдущих главах:

- `Label`: [ **Глава 2. Составляющие приложение**](chapter02.md)
- `BoxView`: [ **Глава 3. Прокрутка стека**](chapter03.md)
- `Button`: [ **Глава 6. Нажатие кнопки**](chapter06.md)
- `Image`: [ **Главе 13. Bitmaps**](chapter13.md)
- `ActivityIndicator`: [ **Главе 13. Bitmaps**](chapter13.md)
- `ProgressBar`: [ **Глава 14. AbsoluteLayout**](chapter14.md)

Пользователи восемь представлений в этой главе эффективно взаимодействовать с базовых типов данных .NET:

<table>
  <tr>
    <th>Тип данных</th>
    <th>Представления</th>
  </tr>
  <tr>
    <td>`Double`</td>
    <td
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a></code>,
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></code>
    </td>
  </tr>
  <tr>
    <td>`Boolean`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a></code>
    </td>
  </tr>
  <tr>
    <td>`String`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></code>
    </td>
  </tr>
  <tr>
    <td>`DateTime`</td>
    <td>
      <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></code>, <code><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></code>
    </td>
  </tr>
</table>

Эти представления можно считать интерактивные визуальные представления базовые типы данных. Эта концепция является более рассмотренных в следующей главе, [ **Глава 16. Привязка данных**](chapter16.md).

Оставшиеся шесть представления рассматриваются в следующих главах:

- `WebView`: [ **Глава 16. Привязка данных**](chapter16.md)
- `Picker`: [ **Глава 19. Представления коллекций**](chapter19.md)
- `ListView`: [ **Глава 19. Представления коллекций**](chapter19.md)
- `TableView`: [ **Глава 19. Представления коллекций**](chapter19.md)
- `Map`: [ **Глава 28. Расположение и карты**](chapter28.md)
- `OpenGLView`: Не описанные в этой книге (и не поддерживается для платформ Windows)

## <a name="slider-and-stepper"></a>Ползунок и пошаговым

Оба [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) и [ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) разрешить пользователю выбирать числовое значение из диапазона. `Slider` Является непрерывного диапазона, а `Stepper` включает в себя дискретные значения.

### <a name="slider-basics"></a>Основы управления "ползунок"

[ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) Является горизонтальной линией представляет диапазон значений от минимального слева на более справа. Он определяет три открытых свойства:

- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) Тип `double`, значение 0 по умолчанию
- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) Тип `double`, значение 0 по умолчанию
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) Тип `double`, значение 1 по умолчанию

Привязываемые свойства, которые поддерживают эти свойства убедитесь, что они согласованы:

- Для всех трех свойств [ `coerceValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+CoerceValueDelegate/) метод, указанный для привязки свойства гарантирует, что `Value` между `Minimum` и `Maximum`.
- [ `validateValue` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty+ValidateValueDelegate/) Метод `MinimumProperty` возвращает `false` Если `Minimum` задано значение больше или равно `Maximum`и действует для `MaximumProperty`. Возвращение `false` из `validateValue` метода заставляет `ArgumentException` вызываемого.

`Slider` активируется [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) событие с [ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) аргумента при `Value` изменения свойств программным путем или когда пользователь управляет `Slider`.

[ **SliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SliderDemo) образце показано простое использование функции `Slider`.

### <a name="common-pitfalls"></a>Распространенные проблемы

В коде и в XAML `Minimum` и `Maximum` свойства задаются в заданном порядке. Убедитесь, что для инициализации этих свойств, чтобы `Maximum` был всегда больше, чем `Minimum`. В противном случае возникнет исключение.

Инициализация `Slider` свойства может привести к `Value` изменение свойства и `ValueChanged` событие. Следует убедиться, что `Slider` обработчик событий не доступа к представлениям, которые еще не были созданы во время инициализации страницы.

`ValueChanged` Не запускает событие во время `Slider` инициализации Если `Value` изменения свойств. Можно вызвать `ValueChanged` обработчик напрямую из кода.

### <a name="slider-color-selection"></a>Выбор цвета "ползунок"

[ **RgbSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/RgbSliders) программа содержит три `Slider` элементы, которые дают возможность интерактивно выберите цвет, указав его значения RGB:

[![Тройной экрана ползунки R G B](images/ch15fg03-small.png "ползунки RGB")](images/ch15fg03-large.png "RGB ползунки")

[ **TextFade** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/TextFade) в образце используется две `Slider` элементов для перемещения двух `Label` по `AbsoluteLayout` и затухание один в другой.

### <a name="the-stepper-difference"></a>Разница пошаговым

[ `Stepper` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Определяет свойства и события как `Slider` , но `Maximum` свойство устанавливается равным 100 и `Stepper` определяет четвертое свойство:

- [`Increment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Stepper.Increment/) Тип `double`, инициализируемое 1

Визуально `Stepper` состоит из двух кнопок с меткой **& #x 2013;** и  **+** . Нажав клавишу **& #x 2013;** уменьшает `Value` по `Increment` для менее `Minimum`. Нажав клавишу  **+**  увеличивает `Value` по `Increment` максимум `Maximum`.

Это продемонстрировано на [ **StepperDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/StepperDemo) образца.

## <a name="switch-and-checkbox"></a>Коммутатор и флажок

[ `Switch` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/) Дает пользователю возможность указать логическое значение.

### <a name="switch-basics"></a>Основы коммутатора

Визуально `Switch` состоит из переключатель, который можно включать и выключать. Этот класс определяет одно свойство:

- [`IsToggled`](https://developer.xamarin.com/api/property/Xamarin.Forms.Switch.IsToggled/) типа `bool`

`Switch` Определяет одно событие:

- [`Toggled`](https://developer.xamarin.com/api/event/Xamarin.Forms.Switch.Toggled/) сопровождается [ `ToggledEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToggledEventArgs/) объекта, возникает, когда `IsToggled` изменения свойств.

[ **SwitchDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SwitchDemo) программы показана `Switch`.

### <a name="a-traditional-checkbox"></a>Традиционные флажок

Некоторым разработчикам может потребоваться более традиционным `CheckBox` для `Switch`. [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека содержит `CheckBox` класс, производный от `ContentView`. `CheckBox` реализуется [CheckBox.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml) и [CheckBox.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CheckBox.xaml.cs) файлов. `CheckBox` задает три свойства (`Text`, `FontSize`, и `IsChecked`) и `CheckedChanged` событий.

[ **CheckBoxDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/CheckBoxDemo) образец демонстрирует это `CheckBox`.

## <a name="typing-text"></a>Ввод текста

Xamarin.Forms определяет три представления, которые позволяют вводить и редактировать текст:

- [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) для отдельной строки текста
- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) для нескольких строк текста
- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) для отдельной строки текста для поиска.

`Entry` и `Editor` являются производными от [ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/), который является производным от `View`. `SearchBar` является производным непосредственно от `View`.

### <a name="keyboard-and-focus"></a>И фокус клавиатуры

На телефонах и планшетах без физической клавиатуры `Entry`, `Editor`, и `SearchBar` все элементы вызвать виртуальной клавиатуры для раскрытия. Наличие этого клавиатуры на экране, связанные с фокус ввода. Представления должны иметь его [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) и [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) значение свойства `true` получить фокус ввода.

С фокусом ввода участвуют два метода, одно свойство только для чтения, а два события. Они определяются `VisualElement`:

- [ `Focus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Focus()/) Метод пытается установить фокус ввода на элемент и возвращает `true` в случае успешного выполнения
- [ `Unfocus` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Unfocus()/) Метод удаляет фокус ввода для элемента
- [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) Указывает свойство только для чтения, если элемент имеет фокус ввода
- [ `Focused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Focused/) Событие указывает, когда элемент получает фокус ввода
- [ `Unfocused` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.Unfocused/) Событие указывает, когда элемент теряет фокус ввода

### <a name="choosing-the-keyboard"></a>Выбор клавиатуры

[ `InputView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.InputView/) Класс, от которого `Entry` и `Editor` являются производными определяет только одно свойство:

- [`Keyboard`](https://developer.xamarin.com/api/property/Xamarin.Forms.InputView.Keyboard/) типа [`Keyboard`](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/)

Указывает тип клавиатуры, который отображается. Некоторые клавиатуры оптимизированы для URI или чисел.

`Keyboard` Класса позволяет определять клавиатуры со статическим [ `Keyboard.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Keyboard.Create/p/Xamarin.Forms.KeyboardFlags/) метода с аргументом типа [ `KeyboardFlags` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardFlags/), перечисление с следующие битовые флаги:

- `None` значение 0
- [`CapitalizeSentence`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.CapitalizeSentence/) значение 1
- [`Spellcheck`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Spellcheck/) значение 2
- [`Suggestions`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.Suggestions/) значение 4
- [`All`](https://developer.xamarin.com/api/field/Xamarin.Forms.KeyboardFlags.All/) значение \xFFFFFFFF

При использовании многострочного [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) при ожидается абзаца или часть текста, вызов `Keyboard.Create` является хорошим подходом в выборе клавиатуры. Для однострочного [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), следующие статические свойства только для чтения из `Keyboard` полезны:

- [`Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Default/)
- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Text/)
- [`Chat`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Chat/)
- [`Url`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Url/)
- [`Email`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Email/)
- [`Telephone`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Telephone/)
- [`Numeric`](https://developer.xamarin.com/api/property/Xamarin.Forms.Keyboard.Numeric/) для положительных чисел, независимо от десятичной запятой.

[ `KeyboardTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.KeyboardTypeConverter/) Можно указать эти свойства в XAML, как показано в предыдущем [ **EntryKeyboards** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/EntryKeyboards) программы.

### <a name="entry-properties-and-events"></a>Запись свойства и события

Однострочный [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) определяет следующие свойства:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Text/) Тип `string`, текст, отображаемый в `Entry`
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) типа `Color`
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontFamily/) типа `string`
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontSize/) типа `double`
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.FontAttributes/) типа `FontAttributes`
- [`IsPassword`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.IsPassword/) Тип `bool`, которое вызывает символов маскируемый
- [`Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) Тип `string`, dimly цвет текста, который отображается в `Entry` перед ничего типизируется
- [`PlaceholderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.PlaceholderColor/) типа `Color`

`Entry` Также определяет два события:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) с [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) объекта, возникает каждый раз, когда `Text` изменения свойств
- [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/), при завершении пользователем и закрывается клавиатуры. Пользователь указывает завершение образом платформой

[ **QuadraticEquations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/QuadaticEquations) образец демонстрирует этих двух событий.

### <a name="the-editor-difference"></a>Разница редактора

Многострочного [ `Editor` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) определяет же `Text` и `Font` свойств в виде `Entry` , но не другие свойства. `Editor` также определяет те же два свойства, как `Entry`.

[**JustNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/JustNotes) — программа ведения заметок произвольной формы, которая сохраняет и восстанавливает содержимое `Editor`.

### <a name="the-searchbar"></a>SearchBar

[ `SearchBar` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Не является производным от `InputView`, поэтому он не имеет `Keyboard` свойства. Но у него все `Text`, `Font`, и `Placeholder` свойства, `Entry` определяет. Кроме того `SearchBar` определяет три дополнительных свойства:

- [`CancelButtonColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.CancelButtonColor/) типа `Color`
- [`SearchCommand`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) Тип [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) для привязки данных и MVVM
- [`SearchCommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) Тип `Object`, для использования с `SearchCommand`

Конкретные платформы отменить кнопка удаляет текст. `SearchBar` Также имеется кнопка поиска конкретной платформы. Нажав любую из этих кнопок вызывает один из двух событий, `SearchBar` определяет:

- [`TextChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.TextChanged/) сопровождается [ `TextChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextChangedEventArgs/) объекта
- [`SearchButtonPressed`](https://developer.xamarin.com/api/event/Xamarin.Forms.SearchBar.SearchButtonPressed/)

[ **SearchBarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SearchBarDemo) образец демонстрирует `SearchBar`.

## <a name="date-and-time-selection"></a>Выбор даты и времени

[ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) И [ `TimePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) представления реализовать платформой элементы управления, позволяющие пользователю задавать даты или времени.

### <a name="the-datepicker"></a>DatePicker

[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) Определяет четыре свойства:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) Тип `DateTime`, инициализируются с 1 января 1900 г.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) Тип `DateTime`, инициализируемое 31 декабря 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Тип `DateTime`, инициализируемое `DateTime.Today`
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) Тип `string`, инициализируется значением «d», шаблон короткого формата даты, строка форматирования .NET приведет к Отображение даты, например «7/20/1969» в США.

Можно задать `DateTime` свойства в XAML, выражения свойства как свойства элементов и используя инвариантный язык и региональные параметры дату формата («7/20/1969»).   

[ **DaysBetweenDates** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/DaysBetweenDates) пример вычисляет количество дней между двумя датами, выбранных пользователем.

### <a name="the-timepicker-or-is-it-a-timespanpicker"></a>TimePicker (или TimeSpanPicker?)

[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/) Определяет два свойства и события отсутствуют.

- [`Time`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Time/) относится к типу `TimeSpan` вместо `DateTime`, указывающее время, истекших после полуночи
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.TimePicker.Format/) Тип `string`, .NET форматирования строки, равным «t», короткий шаблон времени, что приводит к отображения времени, например «1:45 PM» в США.

[ **SetTimer** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15/SetTimer) показано, как использовать `TimePicker` для указания времени таймера. Программа работает только в том случае, если сохранить его на переднем плане.

**SetTimer** также демонстрируется использование [ `DisplayAlert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) метод `Page` для отображения окна предупреждения.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 15 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch15-Apr2016.pdf)
- [Образцы Глава 15](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter15)
- [Ввод](~/xamarin-forms/user-interface/text/entry.md)
- [Редактор](~/xamarin-forms/user-interface/text/editor.md)
