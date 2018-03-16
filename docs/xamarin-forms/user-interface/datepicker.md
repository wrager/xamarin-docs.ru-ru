---
title: "С помощью DatePicker"
description: "Представление Xamarin.Forms, которое позволяет пользователю выбрать дату"
ms.topic: article
ms.prod: xamarin
ms.assetid: 68E8EF8A-42E7-4939-8ABE-64D060E609D9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/12/2018
ms.openlocfilehash: d47499c1e309fbc67c85b55cacbbba3942188f54
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="using-datepicker"></a>С помощью DatePicker

_Представление Xamarin.Forms, которое позволяет пользователю выбрать дату_

Xamarin.Forms [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) вызывает элемент управления выбора даты платформы и позволяет пользователю выбрать дату. `DatePicker` Определяет пять свойств:

- [`MinimumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MinimumDate/) Тип [ `DateTime` ](https://developer.xamarin.com/api/type/System.DateTime/), который по умолчанию в первый день года 1900.
- [`MaximumDate`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.MaximumDate/) Тип `DateTime`, какие значения по умолчанию для последнего дня года 2100.
- [`Date`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Date/) Тип `DateTime`, выбранной даты, по умолчанию используется значение [ `DateTime.Today` ](https://developer.xamarin.com/api/property/System.DateTime.Today/).
- [`Format`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.Format/) типа `string`, [Стандартная](/dotnet/standard/base-types/standard-date-and-time-format-strings/) или [пользовательские](/dotnet/standard/base-types/custom-date-and-time-format-strings/) .NET форматирования строки, которая по умолчанию «D», длинных Дата шаблон.
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.DatePicker.TextColor/) Тип [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/), цвет, используемый для отображения выбранной даты, значение по умолчанию [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).

`DatePicker` Активируется [ `DateSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.DatePicker.DateSelected/) событие, когда пользователь выбирает дату.

> [!WARNING]
> При задании `MinimumDate` и `MaximumDate`, убедитесь, что `MinimumDate` , всегда меньше или равно `MaximumDate`. В противном случае `DatePicker` вызовет исключение.

На внутреннем уровне `DatePicker` гарантирует, что `Date` между `MinimumDate` и `MaximumDate`включительно. Если `MinimumDate` или `MaximumDate` имеет значение, чтобы `Date` не между ними, `DatePicker` будет изменить значение параметра `Date`.

