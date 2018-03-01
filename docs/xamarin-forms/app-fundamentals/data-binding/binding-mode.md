---
title: "Режим привязки"
description: "Поток данных между исходной и целевой элемент управления"
ms.topic: article
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: e85bc98b5da4c6e529f150c6e7945a8556e2c60f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="binding-mode"></a>Режим привязки

В [предыдущей статьи](basic-bindings.md), **привязки кода альтернативой** и **привязки XAML альтернативой** избранные страницы `Label` с его `Scale` свойство привязан к `Value` свойство `Slider`. Поскольку `Slider` начальное значение равно 0, это вызвано `Scale` свойство `Label` задано значение 0, а не 1 и `Label` не отображается.

В [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) образце **отменить привязку** страница подобна программ в предыдущей статьи, за исключением того, что привязка данных определяется на `Slider` , а не на `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label" 
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

Сначала может показаться, что вперед: теперь `Label` является источником привязки данных и `Slider` является целевым. Ссылки привязки `Opacity` свойство `Label`, который имеет значение по умолчанию 1.

Как и следует ожидать, `Slider` инициализируется со значением 1 из первоначального `Opacity` значение `Label`. Это показано на снимке экрана iOS слева:

[![Отменить привязку](binding-mode-images/reversebinding-small.png "отменить привязку")](binding-mode-images/reversebinding-large.png "отменить привязку")

Однако может быть удивлены, `Slider` продолжает работать, как показано на снимке экрана Android и UWP. Это первый взгляд, что привязка к данным работает лучше, когда `Slider` является целевой объект привязки, а не `Label` потому, что инициализация работает как ожидается, возможно.

Разница между **отменить привязку** образца и более ранних образцы включает *режим привязки*.

## <a name="the-default-binding-mode"></a>Режим привязки по умолчанию

Режим привязки указывается с членом [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/) перечисления: 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; данных переходит в обоих направлениях между источником и целью
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; данные отправляются из источника в целевую
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; данные отправляются от целевого объекта в источник

Каждое свойство, связываемое имеет режим привязки, который задается при создании привязываемые свойства по умолчанию и которая доступна из [ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/) свойство `BindableProperty` объекта. Этот режим привязки по умолчанию действует указывает режим, если это свойство является целевым объектом привязки данных.

Режим привязки по умолчанию для большинства свойств, таких как `Rotation`, `Scale`, и `Opacity` — `OneWay`. Если эти свойства имеют цели привязки данных, целевое свойство имеет значение из источника.

Однако режим привязки по умолчанию для `Value` свойство `Slider` — `TwoWay`. Это означает, что при `Value` свойство является целевым объектом привязки данных, а затем целевой набор из источника (как обычно), но источник также имеет значение от целевого объекта. Это позволяет `Slider` для задания из первоначального `Opacity` значение.

Это двусторонней привязки могут показаться Создание бесконечный цикл, но не возникают. Если свойство изменяется и привязываемые свойства означает изменение свойства. Это предотвращает бесконечный цикл.

### <a name="two-way-bindings"></a>Двусторонней привязки

Наиболее привязываемые свойства имеют режима привязки по умолчанию `OneWay` режима привязки по умолчанию, но следующие свойства `TwoWay`:

- `Date` Свойство `DatePicker`
- `Text` Свойство `Editor`, `Entry`, `SearchBar`, и `EntryCell`
- `IsRefreshing` Свойство `ListView`
- `SelectedItem` Свойство `MultiPage`
- `SelectedIndex` и `SelectedItem` свойства `Picker`
- `Value` Свойство `Slider` и `Stepper`
- `IsToggled` Свойство `Switch` 
- `On` Свойство `SwitchCell`
- `Time` Свойство `TimePicker`

Эти конкретные свойства определяются как `TwoWay` для веская причина: 

При использовании привязки данных с помощью архитектуры Model-View-ViewModel (MVVM) приложения ViewModel класс является источник привязки данных и представление, которое состоит из представления, такие как `Slider`, целевых объектов привязки данных. MVVM привязок напоминают **отменить привязку** образец несколько привязок в предыдущих примерах. Это очень вероятно, что требуется, чтобы каждое представление на странице, чтобы инициализировать значением соответствующего свойства в ViewModel, но изменения в представлении также влияет на свойства ViewModel.

Свойства с режимами привязки по умолчанию `TwoWay` — это свойства, скорее всего, для использования в сценариях MVVM.

### <a name="one-way-to-source-bindings"></a>Один-односторонне к источнику привязки

Привязываемые свойства только для чтения имеют режима привязки по умолчанию `OneWayToSource`. Имеется только одно свойство привязываемых чтения и записи, с режима привязки по умолчанию `OneWayToSource`:

- `SelectedItem` Свойство `ListView`

Основной причиной является, привязки на `SelectedItem` свойство следует привести параметр источника привязки. Пример далее в этой статье переопределяет это поведение.

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels и уведомления об изменении свойства

**Простой селектор цвета** страницы показано использование простого ViewModel. Привязки данных позволяет пользователю выбрать цвет с использованием трех `Slider` элементы для цветового тона, насыщенности и яркости.

ViewModel является источником привязки данных. ViewModel *не* определить свойства для привязки, но он реализует механизм уведомлений, который позволяет инфраструктура привязки получать уведомления при изменении значения свойства. Этот механизм уведомлений [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) интерфейс, который определяет одно свойство с именем [ `PropertyChanged` ](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/). Класс, реализующий этот интерфейс, обычно запускает событие при изменении значения одного из его открытые свойства. Событие не возникает в том случае, если свойство никогда не меняется. ( `INotifyPropertyChanged` Также реализован интерфейс `BindableObject` и `PropertyChanged` событие при изменении значения свойства привязки.)

`HslColorViewModel` Класс определяет пять свойств: `Hue`, `Saturation`, `Luminosity`, и `Color` свойства взаимосвязаны. Если один из трех цвета компоненты значение изменений, `Color` пересчитывается свойство, и `PropertyChanged` происходят события для всех четырех свойств:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name; 

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get 
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

При `Color` изменения свойств, статический `GetNearestColorName` метод в `NamedColor` класса (также включаются в **DataBindingDemos** решения) Получает ближайший именованный цвет и задает `Name` свойства. Это `Name` свойство имеет закрытый `set` метод доступа, поэтому его нельзя установить из вне класса.

Когда ViewModel задается как источник привязки, инфраструктура привязки задается обработчик `PropertyChanged` событий. Таким образом можно получать уведомления об изменениях свойств привязки и затем можно задать свойства целевого объекта из измененные значения.

**Простой селектор цвета** файла XAML создает `HslColorViewModel` в словарь ресурсов на странице, а также инициализирует `Color` свойство. `BindingContext` Свойство `Grid` задано значение `StaticResource` привязки расширение для ссылки на этот ресурс:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel" 
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
        
    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />
    
            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`, `Label`И три `Slider` представления наследуют контекст привязки из `Grid`. Эти представления находятся все целевые объекты привязки, которые ссылаются на свойства источника в ViewModel. Для `Color` свойство `BoxView`и `Text` свойство `Label`, привязки данных, `OneWay`: в окне свойств в ViewModel устанавливаются свойства в представлении.

