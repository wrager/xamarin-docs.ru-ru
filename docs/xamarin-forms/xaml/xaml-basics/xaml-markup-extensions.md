---
title: Часть 3. Расширения разметки XAML
description: Расширения разметки XAML является важной характеристикой в XAML, который допускает свойства, чтобы указать на объекты или значения, которые косвенно ссылаться из других источников. Расширения разметки XAML особенно важны для совместное использование объектов, а также привязку константы, используемые во всем приложении, но их наибольшую программы можно найти привязки данных.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F4A37564-B18B-42FF-B841-9A1949895AB6
author: charlespetzold
ms.author: chape
ms.date: 3/27/2018
ms.openlocfilehash: c110223eae2bb06f64adf3e09977d97cc7b5d71b
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="part-3-xaml-markup-extensions"></a>Часть 3. Расширения разметки XAML

_Расширения разметки XAML является важной характеристикой в XAML, который допускает свойства, чтобы указать на объекты или значения, которые косвенно ссылаться из других источников. Расширения разметки XAML особенно важны для совместное использование объектов, а также привязку константы, используемые во всем приложении, но их наибольшую программы можно найти привязки данных._

## <a name="xaml-markup-extensions"></a>Расширения разметки XAML

Как правило можно использовать для задания свойств объекта в явные значения, такие как строка, число, члена перечисления или строка, которая преобразуется в значение в фоновом XAML.

Иногда Однако свойства вместо этого необходимо ссылаться на значения, определенные где-либо еще или которой может потребоваться немного обработки в коде во время выполнения. В этих целях XAML *расширения разметки* доступны.

Эти расширения разметки XAML не расширения XML. XAML — полностью юридических XML. Они называются «расширения», так как они поддерживаются в коде классов, реализующих `IMarkupExtension`. Можно написать собственные расширения разметки пользовательских.

Во многих случаях расширения разметки XAML мгновенно распознается в XAML-файлов, так как они представляются в виде параметров атрибутов с разделителями в фигурные скобки: {и}, но иногда расширения разметки отображается в разметке как обычные элементы.

## <a name="shared-resources"></a>Общие ресурсы

Некоторые страницы XAML содержит несколько представлений с заданы одинаковые значения. Например, многие значения свойств для этих `Button` объекты совпадают:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do that!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

        <Button Text="Do the other thing!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                BorderWidth="3"
                Rotation="-15"
                TextColor="Red"
                FontSize="24" />

    </StackLayout>
</ContentPage>
```

Если необходимо изменить один из этих свойств можно внести изменения только один раз, а не три раза. Если бы это был код, скорее всего используемого константы и статические объекты только для чтения для обеспечения согласованной и легко изменять такие значения.

В XAML, одним из популярных решений для хранения подобных значений или объектов в *словарь ресурсов*. `VisualElement` Класс определяет свойство с именем `Resources` типа `ResourceDictionary`, словаря с ключами типа `string` и значения типа `object`. Можно поместить объекты в данный словарь и затем ссылаться на них из разметки в XAML.

Чтобы использовать словарь ресурсов на странице, включают пару `Resources` теги элементов свойства. Наиболее удобно эти элементы в верхней части страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>

    </ContentPage.Resources>
    ...
</ContentPage>
```

Необходимо явно включить `ResourceDictionary` теги:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>

        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Теперь объекты и значения различных типов можно добавить в словарь ресурсов. Эти типы должны быть допускающих создание экземпляров. Они не могут быть абстрактные классы, например. Эти типы должны также иметь открытый конструктор без параметров. Каждый элемент требует словарь ключ, указанный с `x:Key` атрибута. Пример:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

Эти два элемента — это значения типа структуры `LayoutOptions`и каждый имеет уникальный ключ и один или два свойства. В код и разметку, значительно чаще используйте статические поля `LayoutOptions`, но здесь он является более удобным для задания свойств.

