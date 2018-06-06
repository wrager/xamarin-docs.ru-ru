---
title: Унифицированный раскадровки в Xamarin.iOS
description: В этом документе описывается единой раскадровки в Xamarin.iOS. Унифицированный раскадровки позволяют разработчикам поддерживает несколько размеров экрана с определением единого интерфейса.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6d3324a6485f2d240ec339f6ce7f03aafe51c80c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792025"
---
# <a name="unified-storyboards-in-xamarinios"></a>Унифицированный раскадровки в Xamarin.iOS

iOS 8 включает новые, более простой в использовании механизм для создания пользовательского интерфейса — унифицированной раскадровки. С помощью одного раскадровки, чтобы охватить все размеры экрана другое оборудование, быстрых и отвечать на запросы представления могут создаваться в «конструктора-один раз, используйте многим» стиль.

Как разработчику больше нет необходимости создавать отдельный и конкретные раскадровки для устройств iPhone и iPad, они имеют возможность разработки приложений с помощью общего интерфейса, а затем настроить этот интерфейс для классов другого размера. Таким образом приложение может быть адаптирована для преимущества каждого форм-фактора и каждого пользовательского интерфейса могут быть настроены для повышения эффективности.

<a name="size-classes" />

## <a name="size-classes"></a>Размер классы

До iOS 8, разработчик использовал `UIInterfaceOrientation` и `UIInterfaceIdiom` для разделения между режимами книжной и альбомной и между устройства iPhone и iPad. В iOS8, определяется ориентацию и устройства с помощью *классы размер*.

Устройства определяются классами размер в вертикальной и горизонтальной оси, и существует два типа классов размер в iOS 8:

