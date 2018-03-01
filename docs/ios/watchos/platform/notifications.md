---
title: "Уведомления"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0f1d968dcee0cb9b6cd0cee8fa60be4f4dbb2833
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="notifications"></a>Уведомления

Посмотрите, как приложения могут получать уведомления, если их поддерживает содержащего приложения iOS. Нет уведомлений встроенные обработки, не *требуется* добавить поддержку дополнительных уведомлений, описанных ниже, однако если вы хотите настроить нужное поведение и внешний вид читайте дальше.

Ссылаться на [iOS уведомления](~/ios/platform/user-notifications/deprecated/index.md) doc Дополнительные сведения о добавлении поддержки уведомлений в приложения iOS в решение.

## <a name="creating-notification-controllers"></a>Создание уведомления контроллеров

На раскадровку уведомления контроллеры имеют специальный тип перейти их запуска. При перетаскивании нового **уведомления интерфейса контроллера** на раскадровку будет иметь segue присоединенного:

![](notifications-images/notification-storyboard1.png "Новый контроллер интерфейса уведомлений с присоединенного segue")

Если перейти уведомления выбран можно изменить его свойства:

![](notifications-images/notification-storyboard2.png "Перейти выбранного уведомления")

После настройки контроллера это может выглядеть как в следующем примере из WatchKitCatalog:

![](notifications-images/notifications-segue.png "Свойства уведомлений")


Существует два типа уведомлений:

- **Внешний вид Short** -непрокручиваемый статического представления, определенные системой.

- **Внешний вид долго** - прокрутки, настраиваемые представления, определенные пользователем! Можно указать статическую версию более простой и более сложные динамическую версию.

### <a name="short-look-notification-controller"></a>Short вид уведомления контроллера

Значок приложения, имя приложения и строка заголовка уведомления состоит short вид пользовательского интерфейса.

Если пользователь не будет пропускать уведомления, система автоматически переключается долго вид уведомление, которое предоставляет дополнительные сведения.


### <a name="long-look-notification-controller"></a>Внешний вид долго уведомления контроллера

Операционная система решает, следует ли отображать статические или динамические представления на основе ряда факторов. Необходимо указать статический интерфейс и при необходимости можно также включить динамический интерфейс для уведомлений.

#### <a name="static"></a>Static

Статическое представление должно быть простой и быстрый для отображения.

![](notifications-images/notification-static.png "Статическое представление")

#### <a name="dynamic"></a>Динамический

Динамическое представление можно отображать больше данных и обеспечивать более интерактивными.

![](notifications-images/notification-dynamic.png "Динамическое представление")


## <a name="generating-notifications"></a>Создание уведомления

Уведомления могут поступать из удаленного сервера ([службы уведомлений Push Apple](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), или APNS) или может создаваться в приложении iOS локально.

Ссылаться на [iOS уведомления Пошаговое руководство](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) пример того, как создавать локальные уведомления и [WatchNotifications пример](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) рабочий пример.

Должен иметь локальные уведомления `AlertTitle` набор отображаемых на Apple Watch - `AlertTitle` строка отображается в интерфейсе Short вид. Как `AlertTitle` и `AlertBody` отображаются в списке уведомлений; и `AlertBody` отображается в интерфейсе вид долго.

На этом снимке экрана показано `AlertTitle` будет отображаться в списке уведомлений и `AlertBody` отображается в интерфейсе долго вид (с помощью [пример кода](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "На этом снимке экрана показано AlertTitle, отображаемых в списке уведомлений") ![ ] (notifications-images/watch-notificationcontroller-sml.png "AlertBody, отображается в интерфейсе вид долго")

## <a name="testing-notifications"></a>Тестирование уведомления

Уведомления (локальные и удаленные) можно только правильно протестировать на устройстве, однако их можно имитировать с помощью **.json** файла в симуляторе iOS.

### <a name="testing-on-apple-watch"></a>Тестирование на Apple Watch

При тестировании уведомлений на Apple Watch, следует помнить, что [документации компании Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) указано следующее:

> По прибытии одного приложения локальным или удаленным уведомления на iPhone пользователя iOS решает, следует ли отображать уведомления о на iPhone или Apple Watch.

Это alluding означает, что операций ввода-вывода, решает ли уведомление будет отображаться на iPhone или часами. Если парной iPhone активна при получении уведомления, уведомление скорее всего, будет отображаться на iPhone и *не* направлено в часы.

Чтобы убедиться, что уведомление отображается на часах, отключить экран iPhone (один раз, нажав кнопку питания) или оставить переходят в спящий режим. Если парной Контрольное значение находится в диапазоне, питания и изношен на ваш манжеты, уведомления будут направляться существует и на часах (сопровождается слабая).

### <a name="testing-on-the-ios-simulator"></a>Тестирование на симуляторе iOS

Вы *должен* предоставляют полезные данные JSON тестирования при тестировании режим уведомления в симуляторе iOS. Задайте путь **пользовательские аргументы выполнения** окна в Visual Studio для Mac.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Visual Studio для Mac будут отображаться дополнительные параметры, когда расширение Контрольные значения задается как **запускаемый проект**.
Щелкните правой кнопкой мыши проект расширения контрольных значений и выберите команду **запуска с > пользовательских параметров...** :
    
[![](notifications-images/runwith-customparams-sml.png "Работает с пользовательскими свойствами")](notifications-images/runwith-customparams.png)
    
При этом откроется **аргументы выполнения** окно, которое содержит **WatchKit** вкладки. Выберите **уведомления** и предоставляют полезные данные JSON, нажмите клавишу **Execute** для запуска наблюдения за приложения в симуляторе:
    
[![](notifications-images/runwith-execargs-sml.png "Выбрать значение по умолчанию для полезных данных уведомления")](notifications-images/runwith-execargs.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Установка полезные данные уведомления теста в Visual Studio щелкните правой кнопкой мыши на модуле контрольных значений для изменения **свойства проекта**. Последовательно выберите пункты **отладки** , выберите файл JSON уведомления из списка (он автоматически перечислит все включенные в проект файлы JSON).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Выберите файл JSON уведомления")](notifications-images/runwith-execargs-vs.png)

При расширении **запускаемый проект**, Visual Studio будет отображать дополнительные параметры, как показано ниже. Выберите один из **уведомления** параметров для запуска приложения Контрольные значения **уведомления** режиме (с помощью JSON-файла, выбранного в окне «Свойства»):
    
![](notifications-images/runwith-vs.png "Меню «устройства»")

-----

При тестировании на симуляторе файлом по умолчанию полезные данные JSON, уведомления контроллера по умолчанию выглядит следующим образом:

![](notifications-images/notification-debug-sml.png "Уведомление о примере")

Можно также использовать [командной строки](~/ios/watchos/troubleshooting.md#command_line) для запуска симулятора iOS.

### <a name="example-notification-payload"></a>Пример полезные данные уведомления

В [каталога комплект средств для наблюдения за](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) существует пример приведен пример файла полезных данных JSON **NotificationPayload.json** (указанные ниже).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>Связанные ссылки

- [WatchNotifications (локальном уведомлений) (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Документы Apple Watch комплект средств для уведомления](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
