---
title: "watchOS пользовательского интерфейса"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: 34063be728f8a13bbe5d68440d9aceba417c5627
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-user-interface"></a>watchOS пользовательского интерфейса

[ **WatchKitCatalog** ](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) образец демонстрирует различные элементы управления watchOS. Раскадровка приложения приведен ниже (щелкните здесь, чтобы увеличить):

[ ![](images/storyboard-sml.png "Образец watchOS макета")](images/storyboard.png)

Программный имена всех элементов управления добавляется префикс с `WKInterface` (например) `WKInterfaceLabel`, `WKInterfaceButton`).


<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
        <strong>Control</strong>
      </th>
      <th>
        <strong>Описание</strong>
      </th>
      <th>
        <strong>Screenshot</strong>
      </th>
    </thead>
    <tbody>
    <tr>
      <td valign="top">
Метка </td>
      <td valign="top">
Используйте <code>SetText</code> и другие свойства, определяющие внешний вид текста в элементе управления label. <code>NSAttributedString</code> также поддерживается.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/label.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Кнопка </td>
      <td valign="top">
Создайте и задайте свойства в раскадровку. <kbd>CTRL + перетащите</kbd> добавление <code>Action</code> реализовать обработчик для при его выборе.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/button.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Параметр </td>
      <td valign="top">
Используйте <code>SetOn</code> контроля состояния коммутатора.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/switch.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Slider </td>
      <td valign="top">
Возможны множество различных стилей.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/slider.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Изображение </td>
      <td valign="top">
Используйте <code>myImage.SetImage("MyWatchImage")</code> для загрузки изображения в Watch или <code>WKInterfaceDevice.CurrentDevice.AddCachedImage</code> кэшировать их для повторного использования на часах.
        <br />
        <a href="~/ios/watchos/user-interface/image.md">Изображение элемента управления документации</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/image.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Separator </td>
      <td valign="top">
Разделители используются для создания привлекательным Контрольные значения пользовательских интерфейсов.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/separator.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Карта </td>
      <td valign="top">
Изображение карты статически отображается на часах, но можно изменять множество аспектов его внешнего вида, включая добавление ПИН-кодов.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/map.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Фильма & InlineMove </td>
      <td valign="top">
Фильмы может открыть сами по себе или встроенная <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/movie.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Группа </td>
      <td valign="top">
Группы можно используйте для создания привлекательным Контрольные значения пользовательских интерфейсов.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/group.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Таблица </td>
      <td valign="top">
Упрощенная схема таблицы в iOS.
Реализуйте <code>DidSelectRow</code> реакция на выбор пользователя (или используйте segue).
        <br />
        <a href="~/ios/watchos/user-interface/table.md">Документация по таблице элемента управления</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TableDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/table.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Устройство </td>
      <td valign="top">
        <code>WKInterfaceDevice.CurrentDevice</code> содержит свойства, такие как <code>ScreenBounds</code>, <code>ScreenScale</code>, и <code>PreferredContentSizeCategory</code>.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/device.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
        <a href="~/ios/watchos/user-interface/menu.md">Меню</a>
      </td>
      <td valign="top">
Определить меню нажмите клавишу force в раскадровку и реализовать действия для каждой кнопки в коде.
        <br />
        <a href="~/ios/watchos/user-interface/menu.md">Документация (Force Touch) элемента управления меню</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/controller.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Текстовый ввод </td>
      <td valign="top">
Используйте <code>PresentTextInputController</code> и <code>WKTextInputMode</code> перечисления.
        <br />
        <a href="~/ios/watchos/user-interface/text-input.md">Текст документации входных данных</a>
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/textinput.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Цифровой Главная </td>
      <td valign="top">
Цифровой Главная может использоваться для выбора диска или его поворота можно отслеживать в коде.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/digital-crown.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top">
Жесты </td>
      <td valign="top">
Существует четыре типа распознавания жестов, который может быть добавлен в сцену: Tap, считывание, панорамирование и LongPress.
        <br />
        <a href="https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs">Каталог кода</a>
      </td>
      <td>
        <img src="Images/gestures.png" class="tableimg">
      </td>
    </tr>
    </tbody>
</table>



## <a name="related-links"></a>Связанные ссылки

- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Справочник по API пакета Контрольное значение](https://developer.xamarin.com/api/namespace/WatchKit/)
