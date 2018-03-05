---
title: "Ползунки, коммутаторы и Сегментированный элементов управления"
ms.topic: article
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 1c24b1faf7b108466d6e93ffae8112d0dea6d844
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="sliders-switches-and-segmented-controls"></a>Ползунки, коммутаторы и Сегментированный элементов управления

<a name="Sliders" />


## <a name="sliders"></a>Ползунки

Элемент управления "ползунок" позволяет простого выбора числовое значение в диапазоне. Элемент управления по умолчанию используется значение в диапазоне от 0 до 1, но можно настроить эти ограничения.

 [ ![](slider-switch-segmented-controls-images/image25a.png "Slider")](slider-switch-segmented-controls-images/image25a.png)

На следующем рисунке показан свойства, доступные для редактирования в конструкторе:

 [ ![](slider-switch-segmented-controls-images/image26a.png "Свойства "ползунок"")](slider-switch-segmented-controls-images/image25a.png)

Эти значения можно задать в коде, как показано ниже, включая подключение обработчик для отображения выбранного значения в `UILabel` управления:

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

Также можно настроить внешний вид ползунок, параметр

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

Настраиваемые ползунок выглядит следующим образом:

 [ ![](slider-switch-segmented-controls-images/image27a.png "Пользовательские "ползунок"")](slider-switch-segmented-controls-images/image28a.png)

> [!IMPORTANT]
> В данный момент [ошибки](http://stackoverflow.com/a/19496179) вызывает `ThumbTint` для подготовки к просмотру во время выполнения не должным образом. Можно добавить следующую строку кода **перед** приведенный выше, чтобы избежать этого. [[Источника](http://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> Можно использовать любое изображение, как он будет переопределен, но убедитесь, что он помещается _в_ каталог ресурсов и вызывается в коде.

<a name="Switch" />

## <a name="switch"></a>Параметр

операций ввода-вывода использует `UISwitch` входных логическое значение, может быть представлен переключатель на других платформах. Пользователь может работать с элементом управления, перемещая *thumb* между **Вкл/Выкл** позиций.

 [ ![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png)

Возможности настройки внешнего вида коммутатора **панель свойств** конструктора, который позволит вам управлять состоянием по умолчанию **Вкл/Выкл оттенок** цвета и **ON/OFF изображение**. Это показано на рисунке ниже:

 [ ![](slider-switch-segmented-controls-images/image29a.png "Свойства коммутатора")](slider-switch-segmented-controls-images/image29a.png)

Свойства переключателя можно также задать в коде, например в приведенном ниже коде отобразится параметр со значением по умолчанию `On`:

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />


## <a name="segmented-controls"></a>Сегментированный элементов управления

Сегментированный управления является возможность разрешить пользователям взаимодействовать с небольшим числом параметров. Он располагается горизонтально, и каждый сегмент функции отдельных кнопок. При использовании конструктора, сегментированных управления можно найти в разделе **элементов > элементы управления**и должен выглядеть как на следующем рисунке:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrol.png "Сегментированный управления")](slider-switch-segmented-controls-images/segmentedcontrol.png)

Уникальная возможность конструктора позволяет для каждого сегмента, необходимо выбрать по отдельности в области конструктора, как показано ниже:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Сегментированный управления")](slider-switch-segmented-controls-images/segmentedcontrolselection.png)

Это позволяет панель свойств для более точного управления свойства каждого сегмента. Вы увидите редактируемых свойств на снимке экрана ниже:

 [ ![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Сегментированный управления")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png)

Следует отметить сегментированных стиля элемента управления рекомендуется к использованию в iOS7, что таким образом, Настройка параметров для этого в приложении iOS7 не будет действовать.

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://developer.xamarin.com/samples/Controls/)
- [Предупреждения контроллера](https://developer.xamarin.com/recipes/ios/standard_controls/alertcontroller/)