`Value` Свойство `Slider`, однако `TwoWay`. Это позволит каждой `Slider` задаваемый из ViewModel, а также для ViewModel присвоить из каждого `Slider`. 

При запуске программы `BoxView`, `Label`и три `Slider` элементы имеют значение из ViewModel, в зависимости от первоначального `Color` значение свойства при создании экземпляра ViewModel. Это показано на снимке экрана iOS слева:

[![Средство выбора цвета простой](binding-mode-images/simplecolorselector-small.png "средства выбора цвета простой")](binding-mode-images/simplecolorselector-large.png "средства выбора цвета простой")

Во время работы ползунки, `BoxView` и `Label` изменяются соответствующим образом, как показано на снимке экрана Android и UWP.

При создании экземпляра ViewModel в словаре ресурсов является распространенным подходом. Также можно создать экземпляр ViewModel в теги элементов свойства для `BindingContext` свойства. В **простой селектор цвета** XAML файл, попробуйте удалить `HslColorViewModel` из словаря ресурсов и присвойте ему значение `BindingContext` свойства `Grid` следующим образом:

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

Контекст привязки можно задать в различными способами. В некоторых случаях файл кода создает ViewModel и назначает ей `BindingContext` свойства страницы. Они допускаются все.

## <a name="overriding-the-binding-mode"></a>Переопределение режим привязки

Если режим привязки по умолчанию для целевого свойства не подходит для привязки данных, можно переопределить, задав [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/) свойство `Binding` (или [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/) свойство `Binding` расширение разметки) к одному из членов `BindingMode` перечисления.

Однако задание `Mode` свойства `TwoWay` не всегда работает, как и следует ожидать. Например, попробуйте изменить **привязкой XAML альтернативой** включаемых файлов XAML `TwoWay` в определении привязки:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

Возможно, `Slider` инициализировалась начальное значение `Scale` свойства, равное 1, но не возникают. Когда `TwoWay` привязки инициализируется, целевой объект имеет значение из источника во-первых, это значит, что `Scale` свойству `Slider` значение 0 по умолчанию. Когда `TwoWay` привязки устанавливается на `Slider`, то `Slider` изначально установлено в источнике.

Можно задать режим привязки `OneWayToSource` в **привязкой XAML альтернативой** образца:

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

