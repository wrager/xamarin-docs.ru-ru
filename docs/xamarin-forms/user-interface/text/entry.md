---
title: Запись Xamarin.Forms
description: В этой статье описывается использование класса Xamarin.Forms запись принимать одну строку текста или пароля при вводе в приложении.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: b6188b986589a56229ad2e092d4100ff3f75dbe4
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245557"
---
# <a name="xamarinforms-entry"></a>Запись Xamarin.Forms

_Однострочное текстовое или ввода пароля_

Xamarin.Forms `Entry` используется для ввода одной строки текста. `Entry`, Таких как `Editor` Просмотр, поддерживает несколько типов клавиатуры. Кроме того `Entry` можно использовать в качестве поля пароля.

## <a name="display-customization"></a>Настройки отображения

### <a name="setting-and-reading-text"></a>Задание и чтение текста

`Entry`, Как и другие режимы представления текста, предоставляет `Text` свойство. Это свойство можно использовать для задания и просмотрите сведения, представленные `Entry`. В следующем примере показано задание `Text` свойства в XAML:

```xaml
<Entry Text="I am an Entry" />
```

В C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Для считывания текста, доступ к `Text` свойство в C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> Ширина `Entry` может быть задан путем установки его `WidthRequest` свойство. Не полагайтесь на ширину `Entry` , определенное на основе значения из его `Text` свойство.

### <a name="limiting-input-length"></a>Ограничение длины ввода

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Свойство может использоваться для ограничения длины входных данных, допустимое для [ `Entry` ](xref:Xamarin.Forms.Entry). Это свойство должно быть присвоено положительное целое число:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) свойства значение 0 указывает, что входные данные не будет разрешено и значение `int.MaxValue`, который является значением по умолчанию для [ `Entry` ](xref:Xamarin.Forms.Entry), указывает, что не действующие ограничения на число символов, которые могут быть введены.

### <a name="keyboards"></a>Клавиатура

Сочетания, которое выводится, когда пользователи взаимодействуют с `Entry` могут быть заданы программно через `Keyboard` свойство.

Доступны следующие возможности для типа клавиатуры:

- **По умолчанию** &ndash; клавиатуры по умолчанию
- **Чат** &ndash; для сообщения & местах где полезны emoji
- **По электронной почте** &ndash; используется при вводе адреса электронной почты
- **Числовые** &ndash; используется при вводе чисел
- **Телефон** &ndash; используется при вводе телефонные номера
- **URL-адрес** &ndash; используется для ввода пути к файлам и веб-адреса

Отсутствует [пример каждого сочетания](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/) в нашем разделе рецептами.

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверки орфографии

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Свойство, управляет ли проверка орфографии включена. По умолчанию свойство имеет значение `true`. При нажатии пользователем текст, отображаются ошибки.

Однако в некоторых сценариях записи текста, например ввода имени пользователя, проверку орфографии предоставляет негативной и поэтому должны быть отключены, задав [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойства `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Когда [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойству `false`и не используется пользовательских сочетаний, собственный орфографии будет отключена. Однако если [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) имеет были набор, который отключает орфографии проверки, такие как [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` свойство игнорируется. Таким образом, свойство не может использоваться, чтобы включить проверку орфографии для `Keyboard` , явно отключает его.

### <a name="placeholders"></a>Заполнители

`Entry` можно задать отображать текст заполнителя, когда он не хранит ввод данных пользователем. На практике это часто встречается форм для уточнения содержимого, который подходит для данного поля. Цвет текста заполнителя, не может быть настроен и будет таким же, независимо от `TextColor` параметр. Если планируются вызовы для заполнителя пользовательские цвета, необходимо переключиться на [пользовательское средство отрисовки](). Создает следующие `Entry` с «Username» в качестве заполнителя в XAML:

```xaml
<Entry Placeholder="Username" />
```

В C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Пример записи заполнителя")

### <a name="password-fields"></a>Поля пароля

`Entry` предоставляет `IsPassword` свойство. Когда `IsPassword` — `true`, содержимое поля будут отображаться в виде окружностей черным:

В XAML:

```xaml
<Entry IsPassword="true" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Пример записи IsPassword")

Заполнители, которые могут быть использованы с экземплярами `Entry` , настроенные в качестве пароля:

В XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Запись IsPassword и пример заполнителя")


### <a name="colors"></a>Цвета

Можно задать запись для использования пользовательского фона и цвета текста через следующие свойства привязки.

- **TextColor** &ndash; задает цвет текста.
- **BackgroundColor** &ndash; задает цвет фона для текста показано.

Особое внимание необходима для того, что цвета можно использовать для каждой платформы. Поскольку каждая платформа имеет различные значения по умолчанию для цветов текста и фона, часто необходимо для того, и если задать один.

Чтобы задать цвет текста элемента, используйте следующий код:

В XAML:

```xaml
<Entry TextColor="Green" />
```

В C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Пример записи TextColor")

Обратите внимание, что заполнитель не влияет на заданный `TextColor`.

Чтобы задать цвет фона в XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

В C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Пример записи BackgroundColor")

Следите за тем, чтобы убедиться в том, что цвета фона и текста выбранного могут использоваться для каждой платформы и не закрывают текст заполнителя.

## <a name="events-and-interactivity"></a>События и интерактивность

Запись предоставляет два события:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; событие возникает при изменении текста в записи. Содержит текст до и после изменения.
- [Завершено](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; возникает, когда пользователь завершен входные данные с помощью возвращаемого клавиши на клавиатуре.

### <a name="completed"></a>Завершено

`Completed` Событие используется для реагирования на завершение взаимодействие с записью. `Completed` возникает, когда пользователь завершает входных данных с полем, введя клавиша return на клавиатуре. Обработчик для события является для общего обработчика событий, выполнив отправителя и `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Событие завершения можно подписаться на языке XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

и C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Событие используется для реагирования на изменения в содержимом поля.

`TextChanged` возникает каждый раз, когда `Text` из `Entry` изменения. Обработчик для события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старое и новое значения `Entry` `Text` через `OldTextValue` и `NewTextValue` свойства:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Событие можно подписаться на языке XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

и C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Операции API-Интерфейс](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
