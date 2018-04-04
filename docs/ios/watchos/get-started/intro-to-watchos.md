---
title: Общие сведения о watchOS
description: Обзор структуры решения watchOS и ограничения
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 0a0adbab54fc134eaf2e69cc8088713e54b15d3b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos"></a>Общие сведения о watchOS

> [!NOTE]
> Извлечение [введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md) Общие сведения о новейших функций.

## <a name="about-watchos"></a>О watchOS

Решение watchOS приложение содержит 3 проектов:

- **Просмотр расширения** — проект, который содержит код для приложения контрольных значений.
- **Просмотр приложения** — раскадровки пользовательского интерфейса и ресурсов.
- **iOS родительского приложения** — это приложение является обычной iPhone. Контрольное значение приложении и расширение объединены в приложении iPhone для доставки для наблюдения за пользователя.

В приложениях watchOS 1 код модуля выполняется на iPhone, Apple Watch эффективно стало внешнего дисплея. приложения watchOS 2 и 3 выполняются полностью на Apple Watch. На следующем рисунке показано это различие:

[ ![](intro-to-watchos-images/arch-sml.png "На этой диаграмме показана разница между watchOS 1 и watchOS 2 (и более поздней версии)")](intro-to-watchos-images/arch.png#lightbox)

Независимо от того, какая версия watchOS целью в Visual Studio для Pad решения Mac в комплексное решение будет выглядеть примерно следующим образом:

[![](intro-to-watchos-images/projectstructure-sml.png "Панель решения")](intro-to-watchos-images/projectstructure.png#lightbox)

*Родительского приложения* в watchOS решение представляет собой приложение регулярных операций ввода-вывода. Это единственный проект в решение, которое отображается **на телефоне**. Варианты использования для этого приложения включает учебники, экраны администрирования и средний уровень фильтрации, cacheing и т. д. Тем не менее, пользователь может устанавливать и запускать контрольных значений и расширения приложения без **когда-либо** с открытыми родительского приложения, поэтому при необходимости родительского приложения для выполнения единственной инициализации или администрирования, необходимо запрограммировать часы приложения и расширения сообщает пользователю, который.

Несмотря на то, что родительского приложения доставляет Контрольное значение приложения и расширения, выполняются в разных "песочницы".

На watchOS 1 они могут обмениваться данными через группу общего приложения или с помощью статической функции `WKInterfaceController.OpenParentApplication`, которых будет включен `UIApplicationDelegate.HandleWatchKitExtensionRequest` метода в приложении родительского `AppDelegate` (см. [работа с родительского приложения](~/ios/watchos/app-fundamentals/parent-app.md)).

На watchOS 2 или более поздней версии framework Контрольные значения подключения используется для связи с родительского приложения с помощью `WCSession` класса.

## <a name="application-lifecycle"></a>Жизненный цикл приложения

В расширении watch подкласс `WKInterfaceController` класс создается для каждой сцены раскадровки.

Эти `WKInterfaceController` классы являются аналогами `UIViewController` объекты в программировании операций ввода-вывода, но не имеют один и тот же уровень доступа к представлению.
Для экземпляра невозможно динамически добавлять элементы управления для или Реструктурируйте пользовательского интерфейса.
Можно, однако, скрыть и Показать элементы управления и, с некоторыми элементами управления, изменить их размер, параметры внешнего вида и прозрачности.

Жизненный цикл `WKInterfaceController` объект включает в себя следующие вызовы:

- [Переходит в спящий режим](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) : в этом методе необходимо выполнить большую часть вашей инициализации.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : вызывается незадолго до Контрольное значение приложении отображаются для пользователя. Используйте этот метод для выполнения последнего момента инициализации, запустить анимацию, и т. д.
- На этом этапе приложение Watch отображается и расширения становится доступной пользователю, ввод и обновление отображение приложения Контрольное значение в логике приложения.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) после Контрольное значение приложении закрыто пользователем, этот метод вызывается. После возврата этого метода, элементы управления пользовательского интерфейса не может изменяться до следующего `WillActivate` вызывается. Также этот метод будет вызван при разрыве подключения для iPhone.
- После расширения отключен, недоступен в программу. Ожидающие асинхронные функции **не будет** вызываться. Контрольное значение пакета расширения не могут использовать фоновую обработку режимов. Если программа активируется пользователем, но приложение не было завершено в операционной системе, будет первый метод, вызываемый `WillActivate`.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Общие сведения о жизненным циклом приложений")

## <a name="types-of-user-interface"></a>Типы пользовательского интерфейса

Существует три типа взаимодействие, которое пользователь может иметь вместе с приложением контрольных значений.
Все программируются с помощью пользовательских вложенных классов `WKInterfaceController`, поэтому широко применяется последовательность описывалось ранее жизненного цикла (уведомления осуществляется с помощью вложенных классов `WKUserNotificationController`, который сам является вложенный класс `WKInterfaceController`):

### <a name="normal-interaction"></a>Обычный взаимодействия

Большая часть взаимодействия Контрольное значение расширение веб-приложения будет более вложенные классы `WKInterfaceController` записи соответствуют сцен в приложении Контрольное значение **Interface.storyboard**. Это подробно рассматривается в [установки](~/ios/watchos/get-started/installation.md) и [Приступая к работе](~/ios/watchos/get-started/index.md) статей.
На следующем рисунке показана часть [каталога комплект средств для наблюдения за](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) образца раскадровки. Для каждой сцены, показано ниже, есть соответствующий пользовательский `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`и т.д.) в проекте расширения.

