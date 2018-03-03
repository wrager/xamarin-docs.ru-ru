---
title: "Предупреждения"
description: "В этой статье рассматривается работа с оповещениями в приложении Xamarin.Mac. Здесь описывается создание и отображение оповещений из кода C# и отвечать на запросы на взаимодействие с пользователем."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 73b0a3292d7b1681b4086e8366e8b813194969a9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="alerts"></a>Предупреждения

_В этой статье рассматривается работа с оповещениями в приложении Xamarin.Mac. Здесь описывается создание и отображение оповещений из кода C# и отвечать на запросы на взаимодействие с пользователем._

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к тому же предупреждений, которые разработчик, работающий *Objective-C* и *Xcode* does. 

Предупреждение — это специальный тип диалогового окна, которое появляется при возникновении серьезной ошибки (например, ошибки) или как предупреждение (например, Подготовка к удалению файла). Поскольку диалоговое окно предупреждение, необходимо также ответа пользователя перед закрытием.

[ ![](alert-images/alert06.png "Пример оповещения")](alert-images/alert06.png)

В этой статье мы обсудим основы работы с оповещениями в приложении Xamarin.Mac. 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>Общие сведения о оповещения

Предупреждение — это специальный тип диалогового окна, которое появляется при возникновении серьезной ошибки (например, ошибки) или как предупреждение (например, Подготовка к удалению файла). Поскольку предупреждения мешать работе пользователя, так как они должны отключенные для продолжения пользователь на свои задачи, необходимо исключить отображение оповещение, если это действительно необходимо.

Apple предлагаются следующие рекомендации.

- Не используйте оповещение просто для того, чтобы предоставить пользователям сведения.
- Не отображать оповещение о распространенных, отменяемом действий. Даже если такой ситуации может привести к потере данных.
- Если ситуация Заметьте, предупреждение, избегайте использования любой другой элемент пользовательского интерфейса или метод для его отображения.
- Значок предупреждения должно использоваться ограниченно.
- Описаны аварийной ситуации четко и четко в предупреждающем сообщении.
- Действие, которое описаны в вашей предупреждающее сообщение должно соответствовать имени кнопки по умолчанию.

