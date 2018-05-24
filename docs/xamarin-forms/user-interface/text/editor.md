---
title: Редактор
description: Ввод многострочного текста
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 035365a22c487039ff811756d91ca0a8d392d628
ms.sourcegitcommit: c024f29ff730ae20c15e99bfe0268a0e1c9d41e5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2018
---
# <a name="editor"></a>Редактор

_Ввод многострочного текста_

`Editor` Элемент управления используется для ввода нескольких строк. В этой статье рассматривается:

- **[Настройка](#customization)**  &ndash; параметры клавиатуры и цвета.
- **[Интерактивные возможности](#interactivity)**  &ndash; события, которые можно прослушивать для обеспечения интерактивности.

## <a name="customization"></a>Настройка

### <a name="setting-and-reading-text"></a>Задание и чтение текста

Предоставляет редакторе, таком как другими представлениями представления текста `Text` свойство. `Text` можно использовать для задания и просмотрите сведения, представленные `Editor`. В следующем примере показано задание текста в XAML:

```xaml
<Editor Text="I am an Editor" />
```

В C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Для считывания текста, доступ к `Text` свойство в C#:

```csharp
var text = MyEditor.Text;
```

### <a name="keyboards"></a>Клавиатура

Сочетания, которое выводится, когда пользователи взаимодействуют с `Editor` могут быть заданы программно через [ ``Keyboard`` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Keyboard/) свойство.

Доступны следующие возможности для типа клавиатуры:

- **По умолчанию** &ndash; клавиатуры по умолчанию
- **Чат** &ndash; для сообщения & местах где полезны emoji
- **По электронной почте** &ndash; используется при вводе адреса электронной почты
- **Числовые** &ndash; используется при вводе чисел
- **Телефон** &ndash; используется при вводе телефонные номера
- **URL-адрес** &ndash; используется для ввода пути к файлам & веб-адреса

Отсутствует [пример каждого сочетания](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) в нашем разделе рецептами.

### <a name="colors"></a>Цвета

`Editor` можно задать, чтобы использовать пользовательский цвет фона через `BackgroundColor` свойство. Особое внимание необходима для того, что цвета можно использовать для каждой платформы. Поскольку каждая платформа имеет различные значения по умолчанию для цвета текста, может потребоваться задать пользовательский цвет фона для каждой платформы. В разделе [работа с Tweaks платформы](~/xamarin-forms/platform/device.md) Дополнительные сведения об оптимизации пользовательский Интерфейс для каждой платформы.

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Редактор с примером BackgroundColor")

Убедитесь, что цвета фона и текста выбранного могут использоваться для каждой платформы и не закрывают текст заполнителя.

## <a name="interactivity"></a>Интерактивность

`Editor` предоставляет два события:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.TextChanged/) &ndash; событие возникает при изменении текста в редакторе. Содержит текст до и после изменения.
- [Завершено](http://developer.xamarin.com/api/event/Xamarin.Forms.Editor.Completed/) &ndash; возникает, когда пользователь завершен входные данные с помощью возвращаемого клавиши на клавиатуре.

### <a name="completed"></a>Завершено

`Completed` Событие используется для реагирования на завершение взаимодействия с `Editor`. `Completed` возникает, когда пользователь завершает входных данных с полем, введя клавиша return на клавиатуре. Обработчик для события является для общего обработчика событий, выполнив отправителя и `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Можно подписаться завершенного события в коде и XAML:

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Событие используется для реагирования на изменения в содержимом поля.

`TextChanged` возникает каждый раз, когда `Text` из `Editor` изменения. Обработчик для события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старое и новое значения `Editor` `Text` через `OldTextValue` и `NewTextValue` свойства:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Можно подписаться завершенного события в коде и XAML:

В коде:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Редактор API](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)
