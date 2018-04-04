---
title: Метка
description: Отображаемый текст в Xamarin.Forms
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: c1056626b336dd9b6ce265ab693ceed2a24eae0f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="label"></a>Метка

_Отображаемый текст в Xamarin.Forms_

`Label` Представление используется для отображения текста, одной или нескольких строк. Метки могут иметь пользовательские шрифты (семейств, размеры и параметры) и цвет текста. В этой статье рассматриваются следующие темы:

- **[Усечение и упаковки](#Truncation_and_Wrapping)**  &ndash; усечение и переноса параметров для обработки ситуации, когда текст не может поместиться на одну строку.
- **[Шрифт](#Font)**  &ndash; параметры шрифта.
- **[Цвет](#Color)**  &ndash; текст метки параметры цвета.
- **[Форматированный текст](#Formatted_Text)**  &ndash; параметры отображения текста с несколькими встроенные форматы и стили.

## <a name="styling-label"></a>Стиль метки

В следующих разделах описываются свойства параметра `Label` вручную для каждого экземпляра. Обратите внимание, что задает свойств могут быть сгруппированы в один стиль, который последовательно применяется для одного или нескольких представлений. Это можно повысить читаемость кода и изменения структуры проще реализовать. В разделе [стили](~/xamarin-forms/user-interface/text/styles.md) для получения дополнительной информации.

<a name="Truncation_and_Wrapping" />

## <a name="truncation-and-wrapping"></a>Усечение и упаковки

Метки можно задать для обработки текста, которые не помещаются в одну строку на одним из следующих способов, предоставляемые `LineBreakMode` свойство. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) представляет собой перечисление из следующих вариантов:

- **HeadTruncation** &ndash; усекает заголовок текста, отображение конца.
- **CharacterWrap** &ndash; переносит текст на новую строку на границе символ.
- **MiddleTruncation** &ndash; отображает начало и конец текста со средней replace многоточие.
- **NoWrap** &ndash; не переносить по словам, отображение только всего может текста помещается в одну строку.
- **TailTruncation** &ndash; показывает начало текста, усечение конца.
- **WordWrap** &ndash; переносит текст на границе слова.

## <a name="font"></a>Шрифт

В разделе [работа со шрифтами](~/xamarin-forms/user-interface/text/fonts.md) для получения дополнительной информации.

## <a name="color"></a>Цвет

`Label`s можно задать использование цвета пользовательского текста через привязываемых `TextColor` свойство.

Особое внимание необходима для того, что цвета можно использовать для каждой платформы. Поскольку каждая платформа имеет различные значения по умолчанию для цветов текста и фона, необходимо соблюдать осторожность, чтобы выбрать значение по умолчанию, которая работает на каждом.

Чтобы задать цвет текста метки, используйте следующий код:

В коде:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Пример TextColor метки")

<a name="Formatted_Text" />

## <a name="formatted-text"></a>Форматированный текст

Метки предоставлять `FormattedText` свойство, которое позволяет отображать текст с использованием нескольких шрифтов и цветов в одном представлении.

`FormattedText` Свойство относится к типу [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Форматированные строки состоят из одного или нескольких `Span`s, каждый из них со следующими свойствами:

- **BackgroundColor** &ndash; можно использовать для задания фонового цвета, например для создания эффекта маркера.
- **FontAttributes** &ndash; может быть набор полужирный, курсив или ни один.
- **Семейство FontFamily** &ndash; задает шрифт для использования.
- **FontSize** &ndash; задает размер текста.
- **ForegroundColor** &ndash; задает цвет текста.
- **Текст** &ndash; текста должна быть предоставлена.

В следующем коде C# показано метка, где первое слово имеет полужирным шрифтом, а последними — красным:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
        layout.Children.Add(label);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Пример FormattedText")


## <a name="related-links"></a>Связанные ссылки

- [Создание мобильных приложений с помощью Xamarin.Forms, раздел 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Метка API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
