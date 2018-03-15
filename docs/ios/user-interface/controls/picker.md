---
title: "Средство выбора"
description: "В настоящем руководстве описывается проектирование и работа с выбора в приложения Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2369EFC-285A-44DD-9E80-EC65BC3DF041
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/02/2017
ms.openlocfilehash: b92bdc365cae524cee1f586b293c4638225c6178
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="picker"></a>Средство выбора

Элемент управления выбора отображает «колесо по принципу» элемент управления, содержащий прокручиваемый список значений со значением выделена. Пользователи вращайте колесико мыши, выберите параметр нужные им.

Один конкретного пользователя вариант выбора ее, чтобы задать дату и / или время. Для обеспечения этой Apple создала пользовательскому подклассу UIPickerView класс с именем UIDatePicker.

В статье описаны реализации и использовании [выбора](#picker) и [выбора даты](#datepicker) элементов управления.

<a name="picker" />

## <a name="picker"></a>Средство выбора

### <a name="implementing-a-picker"></a>Реализация элемент выбора

Средство выбора реализуется путем создания нового [`UIPickerView`](https://developer.xamarin.com/api/type/UIKit.UIPickerView/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="pickers-and-storyboards"></a>Выбор и раскадровки

Если вы используете iOS конструктор для создания пользовательского интерфейса, средство выбора могут добавляться в макет из панели элементов:

![](picker-images/image1.png)


### <a name="working-with-picker"></a>Работа с выбора

После создания средства выбора, либо в коде или через раскадровки, вам потребуется назначить _модель_ к нему, чтобы можно было передавать и взаимодействовать с данными;

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();

    var pickerModel = new PeopleModel(personLabel);

    personPicker.Model = pickerModel;
}
```

В следующем примере кода показан пример модели.

```csharp
public class PeopleModel : UIPickerViewModel
{
    public string[] names = new string[] {
            "Amy Burns",
            "Kevin Mullins",
            "Craig Dunn",
            "Joel Martinez",
            "Charles Petzold",
            "David Britch",
            "Mark McLemore",
            "Tom Opegenorth",
            "Joseph Hill",
            "Miguel De Icaza"
        };

    private UILabel personLabel;

    public PeopleModel(UILabel personLabel)
    {
        this.personLabel = personLabel;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 2;
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return names.Length;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        if (component == 0)
            return names[row];
        else
            return row.ToString();
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {   
        personLabel.Text = $"This person is: {names[pickerView.SelectedRowInComponent(0)]},\n they are number {pickerView.SelectedRowInComponent(1)}";
    }

    public override nfloat GetComponentWidth(UIPickerView picker, nint component)
    {
        if (component == 0)
            return 240f;
        else
            return 40f;
    }

    public override nfloat GetRowHeight(UIPickerView picker, nint component)
    {
        return 40f;
    }
```

Сначала необходимо передать некоторые данные, чтобы предоставить пользователю возможность выбрать различные варианты. При попытке сохранить этот список, как можно более коротким возможно или при необходимости попробуйте использовать более одного «набрать» (называется *компоненты*):

![Выбор из двух компонентов](picker-images/image3.png)

Чтобы задать число компонентов, необходимо переопределить `GetComponentCount` метод: 

```csharp
public override nint GetComponentCount(UIPickerView pickerView)
{
    return 2;
}
```

Возвращаемое значение обозначает количество набирает у вашего выбора.

### <a name="customizing-appearance"></a>Настройка внешнего вида
 
Внешний вид `UIPickerView` можно настроить с помощью `UIPickerView.UIPickerViewAppearance` класса или путем переопределения `UIPickerViewModel.GetView` и `UIPickerViewModel.GetRowHeight` методы в `UIPickerViewModel`.


<a name="datepicker" />

## <a name="date-picker"></a>Управляющий элемент выбора даты

### <a name="implementing-a-date-picker"></a>Реализация элемент выбора даты

Выбор даты реализуется путем создания нового [ `UIDatePickerView` ](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/):

```csharp
UIPickerView pickerView = new UIPickerView(
                            new CGRect(
                                UIScreen.MainScreen.Bounds.X-UIScreen.MainScreen.Bounds.Width, UIScreen.MainScreen.Bounds.Height -230, 
                                UIScreen.MainScreen.Bounds.Width, 
                                180));
```


### <a name="date-pickers-and-storyboards"></a>Выбор даты и раскадровки

Если вы используете iOS конструктор для создания пользовательского интерфейса, **выбора даты** можно добавить в макет из области элементов. В панели свойств можно изменить следующие свойства:

![](picker-images/image2.png)

* **Режим** — режим даты и времени. Это может быть даты, времени, даты и времени или таймер обратного отсчета. 
* **Языковой стандарт** — языковой стандарт для выбора даты. Выберите **по умолчанию** для задания в системе по умолчанию или задать для любого определенного языкового стандарта.
* **Интервал** — показывает инкремента, в котором отображаются параметры часов.
* **Даты, дата минимальное, максимальное значение даты** — задает начальной даты, будут отображаться в средстве выбора и ограничения для доступный для выбора дат.

### <a name="configuring-the-datepicker"></a>Настройка DatePicker

Можно ограничить диапазон дат, пользователь может выбрать в с помощью `MinimumDate` и `MaximumDate` свойства. В следующем фрагменте кода показан пример того, как задать диапазон между 60 лет, но сегодня.

```csharp
var calendar = new NSCalendar(NSCalendarType.Gregorian);
var currentDate = NSDate.Now;
var components = new NSDateComponents();

components.Year = -60;

NSDate minDate = calendar.DateByAddingComponents(components, NSDate.Now, NSCalendarOptions.None);

datePickerView.MinimumDate = minDate;
datePickerView.MaximumDate = NSDate.Now;
```

Кроме того чтобы задать диапазон дат минимальное и максимальное также можно использовать элементы управления .NET. Пример:

```csharp
DatePicker.MinimumDate = (NSDate)DateTime.Today.AddDays (-7);
DatePicker.MaximumDate = (NSDate)DateTime.Today.AddDays (7);
```

Можно также задать `MinuteInterval` свойство, чтобы задать интервал, с которой средство выбора будет отображаться в минутах. В следующем фрагменте кода можно использовать для задания селектор для задания с интервалом 10 минут.

```csharp
datePickerView.MinuteInterval = 10;
```

Существует четыре режима, которые могут быть установлены выбора даты с помощью [ `UIDatePicker.Mode` ](https://developer.xamarin.com/api/property/UIKit.UIDatePicker.Mode/) свойство. Ниже показан пример каждый из них и способ его реализации.

#### <a name="time"></a>Время

Режим времени время с помощью селектора час и минуту и необязательный обозначение AM или PM. Он устанавливается с `UIDatePickerMode.Time` свойство. Пример:

```csharp
datePickerView.Mode = UIDatePickerMode.Time;
```

На следующем рисунке показан пример этого режима выбора дат:

![](picker-images/image8.png)



#### <a name="date"></a>Дата

Режим даты отображает дату месяца, дня и года селектора. Он устанавливается с `UIDatePickerMode.Date` свойство. Пример:

```csharp
datePickerView.Mode = UIDatePickerMode.Date;
```

На следующем рисунке показан пример этого DatePicker:

![](picker-images/image7.png)

Порядок селекторы зависит от языкового стандарта `UIDatePicker`. По умолчанию это задается в системе по умолчанию. На рисунке выше показан макет селекторы в `en_US` языкового стандарта, но изменения в день | Месяц | Макет год, можно использовать следующий код для задания языкового стандарта:

```csharp
datePickerView.Locale = NSLocale.FromLocaleIdentifier("en_GB");
```

![](picker-images/image9.png)


#### <a name="date-and-time"></a>Дата и время

Дата и время режим отображает представление shortend даты, время в часах и минутах и необязательный dependings обозначение AM или PM на при использовании в 12 или 24-часовом. Он устанавливается с `UIDatePickerMode.DateAndTime` свойство. Пример:

```csharp
datePickerView.Mode = UIDatePickerMode.DateAndTime;
```

На следующем рисунке показан пример этого DatePicker:

![](picker-images/image6.png)

Как и в [даты](#Date), порядок селекторы и использование 12 или 24-часовом формате зависит от языкового стандарта `UIDatePicker`.

#### <a name="countdown-timer"></a>Таймер обратного отсчета

Режим таймер обратного отсчета отображает час и минуту значения. Он устанавливается с `UIDatePickerMode.CountDownTimer` свойство. Пример:

```csharp
datePickerView.Mode = UIDatePickerMode.CountDownTimer;
```

На следующем рисунке показан пример этого DatePicker:

![](picker-images/image5.png)

Можно использовать `CountDownDuration` свойство для записи dispayed значение путем выбора даты обратного отсчета. Например чтобы добавить значение обратного отсчета до текущей даты, можно использовать следующий код:

```csharp
var currentTime = NSDate.Now;
var countDownTimerTime = datePickerView.CountDownDuration;
var finishCountdown = currentTime.AddSeconds(countDownTimerTime);

dateLabel.Text = "Alarm set for:" + coundownTimeformat.ToString(finishCountdown);
```

#### <a name="formatting"></a>Форматирование 

Значения даты, времени и DateAndTime режимов могут быть захвачены с `Date` свойство UIDatePicker (например: `datePickerView.Date`), который относится к типу NSDate. Чтобы форматировать эта дата на что-нибудь более удобной для чтения, используйте [ `NSDateFormatter` ](https://developer.xamarin.com/api/type/Foundation.NSDateFormatter/). В приведенных ниже примерах показано, как использовать некоторые из доступных свойств данного класса.

`DateFormat` Задается как строка для представления, как должны отображаться Дата:

```csharp
NSDateFormatter dateFormat = new NSDateFormatter();
dateFormat.DateFormat = "yyyy-MM-dd";
```

`TimeStyle` Наборов свойств `NSDateFormatterStyle`:

```csharp
NSDateFormatter timeFormat = new NSDateFormatter();
timeFormat.TimeStyle = NSDateFormatterStyle.Short;
```

Поля для `NSDateFormatterStyle` отображаться следующим образом:

![](picker-images/timestyle.png)

`DateStyle` Наборов свойств `NSDateFormatterStyle`:

```csharp
NSDateFormatter dateTimeformat = new NSDateFormatter();
dateTimeformat.DateStyle = NSDateFormatterStyle.Long;
```

Поля для `NSDateFormatterStyle` отображаться следующим образом:

![](picker-images/datestyle.png)

Затем можно вывести форматированные NSDate в строку, используя следующий код:

```csharp
dateLabel.Text = dateTimeformat.ToString(datePickerView.Date);
```

## <a name="related-links"></a>Связанные ссылки

- [PickerControl (пример)](https://developer.xamarin.com/samples/monotouch/PickerControl/)
