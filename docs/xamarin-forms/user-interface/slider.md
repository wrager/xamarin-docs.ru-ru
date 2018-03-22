---
title: "С помощью ползунка"
description: "Используйте ползунок для выбора из диапазона непрерывных значений."
ms.topic: article
ms.prod: xamarin
ms.assetid: 36B1C645-26E0-4874-B6B6-BDBF77662878
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/16/2018
ms.openlocfilehash: ce5d8c46282d3831edcf58af63b66d1f6292f75b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="using-slider"></a>С помощью ползунка

_Используйте ползунок для выбора из диапазона непрерывных значений._

Xamarin.Forms [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) является горизонтальную панель, можно управлять, чтобы пользователь мог выбрать `double` значение из непрерывного диапазона. 

`Slider` Определяет трех свойств типа `double`:

- [`Minimum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Minimum/) имеет минимум диапазоне, значение по умолчанию 0.
- [`Maximum`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Maximum/) Представляет максимальное значение из диапазона, значение по умолчанию 1.
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Slider.Value/) имеет значение "ползунок", которое может находиться в диапазоне между `Minimum` и `Maximum` и имеет значение по умолчанию 0.

Все три свойства, поддерживаемых `BindableProperty` объектов. `Value` Свойство имеет режима привязки по умолчанию `BindingMode.TwoWay`, это означает, что это может использоваться как источник привязки в приложение, использующее [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) архитектуры. 

> [!WARNING]
> На внутреннем уровне `Slider` гарантирует, что `Minimum` — меньше, чем `Maximum`. Если `Minimum` или `Maximum` никогда не задаются, чтобы `Minimum` — не менее `Maximum`, возникает исключение. В разделе [ **меры предосторожности** ](#precautions) , в разделе ниже дополнительные сведения о подготовке `Minimum` и `Maximum` свойства.

`Slider` Приводит `Value` свойство, чтобы оно было между `Minimum` и `Maximum`включительно. Если `Minimum` свойству присвоено значение больше, чем `Value` свойства `Slider` задает `Value` свойства `Minimum`. Аналогично Если `Maximum` присвоено значение меньше, чем `Value`, затем `Slider` задает `Value` свойства `Maximum`. 

`Slider` Определяет [ `ValueChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Slider.ValueChanged/) событие, возникающее, когда `Value` изменения, либо с помощью пользователя обработку `Slider` или когда программа задает `Value` свойство напрямую. Объект `ValueChanged` событие также создается, когда `Value` приводится как описано в предыдущем абзаце. 

[ `ValueChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ValueChangedEventArgs/) Объекта, который должен сопровождать `ValueChanged` событий имеет два свойства, оба типа `double`: [ `OldValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.OldValue/) и [ `NewValue` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ValueChangedEventArgs.NewValue/). Во время события, значение `NewValue` совпадает со значением `Value` свойство `Slider` объекта.

> [!WARNING]
> Не используйте параметры неограниченного горизонтальное расположение `Center`, `Start`, или `End` с `Slider`. На Android и UWP `Slider` раскроется в строку нулевой длины, а также на iOS, строке имеет небольшой размер. Оставьте значение по умолчанию `HorizontalOptions` параметра `Fill`и не использовать ширину `Auto` при размещении `Slider` в `Grid` макета.

## <a name="basic-slider-code-and-markup"></a>Базовый код ползунка и разметки

[ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) пример начинается с тремя страницами, функционально идентичны, но реализован по-разному. Первая страница используется только код C#, вторая использует XAML с обработчиком событий в коде и третий — избежать обработчик событий с помощью привязки данных в файле XAML.

### <a name="creating-a-slider-in-code"></a>Создание ползунка в коде

**Базовый код ползунок** страницы в [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) приведен пример Показать создание `Slider` и два `Label` объектов в коде:

```csharp
public class BasicSliderCodePage : ContentPage
{
    public BasicSliderCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Slider slider = new Slider
        {
            Maximum = 360
        };
        slider.ValueChanged += (sender, args) =>
        {
            rotationLabel.Rotation = slider.Value;
            displayLabel.Text = String.Format("The Slider value is {0}", args.NewValue);
        };

        Title = "Basic Slider Code";
        Padding = new Thickness(10, 0);
        Content = new StackLayout
        {
            Children =
            {
                rotationLabel,
                slider,
                displayLabel
            }
        };
    }
}
```

