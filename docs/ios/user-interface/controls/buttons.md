---
title: Кнопки в Xamarin.iOS
description: Класс UIButton используется для представления различных различные стили кнопки на экранах операций ввода-вывода. В этом разделе представлены различные параметры для работы с кнопками в iOS.
ms.prod: xamarin
ms.assetid: 304229E5-8FA8-41BD-8563-D19E1D2A0296
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: bf9a36c63e0c153ed950f4c3531e99e6baf77687
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789483"
---
# <a name="buttons-in-xamarinios"></a>Кнопки в Xamarin.iOS

_Класс UIButton используется для представления различных различные стили кнопки на экранах операций ввода-вывода. В этом разделе представлены различные параметры для работы с кнопками в iOS._

`UIButton`Класс представляет элемент управления button в iOS. 

Можно изменить свойства кнопки в `Properties Pad` конструктора операций ввода-вывода:


![](buttons-images/properties.png "Панель свойств конструктора операций ввода-вывода")

## <a name="creating-a-button"></a>Создание кнопки

UIButton могут создаваться в помощью всего нескольких строк кода.

Во-первых создают новые кнопки и укажите тип кнопки, что нужно:

```csharp
UIButton myButton = new UIButton(UIButtonType.System);
```

UIButtonType должно быть указано как одно из следующих:

- **Система** -это тип стандартной кнопки, используемые iOS, тип, который будет использоваться чаще всего.
- **DetailDisclosure** -представляет «выключить» тип, из которой можно скрыть или отобразить подробные сведения.
- **InfoDark** -темной подробные сведения, кнопка «i» отображается в кружке.
- **InfoLight** -индикатор подробные сведения, кнопка «i» отображается в кружке.
- **AddContact** -отображать кнопку в виде кнопки, добавить контакт.
- **Настраиваемый** -позволяет настраивать несколько характеристик кнопки.

Дополнительные сведения о типах кнопку можно найти в [типов кнопки](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/create_different_types_of_buttons/) инструкций.

Затем определите на экране размер и расположение кнопки. Пример

```csharp
myButton.Frame = new CGRect (25, 25, 300, 150);
```

Чтобы изменить текст кнопки, используйте `SetTitle` свойство на кнопке, необходимо задать строку текста и `UIControlStyle`. Пример:

```csharp
myButton.SetTitle("Hello, World!", UIControlState.Normal);
```

Задание различных свойств для каждого состояния позволяет обмениваться данными Дополнительные сведения для пользователя (например) сделать цвет текста для отключенного состояния серым). Можно переключаться между каждого состояния, с помощью iOS конструктор, или можно сделать программно. Дополнительные сведения о текст кнопки параметров и состояния посвящены [задать текст кнопки](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/set_button_text/) инструкций.

## <a name="dealing-with-user-interactions"></a>Работа с взаимодействие с пользователем


Кнопки не очень полезны, если только они заняться при щелчке! 

В iOS события для кнопки, почти всегда событий сенсорного ввода, использование взаимодействует с кнопкой на экранах по границе с ним. Список всех возможных событий UIControl [здесь](https://developer.apple.com/documentation/uikit/uicontrolevents), но наиболее часто используемые событие в iOS `TouchUpInside`. Затем можно создать обработчик событий к каким-либо, если была нажата кнопка:


```csharp
button.TouchUpInside += (sender, e) => {
    DoSomething();
};
```

### <a name="adding-events-in-the-ios-designer"></a>Добавление событий в конструкторе iOS
 
На вкладке событий в панели свойств для добавления событий для элементов управления.

Выберите событие и введите имя нового обработчика событий или выберите один из списка. Это создаст новый разделяемого метода в классе контроллера представления.

![Вкладка "события"](buttons-images/image1.png)

## <a name="styling-a-button"></a>Задание стиля кнопки

UIButtons отличаются от большинства UIKit управляет тем, что они имеют состояние, поэтому нельзя просто изменить заголовок, необходимо изменить его для каждого `UIControlState`. Задавая цвет заголовка и цвет тени выполняется аналогичным образом:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

Кроме того можно использовать атрибутами текст в качестве кнопки заголовка. Пример:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

## <a name="custom-button-types"></a>Типы пользовательских кнопок


При задании `Custom` тип кнопки, объект содержит не отрисовки по умолчанию. Можно настроить внешний вид кнопки, задав изображения для различных состояний. Например, следующий код демонстрирует добавить различные изображения для `Normal`, `Highlighted` и `Selected` состояний:


```csharp
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand.png"), UIControlState.Normal);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_Highlight.png"), UIControlState.Highlighted);
button4.SetImage (UIImage.FromBundle ("Buttons/MagicWand_On.png"), UIControlState.Selected);
```


В зависимости от того, является ли пользователь касается кнопки или нет, оно обрабатывается как один из следующих образов (`Normal`, `Highlighted` и `Selected` соответственно состояния):


![](buttons-images/image22.png "Нормальное состояние UIButton")
![](buttons-images/image23.png "UIButton состояние выделяются")
![](buttons-images/image24.png "UIButton состоянию, выбранному")

Дополнительные сведения о работе с пользовательскими кнопками посвящены [использования изображения для кнопки](https://developer.xamarin.com/recipes/ios/standard_controls/buttons/use_an_image_for_a_button/).


## <a name="related-links"></a>Связанные ссылки

- [UIButton книги](https://developer.xamarin.com/workbooks/ios/user-interface/UIbutton/uibutton.workbook)
