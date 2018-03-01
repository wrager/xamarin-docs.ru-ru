---
title: "Создание расширений разметки XAML"
description: "Определить собственные пользовательские расширения разметки XAML"
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C1EF9-1C8E-4208-8610-9B79CCF17D46
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: eb7189226e4f5d7eb2b55bf61728e65db44ba57b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="creating-xaml-markup-extensions"></a>Создание расширений разметки XAML

На уровне программный расширение разметки XAML является класс, реализующий [ `IMarkupExtension` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension/) или [ `IMarkupExtension<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.IMarkupExtension%3CT%3E/) интерфейса. Можно просмотреть исходный код расширения стандартной разметки описано ниже в [ **MarkupExtensions** каталога](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) репозитория Xamarin.Forms GitHub. 

Также можно определить собственные пользовательские расширения разметки XAML путем наследования от `IMarkupExtension` или `IMarkupExtension<T>`. Используйте универсальную форму, если расширение разметки получает значение определенного типа. Это относится к несколько расширений разметки Xamarin.Forms:

- `TypeExtension` является производным от `IMarkupExtension<Type>`
- `ArrayExtension` является производным от `IMarkupExtension<Array>`
- `DynamicResourceExtension` является производным от `IMarkupExtension<DynamicResource>`
- `BindingExtension` является производным от `IMarkupExtension<BindingBase>`
- `ConstraintExpression` является производным от `IMarkupExtension<Constraint>`

Два `IMarkupExtension` интерфейсы определяют каждого, только один метод с именем `ProvideValue`: 

```csharp
public interface IMarkupExtension
{
    object ProvideValue(IServiceProvider serviceProvider);
}

public interface IMarkupExtension<out T> : IMarkupExtension
{
    new T ProvideValue(IServiceProvider serviceProvider);
}
```

Поскольку `IMarkupExtension<T>` является производным от `IMarkupExtension` и включает `new` ключевое слово на `ProvideValue`, одновременно содержит `ProvideValue` методы.

Очень часто расширения разметки XAML определяют свойства, влияющие к возвращаемому значению. (Очевидно исключение `NullExtension`, в котором `ProvideValue` просто возвращает `null`.) `ProvideValue` Метод имеет один аргумент типа `IServiceProvider` обсуждаются далее в этой статье.

## <a name="a-markup-extension-for-specifying-color"></a>Расширения разметки для цвета

Следующие расширения разметки XAML позволяет создавать `Color` с использованием компонентов оттенка, насыщенности и яркости. Он определяет четыре свойства четыре компонента цвета, включая альфа-компонент, который устанавливается равным 1. Класс является производным от `IMarkupExtension<Color>` для указания `Color` возвращаемое значение:

```csharp
public class HslColorExtension : IMarkupExtension<Color>
{
    public double H { set; get; }

    public double S { set; get; }

    public double L { set; get; }

    public double A { set; get; } = 1.0;

    public Color ProvideValue(IServiceProvider serviceProvider)
    {
        return Color.FromHsla(H, S, L, A);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<Color>).ProvideValue(serviceProvider);
    }
}
```

Поскольку `IMarkupExtension<T>` является производным от `IMarkupExtension`, класс должен содержать два `ProvideValue` методов, который возвращает `Color` и второй, который возвращает `object`, но второй метод может просто вызвать метод first.

**Демонстрация цвет HSL** странице отображаются различными способами, которые `HslColorExtension` могут присутствовать в файле XAML, чтобы указать цвет для `BoxView`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.HslColorDemoPage"
             Title="HSL Color Demo">

    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="BoxView">
                <Setter Property="WidthRequest" Value="80" />
                <Setter Property="HeightRequest" Value="80" />
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <StackLayout>
        <BoxView>
            <BoxView.Color>
                <local:HslColorExtension H="0" S="1" L="0.5" A="1" />
            </BoxView.Color>
        </BoxView>

        <BoxView>
            <BoxView.Color>
                <local:HslColor H="0.33" S="1" L="0.5" />
            </BoxView.Color>
        </BoxView>

        <BoxView Color="{local:HslColorExtension H=0.67, S=1, L=0.5}" />

        <BoxView Color="{local:HslColor H=0, S=0, L=0.5}" />

        <BoxView Color="{local:HslColor A=0.5}" />
    </StackLayout>
</ContentPage>
```

Обратите внимание, что при `HslColorExtension` является XML-тег, четыре свойства заданы как атрибуты, но когда он появится между фигурными скобками, четыре свойства разделяются запятыми, без кавычек. Для значений по умолчанию `H`, `S`, и `L` 0 и значение по умолчанию `A` -1, поэтому эти свойства можно опустить, если их значения по умолчанию. Последний пример показывает пример, где яркость равно 0, что обычно приводит к черным цветом, но альфа-канала — 0,5, поэтому половина прозрачно и отображается серым белом фоне страницы:

[![Демонстрация цвет HSL](creating-images/hslcolordemo-small.png "Демонстрация цвет HSL")](creating-images/hslcolordemo-large.png "Демонстрация цвет HSL")

## <a name="a-markup-extension-for-accessing-bitmaps"></a>Расширения разметки для доступа к растровые изображения

