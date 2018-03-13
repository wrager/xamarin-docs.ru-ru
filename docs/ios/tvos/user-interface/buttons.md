---
title: "Работа с кнопками"
description: "В этой статье описывается проектирование и работа с кнопками внутри приложения Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/07/2017
ms.openlocfilehash: 4b2a470d7fe2a1f9d4b8df40836c934547adf614
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-buttons"></a>Работа с кнопками

_В этой статье описывается проектирование и работа с кнопками внутри приложения Xamarin.tvOS._


Использовать экземпляр `UIButton` класс для создания может иметь фокус, доступный для выбора кнопки в окне tvOS. Когда пользователь выбирает кнопку, он отправляет сообщение действия целевого объекта разрешить входных ваш ответ приложения Xamarin.tvOS для пользователя.

[![](buttons-images/buttons01.png "Пример кнопки")](buttons-images/buttons01.png#lightbox)

Дополнительные сведения о работе с фокусом и переход с удаленной Siri см. в разделе нашей [работа с навигации и фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленного и контроллеров Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) документации.

<a name="About-Buttons" />

## <a name="about-buttons"></a>Сведения о кнопках

В tvOS кнопки используются для действий конкретного приложения и может содержать заголовок, значок или оба. Когда пользователь переходит с помощью пользовательского интерфейса приложения [Siri удаленного](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), фокус перемещается к указанной кнопки, сделав его изменение цвета текста и фона. Тень также применяется к кнопке Добавление объемные эффекты, чтобы он отображался расти выше остальная часть пользовательского интерфейса.

[![](buttons-images/buttons01.png "Пример кнопки")](buttons-images/buttons01.png#lightbox)

Apple имеет следующие рекомендации по работе с кнопками.

- **Заголовок или значок используется** — при оба значка ограничено и заголовок может быть включено в кнопку, поэтому старайтесь не сочетать оба.
- **Четко пометить разрушительных кнопки** — Если кнопке разрушительных действия (например, при удалении файла), пометив его таким образом с помощью текста и/или значок. Команды всегда должен представить [предупреждения](~/ios/tvos/user-interface/alerts.md) запрос для ограничения действия пользователя.
- **Не используйте кнопки обратно** -кнопки меню на удаленном Siri используется для возврата к предыдущему экрану. Единственным исключением из этого правила является для покупки из приложений или разрушительных действий где **отменить** должна появиться кнопка.

Дополнительные сведения о работе с фокусом и навигации, см. в разделе нашей [работа с навигации и фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) документации.

<a name="Button-Icons" />

### <a name="button-icons"></a>Изображения кнопок

Apple рекомендует использовать простой, высокой распознаваемым изображений для значков для кнопок. Излишне сложная значки трудно распознавать на экране ТВ через места на диване, поэтому использовать возможность становится ясно, что собой простейшее представление. По возможности использовать стандартный, хорошо знать изображений для значков (например увеличительного стекла для поиска).

<a name="Button-Titles" />

### <a name="button-titles"></a>Кнопка заголовки

При создании заголовков для кнопки, Apple имеет следующие рекомендации:

- **Отображать описательный текст ниже кнопки значки** — там, где можно поместить понятный и описательный текст под значком только кнопки дальнейших get назначения кнопки по.
- **Используйте глаголы или фразы команды для заголовка** -явно определять поместить действие, которое будет выполнять, когда пользователь нажимает кнопку.
- **Стиль заголовка регистре** — за исключением статей, союзы или союзы (четыре буквы или меньше), должна быть прописной буквой каждого слова кнопки заголовка.
- **Используйте короткие, заголовка для точки** -использовать короткий возможных формулировки для описания действие кнопки.

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>Кнопки и раскадровки

Для работы с кнопками в приложении Xamarin.tvOS проще всего добавить их к пользовательскому Интерфейсу приложения с помощью конструктора Xamarin для операций ввода-вывода.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)


1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **кнопку** из **библиотеки** и поместите его в представлении: 

    [![](buttons-images/storyboard01.png "Кнопки")](buttons-images/storyboard01.png#lightbox)
1. В **свойства обозревателя**, можно настроить несколько свойств кнопки, такие как его **заголовок** и **цвет текста**: 

    [![](buttons-images/storyboard02.png "Свойства кнопок")](buttons-images/storyboard02.png#lightbox)
1. Теперь, переключитесь в **вкладку события** и обработке **событий** из **кнопку** и назовите его `ButtonPressed`: 

    [![](buttons-images/storyboard03.png "Вкладка «событий»")](buttons-images/storyboard03.png#lightbox)
1. Можно автоматически переключаться на `ViewController.cs` представление, где можно размещать в коде с помощью нового действия **копирование** и **работу** клавиши со стрелками: 

    [![](buttons-images/storyboard04.png "Размещение нового действия в коде")](buttons-images/storyboard04.png#lightbox)
1. Нажмите клавишу **ввод** для выбора расположения: 

    [![](buttons-images/storyboard05.png "Редактор кода")](buttons-images/storyboard05.png#lightbox)
1. Сохраните изменения ко всем файлам.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **кнопку** из **библиотеки** и поместите его в представлении: 

    [![](buttons-images/storyboard01vs.png "Кнопки")](buttons-images/storyboard01vs.png#lightbox)
1. В **свойства обозревателя**, можно настроить несколько свойств кнопки, такие как его **заголовок** и **цвет текста**: 

    [![](buttons-images/storyboard02vs.png "Обозреватель свойств")](buttons-images/storyboard02vs.png#lightbox)
1. Теперь, переключитесь в **вкладку события** и обработке **событий** из **кнопку** и назовите его `ButtonPressed`: 

    [![](buttons-images/storyboard03vs.png "Вкладка «событий»")](buttons-images/storyboard03vs.png#lightbox)
1. Сохраните изменения ко всем файлам.



Изменение контроллер представление (пример `ViewController.cs`) и добавьте следующий код для обработки кнопке файла:


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

Пока кнопки `Enabled` свойство `true` и не соответствует другим элементом управления или представление, его можно сделать элемент в фокусе, с помощью удаленного Siri. Если пользователь выбирает кнопку и щелкает область Touch `ButtonPressed` будут выполняться действия, описанный выше.

> [!IMPORTANT]
> **Примечание:** хотя и существует возможность назначить действий, таких как `TouchUpInside` для `UIButton` в конструктор при создании iOS **обработчик событий**, он никогда не вызываться, так как у Apple TV нет сенсорного экрана или в службу поддержки Коснитесь события. Следует всегда использовать значение по умолчанию **тип действия** при создании **действия** для tvOS элементов пользовательского интерфейса.




Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md).

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>Кнопки и кода

При необходимости `UIButton` можно создать в коде C# и добавлении приложения tvOS представление. Пример:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

При создании нового `UIButton` в коде, укажите его `UIButtonType` как один из следующих:

- **Система** -это стандартный тип кнопки, представленный tvOS, тип, который будет использоваться чаще всего.
- **DetailDisclosure** -представляет «выключить» тип, из которой можно скрыть или отобразить подробные сведения.
- **InfoDark** -темной подробные сведения, кнопка «i» отображается в кружке.
- **InfoLight** -индикатор подробные сведения, кнопка «i» отображается в кружке.
- **AddContact** -отображать кнопку в виде кнопки, добавить контакт.
- **Настраиваемый** -позволяет настраивать несколько характеристик кнопки.

Определите на экране размер и расположение кнопки. Пример

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

Задайте заголовок для кнопки. `UIButtons` отличаются от большинства `UIKit` элементов управления, в том, что они имеют состояние, поэтому вы не можете просто изменить заголовок, необходимо изменить его для данного `UIControlState`. Пример:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Затем используйте `AllEvents` событий для просмотра, если пользователь нажал на кнопку. Пример

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

Наконец добавьте кнопку для просмотра его:

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> **Примечание:** хотя и существует возможность назначить действий, таких как `TouchUpInside` для `UIButton`, он никогда не вызываться, так как для Apple TV нет сенсорном экране или поддерживает событий сенсорного экрана. Следует всегда использовать события например **AllEvents** или **PrimaryActionTriggered**.




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>Задание стиля кнопки

tvOS предоставляет несколько свойств `UIButton` может использоваться для предоставления его название и оформить ее с помощью таких вещей, как цвет фона и изображения.

<a name="Button-Titles" />

### <a name="button-titles"></a>Кнопка заголовки

Как было показано выше, `UIButtons` отличаются от большинства `UIKit` элементов управления, в том, что они имеют состояние, поэтому вы не можете просто изменить заголовок, необходимо изменить его для данного `UIControlState`. Пример:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

Можно задать цвет заголовка для кнопки с помощью `SetTitleColor` метод. Пример:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

А также изменять названия тень с помощью `SetTitleShadowColor`. Пример:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Можно задать теневой заголовок для изменения из *Engraved* для *рельеф* после кнопки выделения, используя следующий код:

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

Кроме того можно использовать атрибутами текст в качестве кнопки заголовка. Пример:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>Изображения кнопки

Объект `UIButton` может содержать образ, присоединенные к ней и можно использовать в качестве его фонового изображения.

Чтобы задать фоновое изображение кнопки для данного `UIControlState`, используйте следующий код:

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Задать `AdjustsImageWhenHiglighted` свойства `true` для рисования изображение светлее при выделении кнопки (это значение по умолчанию). Задать `AdjustsImageWhenDisabled` свойства `true` для рисования изображения темнее, когда кнопка отключена (опять же, это значение по умолчанию).

Чтобы задать изображение, отображаемое на кнопке, используйте следующий код:

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

Используйте `TintColor` свойство для задания оттенок цвета, который применяется к заголовок и значок для кнопки. Для кнопок `Custom` тип, это свойство не делает ничего, необходимо реализовать `TintColor` поведение самостоятельно.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован разработки и работы с кнопками внутри приложения Xamarin.tvOS. Показали, как работать с кнопками в конструкторе iOS и способ создания кнопки в коде C#. Наконец оно было показано, как изменить заголовок, кнопки и изменить его стиль и внешний вид.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
