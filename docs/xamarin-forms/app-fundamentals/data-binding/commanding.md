---
title: "Интерфейс командной"
description: "Реализуйте `Command` свойства привязки данных"
ms.topic: article
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 0bc039385a6b2077c3b5fa5114b35b586a14a150
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="the-command-interface"></a>Интерфейс командной

Архитектуры Model-View-ViewModel (MVVM) определены привязки данных между свойствами в ViewModel, который обычно является класс, производный от `INotifyPropertyChanged`и свойства в представлении, который обычно является файл XAML. Иногда приложение имеет потребностей, которые выходят за пределы эти свойства привязки, поскольку требует от пользователя запустить команды, влияющие на что-нибудь в ViewModel. Эти команды обычно обозначаются путем нажатия кнопки или пальца касания и в большинстве случаев они обрабатываются в файле кода в обработчик `Clicked` событие `Button` или `Tapped` событие `TapGestureRecognizer`. 

Запрашивающая интерфейс предоставляет альтернативный подход к реализации команд, гораздо лучше подходит к архитектуре MVVM. ViewModel сам может содержать команды, которые представляют собой методы, которые выполняются в ответ на определенное действие в представлении, такие как `Button` нажмите кнопку. Между этими командами определены привязки данных и `Button`.

Чтобы разрешить привязку данных между `Button` и ViewModel, `Button` определяет два свойства:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) типа [`ICommand`](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) типа `Object`

Чтобы использовать интерфейс команды, определения привязки данных, предназначенный `Command` свойство `Button` где источник — это свойство в ViewModel типа `ICommand`. ViewModel содержит код, связанный с этим `ICommand` свойство, которое выполняется при нажатии кнопки. Можно задать `CommandParameter` к произвольные данные, чтобы различать несколько кнопок, если все они привязаны к тому же `ICommand` свойства ViewModel.

`Command` И `CommandParameter` свойства определяются также следующие классы:

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) и, следовательно [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/), который является производным от `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) и, следовательно [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/), который является производным от `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Определяет [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/) свойство типа `ICommand` и [ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/) свойство. [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/) Свойство [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) также относится к типу `ICommand`. 

Эти команды могут быть обработаны в рамках ViewModel таким способом, который не зависит от объекта конкретного пользовательского интерфейса в представлении.

## <a name="the-icommand-interface"></a>Интерфейс ICommand

[ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) Интерфейс не является частью Xamarin.Forms. Вместо этого оно определено в [ `System.Windows.Input` ](https://developer.xamarin.com/api/namespace/System.Windows.Input/) пространства имен и состоит из двух методов и одно событие:

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

Чтобы использовать интерфейс команды, ваш ViewModel содержит свойства типа `ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel также должен ссылаться на класс, реализующий `ICommand` интерфейса. Этот класс будут описаны чуть ниже. В представлении `Command` свойство `Button` привязан к этому свойству: 

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

Когда пользователь нажимает `Button`, `Button` вызовы `Execute` метод в `ICommand` объект привязан к его `Command` свойство. Это простой частью интерфейса команд.

`CanExecute` Способ является более сложным. При привязке сначала определяются на `Command` свойство `Button`, и при изменении привязки данных определенным образом, `Button` вызовы `CanExecute` метод `ICommand` объекта. Если `CanExecute` возвращает `false`, то `Button` отключается. Это указывает той или иной команды в настоящее время недоступен или недопустим.

`Button` Также присоединяет обработчик на `CanExecuteChanged` событие `ICommand`. Событие из внутри ViewModel. При возникновении этого события `Button` вызовы `CanExecute` еще раз. `Button` Включает себя, если `CanExecute` возвращает `true` и отключается, если `CanExecute` возвращает `false`.

> [!IMPORTANT]
> Не используйте `IsEnabled` свойство `Button` при использовании интерфейса командной.  

## <a name="the-command-class"></a>Класс команд

Если ваш ViewModel определяет propety типа `ICommand`, ViewModel должно также содержать или ссылки на класс, реализующий `ICommand` интерфейса. Этот класс должен содержать или ссылаться на `Execute` и `CanExecute` методы и fire `CanExecuteChanged` событий всякий раз, когда `CanExecute` метод может вернуть другое значение.

Можно написать такой класс самостоятельно, или можно использовать класс, который записан другим пользователем. Поскольку `ICommand` входит в состав Microsoft Windows, он был использован для лет с приложениями Windows MVVM. С помощью класса Windows, который реализует `ICommand` предоставляет общий доступ к вашей ViewModels между приложениями Windows и приложений с помощью Xamarin.Forms.

Если общий доступ к ViewModels между Windows и Xamarin.Forms не имеет значения, то можно использовать [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) или [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) класс включен в Xamarin.Forms для реализации `ICommand`интерфейс. Эти классы позволяют задавать тела `Execute` и `CanExecute` методы в конструкторах классов. Используйте `Command<T>` при использовании `CommandParameter` свойство для различения нескольких представлений, привязанного к тому же `ICommand` свойство и более простой `Command` класса, не является обязательной.

## <a name="basic-commanding"></a>Выполнение основных команд

**Входа пользователя** страницы в [ **демонстрации привязки данных** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) программе показано несколько простых команд, реализованные в ViewModel.

`PersonViewModel` Определяет три свойства с именем `Name`, `Age`, и `Skills` , которые определяют человека. Класс выполняет *не* содержать `ICommand` свойства:

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
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

`PersonCollectionViewModel` Показано ниже, создает новые объекты типа `PersonViewModel` и позволяет пользователю вводить данные. Для этой цели класс определяет свойства `IsEditing` типа `bool` и `PersonEdit` типа `PersonViewModel`. Кроме того, класс определяет трех свойств типа `ICommand` и свойство с именем `Persons` типа `IList<PersonViewModel>`: 

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

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

Этот сокращенный список не включены в конструктор класса, который, если трех свойств типа `ICommand` определены, которой будет отображаться скоро. Обратите внимание, что изменения трех свойств типа `ICommand` и `Persons` свойства не приводят к `PropertyChanged` срабатывание события. Эти свойства устанавливаются при создании класса и не изменяйте соответственно.

Прежде чем изучать конструктор `PersonCollectionViewModel` класса, давайте взглянем на XAML-файл для **входа пользователя** программы. Это поле содержит `Grid` с его `BindingContext` свойство `PersonCollectionViewModel`. `Grid` Содержит `Button` с текстом **New** с его `Command` свойство привязано к `NewCommand` в ViewModel, форма ввода со свойствами привязано к `IsEditing` свойства, как а также свойства `PersonViewModel`, и еще две кнопки привязаны к `SubmitCommand` и `CancelCommand` свойства ViewModel. Конечное `ListView` отображает коллекцию лиц, которые уже были введены:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>
        
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">
            
            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}" 
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal" 
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

Вот как это работает: первый пользователь нажимает **New** кнопки. Это позволяет формы ввода, но отключает **New** кнопки. Здесь пользователь вводит имя, возраст и навыки. В любой момент во время изменения пользователь может нажать **отменить** кнопку, чтобы начать заново. Только если было введено имя и допустимый возраст является **отправить** кнопки включено. Это нажатие **отправить** передача пользователя в коллекцию, отображаемого элементом `ListView`. После того, как **отменить** или **отправить** нажатии кнопки, форма ввода очищается и **New** включена кнопка.

На экране операций ввода-вывода в левом показан макет до входа допустимый возраст. Android и UWP экраны show **отправить** кнопка включена после задания age:

[![Person Entry](commanding-images/personentry-small.png "Person Entry")](commanding-images/personentry-large.png "Person Entry")

Программа не поддерживает все средства для изменения существующих записей и не сохраняет записи, если покинуть страницу.

Вся логика **New**, **отправить**, и **отменить** кнопки обрабатывается в `PersonCollectionViewModel` посредством определения `NewCommand`, `SubmitCommand`, и `CancelCommand` свойства. Конструктор `PersonCollectionViewModel` задает эти три свойства к объектам типа `Command`.  

Объект [конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) из `Command` позволяет передавать аргументы типа `Action` и `Func<bool>` соответствующий `Execute` и `CanExecute` методы. Можно легко определить эти действия и функции как лямбда-выражения, прямо в `Command` конструктора. Вот определение `Command` для объекта `NewCommand` свойство:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }
    
    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

Когда пользователь щелкает **New** кнопки, `execute` передан функции `Command` выполняется конструктор. Это создает новую `PersonViewModel` объекта, задает обработчик для этого объекта `PropertyChanged` задает событие, `IsEditing` для `true`и вызывает `RefreshCanExecutes` метода, определенного после конструктора.

Помимо реализации `ICommand` интерфейс, `Command` класс также определяет метод с именем `ChangeCanExecute`. Следует вызывать ваш ViewModel `ChangeCanExecute` для `ICommand` всякий раз, когда что-то случится, может измениться, возвращаемое значение `CanExecute` метод. Вызов `ChangeCanExecute` вызывает `Command` класс срабатывание `CanExecuteChanged` метод. `Button` Присоединил обработчик для этого события и отвечает, вызвав `CanExecute` еще раз, а затем Включение сам на основе возвращаемого значения этого метода.

При `execute` метод `NewCommand` вызовы `RefreshCanExecutes`, `NewCommand` свойство получает вызов `ChangeCanExecute`и `Button` вызовы `canExecute` метод, возвращающий теперь `false` поскольку `IsEditing`свойство больше не `true`.

`PropertyChanged` Обработчик для нового `PersonViewModel` вызывает `ChangeCanExecute` метод `SubmitCommand`. Ниже приведен реализации этого свойства команды.


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null && 
                       PersonEdit.Name != null && 
                       PersonEdit.Name.Length > 1 && 
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`canExecute` Функции для `SubmitCommand` вызывается каждый раз имеется свойство колебаниями `PersonViewModel` объект редактируется. Он возвращает `true` только если `Name` свойство имеет по крайней мере одного символа и `Age` больше 0. В этот момент **отправить** становится доступной кнопка. 

`execute` Функции для **отправить** Удаляет обработчик изменения свойства из `PersonViewModel`, добавляет объект в `Persons` коллекции и возвращает содержимое начального условия.

`execute` Функции для **отменить** кнопку поддерживает все возможности **отправить** execept выполняет кнопка Добавить объект в коллекцию:

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            }); 
    }

    ···

}
```

`canExecute` Возвращает `true` в любое время `PersonViewModel` редактируется.

Эти методы может быть адаптирован для более сложных сценариев: свойство в `PersonCollectionViewModel` может быть привязан к `SelectedItem` свойство `ListView` для редактирования существующих элементов и **удалить** удалось добавить кнопку для удаления Эти элементы.

Он не является обязательным для определения `execute` и `canExecute` методы как лямбда-функции. Можно записывать их в качестве обычных закрытых методов в ViewModel и ссылаться на них в `Command` конструкторы. Однако такой подход обычно выразиться в большом методов, которые упоминаются в ViewModel только один раз.

## <a name="using-command-parameters"></a>С помощью параметров команды

Иногда бывает удобно для одной или нескольких кнопок (или другие объекты пользовательского интерфейса) для одного и того же `ICommand` свойства ViewModel. В этом случае используется `CommandParameter` свойство для различения кнопок. 

Можно продолжать использовать `Command` класса для этих общих `ICommand` свойства. Этот класс определяет [альтернативный конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/) , принимающий `execute` и `canExecute` методов с параметрами типа `Object`. Это как `CommandParameter` передаваемый для этих методов.

Тем не менее при использовании `CommandParameter`, проще использовать универсальный [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) класса для указания типа объекта, равно `CommandParameter`. `execute` И `canExecute` методов, указанных вами имеют параметры этого типа.

**Десятичное клавиатуры** страницы демонстрирующий этот способ, показывая, как реализовать клавиатуре для ввода десятичных чисел. `BindingContext` Для `Grid` — `DecimalKeypadViewModel`. `Entry` Свойство этого ViewModel привязан к `Text` свойство `Label`. Все `Button` объекты привязаны к различным командам в ViewModel: `ClearCommand`, `BackspaceCommand`, и `DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>
        
        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2" 
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="8" />
        
        <Button Text="9"
                Grid.Row="2" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0" 
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1" 
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2" 
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

11 кнопки для 10 цифр и десятичной запятой имеют привязку к `DigitCommand`. `CommandParameter` Различать эти кнопки. Значение, `CommandParameter` обычно является таким же, как текст, отображаемый на кнопке, за исключением десятичного разделителя, который для большей ясности отображается с символом средней точки.

Вот программы в действии:

[![Десятичное число клавиатуры](commanding-images/decimalkeyboard-small.png "десятичное клавиатуры")](commanding-images/decimalkeyboard-large.png "десятичное клавиатуры")

Обратите внимание, что кнопки для десятичной запятой в все три снимки экрана отключен, так как введенное число уже содержит десятичную точку. 

`DecimalKeypadViewModel` Определяет `Entry` свойство типа `string` (который является единственным свойством, которое запускает `PropertyChanged` событий) и трех свойств типа `ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

Кнопка, соответствующий `ClearCommand` всегда включена и просто задает запись «0»:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···
    
    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···
    
}
```

Так как кнопка доступна всегда, не требуется указывать `canExecute` аргумент в `Command` конструктора.

Логику для ввода чисел и пробела является несколько сложным, так как если было введено без знаков, а затем `Entry` свойство представляет собой строку «0». Если пользователь вводит дополнительные нули, а затем `Entry` по-прежнему содержит только один ноль. Если пользователь вводит любой другая цифра, то эта цифра заменяет ноль. Однако если пользователь вводит десятичной запятой перед другая цифра, затем `Entry` -строка «0».

**Backspace** кнопка включена только в том случае, когда длина записи больше 1, или если `Entry` не равно «0» строки:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···
    
}
```