Теперь необходимо задать `HorizontalOptions` и `VerticalOptions` свойства этих кнопок к этим ресурсам, и это будет сделано с `StaticResource` расширение разметки XAML:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="3"
        Rotation="-15"
        TextColor="Red"
        FontSize="24" />
```

`StaticResource` Расширение разметки всегда с разделителями с фигурными скобками и включает ключ словаря.

Имя `StaticResource` отличает его от `DynamicResource`, который также поддерживает Xamarin.Forms. `DynamicResource` для ключей словаря, связанных со значениями, которые могут измениться во время выполнения, хотя `StaticResource` обращается к элементам словаря только в том случае, когда при создании элементов на странице.

Для `BorderWidth` свойства, это необходимо для хранения типа double в словаре. Код XAML определяет удобно теги для основных типов данных, таких как `x:Double` и `x:Int32`:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

        <x:Double x:Key="borderWidth">
            3
        </x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Не нужно поместить его в трех строках. Для этого угол поворота этой записи словаря принимает только вверх на одну строку:

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <LayoutOptions x:Key="horzOptions"
                       Alignment="Center" />

        <LayoutOptions x:Key="vertOptions"
                       Alignment="Center"
                       Expands="True" />

         <x:Double x:Key="borderWidth">
            3
         </x:Double>

        <x:Double x:Key="rotationAngle">-15</x:Double>
    </ResourceDictionary>
</ContentPage.Resources>
```

Эти два ресурса можно ссылаться в так же, как `LayoutOptions` значения:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="Red"
        FontSize="24" />
```

Для ресурсов типа `Color`, можно использовать же строковые представления, которые используются при непосредственно назначать атрибуты из этих типов. Преобразователи типов вызываются при создании ресурса. Вот ресурс типа `Color`:

```xaml
<Color x:Key="textColor">Red</Color>
```

Часто программы набор `FontSize` свойства члена `NamedSize` перечисления, таких как `Large`. `FontSizeConverter` Работает в фоновом преобразовать его в значение, зависящее от платформы с помощью класса `Device.GetNamedSized` метод. Тем не менее, при определении ресурсов размер шрифта, но более рационально использовать числовые значения, показанные здесь как `x:Double` типа:

```xaml
<x:Double x:Key="fontSize">24</x:Double>
```

Теперь все свойства, кроме `Text` определяются параметры ресурсов:

```xaml
<Button Text="Do this!"
        HorizontalOptions="{StaticResource horzOptions}"
        VerticalOptions="{StaticResource vertOptions}"
        BorderWidth="{StaticResource borderWidth}"
        Rotation="{StaticResource rotationAngle}"
        TextColor="{StaticResource textColor}"
        FontSize="{StaticResource fontSize}" />
```

Можно также использовать `OnPlatform` в словаре ресурсов для определения различных значений для платформ. Вот как `OnPlatform` объект может быть частью словарь ресурсов для другой текстовой цветов:

```xaml
<OnPlatform x:Key="textColor"
            x:TypeArguments="Color">
    <On Platform="iOS" Value="Red" />
    <On Platform="Android" Value="Aqua" />
    <On Platform="UWP" Value="#80FF80" />
</OnPlatform>
```

Обратите внимание, что `OnPlatform` получает оба `x:Key` атрибут, так как это объект словаря и `x:TypeArguments` атрибут, так как он является универсальным классом. `iOS`, `Android`, И `UWP` атрибуты преобразуются в `Color` значениями, при инициализации объекта.

Ниже приведен последний полный файл XAML с тремя кнопками, доступ к шести общие значения:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.SharedResourcesPage"
             Title="Shared Resources Page">

    <ContentPage.Resources>
        <ResourceDictionary>
            <LayoutOptions x:Key="horzOptions"
                           Alignment="Center" />

            <LayoutOptions x:Key="vertOptions"
                           Alignment="Center"
                           Expands="True" />

            <x:Double x:Key="borderWidth">3</x:Double>

            <x:Double x:Key="rotationAngle">-15</x:Double>

            <OnPlatform x:Key="textColor"
                        x:TypeArguments="Color">
                <On Platform="iOS" Value="Red" />
                <On Platform="Android" Value="Aqua" />
                <On Platform="UWP" Value="#80FF80" />
            </OnPlatform>

            <x:String x:Key="fontSize">Large</x:String>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout>
        <Button Text="Do this!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do that!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

        <Button Text="Do the other thing!"
                HorizontalOptions="{StaticResource horzOptions}"
                VerticalOptions="{StaticResource vertOptions}"
                BorderWidth="{StaticResource borderWidth}"
                Rotation="{StaticResource rotationAngle}"
                TextColor="{StaticResource textColor}"
                FontSize="{StaticResource fontSize}" />

    </StackLayout>
</ContentPage>
```

