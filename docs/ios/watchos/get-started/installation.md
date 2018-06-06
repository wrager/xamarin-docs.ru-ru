---
title: Установка и использование watchOS в Xamarin
description: Этот документ описывает, как установить и использовать watchOS с помощью Xamarin. В нем описывается установка проекта watchOS структуры, как использовать конструктор операций ввода-вывода, интеграция Xcode и предоставляет советы по устранению неполадок.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/05/2017
ms.openlocfilehash: ea0c7b6a68077cde83fa211e4e6f3432b3e39d5c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791242"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Установка и использование watchOS в Xamarin

watchOS 4 требует macOS Сьерра (10.12) с Xcode 9.

watchOS 1 первоначально требуется OS X Yosemite (10.10) с Xcode 7.

> [!WARNING]
> [обновления watchOS 1 не будет принят после 1 апреля 2018](https://developer.apple.com/news/?id=11162017a). Будущие обновления необходимо использовать watchOS 2 SDK или более поздней версии; построение с watchOS рекомендуется 4 SDK.

## <a name="project-structure"></a>Структура проекта

Приложение watch состоит из трех проектов.

- **Проект приложения iPhone Xamarin.iOS** -это обычный iPhone проект, это может быть любой из шаблонов Xamarin.iOS. Контрольное значение приложении и его расширение будет объединено внутри этого основного проекта.

- **Проект расширения Контрольное значение** -это содержит код (например, классы контроллера) для приложения контрольных значений.

- **Проект приложения для наблюдения за** -она содержит файл раскадровки пользовательский интерфейс со всеми ресурсами пользовательского интерфейса для приложения контрольных значений.

[Каталога комплект средств для наблюдения за образец](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) решения выглядит следующим образом в Xamarin.Studio:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](installation-images/catalog-solution.png "Решения в Visual Studio")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/catalog-solution-vs.png "Решения в Visual Studio")

-----

Загрузите и запустите [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) образец, чтобы приступить к работе.
Экраны из примера можно найти на [элементов управления](~/ios/watchos/user-interface/index.md) страницы.


## <a name="creating-a-new-project"></a>Создание нового проекта

Не удается создать новое «Контрольные значения решение»... вместо приложение Watch можно добавить в существующее приложение iOS. Выполните следующие действия для создания приложения контрольных значений.