Логика `execute` функции для **Backspace** кнопку гарантирует, что `Entry` — по крайней мере строка «0».

`DigitCommand` Свойство привязано к 11 кнопок, каждый из которых определяет себя с `CommandParameter` свойство. `DigitCommand` Может принимать значение экземпляр обычного `Command` класса, но проще использовать `Command<T>` универсального класса. При использовании команд интерфейс с XAML, `CommandParameter` свойства обычно представляют собой строки, и это тип универсального аргумента. `execute` И `canExecute` затем имеют аргументы типа `string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···
    
}
```

`execute` Метод добавляет строковый аргумент `Entry` свойство. Тем не менее, если результат начинается с нуля (но не ноль и десятичной запятой) затем этого начального нуля необходимо удалить, используя `Substring` функции.

`canExecute` Возвращает `false` только в том случае, если аргумент является десятичной запятой (это означает, что нажатие десятичного разделителя) и `Entry` уже содержит десятичной запятой. 

Все `execute` вызов методов `RefreshCanExecutes`, который затем вызывает `ChangeCanExecute` для обоих `DigitCommand` и `ClearCommand`. Это гарантирует, что становятся активными кнопки backspace и десятичной запятой или отключена на основе текущей последовательности цифр, введенным.

## <a name="adding-commands-to-existing-views"></a>Добавление команд на существующие представления