Аргумент `ProvideValue` — это объект, реализующий [ `IServiceProvider` ](https://developer.xamarin.com/api/type/System.IServiceProvider/) интерфейс, который определен в .NET `System` пространства имен. Этот интерфейс содержит один из элементов, метод с именем `GetService` с `Type` аргумент. 

`ImageResourceExtension` Класс, показанный ниже показан один из возможных способов использования `IServiceProvider` и `GetService` для получения `IXmlLineInfoProvider` объект, способный предоставлять строку и символ, данные для которых была обнаружена конкретной ошибки. В этом случае возникает исключение при `Source` не задано свойство:

```csharp
[ContentProperty("Source")]
class ImageResourceExtension : IMarkupExtension<ImageSource>
{
    public string Source { set; get; }

    public ImageSource ProvideValue(IServiceProvider serviceProvider)
    {
        if (String.IsNullOrEmpty(Source))
        {
            IXmlLineInfoProvider lineInfoProvider = serviceProvider.GetService(typeof(IXmlLineInfoProvider)) as IXmlLineInfoProvider;
            IXmlLineInfo lineInfo = (lineInfoProvider != null) ? lineInfoProvider.XmlLineInfo : new XmlLineInfo();
            throw new XamlParseException("ImageResourceExtension requires Source property to be set", lineInfo);
        }

        string assemblyName = GetType().GetTypeInfo().Assembly.GetName().Name;

        return ImageSource.FromResource(assemblyName + "." + Source);
    }

    object IMarkupExtension.ProvideValue(IServiceProvider serviceProvider)
    {
        return (this as IMarkupExtension<ImageSource>).ProvideValue(serviceProvider);
    }
}
```

`ImageResourceExtension` полезно, когда XAML-файл должен быть доступ к файлу изображения хранятся в виде внедренного ресурса в проекта переносимой библиотеки классов. Она использует `Source` свойство для вызова статического `ImageSource.FromResource` метод. Этот метод требует ресурсов полное доменное имя, которое состоит из имени сборки, имя папки и имя файла, разделенных точками. `ImageResourceExtension` Не требуется имя сборки, части, так как он получает имя сборки, с помощью отражения и добавляет его в `Source` свойство. Независимо от того `ImageSource.FromResource` должна вызываться из сборки, содержащей растрового изображения, это означает, что это расширение ресурсов XAML не может быть частью внешней библиотеки Если образы также находятся в эту библиотеку. (См. [ **внедренные изображения** ](~/xamarin-forms/user-interface/images.md#embedded_images) для получения дополнительных сведений о доступе к растровые изображения, хранящиеся в виде внедренных ресурсов.) 

Несмотря на то что `ImageResourceExtension` требует `Source` должно быть задано свойство `Source` свойство указывается в атрибуте как свойство содержимого класса. Это означает, что `Source=` можно опустить часть выражения в фигурных скобках. В **Демонстрация ресурса изображения** страницы, `Image` элементы выборки два изображения, используя имя папки и имя файла, разделенных точками:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MarkupExtensions"
             x:Class="MarkupExtensions.ImageResourceDemoPage"
             Title="Image Resource Demo">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <Image Source="{local:ImageResource Images.SeatedMonkey.jpg}"
               Grid.Row="0" />

        <Image Source="{local:ImageResource Images.FacePalm.jpg}"
               Grid.Row="1" />

    </Grid>
</ContentPage>
```

Вот программу на всех трех платформ.

[![Демонстрация ресурса изображения](creating-images/imageresourcedemo-small.png "изображения Демонстрация ресурсов")](creating-images/imageresourcedemo-large.png "изображения Демонстрация ресурсов")

## <a name="service-providers"></a>Поставщики услуг

С помощью `IServiceProvider` аргумент `ProvideValue`, расширения разметки XAML могут получить доступ к полезной информации о файле XAML, в котором они используются. Но, чтобы использовать `IServiceProvider` аргумент успешно, необходимо знать, какие службы доступны в определенной контекстах. Лучший способ получить представление об этой функции — путем отслеживания исходный код существующего расширения разметки XAML в [ **MarkupExtensions** папки](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Xaml/MarkupExtensions) в Xamarin.Forms репозитория в GitHub. Имейте в виду, что некоторые типы служб являются внутренними для Xamarin.Forms.

В некоторых расширений разметки XAML эта служба может быть полезной:

```csharp
 IProvideValueTarget provideValueTarget = serviceProvider.GetService(typeof(IProvideValueTarget)) as IProvideValueTarget;
```

`IProvideValueTarget` Интерфейс определяет два свойства `TargetObject` и `TargetProperty`. Если эти сведения можно получить в `ImageResourceExtension` класса, `TargetObject` — `Image` и `TargetProperty` — `BindableProperty` для объекта `Source` свойство `Image`. Это свойство, для которого задана расширение разметки XAML.

`GetService` Вызов с аргументом `typeof(IProvideValueTarget)` фактически возвращает объект типа `SimpleValueTargetProvider`, которая определена в `Xamarin.Forms.Xaml.Internals` пространства имен. При приведении возвращаемое значение `GetService` к этому типу, также можно использовать `ParentObjects` свойство, которое является массив, содержащий `Image` элемент, `Grid` родительского элемента и `ImageResourceDemoPage` родительский `Grid`.

## <a name="conclusion"></a>Заключение

Расширения разметки XAML играют важную роль в XAML, расширяя возможности задавать атрибуты из различных источников. Кроме того Если существующие расширения разметки XAML, не обеспечивают то, что вам нужно, можно также написать собственные. 


## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки (пример)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Глава расширений разметки XAML из книги Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
