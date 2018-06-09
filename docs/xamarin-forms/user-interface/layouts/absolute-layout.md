---
title: Xamarin.Forms AbsoluteLayout
description: В этой статье объясняется, как использовать класс Xamarin.Forms AbsoluteLayout для создания пользовательских интерфейсов точно пикселей. Этот класс размещает и размеры дочерних элементов, пропорциональное в размер и положение или абсолютные значения.
ms.prod: xamarin
ms.assetid: 01A5CCE0-AD45-4806-84FD-72C007005B38
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/25/2015
ms.openlocfilehash: f36334bca9e7401f35d4b6181b47c0f64923f652
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244461"
---
# <a name="xamarinforms-absolutelayout"></a>Xamarin.Forms AbsoluteLayout

[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/) размещает и размеры дочерних элементов, пропорциональное в размер и положение или абсолютные значения. Дочерние представления могут быть помещается и размера, с помощью пропорциональное или статические значения и пропорциональное и могут содержаться статические значения.

[![](absolute-layout-images/layouts-sml.png "Макеты Xamarin.Forms")](absolute-layout-images/layouts.png#lightbox "Xamarin.Forms макетов")

В этой статье рассматривается:

- **[Назначение](#Purpose)**  &ndash; распространенные варианты применения `AbsoluteLayout`.
- **[Использование](#Usage)**  &ndash; использовании `AbsoluteLayout` для достижения нужного макета.
  - **[Пропорциональное макеты](#Proportional_Layouts)**  &ndash; понять, как пропорциональное значения работают в `AbsoluteLayout`.
  - **[Указание значений](#Specifying_Values)**  &ndash; понять, как задаются пропорционально и абсолютные значения.
  - **[Пропорциональное значения](#Proportional_Values)**  &ndash; понять, как пропорциональное значения работать.
    - **[Абсолютные значения](#Absolute_Values)**  &ndash; понять, как работают абсолютные значения.

<a name="Purpose" />

## <a name="purpose"></a>Цель

Из-за позиционирования модели `AbsoluteLayout`, макет делает относительно просто для размещения элементов, чтобы они были записи на диск с любой стороны макета или по центру. С пропорционально размеров и положения, элементы в `AbsoluteLayout` можно автоматически масштабировать до любого размера представления. Для элементов, где только позицией, а не размер следует использовать масштабирование могут содержаться значения абсолютными и пропорционально.

`AbsoluteLayout` можно использовать везде, где элементы должны располагаться в представлении и особенно полезна при выравнивание элементов по краям.

<a name="Usage" />

## <a name="usage"></a>Использование

<a name="Proportional_Layouts" />

### <a name="proportional-layouts"></a>Пропорциональное макетов

`AbsoluteLayout` имеет уникальный привязки модели, согласно которому располагается привязки элемента относительно его элемент как элемент располагается относительно макета при использовании пропорционально позиционирования. При использовании абсолютного позиционирования привязка находится в представлении (0,0). Это имеет два важных последствия:

- Элементы, не может быть расположена вне экрана, с помощью пропорционально значений.
- Элементы надежно может размещаться на любой стороне макета или в центре, независимо от размера макета или устройства.

`AbsoluteLayout`, такие как `RelativeLayout`, может для размещения элементов с перекрытием.

Обратите внимание на следующем снимке экрана, а привязка поля — белый точка. Обратите внимание связь между связывающей и поле при его прохождении через макета:

![](absolute-layout-images/anchor-start.png "Привязки в начале")
![](absolute-layout-images/anchor-center.png "привязки в центре")
![](absolute-layout-images/anchor-end.png "привязки в конце")

<a name="Specifying_Values" />

### <a name="specifying-values"></a>Указание значений

Представления в `AbsoluteLayout` устанавливаются с помощью четырех значений:

- **X** &ndash; оси x (горизонтальную) представления привязки
- **Y** &ndash; положение по оси y (по вертикали) представления привязки
- **Ширина** &ndash; ширину представления
- **Высота** &ndash; высоты представления

Каждое из этих значений могут быть установлены как [пропорционально](#Proportional_Values) значение или [абсолютный](#Absolute_Values) значение.

Значения определяются как сочетание границ и флаг. `LayoutBounds` — [ `Rectangle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Rectangle/) состоящий из четырех значений: `x`, `y`, `width`, `height`.

### <a name="absolutelayoutflags"></a>AbsoluteLayoutFlags

[`AbsoluteLayoutFlags`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayoutFlags/) Задает интерпретацию значений и имеет следующие стандартные параметры:

- **Нет** &ndash; считает все значения, как абсолютные. Это значение по умолчанию, если не заданы никакие флаги макета.
- **Все** &ndash; считает все значения, как пропорционально.
- **WidthProportional** &ndash; интерпретирует `Width` значение в виде пропорционально и все остальные значения как абсолютные.
- **HeightProportional** &ndash; интерпретирует только высоту значение как пропорциональное со всеми другими значениями абсолютный.
- **XProportional** &ndash; интерпретирует `X` значение как пропорциональное, при обработке всех остальных значений как абсолютные.
- **YProportional** &ndash; интерпретирует `Y` значение как пропорциональное, при обработке всех остальных значений как абсолютные.
- **PositionProportional** &ndash; интерпретирует `X` и `Y` значений в виде пропорциональных, пока размер значения интерпретируются как абсолютные.
- **SizeProportional** &ndash; интерпретирует `Width` и `Height` значений в виде пропорциональных при абсолютного значения положения.

В XAML, границ и флаги устанавливаются как часть определения представлений в макете, с помощью `AbsoluteLayout.LayoutBounds` свойство. Границы, задаются список разделенных запятыми значений, `X`, `Y`, `Width`, и `Height`в этом порядке. В объявлении представлений в макета с использованием также указаны флаги `AbsoluteLayout.LayoutFlags` свойство. Обратите внимание, что флаги могут быть объединены в XAML, используя список с разделителями запятыми. Рассмотрим следующий пример.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="LayoutSamples.AbsoluteLayoutExploration"
Title="Absolute Layout Exploration">
    <ContentPage.Content>
        <AbsoluteLayout>
            <Label Text="I'm centered on iPhone 4 but no other device"
                AbsoluteLayout.LayoutBounds="115,150,100,100" LineBreakMode="WordWrap"  />
            <Label Text="I'm bottom center on every device."
                AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"
                LineBreakMode="WordWrap"  />
            <BoxView Color="Olive"  AbsoluteLayout.LayoutBounds="1,.5, 25, 100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Red" AbsoluteLayout.LayoutBounds="0,.5,25,100"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
            <BoxView Color="Blue" AbsoluteLayout.LayoutBounds=".5,0,100,25"
                AbsoluteLayout.LayoutFlags="PositionProportional" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

![](absolute-layout-images/exploration.png "Примеры AbsoluteLayout")

Обратите внимание на следующее условия:

- Метка в центре располагается помощью абсолютный размер и положение значений. За этого он отображается по центру на iPhone 4S и ниже, но не по центру на больших устройствах.
- Текст в нижней части макета располагается пропорционально значениями размера и положения. Он всегда отображается в центре нижней части макета, но его размер будет расти вместе с больших размеров макета.
- Три цвета `BoxView`s, расположенные на краях верхней левой и правой части экрана, с помощью пропорционально положение и размер абсолютный.

Следующие достигается тот же макет, в C#:

```csharp
public class AbsoluteLayoutExplorationCode : ContentPage
{
    public AbsoluteLayoutExplorationCode ()
    {
        Title = "Absolute Layout Exploration - Code";
        var layout = new AbsoluteLayout();

        var centerLabel = new Label {
        Text = "I'm centered on iPhone 4 but no other device.",
        LineBreakMode = LineBreakMode.WordWrap};

        AbsoluteLayout.SetLayoutBounds (centerLabel, new Rectangle (115, 159, 100, 100));
        // No need to set layout flags, absolute positioning is the default

        var bottomLabel = new Label { Text = "I'm bottom center on every device.", LineBreakMode = LineBreakMode.WordWrap };
        AbsoluteLayout.SetLayoutBounds (bottomLabel, new Rectangle (.5, 1, .5, .1));
        AbsoluteLayout.SetLayoutFlags (bottomLabel, AbsoluteLayoutFlags.All);

        var rightBox = new BoxView{ Color = Color.Olive };
        AbsoluteLayout.SetLayoutBounds (rightBox, new Rectangle (1, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (rightBox, AbsoluteLayoutFlags.PositionProportional);

        var leftBox = new BoxView{ Color = Color.Red };
        AbsoluteLayout.SetLayoutBounds (leftBox, new Rectangle (0, .5, 25, 100));
        AbsoluteLayout.SetLayoutFlags (leftBox, AbsoluteLayoutFlags.PositionProportional);

        var topBox = new BoxView{ Color = Color.Blue };
        AbsoluteLayout.SetLayoutBounds (topBox, new Rectangle (.5, 0, 100, 25));
        AbsoluteLayout.SetLayoutFlags (topBox, AbsoluteLayoutFlags.PositionProportional);

        layout.Children.Add (bottomLabel);
        layout.Children.Add (centerLabel);
        layout.Children.Add (rightBox);
        layout.Children.Add (leftBox);
        layout.Children.Add (topBox);

        Content = layout;
    }
}
```
<a name="Proportional_Values" />

### <a name="proportional-values"></a>Пропорциональное значения

Пропорциональное значения определить связь между макета и представления. Эта связь определяет положение или значение масштаба дочернее представление виде пропорциональных долей соответствующее значение родительского макета. Эти значения выражаются в виде `double`s со значениями от 0 до 1.

Пропорциональное значения используются для положение и размер представления в макете. Таким образом, если ширина представления задана как пропорцию, итоговый ширину имеет значение пропорции, умноженное на `AbsoluteLayout`в ширину. Например, с помощью `AbsoluteLayout` ширины `500` и пропорционально ширину.5, готовый для просмотра ширины представления в представлении будут 250 (500 x.5).

Для использования пропорционально значения задайте `LayoutBounds` с помощью (x, y) пропорции и размеры пропорционально, установите `LayoutFlags` для `All`.

В XAML:

```xaml
<Label Text="I'm bottom center on every device."
  AbsoluteLayout.LayoutBounds=".5,1,.5,.1" AbsoluteLayout.LayoutFlags="All"  />
```

В C#:

```csharp
var label = new Label {Text = "I'm bottom center on every device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(.5,1,.5,.1));
AbsoluteLayout.SetLayoutFlags(label, AbsoluteLayoutFlags.All);
```

<a name="Absolute_Values" />

### <a name="absolute-values"></a>Абсолютные значения

Абсолютные значения явно определить положение представления в макете. В отличие от пропорционально значений абсолютные значения способны позиционирование и изменение размеров представление, которое не помещается по центру в границах макета.

Использовать абсолютные значения для размещения может быть опасно, если известен размер макета. При использовании абсолютного позиционирования, элемент в центре экрана в один размер удалось смещения в любых других размеров. Очень важно тестировать приложение на экранах различных размеров поддерживаемых устройств.

Чтобы использовать значения абсолютного макета, задайте `LayoutBounds` с помощью (x, y) координаты и размеры явной, установите `LayoutFlags` для `None`.

В XAML:

```xaml
<Label Text="I'm centered on iPhone 4 but no other device."
  AbsoluteLayout.LayoutBounds="115,150,100,100"  />
```

В C#:

```csharp
var label = new Label {Text = "I'm centered on iPhone 4 but no other device."};
AbsoluteLayout.SetLayoutBounds(label, new Rectangle(115,150,100,100));
```

## <a name="exploring-a-complex-layout"></a>Изучение создания сложных макетов

Каждый из макетов иметь сильные и слабые стороны при компоновке определенного. В этой серии статей макета пример приложения был создан с тем же макетом страницы, реализовано с помощью трех различных макетов.

Рассмотрим следующий код XAML.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TheBusinessTumble.AbsoluteLayoutPage"
Title="AbsoluteLayout">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Save" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <ScrollView>
            <AbsoluteLayout BackgroundColor="Maroon">
                <BoxView BackgroundColor="Gray" AbsoluteLayout.LayoutBounds="0
                    0,1,100" AbsoluteLayout.LayoutFlags="XProportional,YProportional,WidthProportional" />
                <Button BackgroundColor="Maroon"
                    AbsoluteLayout.LayoutBounds=".5,55,70,70" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="35" />
                <Button BackgroundColor="Red" AbsoluteLayout.LayoutBounds=".5
                    60,60,60" AbsoluteLayout.LayoutFlags="XProportional" BorderRadius="30" />
                <Label Text="User Name" FontAttributes="Bold" FontSize="26"
                    TextColor="Black" HorizontalTextAlignment="Center" AbsoluteLayout.LayoutBounds=".5,140,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <Entry Text="Bio + Hashtags" TextColor="White"
                    BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds=".5,180,1,40" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                <AbsoluteLayout BackgroundColor="White"
                        AbsoluteLayout.LayoutBounds="0, 220, 1, 50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional">
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional">
                        <Button BackgroundColor="Black" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Accent Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                    <AbsoluteLayout AbsoluteLayout.LayoutBounds="1,0,.5,1" AbsoluteLayout.LayoutFlags="WidthProportional,HeightProportional,XProportional">
                        <Button BackgroundColor="Maroon" BorderRadius="20"
                            AbsoluteLayout.LayoutBounds="5,.5,40,40" AbsoluteLayout.LayoutFlags="YProportional" />
                        <Label Text="Primary Color" TextColor="Black"
                            AbsoluteLayout.LayoutBounds="50,.55,1,25" AbsoluteLayout.LayoutFlags="YProportional,WidthProportional" />
                    </AbsoluteLayout>
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,270,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Age:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="35" TextColor="White" BackgroundColor="Maroon"
                        AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,320,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Interests:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin.Forms" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
                <AbsoluteLayout AbsoluteLayout.LayoutBounds="0,370,1,50" AbsoluteLayout.LayoutFlags="WidthProportional" Padding="5,0,0,0">
                    <Label Text="Ask me about:" TextColor="White"
                        AbsoluteLayout.LayoutBounds="0,25,.25,50" AbsoluteLayout.LayoutFlags="WidthProportional" />
                    <Entry Text="Xamarin, C#, .NET, Mono" TextColor="White"
                        BackgroundColor="Maroon" AbsoluteLayout.LayoutBounds="1,10,.75,50" AbsoluteLayout.LayoutFlags="XProportional,WidthProportional" />
                </AbsoluteLayout>
            </AbsoluteLayout>
        </ScrollView>
    </ContentPage.Content>
</ContentPage>
```

Приведенный выше код результатов в следующем формате:

![](absolute-layout-images/abs.png "Сложные AbsoluteLayout")

Обратите внимание, что `AbsoluteLayout`вложены s, так как в некоторых случаях вложенности макеты может оказаться удобнее, чем представления все элементы в тот же макет.

## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms, глава 14](https://developer.xamarin.com/r/xamarin-forms/book/chapter14.pdf)
- [AbsoluteLayout](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)
- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