`Slider` Инициализируется имеют `Maximum` свойство 360. `ValueChanged` Обработчик `Slider` использует `Value` свойство `slider` объекта, чтобы зафиксировать `Rotation` свойства первого `Label` и использует `String.Format` метод с `NewValue` свойство аргументы события, чтобы задать `Text` свойство второго `Label`. Эти два подхода, чтобы получить текущее значение `Slider` являются взаимозаменяемыми. 

Вот программу на Android, iOS и универсальной платформы Windows (UWP) устройств.

[![Код основные ползунок](slider-images/BasicSliderCode.png "основные ползунок кода")](slider-images/BasicSliderCode-Large.png#lightbox)

Второй `Label` отображает текст «(неинициализированные)» до `Slider` будут обрабатывать, каких случаях первый `ValueChanged` событие. Обратите внимание, что число десятичных разрядов, которые отображаются различные для трех платформ. Эти отличия связаны реализации платформы `Slider` , которые рассматриваются далее в этой статье в разделе [различий в реализации платформы](#implementations).

### <a name="creating-a-slider-in-xaml"></a>Создание ползунка в XAML

**Базового кода XAML ползунок** страница функционально является таким же, как **базовый код ползунок** Однако осуществляются в основном XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderXamlPage"
             Title="Basic Slider XAML"
             Padding="10, 0">
    <StackLayout>
        <Label x:Name="rotatingLabel" 
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider Maximum="360"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

Файл кода содержит обработчик `ValueChanged` событий:

```csharp
public partial class BasicSliderXamlPage : ContentPage
{
    public BasicSliderXamlPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        double value = args.NewValue;
        rotatingLabel.Rotation = value;
        displayLabel.Text = String.Format("The Slider value is {0}", value);
    }
}
```

Можно также для обработчика событий для получения `Slider` , он генерирует события с помощью `sender` аргумент. `Value` Свойство содержится текущее значение:

```csharp
double value = ((Slider)sender).Value;
```

Если `Slider` объекта было присвоено имя, в XAML-файле с `x:Name` атрибут (например, «ползунок»), а затем обработчик событий может ссылаться на этот объект напрямую:

```csharp
double value = slider.Value;
```

### <a name="data-binding-the-slider"></a>Ползунок привязка данных

**Основных привязок ползунок** страницы показано, как написать практически эквивалентные программу, которая исключает `Value` обработчик событий с помощью [привязки данных](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.BasicSliderBindingsPage"
             Title="Basic Slider Bindings"
             Padding="10, 0">
    <StackLayout>
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference slider}, 
                                  Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                Maximum="360" />

        <Label x:Name="displayLabel"
               Text="{Binding Source={x:Reference slider}, 
                              Path=Value, 
                              StringFormat='The Slider value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

`Rotation` Свойства первого `Label` привязан к `Value` свойство `Slider`, как и `Text` свойство второго `Label` с `StringFormat` спецификации. **Основных привязок ползунок** страничные функции немного по-разному в двух предыдущих страницах: при первом отображении страницы, второй `Label` отображает текстовую строку со значением. Это является преимуществом с использованием привязки данных. Для отображения текста без привязки данных, необходимо специально инициализировать `Text` свойство `Label` или имитации срабатывание `ValueChanged` события путем вызова обработчика событий из конструктора класса.

<a name="precautions" />

## <a name="precautions"></a>Меры предосторожности

Значение `Minimum` свойства всегда должно быть меньше, чем значение `Maximum` свойства. В следующем примере кода причины фрагмент `Slider` для вызова исключения:

```csharp
// Throws an exception!
Slider slider = new Slider
{
    Minimum = 10,
    Maximum = 20
};
```

Компилятор C# создает код, который задает эти свойства в последовательности, и когда `Minimum` свойство имеет значение 10, больше, чем значение по умолчанию `Maximum` значение 1. В этом случае исключения можно избежать, задав `Maximum` свойства первого:

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Установка `Maximum` 20 не является проблемой, так как оно превышает значение по умолчанию `Minimum` значение 0. Когда `Minimum` не установлен, значение меньше, чем `Maximum` значение, равное 20.

Та же проблема существует в XAML. Задайте свойства в порядке, который гарантирует, что `Maximum` больше всегда `Minimum`:

```xaml
<Slider Maximum="20"
        Minimum="10" ... />
```

Можно задать `Minimum` и `Maximum` значения для отрицательных чисел, но только в порядке, где `Minimum` — всегда меньше, чем `Maximum`:

```xaml
<Slider Minimum="-20"
        Maximum="-10" ... />
```

`Value` Свойства всегда больше или равно `Minimum` значение и меньше или равно `Maximum`. Если `Value` задано значение за пределами этого диапазона, значение будет преобразовано в находиться в диапазоне, но исключение не вызывается. Например, этот код будет *не* исключение:

```csharp
Slider slider = new Slider
{
    Value = 10
};
```

Вместо этого `Value` приводится к типу `Maximum` значение 1.

Ниже приведен фрагмент кода, показанный выше. 

```csharp
Slider slider = new Slider
{
    Maximum = 20,
    Minimum = 10
};
```

Когда `Minimum` имеет значение 10, а затем `Value` также имеет значение 10. 

Если `ValueChanged` во время был присоединен обработчик событий, `Value` приводится к типу нечто, отличное от значения по умолчанию 0, то `ValueChanged` событие. Ниже приведен фрагмент кода XAML. 

```xaml
<Slider ValueChanged="OnSliderValueChanged"
        Maximum="20"
        Minimum="10" />
```

Когда `Minimum` равно 10, `Value` также имеет значение 10 и `ValueChanged` событие. Это может произойти, прежде чем остальной части страницы был построен и обработчик может пытаться ссылаться на другие элементы на странице, которые не были созданы. Может потребоваться добавить некоторый код для `ValueChanged` обработчик, который проверяет наличие `null` значения других элементов на странице. Также можно задать `ValueChanged` обработчик событий после `Slider` были инициализированы значения.

<a name="implementations" />

## <a name="platform-implementation-differences"></a>Различия в реализации платформы

Снимки экрана, показанного выше отобразить значение `Slider` с разных количество знаков после запятой. Это относится к как `Slider` реализуется на платформах Android и UWP.

### <a name="the-android-implementation"></a>Реализация Android 

Реализация Android `Slider` основан на Android [ `SeekBar` ](https://developer.xamarin.com/api/type/Android.Widget.SeekBar/) всегда устанавливается [ `Max` ](https://developer.xamarin.com/api/property/Android.Widget.ProgressBar.Max/) свойство до 1000. Это означает, что `Slider` на Android имеет только 1001 дискретные значения. Если задать `Slider` иметь `Minimum` 0 и `Maximum` 5000, а затем как `Slider` будут обрабатывать, `Value` свойство может принимать значения от 0, 5, 10, 15 и т. д. 

### <a name="the-uwp-implementation"></a>Реализация UWP

Реализация UWP `Slider` основана на платформе UWP [ `Slider` ](/uwp/api/windows.ui.xaml.controls.slider) управления. `StepFrequency` Свойство UWP `Slider` равно разность `Maximum` и `Minimum` свойства, разделенное на 10, но не больше 1. 

Например, диапазон от 0 до 1, по умолчанию `StepFrequency` задано значение 0,1. Как `Slider` будут обрабатывать, `Value` свойство доступно только для 0, 0.1, 0.2, 0.3, 0,4, 0,5, 0,6, 0,7, 0,8, 0.9, а 1.0. (Это заметно на последней странице в [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) образца.) Когда разницу между `Maximum` и `Minimum` свойства 10 или более поздней, затем `StepFrequency` имеет значение 1 и `Value` имеет целочисленного значения. 

### <a name="the-stepslider-solution"></a>Решение StepSlider

Более универсально `StepSlider` рассматривается в [Глава 27. Пользовательские модули подготовки отчетов](https://xamarin.azureedge.net/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf) книги *Создание мобильных приложений с помощью Xamarin.Forms*. `StepSlider` Аналогичен `Slider` , но добавляет `Steps` свойство, чтобы указать количество значений между `Minimum` и `Maximum`.

## <a name="sliders-for-color-selection"></a>Ползунки для выбора цвета

Последние два страниц в [ **SliderDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) образец используются три `Slider` экземпляров для выбора цвета. Первая страница обрабатывает все диалоги в файле кода, в то время второй странице показано, как использовать привязку данных с ViewModel. 

### <a name="handling-sliders-in-the-code-behind-file"></a>Обработка ползунков в файле кода 

**Ползунки цвета RGB** создает страницу `BoxView` для отображения цвета, три `Slider` экземпляров для выбора компонентов красного, зеленого и синего цвета и три `Label` элементы для отображения этих цвета значения:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="SliderDemos.RgbColorSlidersPage"
             Title="RGB Color Sliders">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Slider">
                <Setter Property="Maximum" Value="255" />
            </Style>
            
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView x:Name="boxView"
                 Color="Black"
                 VerticalOptions="FillAndExpand" />

        <Slider x:Name="redSlider"
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="redLabel" />

        <Slider x:Name="greenSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="greenLabel" />

        <Slider x:Name="blueSlider" 
                ValueChanged="OnSliderValueChanged" />

        <Label x:Name="blueLabel" />
    </StackLayout>
</ContentPage>
```

Объект `Style` предоставляет все три `Slider` элементов в диапазоне от 0 до 255. `Slider` Элементы одного и того же `ValueChanged` обработчик, который реализован в файле кода программной части:

```csharp
public partial class RgbColorSlidersPage : ContentPage
{
    public RgbColorSlidersPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        if (sender == redSlider)
        {
            redLabel.Text = String.Format("Red = {0:X2}", (int)args.NewValue);
        }
        else if (sender == greenSlider)
        {
            greenLabel.Text = String.Format("Green = {0:X2}", (int)args.NewValue);
        }
        else if (sender == blueSlider)
        {
            blueLabel.Text = String.Format("Blue = {0:X2}", (int)args.NewValue);
        }

        boxView.Color = Color.FromRgb((int)redSlider.Value,
                                      (int)greenSlider.Value,
                                      (int)blueSlider.Value);
    }
}
```

Первый раздел наборы `Text` свойство одного из `Label` экземпляров Короткая текстовая строка, указывающее значение `Slider` в шестнадцатеричном формате. Затем все три `Slider` экземпляры доступны для создания `Color` значение из RGB-компоненты:

[![Ползунки цвета RGB](slider-images/RgbColorSliders.png "RGB шкалы")](slider-images/RgbColorSliders-Large.png#lightbox)

### <a name="binding-the-slider-to-a-viewmodel"></a>Привязка к ViewModel ползунок

**HSL шкалы** страницы показано, как использовать ViewModel для выполнения вычислений, используемый для создания `Color` значение на основе значений цветового тона, насыщенности и яркости. Как и все ViewModels `HSLColorViewModel` класс реализует `INotifyPropertyChanged` интерфейс и активируется `PropertyChanged` событие при изменении одного из свойств:

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;

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
            }
        }
        get
        {
            return color;
        }
    }
}
```

ViewModels и `INotifyPropertyChanged` интерфейс рассматриваются в статье [привязки данных](~/xamarin-forms/app-fundamentals/data-binding/index.md).

**HslColorSlidersPage.xaml** создает файл `HslColorViewModel` и устанавливает его на страницу `BindingContext` свойство. Таким образом, все элементы в файле XAML для привязки к свойствам в ViewModel.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:SliderDemos"
             x:Class="SliderDemos.HslColorSlidersPage"
             Title="HSL Color Sliders">

    <ContentPage.BindingContext>
        <local:HslColorViewModel Color="Chocolate" />
    </ContentPage.BindingContext>

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="HorizontalTextAlignment" Value="Center" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10">
        <BoxView Color="{Binding Color}"
                 VerticalOptions="FillAndExpand" />

        <Slider Value="{Binding Hue}" />
        <Label Text="{Binding Hue, StringFormat='Hue = {0:F2}'}" />

        <Slider Value="{Binding Saturation}" />
        <Label Text="{Binding Saturation, StringFormat='Saturation = {0:F2}'}" />

        <Slider Value="{Binding Luminosity}" />
        <Label Text="{Binding Luminosity, StringFormat='Luminosity = {0:F2}'}" />
    </StackLayout>
</ContentPage> 
```

Как `Slider` элементов управления этим состоянием `BoxView` и `Label` элементы будут обновлены из ViewModel:

[![Шкалы HSL](slider-images/HslColorSliders.png "HSL шкалы")](slider-images/HslColorSliders-Large.png#lightbox)

`StringFormat` Компонент `Binding` расширение разметки установлен для формата «F2» для отображения двух знаков после запятой. (Строка форматирования в привязках данных рассматривается в статье [форматирования строк](~/xamarin-forms/app-fundamentals/data-binding/string-formatting.md).) Однако значения 0, 0.1, 0.2, ограничено версии UWP программы... 0,9 и 1.0. Это прямой результат реализации UWP `Slider` как описано выше в разделе [различий в реализации платформы](#implementations).

## <a name="related-links"></a>Связанные ссылки

- [Пример демонстрации ползунка](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos)
- [Ползунок API](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)
