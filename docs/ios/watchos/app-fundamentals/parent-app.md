---
title: "Работа с родительского приложения"
description: "Совместное использование данных между приложения iOS и наблюдать за watchOS 1"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 82f85808da6776f2f718b21b2e87ff6d4d8087fd
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="working-with-the-parent-application"></a>Работа с родительского приложения

_Совместное использование данных между приложения iOS и наблюдать за watchOS 1_

> [!IMPORTANT]
> Доступ к родительскому приложению только с помощью приведенных ниже примерах работает в приложениях Контрольные значения watchOS 1.


Существуют различные способы взаимодействия между приложением контрольных значений и приложение iOS, которое входит в состав:

- Контрольное значение расширения может [вызвать метод](#code) от родительского приложения, который выполняется в фоновом режиме для iPhone.

- Контрольное значение расширения может [совместно использовать место хранения](#storage) с приложением iPhone родительского.

- Использование перемещение вручную для передачи данных из обзора или уведомления в приложение Watch, отправки пользователя на контроллер определенный интерфейс, в приложении.

Родительского приложения также иногда называют приложение контейнера.


<a name="code" />

## <a name="run-code"></a>Выполнение кода

Взаимодействие между модулем контрольных значений и родительского приложения iPhone демонстрируется [GpsWatch пример](https://developer.xamarin.com/samples/GpsWatch).
Контрольное значение расширения может запросить родительского приложения iOS выполняется некоторая обработка на его имени с помощью `OpenParentApplication` метод.

Это особенно полезно для длительных задач (включая сетевые запросы) - только родительского приложения iOS можно воспользоваться фоновой обработки для выполнения этих задач и сохранения полученных данных в расположении, доступном для расширения контрольных значений.



### <a name="watch-kit-app-extension"></a>Расширение приложения Kit Контрольное значение

Вызовите `WKInterfaceController.OpenParentApplication` расширении вашего приложения. Он возвращает `bool` , указывающее, является ли метод запрос был отправлен успешно или нет. Проверьте `error` параметром в ответе `Action` для определения какой произошла во время метод, выполняемый в приложении iPhone.

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>Приложение iOS

Все вызовы из расширения Контрольное значение приложении маршрутизируются в приложении iPhone `HandleWatchKitExtensionRequest` метод.
Если различных запросов, производимых в приложение watch, то этот метод потребуется запросить `userInfo` словарь для определения способа обработки запроса.


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>Общее хранилище

При настройке [группы приложения](~/ios/watchos/app-fundamentals/app-groups.md) затем расширений iOS 8 (включая контрольные значения расширения) могут обмениваться данными родительского приложения.

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

Можно написать следующий код в расширении приложения и родительского приложения iPhone, чтобы они могут ссылаться на общий набор `NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>Файлы

Расширение iOS приложения и контрольных значений также можно совместно использовать файлы, используя общий путь к файлу.

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

Примечание: Если путь является `null` проверьте [конфигурации группы приложений](~/ios/watchos/app-fundamentals/app-groups.md) для обеспечения профили подготовки настроены правильно и были загружены и установлены на компьютере разработчика.

Дополнительные сведения см. в разделе [возможности группами приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) документации.

## <a name="wormholesharp"></a>WormHoleSharp

Популярные открытая механизм watchOS 1 (на основе [MMWormHole](https://github.com/mutualmobile/MMWormhole)) для передачи данных или команд между родительского приложения и наблюдения за приложением.

Можно настроить группы приложения следующим образом в приложении iOS с помощью норки и посмотрите расширения:

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

Загрузка версии C# [WormHoleSharp](https://github.com/Clancey/WormHoleSharp).



## <a name="related-links"></a>Связанные ссылки

- [GpsWatch (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (пример)](https://github.com/Clancey/WormHoleSharp)
- [Справочник по WKInterfaceController Apple](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple обмена данными с содержащего приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