Снимки экрана проверьте согласованность задания стиля и стилей зависят от платформы.

[![](xaml-markup-extensions-images/sharedresources.png "Оформленных элементов управления")](xaml-markup-extensions-images/sharedresources-large.png#lightbox "оформленных элементов управления")

Несмотря на то, что чаще всего используется для определения `Resources` коллекции в верхней части страницы, необходимо помнить, `Resources` определяется свойство `VisualElement`, и может иметь `Resources` коллекций для других элементов на странице. Например, попробуйте добавить папку к `StackLayout` в этом примере:

```xaml
<StackLayout>
    <StackLayout.Resources>
        <ResourceDictionary>
            <Color x:Key="textColor">Blue</Color>
        </ResourceDictionary>
    </StackLayout.Resources>
    ...
</StackLayout>
```

Вы узнаете, цвет текста кнопки теперь отображается синим цветом. По сути, каждый раз, когда средство синтаксического анализа XAML сталкивается `StaticResource` расширения разметки, он выполняет копирование визуального дерева и использует первый `ResourceDictionary` обнаружении содержит такой ключ.

Одним из наиболее распространенных типов объектов, хранящихся в словарях ресурсов подход — Xamarin.Forms `Style`, который определяет набор свойств. В статье рассматриваются стили [стили](~/xamarin-forms/user-interface/styles/index.md).

Иногда разработчикам с XAML интересно, если они можно поместить такие как визуальный элемент `Label` или `Button` в `ResourceDictionary`. Хотя возможна конечно же, его не имеет смысла. Назначение `ResourceDictionary` является совместное использование объектов. Визуальный элемент будет недоступен. Тот же экземпляр не может присутствовать в два раза на одной странице.

## <a name="the-xstatic-markup-extension"></a>Расширение разметки x: Static

Несмотря на схожесть их имена `x:Static` и `StaticResource` сильно отличаются. `StaticResource` Возвращает объект из словаря ресурсов при `x:Static` обращается к одному из следующих действий:

- открытое статическое поле
- Общее статическое свойство
- открытое поле константы 
- член перечисления. 

`StaticResource` Расширение разметки поддерживается реализации XAML, которые определяют словарь ресурсов во время `x:Static` является неотъемлемой частью XAML, как `x` выявляет префикса.

Ниже приведены несколько примеров, демонстрирующих как `x:Static` может явно ссылаться на статические поля и члены перечисления:

```xaml
<Label Text="Hello, XAML!"
       VerticalOptions="{x:Static LayoutOptions.Start}"
       HorizontalTextAlignment="{x:Static TextAlignment.Center}"
       TextColor="{x:Static Color.Aqua}" />
```

На данный момент это не впечатления. Но `x:Static` расширение разметки можно также ссылаться на статические поля или свойства из собственного кода. Например, вот `AppConstants` класс, который содержит несколько статических полей, которые можно использовать на нескольких страницах на веб-приложения:

```csharp
using System;
using Xamarin.Forms;

namespace XamlSamples
{
    static class AppConstants
    {
        public static readonly Thickness PagePadding;

        public static readonly Font TitleFont;

        public static readonly Color BackgroundColor = Color.Aqua;

        public static readonly Color ForegroundColor = Color.Brown;

        static AppConstants()
        {
            switch (Device.RuntimePlatform)
            {
                case Device.iOS:
                    PagePadding = new Thickness(5, 20, 5, 0);
                    TitleFont = Font.SystemFontOfSize(35, FontAttributes.Bold);
                    break;

                case Device.Android:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(40, FontAttributes.Bold);
                    break;
                    
                case Device.UWP:
                    PagePadding = new Thickness(5, 0, 5, 0);
                    TitleFont = Font.SystemFontOfSize(50, FontAttributes.Bold);
                    break;
            }
        }
    }
}
```

Для ссылки на статические поля этого класса в файле XAML, необходимо каким-либо способом указания в файле XAML, где находится этот файл. Это делается с объявлением пространства имен XML.

Помните, что XAML-файлов, создаваемых как часть стандартного шаблона Xamarin.Forms XAML содержать два объявления пространств имен XML: одна для доступа к Xamarin.Forms классов, а другая — для ссылки на теги и атрибуты, встроенный в XAML:

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

Вам понадобятся дополнительные объявления пространств имен XML для доступа к другим классам. Каждый дополнительный объявление пространства имен XML определяет новый префикс. Доступ к классам, локальным для библиотеки .NET стандартных общих приложений, таких как `AppConstants`, программисты XAML часто используют префикс `local`. Объявление пространства имен необходимо указать имя пространства имен CLR (среды CLR), также называется .NET имя пространства имен, это имя, которое отображается в C# `namespace` определения или в `using` директиву:

```csharp
xmlns:local="clr-namespace:XamlSamples"
```

Можно также определить объявления пространств имен XML для пространства имен .NET, в любой сборке, на который ссылается на библиотеки .NET Standard. Например, вот `sys` префикс для стандартных .NET `System` пространства имен, которое находится в **mscorlib** сборку, которая успешно прошли один раз для «Microsoft библиотекой среды выполнения объекта», но теперь означает «многоязыковой поддержки Standard Объект библиотекой среды выполнения.» Поскольку это другой сборки, необходимо также указать имя сборки, в этом случае **mscorlib**:

```csharp
xmlns:sys="clr-namespace:System;assembly=mscorlib"
```

Обратите внимание, что ключевое слово `clr-namespace` следуют двоеточие и имя пространства имен .NET, точка с запятой, ключевое слово `assembly`, знак равенства и имя сборки.

Да, следует за двоеточием `clr-namespace` схожа знак равенства `assembly`. Синтаксис был намеренно, каким образом определен в этом: объявления пространств имен XML наиболее ссылки URI, который начинается схемы URI, таких как `http`, который всегда следуют двоеточие. `clr-namespace` Часть этой строки предназначена для имитации, соглашение.

Оба эти объявления пространств имен, включенные в **StaticConstantsPage** образца. Обратите внимание, что `BoxView` размеры указываются `Math.PI` и `Math.E`, но масштабированные с коэффициентом 100:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             xmlns:sys="clr-namespace:System;assembly=mscorlib"
             x:Class="XamlSamples.StaticConstantsPage"
             Title="Static Constants Page"
             Padding="{x:Static local:AppConstants.PagePadding}">

    <StackLayout>
       <Label Text="Hello, XAML!"
              TextColor="{x:Static local:AppConstants.BackgroundColor}"
              BackgroundColor="{x:Static local:AppConstants.ForegroundColor}"
              Font="{x:Static local:AppConstants.TitleFont}"
              HorizontalOptions="Center" />

      <BoxView WidthRequest="{x:Static sys:Math.PI}"
               HeightRequest="{x:Static sys:Math.E}"
               Color="{x:Static local:AppConstants.ForegroundColor}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand"
               Scale="100" />
    </StackLayout>
</ContentPage>
```

Размер результирующего `BoxView` относительно экрана зависит от платформы:

 [![](xaml-markup-extensions-images/staticconstants.png "Элементы управления, используя расширение разметки x: Static")](xaml-markup-extensions-images/staticconstants-large.png#lightbox "элементы управления, используя расширение разметки x: Static")

## <a name="other-standard-markup-extensions"></a>Другие расширения стандартной разметки

Несколько расширений разметки, являются внутренними для XAML и поддерживается в Xamarin.Forms XAML-файлов. Некоторые из них не используются очень часто, но необходимы, если они требуются:

-  Если свойство имеет значение, отличное от `null` нужно присвоить значение по умолчанию, но `null`, задайте для него значение `{x:Null}` расширения разметки.
-  Если свойство имеет тип `Type`, можно присвоить его `Type` с помощью расширения разметки `{x:Type someClass}`.
-  Можно определить массивы в XAML с помощью `x:Array` расширения разметки. Это расширение разметки получает требуемый атрибут с именем `Type` , указывающее тип элементов в массиве.
- `Binding` Рассматривается расширение разметки [часть 4. Основные сведения о привязке данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

## <a name="the-constraintexpression-markup-extension"></a>Расширение разметки ConstraintExpression

Расширения разметки может иметь свойства, но они не были заданы как атрибуты XML. В расширении разметки параметры свойств разделяются запятыми и отображаются без кавычек в фигурные скобки.

Это может быть показано с именем расширения разметки Xamarin.Forms `ConstraintExpression`, который используется с `RelativeLayout` класса. Можно указать расположение и размер дочернее представление как константу, относительно родителя или другие именованные представления. Синтаксис `ConstraintExpression` позволяет задать позиции или размера представления с помощью `Factor` раз свойство из другого представления, а также `Constant`. Какие-либо более сложные, чем требуется код.

Ниже приведен пример.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.RelativeLayoutPage"
             Title="RelativeLayout Page">

    <RelativeLayout>

        <!-- Upper left -->
        <BoxView Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Upper right -->
        <BoxView Color="Green"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}" />
        <!-- Lower left -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=Constant,
                                            Constant=0}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />
        <!-- Lower right -->
        <BoxView Color="Yellow"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=1,
                                            Constant=-40}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=1,
                                            Constant=-40}" />

        <!-- Centered and 1/3 width and height of parent -->
        <BoxView x:Name="oneThird"
                 Color="Red"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToParent,
                                            Property=Height,
                                            Factor=0.33}"  />

        <!-- 1/3 width and height of previous -->
        <BoxView Color="Blue"
                 RelativeLayout.XConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=X}"
                 RelativeLayout.YConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Y}"
                 RelativeLayout.WidthConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Width,
                                            Factor=0.33}"
                 RelativeLayout.HeightConstraint=
                     "{ConstraintExpression Type=RelativeToView,
                                            ElementName=oneThird,
                                            Property=Height,
                                            Factor=0.33}"  />
    </RelativeLayout>
</ContentPage>
```

Возможно, наиболее важные занятие, необходимо выполнить из этого примера является синтаксиса расширения разметки: без кавычек должен находиться внутри фигурных скобок расширения разметки. При вводе расширения разметки в XAML-файле, логично необходимо заключить в кавычки значения свойств. Resist стремитесь!

Вот выполнение программы.

[![](xaml-markup-extensions-images/relativelayout.png "Относительный макета с помощью ограничений")](xaml-markup-extensions-images/relativelayout-large.png#lightbox "относительного макета с помощью ограничений")

## <a name="summary"></a>Сводка

Расширения разметки XAML, показанный здесь важно поддерживают XAML-файлов. Возможно, является наиболее полезно расширение разметки XAML, но `Binding`, который описан в следующей части этой серии [часть 4. Основные сведения о привязке данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).



## <a name="related-links"></a>Связанные ссылки

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Часть 1. Начало работы с XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
- [Часть 2. Основной синтаксис XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [Часть 4. Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [Часть 5. Из привязка данных к MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