Все пять свойств, поддерживаемых [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) объектов, что означает, что они могут быть со стилем и свойства могут являться целевыми для привязки данных. `Date` Свойство имеет режима привязки по умолчанию [ `BindingMode.TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/), что означает, что он может быть целевым объектом привязки данных в приложение, использующее [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) архитектура.

## <a name="initializing-the-datetime-properties"></a>Инициализация свойств даты и времени

В коде, можно инициализировать `MinimumDate`, `MaximumDate`, и `Date` свойств для значений типа `DateTime`:

```csharp
DatePicker datePicker = new DatePicker
{
    MinimumDate = new DateTime(2018, 1, 1),
    MaximumDate = new DateTime(2018, 12, 31),
    Date = new DateTime(2018, 6, 21)
};
```

Когда `DateTime` значение в указанный в XAML, средство синтаксического анализа XAML использует `DateTime.Parse` метод с `CultureInfo.InvariantCulture` аргумент для преобразования строки в `DateTime` значение. Даты должен быть указан в формате точный: двумя цифрами месяцев, дней двумя цифрами и четырехзначный год, разделенных косой чертой:

```xaml
<DatePicker MinimumDate="01/01/2018"
            MaximumDate="12/31/2018"
            Date="06/21/2018" />
```

Если `BindingContext` свойство `DatePicker` устанавливается на экземпляр ViewModel, содержащий свойства типа `DateTime` с именем `MinDate`, `MaxDate`, и `SelectedDate` (например), можно создать экземпляр `DatePicker` следующим образом :

```xaml
<DatePicker MinimumDate="{Binding MinDate}"
            MaximumDate="{Binding MaxDate}"
            Date="{Binding SelectedDate}" />
```

В этом примере все три свойства инициализируются соответствующие свойства в ViewModel. Поскольку `Date` свойство имеет режим привязки `TwoWay`, любой новую дату, которое выбирает пользователь автоматически отражаются в ViewModel.

Если `DatePicker` не содержит привязки на его `Date` свойства, приложению следует подключить обработчик `DateSelected` событие было в курсе, когда пользователь выбирает новую дату.

## <a name="datepicker-and-layout"></a>DatePicker и макет

Можно использовать параметр неограниченного горизонтальном расположении, такие как `Center`, `Start`, или `End` с `DatePicker`:

```xaml
<DatePicker ··· 
            HorizontalOptions="Center" 
            ··· />
```

Однако это не рекомендуется. В зависимости от значения `Format` выбранным свойством даты могут потребоваться другим ширины. Например, строка формата «D» вызывает `DateTime` для отображения дат в полном формате, а «Среда, 12 сентябрь 2018» требует больше ширины экрана, чем «Пятница, 4 май 2018». В зависимости от платформы, это различие может привести к `DateTime` представления для изменения ширины в макете, или для отображения, усекаются.

> [!TIP]
> Рекомендуется использовать значение по умолчанию `HorizontalOptions` параметра `Fill` с `DatePicker`и не использовать ширину `Auto` при размещении `DatePicker` в `Grid` ячейки.

## <a name="datepicker-in-an-application"></a>DatePicker в приложении

[ **DaysBetweenDates** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) пример содержит две `DatePicker` представления на странице. Они используются для выбора двух дат и программа вычисляет количество дней между этими датами. Программа не изменять параметры `MinimumDate` и `MaximumDate` свойства, поэтому две даты должен быть в диапазоне от 1900 до 2100.

Ниже приведен файл XAML.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DaysBetweenDates"
             x:Class="DaysBetweenDates.MainPage">
    <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness">
            <On Platform="iOS" Value="0, 20, 0, 0" />
        </OnPlatform>
    </ContentPage.Padding>

    <StackLayout Margin="10">
        <Label Text="Days Between Dates"
               Style="{DynamicResource TitleStyle}"
               Margin="0, 20"
               HorizontalTextAlignment="Center" />

        <Label Text="Start Date:" />

        <DatePicker x:Name="startDatePicker"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <Label Text="End Date:" />

        <DatePicker x:Name="endDatePicker"
                    MinimumDate="{Binding Source={x:Reference startDatePicker},
                                          Path=Date}"
                    Format="D"
                    Margin="30, 0, 0, 30"
                    DateSelected="OnDateSelected" />

        <StackLayout Orientation="Horizontal"
                     Margin="0, 0, 0, 30">
            <Label Text="Include both days in total: "
                   VerticalOptions="Center" />
            <Switch x:Name="includeSwitch"
                    Toggled="OnSwitchToggled" />
        </StackLayout>

        <Label x:Name="resultLabel"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center" />

    </StackLayout>
</ContentPage>
```

Каждый `DatePicker` назначается `Format` свойство «D» для длинный формат даты. Обратите внимание, что `endDatePicker` объект имеет привязку, которая обращается его `MinimumDate` свойство. Источник привязки является выбранного `Date` свойство `startDatePicker` объекта. Это гарантирует, что дата окончания всегда позже или равна дате начала. Помимо двух `DatePicker` объектов, `Switch` меткой «Include обоих день всего». 

Два `DatePicker` представления имеют прикреплены обработчики `DateSelected` события и `Switch` присоединен обработчик его `Toggled` событий. Эти обработчики событий в файле кода и запустит новое вычисление дней между двумя датами:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }

    void OnDateSelected(object sender, DateChangedEventArgs args)
    {
        Recalculate();
    }

    void OnSwitchToggled(object sender, ToggledEventArgs args)
    {
        Recalculate();
    }

    void Recalculate()
    {
        TimeSpan timeSpan = endDatePicker.Date - startDatePicker.Date +
            (includeSwitch.IsToggled ? TimeSpan.FromDays(1) : TimeSpan.Zero);

        resultLabel.Text = String.Format("{0} day{1} between dates",
                                            timeSpan.Days, timeSpan.Days == 1 ? "" : "s");
    }
}
```

При первом запуске образца, оба `DatePicker` представления инициализируются до сегодняшней даты. На следующем рисунке показан программу на iOS, Android и универсальной платформе Windows:

[![Дней между датами начала](datepicker-images/DaysBetweenDatesStart.png "дней между датами начала")](datepicker-images/DaysBetweenDatesStart-Large.png#lightbox "дней между датами начала")

Коснитесь любого из `DatePicker` отображает вызывает выбор даты платформы. Три платформы реализовать этот управляющий элемент выбора даты очень разными способами, но каждого подхода есть уже знакомые пользователям этой платформы:

[![Выберите дней между датами](datepicker-images/DaysBetweenDatesSelect.png "дней между датами выберите")](datepicker-images/DaysBetweenDatesSelect-Large.png#lightbox "выберите дней между датами")

После выбора двумя датами приложение отображает количество дней между этими датами:

[![Дней от даты результат](datepicker-images/DaysBetweenDatesResult.png "дней от даты результат")](datepicker-images/DaysBetweenDatesResult-Large.png#lightbox "дней от даты результат")

## <a name="related-links"></a>Связанные ссылки

- [Образец DaysBetweenDates](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker)
- [DatePicker API](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)