Если вы хотите использовать интерфейс команд с представлениями, которые не поддерживают его, можно использовать поведение Xamarin.Forms, который преобразует события в команде. Это описано в статье [ **EventToCommandBehavior для повторного использования**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md).

## <a name="asynchronous-commanding-for-navigation-menus"></a>Асинхронные выполнение команд для меню навигации

Выполнение команд удобно для реализации меню навигации, например, в [ **демонстрации привязки данных** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) программах. Сюда входит в состав **MainPage.xaml**:


```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

При использовании с XAML, заставляя `CommandParameter` обычно задано значение строки. В этом случае, однако используется расширение разметки XAML, чтобы `CommandParameter` относится к типу `System.Type`.

Каждый `Command` свойство привязано к свойству с именем `NavigateCommand`. Свойство определено в файле кода **MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

Конструктор наборов `NavigateCommand` свойства `execute` метод, создающий экземпляр `System.Type` параметра и затем переходит к нему. Поскольку `PushAsync` вызов требует `await` оператор, `execute` метод должен быть помечен как асинхронный. Это осуществляется с помощью `async` ключевое слово перед списком параметров. 

Конструктор также задает `BindingContext` страницы к самому себе, что ссылки привязки `NavigateCommand` в этом классе.

Порядок код в этот конструктор устанавливает различие: `InitializeComponent` вызов приводит XAML для синтаксического анализа, но в это время привязки к свойству с именем `NavigateCommand` не может быть разрешен, поскольку `BindingContext` равно `null`. Если `BindingContext` задать в конструкторе *перед* `NavigateCommand` имеет значение, то привязки можно разрешить при `BindingContext` имеет значение, но в этот момент `NavigateCommand` по-прежнему `null`. Установка `NavigateCommand` после `BindingContext` не повлияет на привязку из-за изменений в `NavigateCommand` не запускает `PropertyChanged` события и привязку, не знает, что `NavigateCommand` теперь является допустимой.

Установка `NavigateCommand` и `BindingContext` (в любом порядке) до вызова `InitializeComponent` будут работать, поскольку оба компонента привязки заданы, когда средство синтаксического анализа XAML обнаруживает определение привязки. 

Привязки данных иногда может быть непростой задачей, но, как показано выше в этой серии статей, они мощным и гибким, поэтому помогают значительно организации кода, разделяя базовую логику из пользовательского интерфейса.



## <a name="related-links"></a>Связанные ссылки

- [Демонстрация привязки данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Данные привязки Глава из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
