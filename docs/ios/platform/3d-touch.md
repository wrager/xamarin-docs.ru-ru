---
title: Общие сведения о 3D Touch в Xamarin.iOS
description: В этой статье описывается использование объемных сенсорный ввод, представленных в iPhone 6s, iPhone 6s Plus. Включить эти жесты чувствительность к нажатию, считывания и pop и быстрых действий.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f6eb71409317661cdd571c708db062e06e63ff55
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786587"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Общие сведения о 3D Touch в Xamarin.iOS

_В этой статье описан с помощью нового iPhone 6s и iPhone 6s Plus 3D Touch жесты в вашем приложении._

[![](3d-touch-images/info01.png "Примеры 3D Touch приложениями с включенной поддержкой")](3d-touch-images/info01.png#lightbox)

В этой статье приводятся и общие сведения об использовании новых 3D Touch API Добавление конфиденциальные жесты давление Xamarin.iOS приложений, работающих под управлением на новый iPhone 6s и iPhone 6s Plus устройств.

3D Touch iPhone приложения является возможность не только сообщить пользователь касается экрана устройства, что оно способно для определения давление, exerting пользователя и реагировать на разных высоким уровнями.

3D Touch предоставляет следующие возможности для приложения:

- [Нехватка чувствительности](#Pressure-Sensitivity) - приложений теперь можно оценить жестких или light пользователь касается экрана и имеют преимущество этой информации.
  Например приложение рисования можно сделать линии толще или тонкие основании жестких пользователь касается экрана.
- [Выбор и Pop](#Peek-and-Pop) -приложения можно теперь позволяют пользователю взаимодействовать с данными, без необходимости перехода от их текущего контекста. Нажимая клавишу жестких на экране экрана, их можно считывать элементов, которые они нужны (например, предварительный просмотр сообщения). Нажимая клавишу сложнее, их может быть взято в элемент.
- [Быстрые действия](#Quick-Actions) -обработки для быстрого действий, таких как контекстные меню, которые может быть извлекается копии при щелчке элемента в приложении для настольных систем.
  С помощью быстрых действий, можно добавить ярлыки функций в вашем приложении непосредственно из значка приложения на главной странице.
- [Тестирование 3D Touch в симуляторе](#Testing-3D-Touch-in-the-Simulator) -необходимое оборудование Mac трехмерных приложениях поддержка сенсорного ввода можно протестировать в эмуляторе iOS.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Чувствительность к нажатию

Как упоминалось выше, с помощью новых свойств [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) класса можно измерить объем давление, пользователь применяет экран устройства iOS и эти сведения можно использовать в пользовательском интерфейсе. Например что кисть штриха более полупрозрачными или непрозрачный на основе на объема памяти.

[![](3d-touch-images/pressure01.png "Кисть штриха отображается как более полупрозрачными или непрозрачный основанные на объеме свободной")](3d-touch-images/pressure01.png#lightbox)

В результате 3D Touch, если приложение запущено на iOS 9 (или более поздней), и устройство iOS может поддерживающих 3D Touch, внесение изменений в давление приведет к `TouchesMoved` вызова события.

Например, при мониторинге `TouchesMoved` событие [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), для получения текущего давление, пользователь применяет экрана можно использовать следующий код:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce` Свойство возвращает наибольшее возможное значение для `Force` свойство [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) на устройстве iOS, приложение выполняется на основе.

> [!IMPORTANT]
> Внесение изменений в давление приведет к `TouchesMoved` событие, даже если X / Y координаты не изменились. Из-за этого изменения в поведении приложения iOS должно быть подготовлено для `TouchesMoved` событий для вызова более часто, так и для X / координаты Y должен совпадать с последнего `TouchesMoved` вызова.




Дополнительные сведения см. в разделе Apple [TouchCanvas: с помощью UITouch эффективно и рационально](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) пример приложения и [ссылку на класс UITouch](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Просмотр и Pop

3D Touch обеспечивает новые возможности, чтобы пользователь мог взаимодействовать с данными в приложении быстрее, чем когда-либо, без необходимости перехода от их текущего местоположения.

Например, если приложение отображает таблицу сообщений, пользователь может нажать жестких на элемент, чтобы просмотреть его содержимое в представлении наложения (который Apple называется *Показать*).

[![](3d-touch-images/peekandpop01.png "Пример просмотр на содержимое")](3d-touch-images/peekandpop01.png#lightbox)

Если пользователь нажмет сложнее, пользователи должны будут указать представление обычное сообщение (который называется *Pop*-ping в представление).

### <a name="checking-for-3d-touch-availability"></a>Проверка доступности 3D Touch

При работе с [UIViewController]() можно использовать следующий код, чтобы узнать, поддерживает ли приложение выполняется на устройстве iOS 3D Touch:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Этот метод может быть вызван перед *или после* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>Обработка Peek и Pop

На устройстве iOS, которая может обрабатывать 3D Touch, можно использовать экземпляр `UIViewControllerPreviewingDelegate` класса для управления отображением **Показать** и **Pop** элемента сведений. Например, при наличии контроллер представление таблицы с именем `MasterViewController ` можно использовать следующий код для поддержки **Показать** и **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview` Метод используется для выполнения **Показать** операции. Получает доступ к данным ячеек и резервное копирование таблицы, а затем загружает `DetailViewController` из текущей раскадровки. Установив `PreferredContentSize` с (0,0), мы просим по умолчанию **Показать** размер представления. Наконец, мы все, кроме ячейки, мы уже отображается с Размытие `previewingContext.SourceRect = cell.Frame` и мы возвращаем новое представление для отображения.

`CommitViewController` Повторно использует представление, мы создали **Показать** для **Pop** просмотра при нажатии сложнее.

### <a name="registering-for-peek-and-pop"></a>Регистрация для считывания и Pop

От контроллера представления, чтобы предоставить пользователю **Показать** и **Pop** элементов из мы должна быть зарегистрирована для этой службы. В примере, приведенном выше контроллера представления таблицы (`MasterViewController`), мы бы использовать следующий код:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Здесь мы вызов `RegisterForPreviewingWithDelegate` метода с экземпляром `PreviewingDelegate` созданных ранее. На устройствах iOS, поддерживающих 3D Touch пользователь может нажать жестких элемент, чтобы просмотреть его. Если они еще сложнее клавишу, элемент будет Pop в нее стандартный вид.

Дополнительные сведения см. в разделе нашей [iOS 9 образец ApplicationShortcuts](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) и Apple [ViewControllerPreviews: с помощью UIViewController, предварительный просмотр интерфейсов API](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) пример приложения [ Ссылку на класс UIPreviewAction](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [ссылку на класс UIPreviewActionGroup](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) и [UIPreviewActionItem протокол](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Быстрые действия

Используя 3D Touch и быстрых действий, можно добавить общие быстрый и простой для быстрого доступа к функциям в приложении домашней значок Экран на устройстве iOS.

Как уже говорилось выше, быстрые действия можно представить как контекстные меню, которые может быть извлекается копии при щелчке элемента в приложении для настольных систем. Быстрые действия следует использовать для предоставления ярлыки наиболее распространенные функции и компоненты приложения.

[![](3d-touch-images/quickactions01.png "Пример меню быстрых действий")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Определение статического Быстрые действия

Если один или несколько быстрых действий, необходимых для приложения являются статическими и не требуют изменений, их можно определить в приложении `Info.plist` файла. Измените этот файл во внешнем редакторе и добавьте следующие разделы:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Здесь определяются два статические элементы быстрое действие со следующими ключами:

- `UIApplicationShortcutItemIconType` -Определяет значок, отображаемый элементом быстрое действие как один из следующих значений:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Определяет подзаголовок для элемента.
* `UIApplicationShortcutItemTitle` -Определяет заголовок элемента.
* `UIApplicationShortcutItemType` — Это строковое значение, мы будем использовать для идентификации элемента в нашем приложении. Дополнительные сведения см. в следующем разделе.

> [!IMPORTANT]
> Быстрый пункты контекстного действия, которые задаются в `Info.plist` не удается открыть файл с `Application.ShortcutItems` свойство. Они только передаются в `HandleShortcutItem` обработчика событий. 





### <a name="identifying-quick-action-items"></a>Определение элементов быстрое действие

Как было показано выше, если определенный элементы быстрое действие в вашем приложении `Info.plist`, назначенный строковое значение для `UIApplicationShortcutItemType` ключа для их идентификации.

Чтобы сделать эти идентификаторы проще работать в коде, добавьте класс с именем `ShortcutIdentifier` для вашего приложения проекта и сделать ее выглядеть следующим образом:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Обработка быстрое действие

Далее необходимо изменить свое приложение `AppDelegate.cs` файл, чтобы обрабатывать действия, выбрав элемент быстрое действие из значков приложения на главной странице.

Необходимо сделать следующие изменения:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Во-первых, определим открытый `LaunchedShortcutItem` свойства для отслеживания последнего быстрое действие элемент, выбранный пользователем. Затем мы Переопределите `FinishedLaunching` метод и см. Если `launchOptions` переданы, если они содержит элемент быстрое действие. Если они есть, хранятся быстрое действие в `LaunchedShortcutItem` свойство.

Затем мы Переопределите `OnActivated` метод и передайте, выбрать любой элемент на панели быстрого запуска для `HandleShortcutItem` метод, чтобы на них реагировать. В настоящее время мы только записи сообщения **консоли**. В реальном приложении будет обрабатываться, что любой действия была необходима. После действие `LaunchedShortcutItem` свойство очищается.

Наконец, если приложение уже запущена, `PerformActionForShortcutItem` метод будет вызываться для обработки элемента быстрое действие, поэтому необходимо переопределить и вызовите нашей `HandleShortcutItem` метод таким же образом.


### <a name="creating-dynamic-quick-action-items"></a>Создание динамических быстрое действие элементов

Кроме определения статических быстрое действие элементы в вашем приложении `Info.plist` файл, можно создать динамический на лету быстрых действий. Чтобы определить два новых динамических быстрых действий, отредактируйте вашей `AppDelegate.cs` файл еще раз и измените `FinishedLaunching` метод для поиска как в следующем:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Теперь выполняется проверка на наличие `application` уже содержит ряд динамически созданные `ShortcutItems`, если это не так, мы создадим две новые `UIMutableApplicationShortcutItem` объектов для определения новых элементов и добавлять их в `ShortcutItems` массива.

Код уже добавлена в [обработки быстрое действие](#Handling-a-Quick-Action) выше будет обрабатывать эти динамические быстрых действий, как статические методы.

Следует отметить, что можно создать как статические и динамические элементы быстрое действие (как это делается здесь), не только одно из них.

Дополнительные сведения см нашей [iOS 9 образец ViewControllerPreview](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) и Apple. в разделе [ApplicationShortcuts: с помощью UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) пример приложения [ Ссылку на класс UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [ссылку на класс UIMutableApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) и [ссылку на класс UIApplicationShortcutIcon](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Тестирование 3D Touch в симуляторе

Если используется последняя версия Xcode и симулятора iOS на совместимый Mac с Touch принудительно включить сенсорной панели, можно протестировать 3D Touch функциональность в симуляторе.

Чтобы включить эту функцию, запустите любое приложение в моделируемой iPhone оборудованием, поддерживающим 3D Touch (iPhone 6s и более поздней версии). Затем выберите **оборудования** меню в iOS Simulator и включить **принудительное использование сенсорной панели для 3D touch** элемента меню:

[![](3d-touch-images/simulator01.png "Выберите в меню оборудования в симуляторе iOS и включите принудительного использования сенсорной панели для пункта меню 3D touch")](3d-touch-images/simulator01.png#lightbox)

Новые возможности active можно нажать труднее на Mac сенсорной панели для включения 3D Touch так же, как и на iPhone на реальном оборудовании.

## <a name="summary"></a>Сводка

В этой статье внес новых 3D Touch API доступны в iOS 9 для iPhone 6s и iPhone 6s Plus. Он рассматривается добавление давление чувствительности к приложению; с помощью считывания и извлекать для быстрого отображения информации в приложении в текущем контексте, без навигации; и предоставление сочетания клавиш в приложение с помощью быстрых действий наиболее часто используемые функции.



## <a name="related-links"></a>Связанные ссылки

- [iOS 9 ViewControllerPreview образца](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts образца](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Подготовка приложения iPhone 3D сенсорного ввода](https://developer.apple.com/ios/3d-touch/)