1. Если у вас нет существующего проекта, сначала выберите **файл > новое решение** и создать приложение iOS (например, **одно представление приложение**):

    [![](installation-images/cycle8-2-sml.png "Выберите Файл > новое решение и создания приложения iOS")](installation-images/cycle8-2.png#lightbox)

2. После создания приложения iOS (или вы планируете использовать существующие приложения iOS), щелкните правой кнопкой мыши решение и выберите **Добавить > Добавить новый проект...** . В **новый проект** выберите **watchOS > приложения > приложения WatchKit**:

    [![](installation-images/cycle8-6-sml.png "Выберите watchOS > приложения > WatchKit приложения")](installation-images/cycle8-6.png#lightbox)

3. Следующий экран позволяет выбрать, какой проект приложения iOS должно включать Контрольное значение приложении:

    [![](installation-images/cycle8-7-sml.png "Выбрать, какой проект приложения iOS следует включать приложение watch")](installation-images/cycle8-7.png#lightbox)

4. Наконец, выберите расположение для сохранения проекта (и при необходимости включить систему управления версиями):

    [![](installation-images/cycle8-8-sml.png "Выберите расположение для сохранения проекта")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio для Mac автоматически настраивает [ссылки на проект и **Info.plist** параметры](~/ios/watchos/get-started/project-references.md) для вас.

## <a name="creating-the-watch-user-interface"></a>Создание пользовательского интерфейса Контрольное значение

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>С помощью конструктора Xamarin iOS

Дважды щелкните приложение watch **Interface.storyboard** для редактирования с помощью iOS конструктора. Можно перетащить интерфейс контроллеров и элементов управления пользовательского интерфейса на раскадровку из **элементов** и настроить их с помощью **свойства** заполнения:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](installation-images/iosdesigner-sml.png "Раскадровка в конструкторе")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](installation-images/iosdesigner-sml-vs.png "Раскадровка в конструкторе")](installation-images/iosdesigner-vs.png#lightbox)

-----

Каждый новый контроллер интерфейса, следует предоставить **класса** , выбрав его и введя имя в **свойства** pad (создает необходимые файлы фонового кода C# автоматически):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![](installation-images/iosdesigner-classname.png "Предоставьте каждого нового контроллера интерфейс класса")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](installation-images/iosdesigner-classname-vs.png "Предоставьте каждого нового контроллера интерфейс класса")

-----

Создание segues по **Ctrl + перетаскивания** от кнопки, таблицы или интерфейс контроллера на другой контроллер интерфейса.


### <a name="using-xcode-on-the-mac"></a>С помощью Xcode на компьютере Mac

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Можно продолжать использовать Xcode для формирования пользовательского интерфейса путем щелчка правой кнопкой мыши файл Interface.storyboard и выбора **открыть с помощью > Xcode интерфейс построителя**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Пользователи Visual Studio также позволяет Xcode создавать собственный пользовательский интерфейс, переключение непосредственно использовать узла Mac построения.
Откройте решение в Visual Studio для Mac, а затем правой кнопкой мыши файл Interface.storyboard и выберите **открыть с помощью > Xcode интерфейс построителя**:

-----

![](installation-images/openwith-xcode.png "Откройте в Xcode интерфейс построителя Interface.storyboard")

Если с помощью Xcode, вы должны выполните те же шаги для приложений Контрольные значения как для обычный [раскадровки iOS приложения](~/ios/user-interface/storyboards/index.md) (таких как создание выходов и действия по **Ctrl + перетаскивания** в **.h**заголовочный файл).

При сохранении раскадровки в построителе интерфейс Xcode, будут автоматически добавлены выходов и создания действий в C# **. designer.cs** файлы в проекте расширения контрольных значений.


### <a name="adding-additional-screens-in-xcode"></a>Добавление дополнительных экранов в Xcode

При добавлении дополнительных экранов (помимо тех, что входит в шаблоне по умолчанию) с помощью Xcode интерфейс построителя раскадровки **необходимо вручную добавить файлы кода C#** для каждого нового интерфейса контроллера.

Ссылаться на [Дополнительные инструкции по добавлению новых контроллеров интерфейс раскадровки](~/ios/watchos/troubleshooting.md#add).

*Xamarin iOS конструктор выполняет это автоматически, без ручного действия не требуются.*


## <a name="building"></a>Сборка

Выполняет построение проекта, содержащего приложение watch как и другие проекты iOS. Процесс построения приведет к iPhone приложение (App), с расширением Контрольные значения (.appex), который в свою очередь содержит контрольные значения меньше кода приложение (App).


## <a name="launching"></a>Запуск

Вы можете запустить Контрольное значение приложений в симуляторе, с помощью Visual Studio для Mac или Visual Studio (запускается на узле построения Mac).

Существует два режима для запуска приложения WatchKit:

 - режим обычного приложения (по умолчанию), и
- [Уведомления о](~/ios/watchos/platform/notifications.md) (требуется полезные данные уведомления теста в формате JSON).

### <a name="xcode-8-support"></a>Поддержка Xcode 8

После установки Xcode 8 (или более поздней версии), Apple Watch симуляторов отделены от операций ввода-вывода симуляторов (в отличие от [Xcode 6](#xcode6), где бы они были указаны как *внешний дисплей*).
При выборе проекта приложения контрольных значений и сделайте его запускаемым проектом, в симуляторе списке будет отображаться *iOS симуляторов* для выбора (как показано ниже).

[![](installation-images/xs-xcode8-watchos3-sml.png "Выбор типа симулятора")](installation-images/xs-xcode8-watchos3.png#lightbox)

При запуске отладки, *два* симуляторов должна начинаться - симулятор iOS *и* имитатор Apple Watch. Используйте **команда + Shift + H** перейти к меню и часами циферблате; и использовать **оборудования** меню, чтобы задать **давление Touch Force**. Прокрутки на мыши или сенсорной панели имитировать с помощью цифровых Главная.

#### <a name="troubleshooting"></a>Устранение неполадок

Следующая ошибка появится в **выходных данных приложения** при попытке запуска в имитаторе, который не имеет парной Контрольное значение:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Ссылаться на [форумы Apple](https://forums.developer.apple.com/thread/7783) инструкции по настройке симуляторов, если значения по умолчанию не работают.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 и watchOS 1

Необходимо внести *проекта расширения Контрольное значение* **запускаемый проект** перед выполнением или отладкой приложения. Не удается «запустить» Контрольное значение само приложение, и при выборе приложения iOS затем запустится в обычном режиме, в симуляторе iOS.

По умолчанию приложение watch запускается в обычном режиме **приложения** режиме (режим уведомлений или не сразу) из Visual Studio для Mac в **запуска** или **отладки** команд.

При использовании Xcode 6 только iPhone 5, iPhone 6 iPhone 5 и iPhone 6 Plus можно активировать внешний дисплей либо **Apple Watch - 38 мм** или **Apple Watch - 42 мм** которых будет приложения watch отображается.

**Примечание:** помните, что на экране Контрольное значение не отображается автоматически в симуляторе iOS при использовании Xcode 6.
Используйте **оборудования > внешних отображает** меню, чтобы показывать окно контрольных значений.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>При запуске режим уведомления

Ссылаться на [страницу уведомления](~/ios/watchos/platform/notifications.md) сведения том, как обрабатывать уведомления в коде.


Visual Studio для Mac можно запустить приложение Контрольные значения с уведомлением _режимов запуска_ для уведомлений:



Правой кнопкой мыши проект приложения контрольных значений и выберите команду **запуска с > настраиваемой конфигурации...** :


[![](installation-images/runwith-customparams-sml.png "Выполнение настраиваемой конфигурации")](installation-images/runwith-customparams.png#lightbox)


Откроется **пользовательские параметры** окно, в котором можно выбрать **уведомления** (и предоставляют полезные данные JSON), нажмите клавишу **запуска** для запуска наблюдения за приложения в симуляторе:


[![](installation-images/runwith-execargs-sml.png "Настройка уведомлений и полезных данных")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>Отладка

Отладка в Visual Studio и Visual Studio для Mac поддерживается.
Не забудьте указать файл JSON уведомления при отладке в режиме уведомления. На этом снимке экрана показана точка останова отладки попадании в приложении контрольных значений:

![](installation-images/debug-sml.png "На этом снимке экрана показана точка останова отладки попадании в приложении Контрольное значение")

После запуска инструкций, появятся вместе с приложением Контрольные значения, работающих на **iOS Simulator (Контрольное)**.
Режим уведомлений можно выбрать **Отладка > откройте системный журнал** (**CMD + /**) и использовать `Console.WriteLine` в коде.

### <a name="debugging-lifecycle-event-handlers"></a>Отладка обработчики событий жизненного цикла

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

Файлы шаблонов watchOS (такие как `InterfaceController`, `ExtensionDelegate`, `NotificationController`, и `ComplicationController`) входят в состав их методов требуется жизненный цикл уже реализован. Добавить `Console.WriteLine` вызовы и чтения **выходных данных приложения** для лучшего понимания событий жизненного цикла.



## <a name="related-links"></a>Связанные ссылки

- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Первый видео приложение Watch](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Советы по WatchKit Apple](https://developer.apple.com/watchkit/tips/)