Теперь `Slider` устанавливается равным 1 (значение по умолчанию `Scale`) но управление `Slider` не влияет на `Scale` свойство, поэтому это не очень удобен.

Удобно использовать приложения переопределения режимом по умолчанию с `TwoWay` включает в себя `SelectedItem` свойство `ListView`. Режим привязки по умолчанию — `OneWayToSource`. Если привязка данных имеет значение на `SelectedItem` свойства для свойства source в ViewModel, ссылки, задайте свойства источника из `ListView` выделения. Однако в некоторых случаях можно также `ListView` инициализированную из ViewModel.

**Параметры примеров** этот метод продемонстрирован страницы. Эта страница представляет простой реализации параметры приложения, которые определены в ViewModel, такие как это очень часто `SampleSettingsViewModel` файла:

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Каждый параметр приложения является свойством, которое сохраняется в словарь свойств Xamarin.Forms в метод с именем `SaveState` и загружаются из этого словаря в конструкторе. В нижней части класса являются два метода, которые упрощают всей ViewModels и их менее подвержены возникновению ошибок. `OnPropertyChanged` Метод внизу имеет необязательный параметр, задайте свойству в вызовах. Это позволяет избежать орфографических ошибок, при указании имени свойства в виде строки. 

`SetProperty` Метод в классе делает более: он сравнивает значение, задаваемое для свойства со значением, храниться в виде поля и вызывает только `OnPropertyChanged` Если два значения не равны. 

`SampleSettingsViewModel` Класс определяет два свойства для цвета фона: `BackgroundNamedColor` свойство относится к типу `NamedColor`, который класс также входит в **DataBindingDemos** решения. `BackgroundColor` Свойство относится к типу `Color`и берется из `Color` свойство `NamedColor` объекта.

`NamedColor` Класс использует отражение .NET перечислить все открытые статические поля в Xamarin.Forms `Color` структуры и для их хранения с именами в коллекции, доступный из статического `All` свойство:

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App` Класса в **DataBindingDemos** проект определяет свойство с именем `Settings` типа `SampleSettingsViewModel`. Это свойство инициализируется при `App` создается экземпляр класса и `SaveState` метод вызывается, когда `OnSleep` вызывается метод:

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

Дополнительные сведения о методов жизненного цикла приложений см. в статье [ **жизненный цикл приложения**](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

Практически все остальное обрабатывается в **SampleSettingsPage.xaml** файла. `BindingContext` Страницы задается с помощью `Binding` расширения разметки: источник привязки является статическим `Application.Current` свойство, которое является экземпляром из `App` класса в проекте и `Path` равно `Settings` свойство, которое является `SampleSettingsViewModel` объекта:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

Все дочерние элементы страницы наследуют контекст привязки. Большинство других привязок на этой странице, к свойствам в `SampleSettingsViewModel`. `BackgroundColor` Свойство используется для задания `BackgroundColor` свойство `StackLayout`и `Entry`, `DatePicker`, `Switch`, и `Stepper` свойства связаны с другими свойствами в ViewModel.

`ItemsSource` Свойство `ListView` присваивается статический `NamedColor.All` свойство. Заполняет это `ListView` со всеми `NamedColor` экземпляров. Для каждого элемента в `ListView`, контекст привязки для элемента задано значение `NamedColor` объекта. `BoxView` И `Label` в `ViewCell` привязаны к свойствам в `NamedColor`.

`SelectedItem` Свойство `ListView` относится к типу `NamedColor`и привязывается к `BackgroundNamedColor` свойства `SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

Режим привязки по умолчанию для `SelectedItem` — `OneWayToSource`, который задает свойства ViewModel из выбранного элемента. `TwoWay` Режим позволяет `SelectedItem` инициализированную из ViewModel. 

Тем не менее, когда `SelectedItem` задано таким образом, `ListView` не прокручивается автоматически для отображения выбранного элемента. Небольшой код в файле кода, который требуется:

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem, 
                                   ScrollToPosition.MakeVisible, 
                                   false);
        }
    }
}
``` 

На снимке экрана iOS слева показано программой, при первом запуске. Конструктор в `SampleSettingsViewModel` инициализирует белый цвет фона и, что было выбрано `ListView`:

[![Примеры параметров](binding-mode-images/samplesettings-small.png "примеры параметров")](binding-mode-images/samplesettings-large.png "примеры параметров")

Две другие снимки экрана Показать измененных параметров. При проведении экспериментов с этой страницей, не забудьте поместить программу в спящий режим или завершить его на устройство или эмулятор, на котором он выполняется. Завершение программы с помощью отладчика Visual Studio не приведет к `OnSleep` переопределить в `App` вызов класса.

В следующей статье вы увидите, как задать [ **форматирования строк** ](string-formatting.md) привязки данных, которые устанавливаются на `Text` свойство `Label`.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Данные привязки Глава из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
