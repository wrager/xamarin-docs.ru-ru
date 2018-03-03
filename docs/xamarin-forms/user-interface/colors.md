---
title: "Цвета"
description: "Xamarin.Forms предоставляет гибкий класс цвета между различными платформами."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 69ab69efd60c486255164987795db1937b36b97d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="colors"></a>Цвета

_Xamarin.Forms предоставляет гибкий класс цвета между различными платформами._

В этой статье описаны различные способы `Color` класс может использоваться в Xamarin.Forms.

`Color` Класс предоставляет ряд методов для создания экземпляра цвета

-  **Именованные цвета** -коллекцию общих именованные цвета, включая `Red`, `Green`, и `Blue`.
-  **FromHex** -строковое значение напоминает синтаксис, используемый в формате HTML, например «00FF00, соответствующее». Альфа-это может быть указана как первый пары символов («CC00FF00»).
-  **FromHsla** -цветового тона, насыщенности и яркости `double` значения, с необязательным значением альфа-канала (0.0-1.0).
-  **FromRgb** -красного, зеленого и синего `int` значения (0 – 255).
-  **FromRgba** -красный, зеленый, синий и альфа-канал `int` значения (0 – 255).
-  **FromUint** -задать один `double` значение, представляющее **argb**.

Вот некоторые примере цвета, назначенные `BackgroundColor` некоторых меток с помощью различных вариантов разрешенных синтаксиса:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Эти цвета отображаются на каждой из платформ. Обратите внимание, окончательный цвет - `Accent` -blue-ish цвет для iOS и Android; это значение определяется с помощью Xamarin.Forms. На Windows Phone `Accent` показаны красным цветом *так, как цвет диакритических знаков, выбранные пользователем для этого устройства*; это значение изменяется в зависимости от настроек пользователя.

 [ ![Демонстрация цвет](colors-images/colors-sml.png "Демонстрация цвет")](colors-images/colors.png "Демонстрация цвета")

## <a name="colordefault"></a>Color.Default

Используйте `Default` установите (или повторно задать) значение цвета по умолчанию платформы (представление, это представляет основной цвет для каждой платформы, для каждого свойства).

Разработчики могут использовать это значение, чтобы задать `Color` свойства, но должен **не** запрос данного экземпляра для его компонент значения RGB (они все настроек значение -1).

## <a name="colortransparent"></a>Color.Transparent

Задание цвета для очистки.

## <a name="coloraccent"></a>Color.Accent

В Windows Phone это Комплементарный, выбранного пользователем. Хорошие приложения Windows Phone использовать это как часть их стилей для предоставления собственного интерфейса.

В iOS и Android контрастных цвет, который является видимым в фоновом режиме по умолчанию, но не обязательно является цвет по умолчанию имеет значение данного экземпляра.

## <a name="additional-methods"></a>Дополнительные методы

`Color` экземпляры следующие дополнительные методы, которые можно использовать для создания новых цветов.

-  **AddLuminosity** -возвращает новый цвет путем изменения яркость указанный интервал.
-  **WithHue** -возвращает новый цвет, заменив значение, указанное в цветовой тон.
-  **WithLuminosity** -возвращает новый цвет, заменив яркость указанное значение.
-  **WithSaturation** -возвращает новый цвет, насыщенность заменив указанное значение.
-  **MultiplyAlpha** -возвращает новый цвет, изменив альфа-канал, его умножения на указанное значение.

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Этот фрагмент кода использует `Device.RuntimePlatform` свойство, чтобы задать цвет выборочно `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>С помощью из XAML

Цвета можно легко ссылаться в XAML, используя заданный цвет имена или представления Hex, показано ниже:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

## <a name="summary"></a>Сводка

Xamarin.Forms `Color` класс используется для создания ссылок поддержкой платформы цвет. Он может использоваться в общий код и XAML.


## <a name="related-links"></a>Связанные ссылки

- [ColorsSample](https://developer.xamarin.com/samples/WorkingWithColors)
- [Выбор привязываемые (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindablePicker/)