![](intro-to-watchos-images/scenes.png "Примеры взаимодействия норм.")

### <a name="notifications"></a>Уведомления

[Уведомления о](~/ios/watchos/platform/notifications.md) предназначены для Apple Watch основного варианта использования. Уведомления о локальных и удаленных поддерживаются. Взаимодействие с уведомлениями, происходит в два этапа, вызывается короткое и длинное вид.

Короткие выглядит кратко, отображаться значок приложения, его имя и название (в соответствии с `WKInterfaceController.SetTitle`).

Длинные выглядеть объединяет предоставляемой системой **переплета** области и закрыть кнопка с собственного содержимого на основе раскадровки.

`WKUserNotificationInterfaceController` расширяет `WKInterfaceController` с методами `DidReceiveLocalNotification` и `DidReceiveRemoteNotification`.
Переопределите эти методы для реагирования на события уведомления.

Дополнительные сведения о разработке пользовательского интерфейса уведомлений посвящены [рекомендациям по интерфейсам Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "Пример уведомления")

## <a name="screen-sizes"></a>Размер экрана

Apple Watch имеет два размера начертания: 38 до 42 мм, как с соотношением отображения 5:4 и дисплеем retina. Готовый к применению размеров являются:

- логических пикселях 38 мм: 136 x 170 (272 x 340 физических пикселей)
- 42 мм: 156 x 195 логических пикселях (312 x 390 физических пикселях).

Используйте `WKInterfaceDevice.ScreenBounds` для определения, какие отображение выполняется приложение контрольных значений.

Как правило это облегчает разработку текст и макет с более ограниченный отображения 38 мм и затем вертикальное масштабирование.
Если вы запустите среду большего размера, уменьшения масштаба может привести к сложный перекрытие или усечения текста.

Дополнительные сведения о [работа с размерами экранов](~/ios/watchos/app-fundamentals/screen-sizes.md).


## <a name="limitations-of-watchos"></a>Ограничения watchOS

Существуют некоторые ограничения watchOS следует учитывать при разработке приложений watchOS:

- Apple Watch устройства обеспечивают ограниченную хранилища — учитывайте доступное пространство перед загрузкой больших файлов (например) аудио- или фильма файлы).

- Многие watchOS [элементов управления](~/ios/watchos/user-interface/index.md) имеют аналоги в UIKit, но различные классы (`WKInterfaceButton` вместо `UIButton`, `WKInterfaceSwitch` для `UISwitch`, т. д.) и имеют ограниченный набор методов, по сравнению с их UIKit эквиваленты. Кроме того, watchOS имеет некоторые элементы управления, такие как `WKInterfaceDate` (для отображения даты и времени), UIKit не поддерживает.

  - Нельзя направлять уведомления только в часы или iPhone только (тип элемента управления, у пользователя есть по маршрутизация не не предусмотрено в Apple).

Другие известные ограничения / часто задаваемые вопросы:

- Apple не позволит гарнитуры Контрольные значения пользовательских сторонних поставщиков.

- API-интерфейсы, позволяющие Контрольные значения для управления iTunes на телефоне, подключенных являются закрытыми.


## <a name="further-reading"></a>Дополнительные сведения

См. в документации от Apple.

* [Разработка для набора контрольных значений](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [Просмотрите руководство по программированию набора](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Рекомендации по пользовательского интерфейса Apple Watch](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>Связанные ссылки

- [watchOS 3 каталога (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 каталога (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Программу установки и установите](~/ios/watchos/get-started/installation.md)
- [Первый видео приложение Watch](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple разработке для руководство по пакету Контрольное значение](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Советы по WatchKit Apple](https://developer.apple.com/watchkit/tips/)
- [Введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
