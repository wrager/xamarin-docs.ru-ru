---
title: watchOS элементы управления пользовательского интерфейса в Xamarin
description: В этом документе описываются различные элементы, которые доступны для использования в пользовательском интерфейсе watchOS. Он содержит описание меток, кнопки, переключатели, ползунки, изображения, разделители, карты и многое другое.
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: b56cfed8f045d824996a004539533b27d66c8cb1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791414"
---
# <a name="watchos-user-interface-controls-in-xamarin"></a>watchOS элементы управления пользовательского интерфейса в Xamarin

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) образец демонстрирует различные элементы управления watchOS. Раскадровка приложения приведен ниже (щелкните здесь, чтобы увеличить):

[![](images/storyboard-sml.png "Образец watchOS макета")](images/storyboard.png#lightbox)

Программный имена всех элементов управления добавляется префикс с `WKInterface` (например) `WKInterfaceLabel`, `WKInterfaceButton`).

|Элемент управления|Описание:|Снимок экрана|
|---|---|---|
|Метка|Используйте `SetText` и другие свойства, определяющие внешний вид текста в элементе управления label. `NSAttributedString` также поддерживается.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Кнопка|Создайте и задайте свойства в раскадровку. CTRL + перетаскивания для добавления `Action` реализовать обработчик для при его выборе.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Параметр|Используйте `SetOn` контроля состояния коммутатора.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Возможны множество различных стилей.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Изображение|Используйте `myImage.SetImage("MyWatchImage")` для загрузки изображения в Watch или `WKInterfaceDevice.CurrentDevice.AddCachedImage` кэшировать их для повторного использования на часах.<br />[Изображение элемента управления документации](~/ios/watchos/user-interface/image.md)<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Separator|Разделители используются для создания привлекательным Контрольные значения пользовательских интерфейсов.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Карта|Изображение карты статически отображается на часах, но можно изменять множество аспектов его внешнего вида, включая добавление ПИН-кодов.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Фильма & InlineMove|Фильмы может открыть сами по себе или встроенная<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Группа|Группы можно используйте для создания привлекательным Контрольные значения пользовательских интерфейсов.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Таблица|Упрощенная схема таблицы в iOS. Реализуйте `DidSelectRow` реакция на выбор пользователя (или используйте segue).<br />[Документация по таблице элемента управления](~/ios/watchos/user-interface/table.md)<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Устройство|`WKInterfaceDevice.CurrentDevice` содержит свойства, такие как `ScreenBounds`, `ScreenScale`, и `PreferredContentSizeCategory`.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Определить меню нажмите клавишу force в раскадровку и реализовать действия для каждой кнопки в коде.<br />[Документация (Force Touch) элемента управления меню](~/ios/watchos/user-interface/menu.md)<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Текстовый ввод|Используйте `PresentTextInputController` и `WKTextInputMode` перечисления.<br />[Текст документации входных данных](~/ios/watchos/user-interface/text-input.md)<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Цифровой Главная|Цифровой Главная может использоваться для выбора диска или его поворота можно отслеживать в коде.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Жесты|Существует четыре типа распознавания жестов, который может быть добавлен в сцену: Tap, считывание, панорамирование и LongPress.<br />[Каталог кода](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## <a name="related-links"></a>Связанные ссылки

- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Справочник по API пакета Контрольное значение](https://developer.xamarin.com/api/namespace/WatchKit/)