Дополнительные сведения см. в разделе [оповещения](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>Составляющие оповещение

Как уже говорилось выше, предупреждения должно отображаться для пользователя приложения при возникновении серьезной проблемы или как предупреждение к потере данных (например, закрытие записи несохраненный файл). В Xamarin.Mac предупреждение создается в коде C#, например:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

Приведенный выше код выводит на экран оповещение со значком приложения, наложенные на значок предупреждения, заголовок, предупреждающее сообщение и один **ОК** кнопки:

[ ![](alert-images/alert01.png "Предупреждение с кнопка "ОК"")](alert-images/alert01.png)

Apple предоставляет несколько свойств, которые можно использовать для настройки предупреждения:

- **AlertStyle** определяет тип предупреждения как один из следующих:
    - **Предупреждение** — используется, чтобы предупредить пользователя текущего или каком-либо событие, которое не является критически важным. Это стиль по умолчанию.
    - **Информационные** — используется, чтобы предупредить пользователя о текущей или предстоящих событий. В настоящее время нет разницы между видимым **предупреждение** и **информационное сообщение**
    - **Критические** — используется, чтобы предупредить пользователя о серьезных последствий предстоящих события (например, при удалении файла). Этот тип предупреждений должно использоваться ограниченно.
- **MessageText** -это основного сообщения или заголовок оповещения и быстро определить ситуации для пользователя.
- **InformativeText** -это текст предупреждения, где необходимо четко определить ситуации и функциональны более удобные для пользователя.
- **Значок** -позволяет пользовательский значок, отображаемый для пользователя.
- **HelpAnchor** & **ShowsHelp** -позволяет предупреждение, чтобы быть интегрированы в приложение HelpBook и отобразить справку для предупреждения.
- **Кнопки** -по умолчанию, предупреждение имеет только **ОК** кнопки, но **кнопки** коллекции можно добавить дополнительные варианты по мере необходимости.
- **ShowsSuppressionButton** — Если `true` отображает флажок, который пользователь может использовать для подавления данного предупреждения, последующие вхождения событие, вызвавшее его.
- **AccessoryView** -позволяет присоединить другой вложенное представление с оповещением для предоставления дополнительных сведений, таких как добавление **текстовое поле** для ввода данных. Если задать новый **AccessoryView** или измените существующую, необходимо вызвать `Layout()` метод для настройки макета, отображается предупреждение.

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>Отображение оповещения

Существует два разных способа, что предупреждение может быть отображается, свободные или в качестве листа. Следующий код отображает предупреждение как свободные:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
Если этот код выполняется, отображается следующее:

[ ![](alert-images/alert02.png "Простое предупреждение")](alert-images/alert02.png)

Следующий код отображает одного оповещения в качестве листа:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

При выполнении этого кода ниже будет отображаться:

[ ![](alert-images/alert03.png "Оповещения отображаются в виде листа")](alert-images/alert03.png)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>Работа с кнопками, предупреждения

По умолчанию отображается оповещение только **ОК** кнопки. Однако вы не ограничены, можно создать дополнительные кнопки путем добавления их к **кнопки** коллекции. В следующем коде создается свободные предупреждение с **ОК**, **отменить** и **может быть** кнопки:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

Самый первый добавлена кнопка будет _кнопку по умолчанию_ , будут активированы, если пользователь нажмет клавишу ВВОД. Возвращаемое значение будет целое, представляющее какую кнопку нажатой пользователем. В нашем случае возвращаются следующие значения:

- **ОК** — 1000.
- **Отмена** — 1001.
- **Может быть** - 1002.

Если мы запустим код, ниже будет отображаться:

[ ![](alert-images/alert04.png "Предупреждение с трех кнопок")](alert-images/alert04.png)

Ниже приведен код для одного оповещения в качестве листа:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
При выполнении этого кода ниже будет отображаться:

[ ![](alert-images/alert05.png "Три кнопки предупреждения, отображаемого в качестве листа")](alert-images/alert05.png)

> [!IMPORTANT]
> Никогда не следует добавить более трех кнопок на предупреждение.

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>Отображение Отключение кнопки

Если предупреждение `ShowSuppressButton` свойство `true`, предупреждение отображается флажок, который пользователь может использовать для подавления данного предупреждения, последующие вхождения событие, вызвавшее его. Следующий код отображает свободные предупреждение с Отключение кнопки:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Если значение `alert.SuppressionButton.State` — `NSCellStateValue.On`, пользователь проверен флажок исключить, в противном случае они не имеют.

Если код выполняется, ниже будет отображаться:

[ ![](alert-images/alert06.png "Предупреждение с Отключение кнопки")](alert-images/alert06.png)

Ниже приведен код для одного оповещения в качестве листа:

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

При выполнении этого кода ниже будет отображаться:

[ ![](alert-images/alert07.png "Отображать оповещение с кнопкой подавлять как лист")](alert-images/alert07.png)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>Добавление пользовательских вложенное представление

Оповещения имеют `AccessoryView` свойство, которое можно использовать для настройки предупреждения дальнейшей и добавления такие операции, как **текстовое поле** для ввода данных пользователем. В следующем коде создается свободные предупреждение с полем ввода добавлен текст:

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

Строки ключей являются `var input = new NSTextField (new CGRect (0, 0, 300, 20));` и создается новый **текстовое поле** , мы будем добавлять оповещения. `alert.AccessoryView = input;` какие присоединяет **текстовое поле** предупреждения и вызов `Layout()` метод, который необходим для изменения размера предупреждения и не умещается в новый вложенное представление.

Если мы запустим код, ниже будет отображаться:

[ ![](alert-images/alert08.png "Если выполняется код ниже отображается")](alert-images/alert08.png)

Вот как лист одно и то же предупреждение.

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

Если мы выполнения этого кода ниже будет отображаться:

[ ![](alert-images/alert09.png "Оповещение с помощью пользовательского представления")](alert-images/alert09.png)

<a name="Summary" />

## <a name="summary"></a>Сводка

Подробное описание работы с оповещениями в приложении Xamarin.Mac вступила в этой статье. Мы узнали, что использует предупреждений, создание и Настройка предупреждений и способы работы с оповещениями в коде C# и различных типов.

## <a name="related-links"></a>Связанные ссылки

- [MacWindows (пример)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с окнами](~/mac/user-interface/window.md)
- [Рекомендации по интерфейсу отдела OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