-  **Регулярные** — это размер больших экрана (таких как iPad) или мини-приложения, который создает впечатление большого размера (такие как `UIScrollView`
-  **Compact** — это для более мелких устройств (таких как iPhone). Этот размер учитывает ориентации устройства.


Если эти два понятия используются совместно, результатом является сетки 2 x 2, который определяет различные возможные размеры, которые могут использоваться в обоих разные ориентации, как показано на следующей схеме:

 [![](unified-storyboards-images/sizeclassgrid.png "Сетка 2 x 2, определяет различные возможные размеры, которые могут использоваться в обычных и Compact ориентации")](unified-storyboards-images/sizeclassgrid.png#lightbox)

Разработчик может создать View Controller, в котором используется любой из четырех возможности, приведет к появлению различных макетов (как показано на рисунке выше).

### <a name="ipad-size-classes"></a>iPad размер классы

IPad, в зависимости от размера, имеет **регулярного** класса размер для обоих ориентации.

 [![](unified-storyboards-images/image1.png "iPad размер классы")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone размер классы

IPhone содержит классы другого размера, в зависимости от ориентации устройства:

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone размер классы")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  Если устройство находится в книжной ориентации, имеет экрана **compact** класса по горизонтали и **регулярного** по вертикали
-  Если устройство находится в режиме альбомной ориентации, классы экран меняется с книжной ориентации.

### <a name="iphone-6-plus-size-classes"></a>Классы размер iPhone 6 Plus

Размеры аналогичны предыдущих устройства iPhone в книжной ориентации, но разные в альбомной ориентации.

[![](unified-storyboards-images/iphone6sizeclasses.png "Классы размер iPhone 6 Plus")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

Так как iPhone 6 Plus имеет достаточно большой экран, что может иметь обычный размер класс ширины в альбомной ориентации.

### <a name="support-for-a-new-screen-scale"></a>Поддержка нового экрана масштаба

IPhone 6 Plus использует новый дисплей HD Сетчатка с коэффициентом масштабирования экрана 3.0 (три раза исходного iPhone разрешения экрана). Чтобы обеспечить наилучшее воспроизведение на этих устройствах, включают новый графический объект, предназначенный для этой шкалы экрана. В Xcode 6 и более поздних версий, активов каталоги могут включать изображения на 1 x, 2 x и 3 x размеров. просто добавьте новый графических ресурсов и операций ввода-вывода будет выбирать правильный активы при работе на iPhone 6 Plus.

Изображения, поведение в iOS при загрузке также распознает `@3x` суффикса в файлы изображений. Например, если разработчик включает образ ОС (различные разрешения) в пакет приложения со следующими именами файла: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, и `MonkeyIcon@3x.png`. На iPhone 6 Plus `MonkeyIcon@3x.png` будет использоваться изображение автоматически, если разработчик загружает изображение, используя следующий код:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
Или если они назначены изображения элемента пользовательского интерфейса с помощью iOS конструктора как `MonkeyIcon.png`, `MonkeyIcon@3x.png` будет использован повторно автоматически на iPhone 6 Plus.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>Динамические запуск экранов

Запуск файла экрана отображается как экран-заставку при запуске приложения iOS для предоставления отзывов для пользователя, который приложение фактически начиная сверху. До iOS 8, разработчику придется включать несколько `Default.png` активы для каждого устройства типа, ориентацию и разрешения экрана, приложение будет выполняться на рисунке.

Новые в iOS 8, разработчик может создать отдельный atomic `.xib` файл в Xcode, который использует автоматический макет и размер классы для создания *динамического экрана запуска* , будет работать для всех устройств, разрешения и ориентации. Это не только снижает объем работ разработчика для создания и настройки всех необходимых графических ресурсов, но оно уменьшает размер установленного пакета приложения.

## <a name="traits"></a>Признаки

Признаки являются свойствами, которые можно использовать для определения того, как макет изменяется, если его среды изменяется. Они состоят из набора свойств ( `HorizontalSizeClass` и `VerticalSizeClass` на основе `UIUserInterfaceSizeClass`), а также интерфейс идиому ( `UIUserInterfaceIdiom`) и масштаб отображения.

Все состояния выше обернутых в контейнере, который ссылается Apple как коллекция признака ( `UITraitCollection`), которая содержит не только свойства, но также их значения.

## <a name="trait-environment"></a>Признак среды

Признак среды — это новый интерфейс в iOS 8 и могут возвращать коллекцию признака для следующих объектов:

-  Экраны ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  Просмотр контроллеров ( `UIViewController` ).
-  Представления ( `UIView` ).
-  Контроллер презентации ( `UIPresentationController` ).


Разработчик использует Признак коллекции, возвращенной в среде признака для определения того, каким образом должен быть размещен пользовательский интерфейс.

Все среды признака сделать иерархии, как показано на следующей схеме:

 [![](unified-storyboards-images/viewhierarchy.png "Схема иерархии признака сред")](unified-storyboards-images/viewhierarchy.png#lightbox)

Признак коллекции, в каждом выше сред признака которых будет передаваться по умолчанию от родительского среды дочерних.

Кроме получения текущей коллекции признака, среда признака имеет `TraitCollectionDidChange` метод, который может быть переопределен в подклассах представления или представление контроллер. Разработчик может использовать этот метод для изменения любого из элементов пользовательского интерфейса, зависящие от характеристик при изменении этих характеристик.

## <a name="typical-trait-collections"></a>Типичные признака коллекций

В этом разделе мы рассмотрим стандартных типов коллекций признака, пользователь будет возникать при работе с iOS 8.

Ниже приведен типичный коллекция признака, разработчик может появиться на iPhone.

|Свойство.|Значение|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Регулярное|
|`UserInterfaceIdom`|Номер телефона|
|`DisplayScale`|2.0|

Выше набор представляют собой полностью уточненное признака коллекции, как он содержит значения для всех свойств признака.

Также возможна признака коллекция, в которой отсутствуют некоторые его значения (который Apple называется *не указан*):

|Свойство.|Значение|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|Не указан|
|`UserInterfaceIdom`|Не указан|
|`DisplayScale`|Не указан|

Как правило тем не менее, при разработчик среды признака запрашивает его признака коллекции, он будет возвращать полное коллекции как показано в приведенном выше примере.

При среде признака (например, представление или View Controller) не внутри текущей иерархии представления, разработчик может получить Незаданные значения для одного или нескольких свойств признака.

Разработчик также получат частичные признака коллекцию, если они используют один из методов создания компании Apple, такие как `UITraitCollection.FromHorizontalSizeClass`, чтобы создать новую коллекцию.

Одну операцию, которая может выполняться в нескольких коллекциях признака их сравнением друг с другом, который включает в себя запросом одну коллекцию признака, если он содержит еще один. Что подразумевается под *вложения* состоит в том, для любого признака, указанный во второй коллекции, значение должно совпадать точно со значением в первой коллекции.

Для проверки используются два признаки `Contains` метод `UITraitCollection` передав значение признака, который необходимо протестировать.

Разработчик может выполнить сравнение вручную код, чтобы определить способ представления макета или просмотр контроллеров. Однако `UIKit` внутренне использует этот метод позволяет предоставлять некоторые функции, как внешний вид прокси-сервера, например.

## <a name="appearance-proxy"></a>Внешний вид прокси-сервера

Внешний вид прокси-сервера была представлена в более ранних версиях iOS, чтобы позволить разработчикам настраивать свойства их представления. Она была расширена в iOS 8, чтобы обеспечить поддержку коллекций признака.

Внешний вид учетных записей-посредников теперь включают новый метод `AppearanceForTraitCollection`, возвращающий новый внешний прокси для заданной коллекции признаков, которая была передана в. Все настройки, которые разработчик выполняет, внешний вид прокси-сервер только вступят в силу в представлениях, которые соответствуют в указанную коллекцию признака.

Обычно разработчик будет передавать в частично указанной коллекции признака для `AppearanceForTraitCollection` метода, например, только что указали горизонтальный размер класса из Compact, чтобы они могли настраивать любого представления в приложении, которое является компактным по горизонтали.

## <a name="uiimage"></a>UIImage

Другой класс, который добавлен Apple коллекция признака для `UIImage`. В прошлом было задавать разработчик @1X и @2x версии любой растровыми изображениями графические средства, которое они предназначались для включения в приложение (например, значок).

iOS 8 была увеличена, чтобы разработчик мог включать несколько версий образа в каталог образа на основе признака коллекции. Например разработчик может содержать изображение меньшего размера для работы с Compact класс признаков и полного размера образа для любой другой коллекции.

При использовании одного из образов внутри `UIImageView` класс, представление изображения, автоматически отобразит правильную версию изображения для его признака коллекции. Признак среде изменение (например, пользователь, устройство с книжной переключения альбомная), новый размер изображения в соответствии в новую коллекцию Признак и изменить его размер в соответствии с текущей версии образа, автоматически выберет представление изображения отображается.

## <a name="uiimageasset"></a>UIImageAsset

Apple добавлен новый класс для iOS 8 `UIImageAsset` для предоставления разработчику еще большего контроля над выделенной области изображения.

Актив изображения мы и добрались до другой версии образа и позволяет разработчику запросить конкретный образ, который соответствует коллекции признаков, которая была передана в. Изображения можно добавлять или удалять из ресурса изображения на лету.

Дополнительные сведения о графических ресурсов см. в разделе Apple [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) документации.

## <a name="combining-trait-collections"></a>Объединение коллекций признака

Другая функция, разработчик может выполнять с коллекциями признака является для сложения двух позволит привести в объединенный набор разделов, где Незаданные значения из одной коллекции заменяются в второй указанные значения. Это делается с помощью `FromTraitsFromCollections` метод `UITraitCollection` класса.

Как упоминалось выше, если любой из признаки не указывается в одной из коллекций Признак и указан в другом, будет иметь значение в указанную версию. Тем не менее, если имеется несколько версий заданное значение указано, значение за последние признака коллекция будет значение, которое будет использоваться.

## <a name="adaptive-view-controllers"></a>Просмотр адаптивной контроллеров

В этом разделе мы рассмотрим процесс как iOS представление и просмотр контроллеров применяют понятия признаки и размер классов автоматически быть более адаптивной разработчика приложений.

### <a name="split-view-controller"></a>Разбиение View Controller

Один контроллер представление классов, которые наиболее в iOS 8 изменились является `UISplitViewController` класса. В прошлом часто используется контроллер представление разбиение на iPad версии приложения, и они будут иметь для обеспечения полностью другой версии той же иерархии представление версия приложения для iPhone.

В iOS 8 `UISplitViewController` класс доступен на обеих платформах (iPad и iPhone), которая позволяет разработчикам создавать одной иерархии представление контроллер, который будет работать для iPhone и iPad.

Когда в альбомной ориентации iPhone, View Controller разбиение будет предоставлять его представления side-by-side, так же, как это происходит, когда будут отображаться на iPad.

### <a name="overriding-traits"></a>Переопределение признаки

Признак средах каскадом от родительского контейнера до их дочерние контейнеры, как показано в следующем график, отображающий контроллер представление разбиение на iPad в альбомной ориентации:

 [![](unified-storyboards-images/cascadingclasses01.png "Контроллер представление разбиение на iPad в альбомной ориентации")](unified-storyboards-images/cascadingclasses01.png#lightbox)

Так как iPad имеет обычный размер класс в горизонтальной и вертикальной ориентации, разделенное представление будет отображаться основные и подробные представления.

На iPhone, где размер класса compact в обоих ориентации, View Controller разбиения отображаются только подробное представление, как показано ниже:

 [![](unified-storyboards-images/cascadingclasses02.png "Разбиение View Controller отображаются только подробное представление")](unified-storyboards-images/cascadingclasses02.png#lightbox)

В приложении, где разработчик хочет отображение основные и подробные представления на iPhone в альбомной ориентации разработчик должен вставить родительского контейнера для разбиения View Controller и переопределить Признак коллекции. Как показано на следующем рисунке:

 [![](unified-storyboards-images/cascadingclasses03.png "Разработчик должен вставить родительского контейнера для разбиения View Controller и переопределить Признак коллекции")](unified-storyboards-images/cascadingclasses03.png#lightbox)

Объект `UIView` задать в качестве родительского представления контроллера разбиения и `SetOverrideTraitCollection` метод вызывается для передачи в новую коллекцию Признак и View Controller разбиение предназначенные для представления. Новая коллекция признака переопределяет `HorizontalSizeClass`, значения этого свойства `Regular`, после чего View Controller разбиения отображаются основные и подробные представления на iPhone с альбомной ориентацией.

Обратите внимание, что `VerticalSizeClass` было задано значение `unspecified`, который позволяет коллекции признака для добавления в коллекцию признаков на основе родителя, в результате чего `Compact VerticalSizeClass` дочернего разбиение View Controller.

### <a name="trait-changes"></a>Признак изменения

В этом разделе будут рассмотрены, подробно в как признак коллекций переход при изменении среды признака. Например, при повороте устройства с книжной на альбомную.

 [![](unified-storyboards-images/traittransitions01.png "Книжная, альбомная изменения признака Обзор")](unified-storyboards-images/traittransitions01.png#lightbox)

Во-первых iOS 8 выполняет некоторые настройки для подготовки к переходу вступили в силу. Затем система анимирует переход состояния. Наконец iOS 8 очищает все временные состояния, необходимые во время перехода.

iOS 8 предоставляет несколько обратных вызовов, которые разработчик может использовать для участия в изменении признака, как показано в следующей таблице:

|Phase|Callback|Описание:|
|--- |--- |--- |
|Установка|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>Этот метод вызывается в начале изменения признака перед коллекции признака возвращает значение его новым значением.</li><li>Метод вызывается при изменении значения признака коллекции, но перед любой анимации.</li></ul>|
|Анимация|`WillTransitionToTraitCollection`|Координатор перехода, переданного этому методу `AnimateAlongside` свойство, которое позволяет разработчику добавлять анимации, которые будут выполняться вместе с анимации по умолчанию.|
|Очистка|`WillTransitionToTraitCollection`|Предоставляет метод для разработчиков для включения свой собственный код очистки после перехода.|

`WillTransitionToTraitCollection` Метод отлично подходит для анимации Просмотр контроллеров вместе с изменения признака коллекции. `WillTransitionToTraitCollection` Метод доступен только на просмотр контроллеров ( `UIViewController`), а не на других признаков средах, таких как `UIViews`.

`TraitCollectionDidChange` Отлично подходит для работы с `UIView` класса, где разработчику необходимо обновить пользовательский Интерфейс, как изменяется признаки.

### <a name="collapsing-the-split-view-controllers"></a>Свертывание контроллеров разделение представления

Теперь давайте take внимание на то, что происходит, когда контроллер представление Split сворачивает из двух столбцов один столбец представления. В рамках этого изменения существует два процесса, которые нужно выполнить.

-  По умолчанию разбиение представление контроллер будет использовать контроллер основное представление как представление после свертывания. Разработчик может переопределить это поведение путем переопределения `GetPrimaryViewControllerForCollapsingSplitViewController` метод `UISplitViewControllerDelegate` и предоставляя любой контроллер представления, которые требуется отображать в свернутом состоянии.
-  Дополнительный контроллер представление должно объединяются в основной контроллер представление. Обычно разработчику не требуется предпринимать никаких действий для выполнения этого шага; Разбиение View Controller включает автоматическую обработку на этом этапе, в зависимости от устройства. Тем не менее могут быть некоторые особые случаи, где разработчик может потребоваться взаимодействовать с этим изменением. Вызов `CollapseSecondViewController` метод `UISplitViewControllerDelegate` позволяет контроллеру главное представление для отображения при возникновении свернуть, а не в представлении сведений.


### <a name="expanding-the-split-view-controller"></a>Развернув View Controller разбиение

Теперь давайте take внимание на то, что происходит, когда контроллер разбиение представление разворачивается в свернутом состоянии. Опять же существует два этапа, которые нужно выполнить.

-  Во-первых определите новый основной контроллер представление. По умолчанию разбиение View Controller будет автоматически использовать основной контроллер представление в свернутом представлении. Опять же, разработчик может переопределить это поведение, используя `GetPrimaryViewControllerForExpandingSplitViewController` метод `UISplitViewControllerDelegate` .
-  После выбора основной контроллер представление, необходимо повторно создать вторичный контроллер представление. Опять же View Controller разбиение включает автоматическую обработку на этом этапе, в зависимости от устройства. Разработчик может переопределить это поведение путем вызова `SeparateSecondaryViewController` метод `UISplitViewControllerDelegate` .


В контроллере представление Split основной контроллер представление играет роль в развертывание и свертывание представления путем реализации `CollapseSecondViewController` и `SeparateSecondaryViewController` методы `UISplitViewControllerDelegate`. `UINavigationController` реализует эти методы для автоматически отправка и отображение дополнительное представление-контроллер.

### <a name="showing-view-controllers"></a>Отображение представления контроллеров

Еще одно изменение, от которого поступил Apple для iOS 8 — так, что разработчик показывает Просмотр контроллеров. В прошлом, если приложение должно было конечного View-Controller (например, контроллер представление таблицы), а разработчик показал его (например, в ответ на пользователя коснувшись ячейки), приложения и достигают назад по иерархии контроллера Представление-контроллер навигации и вызовите `PushViewController` метода для ее, чтобы открыть новое представление.

Это окно очень тесного взаимодействия между контроллером навигации и, запущенного в среде. В iOS 8 Apple связано это, предоставляя два новых метода:

-  `ShowViewController` — Адаптируется для отображения нового контроллера представления, в зависимости от его среды. Например, в `UINavigationController` он просто помещает в стек новое представление. В контроллере представление Split новый контроллер представление отображается с левой стороны как новый основной контроллер представление. Если ни один контроллер представление контейнер присутствует, новое представление будет отображаться как модальное представление-контроллер.
-  `ShowDetailViewController` — Работает таким же образом, чтобы `ShowViewController`, но реализуется на контроллере представление Split заменить новый контроллер представление, передаваемых в представлении сведений о. Если разбиение View Controller свернут (как может отображаться в приложении iPhone), вызов будет перенаправлен на `ShowViewController` метод и новое представление будет отображаться как основной контроллер представление. Опять же если ни один контроллер представление контейнер присутствует, новое представление отображается как модальное представление-контроллер.


Эти методы работы, начиная с конечного View Controller и пройти вверх по иерархии представления, пока они найти контроллер представление правый контейнер для управления отображением нового представления.

Разработчики могут реализовывать `ShowViewController` и `ShowDetailViewController` свои собственные пользовательские представления контроллеры для получения же автоматических функциональные возможности, `UINavigationController` и `UISplitViewController` предоставляет.

### <a name="how-it-works"></a>Принцип работы

В этом разделе мы будет рассмотрим фактически реализация этих методов в iOS 8. Первый давайте взглянем на новый `GetTargetForAction` метод:

 [![](unified-storyboards-images/gettargetforaction.png "Новый метод GetTargetForAction")](unified-storyboards-images/gettargetforaction.png#lightbox)

Этот метод обходит цепочку иерархии, пока не будет найден контроллер представление контейнера. Пример:

1.  Если `ShowViewController` вызывается метод, первый контроллер представление в цепочке, которые реализует этот метод является контроллеру навигации, поэтому он используется в качестве родительского для нового представления.
1.  Если `ShowDetailViewController` вызван метод, Split View Controller является первый контроллер представление для реализации, поэтому он будет использоваться в качестве родительской.


`GetTargetForAction` Метод работает путем обнаружения представления контроллера, который реализует данное действие и затем запрос этого представления контроллера, если требуется получать это действие. Так как этот метод является общим, разработчики могут создавать свои собственные пользовательские методы, которые работают так же как встроенные в `ShowViewController` и `ShowDetailViewController` методы.

## <a name="adaptive-presentation"></a>Адаптивная презентации

В iOS 8 Apple внес презентаций Popover ( `UIPopoverPresentationController`) адаптивной также. Поэтому Popover представления View Controller автоматически будут представлять обычный режим Popover в обычный размер класс, но будет отображаться в полноэкранном режиме в классе размер по горизонтали compact (например, на устройстве iPhone).

В соответствии с изменениями в системе единой раскадровки, создан новый объект контроллера для представленному Просмотр контроллеров управления — `UIPresentationController`. Этот контроллер создается с момента представлены View Controller до его закрытия. Как управление класса, его можно считать суперкласса через представление-контроллер, он реагирует на изменения устройства, которые влияют на View Controller (например ориентацию страницы), которой затем последовательно сбоев в представлении контроллер управляет контроллера представления.

Когда разработчик отображает представление контроллере, используя `PresentViewController` метода управления процессом представления передается `UIKit`. UIKit обрабатывает (среди прочего) правильно контроллера для создания стиля, с единственным исключением является когда контроллер представление имеет набор стилей `UIModalPresentationCustom`. Здесь, приложение может обеспечивать его собственный PresentationController вместо `UIKit` контроллера.

### <a name="custom-presentation-styles"></a>Стили пользовательского представления

Стиль пользовательское представление разработчики имеют возможность использовать настраиваемый контроллер презентации. Этот настраиваемый контроллер может использоваться для изменения внешнего вида и поведения, оно является allied для представления.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Работа с классами размер

Адаптивная проекта Xamarin фотографии, включенную в этой статье дает рабочий пример использования классов размер и адаптивной Просмотр контроллеров в приложении iOS 8 единого интерфейса.

Во время приложение создает его пользовательского интерфейса полностью из кода, а не с помощью конструктора операций ввода-ВЫВОДА и создание единой раскадровки, применяются те же способы. Далее в этой статье мы покажем, как использовать классы размер с единой раскадровки и конструктор в приложении Xamarin iOS.

Теперь давайте более подробно рассмотрим как проект адаптивной фотографии реализует некоторые возможности, размер класса в iOS 8 для создания адаптивной приложения.

### <a name="adapting-to-trait-environment-changes"></a>Адаптация для изменения среды признака

При запуске приложения адаптивной фотографий на устройстве iPhone при повороте устройства с книжной на альбомную, View Controller разбиение отобразит master и подробные сведения:

 [![](unified-storyboards-images/rotation.png "Разбиение View Controller отобразит оба главного и просмотреть подробные сведения, как показано ниже")](unified-storyboards-images/rotation.png#lightbox)

Это достигается путем переопределения `UpdateConstraintsForTraitCollection` View-Controller и корректировки ограничения на основании значения `VerticalSizeClass`. Пример:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>Добавление анимации перехода

При свертывании в развернутом контроллер представление разбиения в адаптивной фотографии, приложение выходит из анимации добавляются к анимациям по умолчанию путем переопределения `WillTransitionToTraitCollection` метод контроллера представления. Пример:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>Переопределение признака среды

Как показано выше, приложение адаптивной фотографии принудительно View Controller разбиение, и сведения о и режимов образца после перехода устройства iPhone в альбомном режиме.

Это осуществлялось с помощью следующий код в представление-контроллер:

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>Развертывание и свертывание View Controller разбиение

Далее рассмотрим реализации расширяемые и сворачивание поведение View Controller разбиения в Xamarin. В `AppDelegate`при создании View Controller разбиение назначено своего делегата для этих изменений:

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

`SeparateSecondaryViewController` Проверяет метод, если фотографии отображается и выполняет действие, исходя из этого состояния. Если не фотография показывается его сворачивает вторичный контроллер представление для отображения Master View Controller.

`CollapseSecondViewController` Метод используется при расширении View Controller разбиение на предмет наличия фотографии в стеке, поэтому сворачивает обратно к этому представлению.

### <a name="moving-between-view-controllers"></a>Перемещение между контроллеров представления

Теперь давайте рассмотрим как приложение адаптивной фотографии перемещается между контроллерами представления. В `AAPLConversationViewController` класса, когда пользователь выбирает ячейку в таблице `ShowDetailViewController` метод вызывается для отображения в представлении сведений:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>Отображение раскрытие показателей

В приложении адаптивной фото есть несколько мест, где скрыто или показано на основе об изменениях в среде признака индикаторы раскрытия. Эта операция обрабатывается с помощью следующего кода:

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

Они реализованы с помощью `GetTargetViewControllerForAction` метод подробно рассматривается выше.

При отображении контроллер представление таблицы данных, она использует методы, реализованные выше ли push должно произойти и ли отображение или скрытие индикатора раскрытие соответствующим образом:

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>Новый `ShowDetailTargetDidChangeNotification` типа

Добавлен новый тип уведомлений Apple для работа с классами размер и признак среды на основе значений в контроллере представление Split `ShowDetailTargetDidChangeNotification`. Это уведомление отправляется при каждом изменении целевой подробное представление для контроллера представления разбиение, например когда контроллер раскрытию или свертыванию.

Приложение адаптивной фотографии использует это уведомление для обновления состояния индикатора раскрытия при изменении подробное представление контроллер:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

Внимательно ознакомьтесь адаптивной фотографии приложение, чтобы увидеть все способы, что размер классы, Признак коллекций и адаптивной Просмотр контроллеров можно легко создавать приложения, единой в Xamarin.iOS.

## <a name="unified-storyboards"></a>Унифицированный раскадровки

Новые единой раскадровки iOS 8, разработчик может создать его, унифицированная файл раскадровки, которые могут отображаться на устройствах iPhone и iPad, обращаясь к несколько классов размер. С помощью единой раскадровки, разработчик записывает меньше определенного кода пользовательского интерфейса и имеет только один интерфейс разработки для создания и обслуживания.

Ниже перечислены основные преимущества единого раскадровок

-  Используйте один и тот же файл раскадровки для iPhone и iPad.
-  Разверните обратной iOS 6 и iOS 7.
-  Предварительный просмотр макета для различных устройств, ориентацию и версии ОС из в Xamarin iOS конструктора.

Эта функция полностью поддерживается в Visual Studio для Mac

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Включение классов размер

По умолчанию, любой новый проект Xamarin.iOS будет размер классы. Чтобы использовать размер и классы адаптивной Segues внутри раскадровки из старого проекта, необходимо преобразовать в Xcode 6 единой раскадровки формат внутри конструктора iOS.

Чтобы сделать это открыть раскадровку, должны преобразовываться в конструкторе и проверьте iOS **используют классы размер** флажок:

 [![](unified-storyboards-images/sizeclass01.png "Использование классов размер флажок.")](unified-storyboards-images/sizeclass01.png#lightbox)

Конструктор iOS подтверждает разработчик хочет преобразования формата раскадровки для использования классов размер:

 [![](unified-storyboards-images/sizeclass02.png "Использование классов размер оповещения")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> Автоматический макет, также следует проверять размер классы для правильной работы.

### <a name="generic-device-types"></a>Типы универсальных устройств

После преобразования раскадровки для использования классов размер он будет вновь в рабочей области конструирования и **представление как** устройства будет иметь значение Generic:

 [![](unified-storyboards-images/sizeclass03.png "Просматривать как тип универсального устройства")](unified-storyboards-images/sizeclass03.png#lightbox)

При выборе типа универсального устройства все контроллеры представление будет изменен в квадрат 600 x 600. Это квадрат представляет размеры любой ширины и высоты, любой. Когда iOS конструктор находится в этом режиме, любые изменения будут установлены для всех классов размер.

Разработчик также имеет возможность просмотра рабочей области конструирования как iPhone:

 [![](unified-storyboards-images/sizeclass04.png "Просмотр рабочей области конструирования как iPhone")](unified-storyboards-images/sizeclass04.png#lightbox)

Или просмотр его iPad:

 [![](unified-storyboards-images/sizeclass05.png "Просмотр рабочей области конструктора в виде iPad")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Выберите класс размер

Селектор класса размер кнопка находится в левый верхний угол поверхности разработки (рядом с представление в виде раскрывающегося списка). Он позволяет разработчику выберите сейчас изменяется размер классы:

 [![](unified-storyboards-images/sizeclass06.png "Выберите класс размер")](unified-storyboards-images/sizeclass06.png#lightbox)

Селектор представляется выбранный размер класса как таблица 3 x 3. Каждый из квадратов в сетке представляет комбинацию классов ширины и высоты. Центральный квадрат выбирает Any Width/Any высота размер класса (который является представлением по умолчанию для раскадровки единой). При выборе этого квадратные разработчик изменяет макет по умолчанию, для которого наследуются все конфигурации.

Квадрат в верхний левый угол сетки представляет класс Compact размер ширину/Compact Высота:

 [![](unified-storyboards-images/sizeclass07.png "Класс размер высота Compact ширина/Compact")](unified-storyboards-images/sizeclass07.png#lightbox)

Этот режим соответствует iPhone с альбомной ориентацией. Квадрат в правом нижнем углу сетки представляет регулярные ширины или Regular высота размер класс, который представляет iPad:

 [![](unified-storyboards-images/sizeclass08.png "Класс регулярных ширины или Regular высота размер")](unified-storyboards-images/sizeclass08.png#lightbox)

Чтобы изменить макет для iPhone в книжной ориентации, выберите квадрат в левом нижнем углу. Это представляет класс Compact размер ширины или Regular Высота:

 [![](unified-storyboards-images/sizeclass09.png "Класс размер Compact ширины или обычной высоты")](unified-storyboards-images/sizeclass09.png#lightbox)

Щелкните в квадрате, чтобы выбрать его и рабочей области конструктора приведет к изменению размера Просмотр контроллеров в соответствии с выбранным новый:

 [![](unified-storyboards-images/sizeclass10.png "Область конструктора приведет к изменению размера Просмотр контроллеров совпадение новое выделение, как показано")](unified-storyboards-images/sizeclass10.png#lightbox)

В разделе размер класса этой статьи для получения дополнительной информации о классах размер и как они влияют на макет для iPhone и iPad.

### <a name="adaptive-segue-types"></a>Адаптивная перейти типов

Если разработчик использовал раскадровки перед, то они будут знакомы с существующие типы segue **Push**, **модального** и **Popover**. Если размер классы включены в файл единой раскадровки, адаптивной перейти перечисленные ниже (которые соответствуют новым API контроллера представления, описанных выше), становятся доступными: **Показать** и **структура** .

> [!IMPORTANT]
> При включении классов размер любой существующий segues будут преобразовываться в новых типов.

Рассмотрим пример iOS 8 приложение, которое использует единой раскадровки с контроллером представление Split с меню простой игры переходов в режим образца. При нажатии кнопки меню View Controller выбранный элемент должен отображаться в разделе "Сведения" View Controller разбиение при выполнении на iPad. На iPhone контроллера представления элемента должно быть в стек навигации.

Чтобы этого добиться, в конструкторе iOS удерживая нажатой кнопку и перетащить линию до контроллера представления для отображения. При отпускании кнопки мыши, выберите `Show Detail` перейти тип меню:

 [![](unified-storyboards-images/segue01.png "Выберите отображение сведений из перейти типа всплывающего меню")](unified-storyboards-images/segue01.png#lightbox)

Будет создан новый segue между кнопки и представление-контроллер. Теперь запустите приложение в симуляторе iPhone и будет отображаться в главном меню.

 [![](unified-storyboards-images/segue02.png "Главное меню")](unified-storyboards-images/segue02.png#lightbox)

Щелкните **выберите игры** кнопки и View Controller элемента будет помещено в стек навигации:

 [![](unified-storyboards-images/segue03.png "Элементы View Controller будет помещено в стек переходов, как показано")](unified-storyboards-images/segue03.png#lightbox)

Остановите iPhone симулятора и запустить приложение в симуляторе iPad. Переключитесь в альбомной ориентации и главного меню снова отображается:

 [![](unified-storyboards-images/segue04.png "Главное меню отображается")](unified-storyboards-images/segue04.png#lightbox)

Снова нажмите кнопку на **выберите игры** кнопки и View Controller элемента отображается в разделе "Сведения" View Controller разбиение:

 [![](unified-storyboards-images/segue05.png "Элементы показанной в поле сведения контроллера разбиение представления View Controller")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Исключение элемента из класса размер

Бывают случаи, когда данный элемент (например, представление элемента управления или ограничение) не требуется внутри определенного класса размер. Чтобы исключить элемент из класса размер, выделите нужный элемент исключаемых из **конструктора**. Прокрутите до нижней части **обозреватель свойств** и нажмите кнопку **шестеренки** раскрывающееся меню. Выберите сочетание **ширина** и **высота** исключаемый элемент из:

[![](unified-storyboards-images/exclude-a.png "Выберите сочетание ширины и высоты")](unified-storyboards-images/exclude-a.png#lightbox)

Новый *случае исключения* будут добавлены к элементу в нижней части **обозреватель свойств**. Затем снимите флажок **установленные** флажок для данного класса размер:

[![](unified-storyboards-images/exclude-b.png "Снимите этот флажок, установленные")](unified-storyboards-images/exclude-b.png#lightbox)

Переключитесь в ширину и высоту, элемент был исключен из рабочей области конструирования, оно было удалено из заданного класса размера, но не всю модель пользовательского интерфейса:

 [![](unified-storyboards-images/exclude02.png "Переключитесь в ширину и высоту, элемент был исключен из области конструктора")](unified-storyboards-images/exclude02.png#lightbox)

Переключение обратно на класс размер Any Width/Any высоту и элементом по-прежнему месте:

 [![](unified-storyboards-images/exclude03.png "Переключение обратно на размер класса Any высота Width/Any")](unified-storyboards-images/exclude03.png#lightbox)

При запуске приложения в симуляторе iPad находится этот элемент:

 [![](unified-storyboards-images/exclude04.png "Элемент, отображаемую при выполняемого приложения в симуляторе iPad")](unified-storyboards-images/exclude04.png#lightbox)

И при запуске приложения на iPhone симулятор отсутствует элемент:

 [![](unified-storyboards-images/exclude05.png "Если отсутствует элемент выполняемого приложения в симуляторе iPhone")](unified-storyboards-images/exclude05.png#lightbox)

Чтобы удалить ветвь исключения из элемента, просто выберите элемент в **конструктора**, прокрутите до нижней части **обозреватель свойств** и нажмите кнопку **-** кнопку рядом с полем так, чтобы удалить.

Реализация единой раскадровки можно найти в `UnifiedStoryboard` образец приложения Xamarin iOS 8, присоединенную к документу.

## <a name="dynamic-launch-screens"></a>Динамические запуск экранов

Запуск файла экрана отображается как экран-заставку при запуске приложения iOS для предоставления отзывов для пользователя, который приложение фактически начиная сверху. До iOS 8, разработчику придется включать несколько `Default.png` активы для каждого устройства типа, ориентацию и разрешения экрана, приложение будет выполняться на рисунке. Например `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`и т. д.

Разбиение на новых устройствах iPhone 6 и iPhone 6 Plus устройств (и предстоящих Apple Watch) с существующие iPhone и iPad устройств, представляет большого числа разного размера, ориентацию и устранения `Default.png` запуска экрана графических ресурсов, которые необходимо создаются и обслуживаются. Кроме того эти файлы могут быть очень большими и будет «Раздувание» пакета приложения конечного результата, увеличить интервал времени, необходимого для загрузки приложения в iTunes App Store (возможно ее постоянное не сможет доставить по сотовой сети) и увеличение объема хранилища на устройстве конечного пользователя.

Новые в iOS 8, разработчик может создать отдельный atomic `.xib` файл в Xcode, который использует автоматический макет и размер классы для создания *динамического экрана запуска* , будет работать для всех устройств, разрешения и ориентации. Это не только снижает объем работ разработчика для создания и настройки всех необходимых графических ресурсов, но он позволяет существенно уменьшить размер установленного пакета приложения.


Динамические экраны запуска имеют следующие ограничения и рекомендации:

 - Используйте только `UIKit` классов.
 - Используйте представление один корневой, `UIView` или `UIViewController` объекта.
 - Не делать все подключения к коду приложения (не добавляйте **действия** или **торговцам**).
 - Не добавляйте `UIWebView` объектов.
 - Не следует использовать все пользовательские классы.
 - Не используйте атрибуты среды выполнения.

Выше рекомендациям по помните давайте взглянем на добавление в существующий проект iOS 8 Xamarin динамического экрана запуска.

Выполните следующие действия:

1. Откройте **Visual Studio для Mac** и загрузить **решения** Добавление динамического экрана запуска для.
2. В **обозревателе решений**, щелкните правой кнопкой мыши `MainStoryboard.storyboard` файла и выберите **открыть с помощью** > **Xcode интерфейс построителя**:

    [![](unified-storyboards-images/dls01.png "Открыть в построителе интерфейс Xcode")](unified-storyboards-images/dls01.png#lightbox)
3. В Xcode, выберите **файл** > **New** > **файла...** :

    [![](unified-storyboards-images/dls02.png "Выберите файл / New")](unified-storyboards-images/dls02.png#lightbox)
4. Выберите **iOS** > **пользовательский интерфейс** > **запуска экрана** и нажмите кнопку **Далее** кнопки:

    [![](unified-storyboards-images/dls03.png "Выберите iOS / интерфейса пользователя или запуска экрана")](unified-storyboards-images/dls03.png#lightbox)
5. Назовите файл `LaunchScreen.xib` и нажмите кнопку **создать** кнопки:

    [![](unified-storyboards-images/dls04.png "Имя файла LaunchScreen.xib")](unified-storyboards-images/dls04.png#lightbox)
6. Измените макет экрана запуска, добавляя графических элементов с использованием ограничения макета для размещения их для данного устройства, ориентацию и размерами экранов:

    [![](unified-storyboards-images/dls05.png "Изменение макета экрана запуска")](unified-storyboards-images/dls05.png#lightbox)
7. Сохранить изменения в `LaunchScreen.xib`.
8. Выберите **целевого приложения** и **Общие** вкладки:

    [![](unified-storyboards-images/dls06.png "Выберите целевой объект приложения и вкладке \"Общие\"")](unified-storyboards-images/dls06.png#lightbox)
9. Нажмите кнопку **выберите Info.plist** кнопку, выберите `Info.plist` приложение Xamarin и нажмите кнопку **Выбор** кнопки:

    [![](unified-storyboards-images/dls07.png "Выберите Info.plist приложения Xamarin")](unified-storyboards-images/dls07.png#lightbox)
10. В **значков приложения и запустить изображения** раздела, откройте **запустите файл экрана** раскрывающийся список и выберите `LaunchScreen.xib` созданной выше:

    [![](unified-storyboards-images/dls08.png "Выберите LaunchScreen.xib")](unified-storyboards-images/dls08.png#lightbox)
11. Сохраните изменения в файле и вернуться в Visual Studio для Mac.
12. Дождитесь Visual Studio для Mac для завершения синхронизации изменений с Xcode.
13. В **обозревателе решений**, щелкните правой кнопкой мыши **ресурсов** папку и выберите **добавить** > **добавить файлы...** :

    [![](unified-storyboards-images/dls09.png "Выберите Добавление или добавьте файлы...")](unified-storyboards-images/dls09.png#lightbox)
14. Выберите `LaunchScreen.xib` файл, созданный выше и нажмите кнопку **откройте** кнопки:

    [![](unified-storyboards-images/dls10.png "Выберите файл LaunchScreen.xib")](unified-storyboards-images/dls10.png#lightbox)
15. Постройте приложение.

### <a name="testing-the-dynamic-launch-screen"></a>Тестирование экрана динамического запуска

В Visual Studio для Mac установите симулятор Сетчатка iPhone 4 и запустите приложение. Динамические экрана запуска будет отображаться в правильном формате и ориентацию:

[![](unified-storyboards-images/dls11.png "Динамические отображаться в вертикальной ориентацией экрана запуска")](unified-storyboards-images/dls11.png#lightbox)

Завершить работу приложения в Visual Studio для Mac и выберите устройства iOS 8 iPad. Запустите приложение и экран запуска будет правильно отформатирован для этого устройства и ориентацию.

[![](unified-storyboards-images/dls12.png "Динамические отображаются в горизонтальной ориентации экрана запуска")](unified-storyboards-images/dls12.png#lightbox)

Вернитесь в Visual Studio для Mac и остановить запуск приложения.

### <a name="working-with-ios-7"></a>Работа с iOS 7

Чтобы обеспечить обратную совместимость с iOS 7, просто содержат обычные `Default.png` изображения активы в обычном режиме в приложении iOS 8. операций ввода-вывода будет вернуться к предыдущему поведению и использовать эти файлы в качестве заставки при выполнении на устройстве iOS 7.

Реализация динамической экрана запуска в Xamarin можно найти в [динамического экраны запуска](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) образец приложения iOS 8, присоединенную к документу.

## <a name="summary"></a>Сводка

В данной статье рассмотрен быстрого размер классы и их влияние на макет на устройствах iPhone и iPad. Он рассматривается как признаки, Признак сред и признак коллекций работают с размер классы для создания единого интерфейса. Затрачено времени: краткий обзор адаптивной Просмотр контроллеров и работа с классами размера внутри единой интерфейсов. Хотя рассматривались реализации размер классы и интерфейсы единой полностью из кода C# в приложении iOS 8 Xamarin.

Наконец в этой статье рассматриваются основные принципы создания единой раскадровки с Xamarin iOS конструктор, который будет работать на устройствах iOS и создание один динамический экрана запуска, который будет отображаться в виде заставки на каждое устройство iOS 8.

## <a name="related-links"></a>Связанные ссылки

- [Адаптивная фотографии (пример)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [Образец StoryboardIntro](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [Динамические экраны запуска (пример)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Динамических макетов в iOS8 - развивать 2014 (видео)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
