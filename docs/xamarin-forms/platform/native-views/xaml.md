---
title: Собственные представления в XAML
description: Собственные представления из iOS, Android и универсальная платформа Windows могут существовать прямые ссылки из Xamarin.Forms XAML-файлов. Свойства и обработчики событий могут быть установлены для собственного представления, и они могут взаимодействовать с Xamarin.Forms представления. В этой статье показано, как использовать собственные представления из Xamarin.Forms XAML-файлов.
ms.prod: xamarin
ms.assetid: 7A856D31-B300-409E-9AEB-F8A4DB99B37E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/24/2016
ms.openlocfilehash: 6dbad7352a089f482fa3a396505507da58771cef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="native-views-in-xaml"></a>Собственные представления в XAML

_Собственные представления из iOS, Android и универсальная платформа Windows могут существовать прямые ссылки из Xamarin.Forms XAML-файлов. Свойства и обработчики событий могут быть установлены для собственного представления, и они могут взаимодействовать с Xamarin.Forms представления. В этой статье показано, как использовать собственные представления из Xamarin.Forms XAML-файлов._

В этой статье рассматриваются следующие вопросы:

- [Использование собственного представления](#consuming) — процесс для использования собственного представления из XAML.
- [Использование собственного привязок](#native_bindings) — привязка к и из свойств собственного представления данных.
- [Передача аргументов в собственном представления](#passing_arguments) — передача аргументов в собственных конструкторов и вызова собственных методов фабрики.
- [Ссылается на собственном представления из кода](#native_view_code) — получение собственных экземпляров объявлен в XAML-файл, из соответствующего файла кода.
- [Создание подклассов собственного представления](#subclassing) — Создание подкласса собственного представления для определения API понятного имени XAML.  

<a name="overview" />

## <a name="overview"></a>Обзор

Чтобы внедрить собственного представления в файл Xamarin.Forms XAML:

1. Добавить `xmlns` объявление пространства имен в XAML-файле пространство имен, содержащее собственного представления.
1. Создайте экземпляр собственного представления в XAML-файле.

> [!NOTE]
> XAMLC необходимо отключить для всех страниц XAML, использующие собственные представления.

Для ссылки собственного представления из файла кода, необходимо использовать общие средства проекта (SAP) и перенос кода под конкретную платформу с директивами условной компиляции. Дополнительные сведения см. [ссылками собственного представления из кода](#native_view_code).

<a name="consuming" />

## <a name="consuming-native-views"></a>Использование собственного представления

В следующем примере кода демонстрируется использование собственного представления для каждой платформы, чтобы Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        x:Class="NativeViews.NativeViewDemo">
    <StackLayout Margin="20">
        <ios:UILabel Text="Hello World" TextColor="{x:Static ios:UIColor.Red}" View.HorizontalOptions="Start" />
        <androidWidget:TextView Text="Hello World" x:Arguments="{x:Static androidLocal:MainActivity.Instance}" />
        <win:TextBlock Text="Hello World" />
    </StackLayout>
</ContentPage>
```

А также указание `clr-namespace` и `assembly` для пространства имен собственных, `targetPlatform` также должен быть указан. Это должно быть присвоено одно из значений [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) перечисления и обычно устанавливается в `iOS`, `Android`, или `Windows`. Во время выполнения, синтаксического анализа XAML будет игнорировать любые префиксы пространства имен XML, имеющие `targetPlatform` , не соответствует платформы, на котором выполняется приложение.

Каждое объявление пространства имен можно использовать для ссылки из указанного пространства имен любого класса или структуры. Например `ios` объявление пространства имен можно использовать для ссылки на любой класс или структура с iOS `UIKit` пространства имен. Можно задать свойства собственного представления с помощью XAML, но типы свойств и объектов, должны совпадать. Например `UILabel.TextColor` свойству `UIColor.Red` с помощью `x:Static` расширения разметки и `ios` пространства имен.

Привязываемые свойства и присоединенного привязываемые свойства можно также задать для собственного представлений с помощью `Class.BindableProperty="value"` синтаксиса. Каждое представление собственного упаковывается в конкретную платформу `NativeViewWrapper` экземпляр, который является производным от [ `Xamarin.Forms.View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класса. Установка в собственном представлении привязываемые свойства или вложенное свойство привязываемых передает значение свойства оболочки. Например, можно указать по центру горизонтальном расположении, задав `View.HorizontalOptions="Center"` собственного представления.

> [!NOTE]
> Обратите внимание, что стили нельзя использовать с представлениями в машинном коде, поскольку стили можно только свойства, которые архивируются `BindableProperty` объектов.

Конструкторы Android мини-приложения, как правило, требуют Android `Context` как аргумент и это можно сделать доступными через статическое свойство в `MainActivity` класса. Таким образом, при создании мини-приложение Android в XAML, `Context` объект обычно должен быть передан в конструктор мини-приложения с помощью `x:Arguments` атрибутом `x:Static` расширения разметки. Дополнительные сведения см. в разделе [передачи аргументов собственного представления](#passing_arguments).

> [!NOTE]
> Обратите внимание, именование собственного представления с `x:Name` не поддерживается в проекте переносимой библиотеки классов (PCL) или общего ресурса проекта (SAP). Это приведет к созданию переменной собственный тип, что вызовет ошибку компиляции. Однако собственного представления может быть заключен в `ContentView` экземпляров и получить в файле кода, при условии, что используется SAP. Дополнительные сведения см. в разделе [ссылками собственного представления из кода](#native_view_code).

<a name="native_bindings" />

## <a name="native-bindings"></a>Собственный привязок

Привязка данных используется для синхронизации пользовательского интерфейса со своим источником данных и упрощает приложения Xamarin.Forms отображает и взаимодействует с его данными. Условии, что объект источника реализует `INotifyPropertyChanged` изменения в интерфейса *источника* объект автоматически переносятся в *целевой* объекта привязки framework и изменений в *целевой* объекта можно при необходимости принудительно отправить *источника* объекта.

Свойства собственного представления можно также использовать привязку данных. В следующем примере кода демонстрируется привязка данных, с помощью свойств собственного представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:win="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeSwitch"
        x:Class="NativeSwitch.NativeSwitchPage">
    <StackLayout Margin="20">
        <Label Text="Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <Entry Placeholder="This Entry is bound to the native switch" IsEnabled="{Binding IsSwitchOn}" />
        <ios:UISwitch On="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=ValueChanged}"
            OnTintColor="{x:Static ios:UIColor.Red}"
            ThumbTintColor="{x:Static ios:UIColor.Blue}" />
        <androidWidget:Switch x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            Checked="{Binding Path=IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=CheckedChange}"
            Text="Enable Entry?" />
        <win:ToggleSwitch Header="Enable Entry?"
            OffContent="No"
            OnContent="Yes"
            IsOn="{Binding IsSwitchOn, Mode=TwoWay, UpdateSourceEventName=Toggled}" />
    </StackLayout>
</ContentPage>

```

На этой странице содержатся [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) которого [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) привязывает свойство `NativeSwitchPageViewModel.IsSwitchOn` свойство. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Страницы устанавливается в новый экземпляр `NativeSwitchPageViewModel` класс в файле кода с ViewModel класс, реализующий `INotifyPropertyChanged` интерфейса.

Данная страница также содержит собственный ключ для каждой платформы. Использует каждого встроенного коммутатора [ `TwoWay` ](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) привязки, чтобы обновить значение `NativeSwitchPageViewModel.IsSwitchOn` свойства. Таким образом, если параметр отключен, `Entry` отключена, и если включен параметр `Entry` включен. Следующих снимках экрана отображение этой функции на каждой платформе:

![](xaml-images/native-switch-disabled.png "Отключено встроенного коммутатора")
![](xaml-images/native-switch-enabled.png "встроенного коммутатора включена")

Условии, что реализует встроенного свойства автоматически поддерживаются двусторонней привязки `INotifyPropertyChanged`, поддерживает наблюдение за ключ-значение (KVO) для операций ввода-вывода или является `DependencyProperty` на UWP. Тем не менее многие собственного представления не поддерживают уведомления об изменении свойства. Эти представления можно указать [ `UpdateSourceEventName` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.UpdateSourceEventName/) значение свойства как часть выражения привязки. Это свойство должно быть присвоено имя события в представлении native, который сигнализирует при изменении целевого свойства. Затем, когда значение встроенного коммутатора изменяется, `Binding` класс получает уведомление, что пользователь изменил значение переключателя и `NativeSwitchPageViewModel.IsSwitchOn` обновляется значение свойства.

<a name="passing_arguments" />

## <a name="passing-arguments-to-native-views"></a>Передача аргументов собственного представления

Можно передать аргументы конструктора собственного представления, используя `x:Arguments` атрибутом `x:Static` расширения разметки. Кроме того, собственных фабричные методы (`public static` методы, возвращающие объекты или значения того же типа, как класс или структура, определяющая методы) может быть вызвана путем указания метода имя с помощью `x:FactoryMethod` атрибута и его аргументы с помощью `x:Arguments` атрибута.

В следующем примере кода показаны оба способа:

```xaml
<ContentPage ...
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidGraphics="clr-namespace:Android.Graphics;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winMedia="clr-namespace:Windows.UI.Xaml.Media;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winText="clr-namespace:Windows.UI.Text;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:winui="clr-namespace:Windows.UI;assembly=Windows, Version=255.255.255.255, Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows">
        ...
        <ios:UILabel Text="Simple Native Color Picker" View.HorizontalOptions="Center">
            <ios:UILabel.Font>
                <ios:UIFont x:FactoryMethod="FromName">
                    <x:Arguments>
                        <x:String>Papyrus</x:String>
                        <x:Single>24</x:Single>
                    </x:Arguments>
                </ios:UIFont>
            </ios:UILabel.Font>
        </ios:UILabel>
        <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                    Text="Simple Native Color Picker"
                    TextSize="24"
                    View.HorizontalOptions="Center">
            <androidWidget:TextView.Typeface>
                <androidGraphics:Typeface x:FactoryMethod="Create">
                    <x:Arguments>
                        <x:String>cursive</x:String>
                        <androidGraphics:TypefaceStyle>Normal</androidGraphics:TypefaceStyle>
                    </x:Arguments>
                </androidGraphics:Typeface>
            </androidWidget:TextView.Typeface>
        </androidWidget:TextView>
        <winControls:TextBlock Text="Simple Native Color Picker"
                    FontSize="20"
                    FontStyle="{x:Static winText:FontStyle.Italic}"
                    View.HorizontalOptions="Center">
            <winControls:TextBlock.FontFamily>
                <winMedia:FontFamily>
                    <x:Arguments>
                        <x:String>Georgia</x:String>
                    </x:Arguments>
                </winMedia:FontFamily>
            </winControls:TextBlock.FontFamily>
        </winControls:TextBlock>
        ...
</ContentPage>
```

[ `UIFont.FromName` ](https://developer.xamarin.com/api/member/UIKit.UIFont.FromName/) Фабричный метод используется для задания [ `UILabel.Font` ](https://developer.xamarin.com/api/property/UIKit.UILabel.Font/) для нового [ `UIFont` ](https://developer.xamarin.com/api/type/UIKit.UIFont/) в iOS. `UIFont` С аргументами метода, которые являются потомками указаны имя и размер `x:Arguments` атрибута.

[ `Typeface.Create` ](https://developer.xamarin.com/api/member/Android.Graphics.Typeface.Create/p/System.String/Android.Graphics.TypefaceStyle/) Фабричный метод используется для задания [ `TextView.Typeface` ](https://developer.xamarin.com/api/property/Android.Widget.TextView.Typeface/) для нового [ `Typeface` ](https://developer.xamarin.com/api/type/Android.Graphics.Typeface/) на Android. `Typeface` С аргументами метода, которые являются потомками указаны имя семейства и стиля `x:Arguments` атрибута.

[ `FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.fontfamily) Конструктор используется для задания [ `TextBlock.FontFamily` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.fontfamily) для нового `FontFamily` на универсальной платформы Windows (UWP). `FontFamily` Имя задано аргумента метода, который является дочерним элементом `x:Arguments` атрибута.

> [!NOTE]
> Аргументы должны совпадать с типами, необходимый для метода, конструктора или фабрики.

Следующих снимках экрана отобразится результат аргументы фабрики метод и конструктор для задания шрифта на различные собственного представления:

![](xaml-images/passing-arguments.png "Настройка шрифтов для собственного представлений")

Дополнительные сведения о передача аргументов в XAML см. в разделе [передачи аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md).

<a name="native_view_code" />

## <a name="referring-to-native-views-from-code"></a>Ссылки на собственном представления из кода

Несмотря на то, что невозможно присвоить имя собственного представления с `x:Name` атрибут, ее можно вернуть собственного представления экземпляра, объявленных в файле XAML из соответствующего файла кода в проекте общего доступа, при условии, что собственного представления является дочерним элементом [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) , указывающий `x:Name` значение атрибута. Затем внутри директивы условной компиляции в файле кода должен выполнить следующее.

1. Получить [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) свойство значение и выполнить его приведение к определенной платформы `NativeViewWrapper` типа.
1. Получить `NativeViewWrapper.NativeElement` свойство и приведите его к типу собственного представления.

Собственный API можно вызвать для собственного представления для выполнения требуемой операции. Такой подход также предлагает преимущество в том, что несколько представлений собственного XAML для различных платформ могут быть дочерние элементы одной и той же [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Этот метод продемонстрирован в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:androidWidget="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:NativeViewInsideContentView"
        x:Class="NativeViewInsideContentView.NativeViewInsideContentViewPage">
    <StackLayout Margin="20">
        <ContentView x:Name="contentViewTextParent" HorizontalOptions="Center" VerticalOptions="CenterAndExpand">
            <ios:UILabel Text="Text in a UILabel" TextColor="{x:Static ios:UIColor.Red}" />
            <androidWidget:TextView x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Text in a TextView" />
            <winControls:TextBlock Text="Text in a TextBlock" />
        </ContentView>
        <ContentView x:Name="contentViewButtonParent" HorizontalOptions="Center" VerticalOptions="EndAndExpand">
            <ios:UIButton TouchUpInside="OnButtonTap" View.HorizontalOptions="Center" View.VerticalOptions="Center" />
            <androidWidget:Button x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
                Text="Scale and Rotate Text"
                Click="OnButtonTap" />
            <winControls:Button Content="Scale and Rotate Text" />
        </ContentView>
    </StackLayout>
</ContentPage>
```

В приведенном выше примере собственного представления для каждой платформы являются потомками [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) элементов управления, с `x:Name` , используемого для получения значения атрибута `ContentView` в код программной части:

```csharp
public partial class NativeViewInsideContentViewPage : ContentPage
{
    public NativeViewInsideContentViewPage()
    {
        InitializeComponent();

#if __IOS__
        var wrapper = (Xamarin.Forms.Platform.iOS.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (UIKit.UIButton)wrapper.NativeView;
        button.SetTitle("Scale and Rotate Text", UIKit.UIControlState.Normal);
        button.SetTitleColor(UIKit.UIColor.Black, UIKit.UIControlState.Normal);
#endif
#if __ANDROID__
        var wrapper = (Xamarin.Forms.Platform.Android.NativeViewWrapper)contentViewTextParent.Content;
        var textView = (Android.Widget.TextView)wrapper.NativeView;
        textView.SetTextColor(Android.Graphics.Color.Red);
#endif
#if WINDOWS_UWP
        var textWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewTextParent.Content;
        var textBlock = (Windows.UI.Xaml.Controls.TextBlock)textWrapper.NativeElement;
        textBlock.Foreground = new Windows.UI.Xaml.Media.SolidColorBrush(Windows.UI.Colors.Red);
        var buttonWrapper = (Xamarin.Forms.Platform.UWP.NativeViewWrapper)contentViewButtonParent.Content;
        var button = (Windows.UI.Xaml.Controls.Button)buttonWrapper.NativeElement;
        button.Click += (sender, args) => OnButtonTap(sender, EventArgs.Empty);
#endif
    }

    async void OnButtonTap(object sender, EventArgs e)
    {
        contentViewButtonParent.Content.IsEnabled = false;
        contentViewTextParent.Content.ScaleTo(2, 2000);
        await contentViewTextParent.Content.RotateTo(360, 2000);
        contentViewTextParent.Content.ScaleTo(1, 2000);
        await contentViewTextParent.Content.RelRotateTo(360, 2000);
        contentViewButtonParent.Content.IsEnabled = true;
    }
}
```

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Доступ к свойству для получения оболочки собственного представления в виде определяемых платформой `NativeViewWrapper` экземпляра. `NativeViewWrapper.NativeElement` Для получения собственного представления в качестве собственного типа затем обратиться к свойству. API собственного представления затем вызывается для выполнения требуемой операции.

IOS и Android системные кнопки одного и того же `OnButtonTap` обработчика событий, так как каждая кнопка собственного потребляет `EventHandler` делегата в ответ на события касания. Однако универсальной платформы Windows (UWP) использует отдельную `RoutedEventHandler`, который в свою очередь использует `OnButtonTap` обработчик событий в этом примере. Таким образом, при нажатии кнопки собственного `OnButtonTap` выполняется обработчик событий, который масштабирует и поворачивает собственного элемента управления, содержащегося в [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) с именем `contentViewTextParent`. Следующих снимках экрана демонстрируют это происходит на каждой платформе.

![](xaml-images/contentview.png "ContentView, содержащий собственном элементе управления")

<a name="subclassing" />

## <a name="subclassing-native-views"></a>Создание подклассов собственного представления

Многие iOS и Android собственного представления не подходят для экземпляров в XAML, так как они используют методы, а не свойства, для настройки элемента управления. Решение этой проблемы является подклассом собственного представления в оболочек, которые определяют дополнительные API понятного имени XAML, использует свойства для настройки элемента управления, и события независимая от платформы. Упакованное собственного представления можно поместить в общий активов проекта (SAP) и окружено директив условной компиляции или помещены в проекты под конкретные платформы и ссылка из XAML-кода в проекте переносимой библиотеки классов (PCL).

В следующем примере кода демонстрируется страницы Xamarin.Forms, который использует подкласса собственного представления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:ios="clr-namespace:UIKit;assembly=Xamarin.iOS;targetPlatform=iOS"
        xmlns:iosLocal="clr-namespace:SubclassedNativeControls.iOS;assembly=SubclassedNativeControls.iOS;targetPlatform=iOS"
        xmlns:android="clr-namespace:Android.Widget;assembly=Mono.Android;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SimpleColorPicker.Droid;assembly=SimpleColorPicker.Droid;targetPlatform=Android"
        xmlns:androidLocal="clr-namespace:SubclassedNativeControls.Droid;assembly=SubclassedNativeControls.Droid;targetPlatform=Android"
        xmlns:winControls="clr-namespace:Windows.UI.Xaml.Controls;assembly=Windows, Version=255.255.255.255,
            Culture=neutral, PublicKeyToken=null, ContentType=WindowsRuntime;targetPlatform=Windows"
        xmlns:local="clr-namespace:SubclassedNativeControls"
        x:Class="SubclassedNativeControls.SubclassedNativeControlsPage">
    <StackLayout Margin="20">
        <Label Text="Subclassed Native Views Demo" FontAttributes="Bold" HorizontalOptions="Center" />
        <StackLayout Orientation="Horizontal">
          <Label Text="You have chosen:" />
          <Label Text="{Binding SelectedFruit}" />      
        </StackLayout>
        <iosLocal:MyUIPickerView ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectedItemChanged}" />
        <androidLocal:MySpinner x:Arguments="{x:Static androidLocal:MainActivity.Instance}"
            ItemsSource="{Binding Fruits}"
            SelectedObject="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=ItemSelected}" />
        <winControls:ComboBox ItemsSource="{Binding Fruits}"
            SelectedItem="{Binding SelectedFruit, Mode=TwoWay, UpdateSourceEventName=SelectionChanged}" />
    </StackLayout>
</ContentPage>
```

На этой странице содержатся [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , отображающий фруктов, выбранного пользователем из собственного элемента управления. `Label` Привязывает `SubclassedNativeControlsPageViewModel.SelectedFruit` свойство. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Страницы устанавливается в новый экземпляр `SubclassedNativeControlsPageViewModel` класс в файле кода с ViewModel класс, реализующий `INotifyPropertyChanged` интерфейса.

Данная страница также содержит собственный выбор представления для каждой платформы. Каждое собственного представление показывает коллекцию фруктов путем привязки его `ItemSource` свойства `SubclassedNativeControlsPageViewModel.Fruits` коллекции. Это позволяет пользователю выбрать фруктов, как показано на следующем снимке экрана:

![](xaml-images/sub-classed.png "Подклассов собственного представления")

В iOS и Android native выбора использовать методы для настройки элементов управления. Таким образом эти средства выбора должен быть подклассом предоставляют свойства, чтобы сделать их понятного имени XAML. В универсальной платформы Windows (UWP), `ComboBox` уже понятного имени XAML и поэтому не требует создания подкласса.

### <a name="ios"></a>iOS

Подклассы реализации iOS [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) представление и предоставляет свойства и события, можно легко использовать из XAML-кода:

```csharp
public class MyUIPickerView : UIPickerView
{
    public event EventHandler<EventArgs> SelectedItemChanged;

    public MyUIPickerView()
    {
        var model = new PickerModel();
        model.ItemChanged += (sender, e) =>
        {
            if (SelectedItemChanged != null)
            {
                SelectedItemChanged.Invoke(this, e);
            }
        };
        Model = model;
    }

    public IList<string> ItemsSource
    {
        get
        {
            var pickerModel = Model as PickerModel;
            return (pickerModel != null) ? pickerModel.Items : null;
        }
        set
        {
            var model = Model as PickerModel;
            if (model != null)
            {
                model.Items = value;
            }
        }
    }

    public string SelectedItem
    {
        get { return (Model as PickerModel).SelectedItem; }
        set { }
    }
}
```

`MyUIPickerView` Предоставляемых классами `ItemsSource` и `SelectedItem` свойства и `SelectedItemChanged` событий. Объект [ `UIPickerView` ](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) требуется базовая [ `UIPickerViewModel` ](https://developer.xamarin.com/api/type/UIKit.UIPickerViewModel/) модель данных, которая обращается к `MyUIPickerView` свойства и события. `UIPickerViewModel` Модель данных обеспечивается `PickerModel` класса:

```csharp
class PickerModel : UIPickerViewModel
{
    int selectedIndex = 0;
    public event EventHandler<EventArgs> ItemChanged;
    public IList<string> Items { get; set; }

    public string SelectedItem
    {
        get
        {
            return Items != null && selectedIndex >= 0 && selectedIndex < Items.Count ? Items[selectedIndex] : null;
        }
    }

    public override nint GetRowsInComponent(UIPickerView pickerView, nint component)
    {
        return Items != null ? Items.Count : 0;
    }

    public override string GetTitle(UIPickerView pickerView, nint row, nint component)
    {
        return Items != null && Items.Count > row ? Items[(int)row] : null;
    }

    public override nint GetComponentCount(UIPickerView pickerView)
    {
        return 1;
    }

    public override void Selected(UIPickerView pickerView, nint row, nint component)
    {
        selectedIndex = (int)row;
        if (ItemChanged != null)
        {
            ItemChanged.Invoke(this, new EventArgs());
        }
    }
}
```

`PickerModel` Класс предоставляет базовое хранилище `MyUIPickerView` класс, через `Items` свойство. Каждый раз, когда элемент, выбранный в `MyUIPickerView` изменений, [ `Selected` ](https://developer.xamarin.com/api/member/UIKit.UIPickerViewModel.Selected/) метод выполняется, какие обновления выбранного индекса и вызывает `ItemChanged` событий. Это гарантирует, что `SelectedItem` свойство всегда возвращает последний элемент, выбрать пользователь. Кроме того `PickerModel` класс переопределяет методы, которые используются для установки `MyUIPickerView` экземпляра.

### <a name="android"></a>Android

Подклассы Android реализацию [ `Spinner` ](https://developer.xamarin.com/api/type/Android.Widget.Spinner/) представление и предоставляет свойства и события, можно легко использовать из XAML-кода:

```csharp
class MySpinner : Spinner
{
    ArrayAdapter adapter;
    IList<string> items;

    public IList<string> ItemsSource
    {
        get { return items; }
        set
        {
            if (items != value)
            {
                items = value;
                adapter.Clear();

                foreach (string str in items)
                {
                    adapter.Add(str);
                }
            }
        }
    }

    public string SelectedObject
    {
        get { return (string)GetItemAtPosition(SelectedItemPosition); }
        set
        {
            if (items != null)
            {
                int index = items.IndexOf(value);
                if (index != -1)
                {
                    SetSelection(index);
                }
            }
        }
    }

    public MySpinner(Context context) : base(context)
    {
        ItemSelected += OnBindableSpinnerItemSelected;

        adapter = new ArrayAdapter(context, Android.Resource.Layout.SimpleSpinnerItem);
        adapter.SetDropDownViewResource(Android.Resource.Layout.SimpleSpinnerDropDownItem);
        Adapter = adapter;
    }

    void OnBindableSpinnerItemSelected(object sender, ItemSelectedEventArgs args)
    {
        SelectedObject = (string)GetItemAtPosition(args.Position);
    }
}
```

`MySpinner` Предоставляемых классами `ItemsSource` и `SelectedObject` свойства и `ItemSelected` событий. Элементов, отображаемых на `MySpinner` класса предоставляются [ `Adapter` ](https://developer.xamarin.com/api/type/Android.Widget.Adapter/) связанные с представлением, а элементы заполняются в `Adapter` при `ItemsSource` сначала свойству. Каждый раз, когда элемент, выбранный в `MySpinner` класса изменения, `OnBindableSpinnerItemSelected` обновления обработчик событий `SelectedObject` свойство.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать собственные представления из Xamarin.Forms XAML-файлов. Свойства и обработчики событий могут быть установлены для собственного представления, и они могут взаимодействовать с Xamarin.Forms представления.


## <a name="related-links"></a>Связанные ссылки

- [NativeSwitch (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeSwitch/)
- [Forms2Native (пример)](https://developer.xamarin.com/samples/xamarin-forms/Forms2Native/)
- [NativeViewInsideContentView (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/NativeViewInsideContentView/)
- [SubclassedNativeControls (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeViews/SubclassedNativeControls/)
- [Исходные формы](~/xamarin-forms/platform/native-forms.md)
- [Передача аргументов в XAML](~/xamarin-forms/xaml/passing-arguments.md)
