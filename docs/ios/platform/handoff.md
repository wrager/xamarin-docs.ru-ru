---
title: Перемещение вручную в Xamarin.iOS
description: В этой статье рассматриваются работа перемещение вручную в приложения Xamarin.iOS для передачи действий пользователей между приложений, выполняющихся на пользователя в других устройствах.
ms.prod: xamarin
ms.assetid: 405F966A-4085-4621-AA15-33D663AD15CD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec324e8fb8327b622424311b89567608311a6a19
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787524"
---
# <a name="handoff-in-xamarinios"></a>Перемещение вручную в Xamarin.iOS

_В этой статье рассматриваются работа перемещение вручную в приложения Xamarin.iOS для передачи действий пользователей между приложений, выполняющихся на пользователя в других устройствах._

Apple реализовала перемещение вручную в iOS 8 и OS X Yosemite (10.10) для предоставления общих механизма для пользователя, для передачи действий работы на одном из своих личных устройств, на другое устройство под управлением одного приложения или другое приложение, которое поддерживает одного действия.

[![](handoff-images/handoff02.png "Пример выполнения операции передачи")](handoff-images/handoff02.png#lightbox)

В этой статье кратко рассмотрим Включение совместного использования в приложении Xamarin.iOS действия примет и покрытия framework сдачи подробно:

## <a name="about-handoff"></a>О перемещение вручную

Перемещение вручную (также известный как непрерывности) был создан Apple в iOS 8 и OS X Yosemite (10.10) как способ для пользователя начать действие на одном из своих устройств (iOS или Mac) и продолжить это же действие на другом свои устройства (как определено в iClou пользователя d учетной записи).

Перемещение вручную был развернут в iOS 9, чтобы новые, также поддерживают улучшенные возможности поиска. Дополнительные сведения см. в разделе нашей [поиска усовершенствования](~/ios/platform/search/index.md) документации.

Например пользователь может запустить сообщение электронной почты на их iPhone и автоматически продолжается по электронной почте на их Mac, все же сведения о сообщении, заполнено и курсор в том же расположении, что они остался в iOS.

Любой из приложений, использующих одинаковый _идентификатор команды_ , подходящих для использования перемещение вручную для продолжения действий пользователя в приложениях, при условии, что эти приложения являются либо предоставляемый через iTunes App Store или знаком зарегистрированным разработчиком (для Mac, Enterprise или нерегламентированных приложений).

Любой `NSDocument` или `UIDocument` на основе приложения автоматически имеют сдачи встроенной поддержки и требуют минимальными изменениями для поддержки перемещение вручную.

### <a name="continuing-user-activities"></a>Продолжение действия пользователя

`NSUserActivity` Класса (а также некоторые небольшие изменения в `UIKit` и `AppKit`) обеспечивает поддержку для определения действий пользователей, который потенциально может быть продолжен на другом устройстве пользователя.

Для действия должны быть переданы другой устройствах пользователя, он должен быть инкапсулирован в экземпляре `NSUserActivity`, помеченных как _текущей активности_, задать его полезные данные (данные, используемые для выполнения продолжения) и действие, затем должен быть передан в это устройство.

Перемещение вручную передает минимальные сведения, чтобы определить действие будет продолжен с более крупные пакеты данных, синхронизируемого с помощью iCloud.

На принимающем устройстве пользователь получит уведомление о том, что действие доступно для продолжения. Запускается при выборе для продолжения действий на новое устройство, указанного приложения (если он еще не работает) и полезных данных из `NSUserActivity` для перезагрузки действия.

[![](handoff-images/handoffinteractions.png "Обзор продолжение действий пользователя")](handoff-images/handoffinteractions.png#lightbox)

Только приложения, которые совместно используют одного разработчика идентификатор команды и реагировать на данной _типа действия_ пригодны для продолжения. Приложение определяет типы действий, поддерживающем под `NSUserActivityTypes` ключа из его **Info.plist** файла. Это приложение, чтобы выполнить продолжение на основе идентификатора команды, типа действия выбирает Продолжение устройства и при необходимости _название действия_.

Принимающее приложение использует сведения из `NSUserActivity` `UserInfo` словарь для настройки его пользовательского интерфейса и восстановить состояние определенного действия, чтобы переход происходит незаметно для пользователя.

Если продолжение требуется больше информации, чем могут отправляться эффективно через `NSUserActivity`, возобновление работы приложения можно отправить вызов исходного приложения и установить один или несколько потоков для передачи необходимых данных. Например если действие было редактирование большого текстового документа с несколько образов, потоковой передачи будут необходимы для передачи информации, необходимой для продолжения действий на принимающем устройстве. Дополнительные сведения см. в разделе [потоки продолжения поддерживающие](#Supporting-Continuation-Streams) разделе ниже.

Как упоминалось выше, `NSDocument` или `UIDocument` на основе приложения автоматически имеют встроенной поддержки перемещение вручную. Дополнительные сведения см. в разделе [поддержка переадресации в приложения на основе документа](#Supporting-Handoff-in-Document-Based-Apps) разделе ниже.

### <a name="the-nsuseractivity-class"></a>Класс NSUserActivity

`NSUserActivity` Класс является основным объектом в exchange перемещение вручную и использовать для инкапсуляции состояние активности пользователей, которая доступна для продолжения. Приложение будет создавать копию `NSUserActivity` все действия, он поддерживает и хочет продолжить на другом устройстве. Например редактор текста документа создаст действия для каждого документа открыт. Однако только переднее документа (показаны в первом окно или вкладку) является _текущей активности_ и таким образом, доступных для продолжения.

Экземпляр `NSUserActivity` идентифицируется как его `ActivityType` и `Title` свойства. `UserInfo` Словарь свойство используется для передачи информации о состоянии действия. Задать `NeedsSave` свойства `true` Если требуется с отложенной загрузки сведений о состоянии через `NSUserActivity`этого делегата. Используйте `AddUserInfoEntries` для объединения новых данных с других клиентов в `UserInfo` словарь по необходимости, чтобы сохранить состояние действия.

### <a name="the-nsuseractivitydelegate-class"></a>Класс NSUserActivityDelegate

`NSUserActivityDelegate` Используется для хранения сведений `NSUserActivity` `UserInfo` словарь синхронизирован с текущим состоянием действия и обновление. Сведения в действие обновления, когда потребуется системы (например, до продолжения на другом устройстве), он вызывает `UserActivityWillSave` метод делегата.

Необходимо реализовать `UserActivityWillSave` метод и вносить изменения `NSUserActivity` (такие как `UserInfo`, `Title`и т.д.) чтобы убедиться, что он по-прежнему отражает состояние текущего действия. Когда система вызывает `UserActivityWillSave` метода `NeedsSave` флаг будут очищены. При изменении свойства данных действия, необходимо задать `NeedsSave` для `true` еще раз.

Вместо использования `UserActivityWillSave` способом, описанным выше, при необходимости возможно `UIKit` или `AppKit` автоматически управлять активности пользователей. Чтобы сделать это, задайте объект респондент `UserActivity` свойство и реализуйте `UpdateUserActivityState` метод. В разделе [поддержка переадресации в ответчики](#Supporting-Handoff-in-Responders) ниже, в разделе для получения дополнительной информации.

### <a name="app-framework-support"></a>Поддержка платформа приложения

Оба `UIKit` (iOS) и `AppKit` (OS X) предоставляют встроенную поддержку для обработки в `NSDocument`, отвечающая сторона (`UIResponder`/`NSResponder`), и `AppDelegate` классы. Хотя каждая ОС реализует перемещение вручную немного по-разному, базовый механизм и API-интерфейсы одинаковы.

#### <a name="user-activities-in-document-based-apps"></a>Действия пользователя в приложениях на основе документа

Документ под управлением iOS и OS X приложения автоматически имеют встроенной поддержки перемещение вручную. Чтобы активировать такая поддержка, вам потребуется добавить `NSUbiquitousDocumentUserActivityType` ключ и значение для каждого `CFBundleDocumentTypes` входа в приложения **Info.plist** файла.

Если этот ключ присутствует, оба `NSDocument` и `UIDocument` автоматически создавать `NSUserActivity` экземпляров типа, указанного для документов на основе iCloud. Необходимо будет указать действия данного типа для каждого типа документа, который поддерживает приложение, и несколько типов документов можно использовать один и тот же тип действия. Оба `NSDocument` и `UIDocument` автоматически заполнять `UserInfo` свойство `NSUserActivity` с их `FileURL` значение свойства.

В OS X `NSUserActivity` управляется `AppKit` и связанная с ответчики автоматически становятся текущего действия, когда окно документа становится главного окна. На iOS для `NSUserActivity` объектов, управляемых `UIKit`, должен либо вызвать метод `BecomeCurrent` метод явным образом или документ `UserActivity` свойство, заданное для `UIViewController` при приложения отображается на переднем плане.

`AppKit` автоматически восстанавливать любые `UserActivity` свойства, созданные таким образом в OS X. Это происходит, если `ContinueUserActivity` возвращает метод `false` или если это нереализованные. В этом случае документ открывается с `OpenDocument` метод `NSDocumentController` и он будет получать `RestoreUserActivityState` вызова метода.

В разделе [поддержка переадресации в приложения на основе документа](#Supporting-Handoff-in-Document-Based-Apps) ниже, в разделе для получения дополнительной информации.

#### <a name="user-activities-and-responders"></a>Действия пользователя и ответчики.

Оба `UIKit` и `AppKit` автоматически управлять действий пользователей, если задать в качестве объекта респондент `UserActivity` свойство. Если состояние было изменено, необходимо задать `NeedsSave` свойство отвечающего `UserActivity` для `true`. Система автоматически сохранит `UserActivity` при необходимости, предоставив респондент время для обновления состояния путем вызова его `UpdateUserActivityState` метод.

Если несколько ответчики совместно используют один `NSUserActivity` экземпляр, они получают `UpdateUserActivityState` обратного вызова, когда система обновляет объект действия пользователя. Респондент должен вызывать `AddUserInfoEntries` метод обновления `NSUserActivity` `UserInfo` словарь для отражения текущего состояния действия на этом этапе. `UserInfo` Словарь очищается перед каждой `UpdateUserActivityState` вызова.

Разорвать связь самого действия, можно задать респондент его `UserActivity` свойства `null`. Когда платформа приложения управляемых `NSUserActivity` экземпляр не имеет более связанных ответчики или документы, он автоматически недействительным.

В разделе [поддержка переадресации в ответчики](#Supporting-Handoff-in-Responders) ниже, в разделе для получения дополнительной информации.

#### <a name="user-activities-and-the-appdelegate"></a>Действия пользователя и AppDelegate

Ваше приложение `AppDelegate` является его основной точкой входа при обработке продолжение перемещение вручную. Когда пользователь отвечает на уведомление перемещение вручную, соответствующее приложение запускается (если она не запущена) и `WillContinueUserActivityWithType` метод `AppDelegate` будет вызван. На этом этапе приложение сообщает о пользователя, который запускается продолжение.

`NSUserActivity` Экземпляр доставляется `AppDelegate` `ContinueUserActivity` вызывается метод. На этом этапе необходимо настроить пользовательский интерфейс приложения и продолжить данное действие.

В разделе [реализации сдачи](#Implementing-Handoff) ниже, в разделе для получения дополнительной информации.

## <a name="enabling-handoff-in-a-xamarin-app"></a>Включение передачи в приложение Xamarin

Из-за требований безопасности, накладываемые перемещение вручную Xamarin.iOS приложения, в котором используется платформа передачи должна быть настроена на оба портала разработчиков Apple и в файле проекта Xamarin.iOS.

Выполните следующие действия:

1. Войдите на [портала разработчиков Apple](http://developer.apple.com).
2. Щелкните **сертификатов, профили & идентификаторы**.
3. Если это еще не сделано, щелкните **идентификаторы** и создайте код для приложения (например `com.company.appname`), в противном случае изменить свой существующий идентификатор.
4. Убедитесь, что **iCloud** службы проверки для данного ИД: 

    [![](handoff-images/provision01.png "Включить службу iCloud для данного ИД")](handoff-images/provision01.png#lightbox)
5. Сохраните изменения.
4. Щелкните **профили подготовки** > **разработки** и создание новых разработок, подготовительный профиль для вас приложения: 

    [![](handoff-images/provision02.png "Создание нового профиля подготовки для приложения")](handoff-images/provision02.png#lightbox)
5. Загрузите и установите новый профиль подготовки либо использовать Xcode для загрузки и установки профиля.
6. Измените параметры проекта Xamarin.iOS и убедитесь, что вы используете подготовительный профиль, который вы только что создали: 

    [![](handoff-images/provision03.png "Выберите только что созданный профиль подготовки")](handoff-images/provision03.png#lightbox)
7. Далее следует изменить на **Info.plist** файл и убедитесь, что вы используете идентификатор приложения, который использовался для создания профиля подготовки: 

    [![](handoff-images/provision04.png "Задать идентификатор приложения")](handoff-images/provision04.png#lightbox)
8. Перейдите к пункту **фоновые режимы** раздел и выполните следующие действия: 

    [![](handoff-images/provision05.png "Включить необходимые фоновые режимы")](handoff-images/provision05.png#lightbox)
9. Сохраните изменения ко всем файлам.

С этими параметрами на месте приложения готова для доступа к API Framework перемещение вручную. Подробные сведения о подготовке см. в разделе нашей [подготовки устройства](~/ios/get-started/installation/device-provisioning/index.md) и [подготовки приложения](~/ios/get-started/installation/device-provisioning/index.md) руководства.

## <a name="implementing-handoff"></a>Реализация перемещение вручную

Действия пользователя может быть продолжен среди приложений, которые должны быть подписаны одного разработчика идентификатор команды и поддерживают один и тот же тип действия. Перемещение вручную для реализации в приложении Xamarin.iOS требуется для создания объекта действия пользователя (либо в `UIKit` или `AppKit`), обновление состояния объекта для отслеживания действий и продолжение действия на устройстве, принимающей.

### <a name="identifying-user-activities"></a>Определение действий пользователей

Является первым этапом реализации перемещение вручную для определения типов действий пользователей, которые поддерживает приложение и видеть, какие из этих действий хорошо подходят для продолжения на другом устройстве. Например: приложения ToDo, поддерживающие редактирование элементов в виде одного _типа действия пользователя_и поддерживают просмотр списка доступных элементов, как другой.

Приложение можно создать столько пользователя типов действий при необходимости, один для любой функции, которая предоставляет приложению. Для каждого типа действия пользователя приложения необходимо отследить, когда действие типа начинается и заканчивается и необходимость поддерживать актуальность состояния для продолжения этой задачи на другом устройстве.

Действия пользователя может быть продолжен любое приложение, подписана тем же Идентификатором команды без любой взаимно-однозначное сопоставление между отправляющим и принимающим приложениями. Например приложения можно создать четыре различных типа действий, используемых другой, отдельные приложения на другом устройстве. Это обычное дело между Mac версии приложения (который может иметь множество функций и функции) и приложений iOS, где каждое приложение меньшего размера и ориентированы на определенную задачу.

### <a name="creating-activity-type-identifiers"></a>Создание типа идентификаторы действий

_Идентификатор типа действия_ короткая строка добавляется в `NSUserActivityTypes` массив приложения **Info.plist** файл, используемый для идентификации данного типа действий пользователя. Будет существовать одна запись в массиве для каждого действия, который поддерживает приложение. Apple предлагает, использовать обратное DNS-нотацию для идентификатора типа действия для предотвращения конфликтов. Например: `com.company-name.appname.activity` для конкретного приложения на основе действий или `com.company-name.activity` для действий, которые могут выполняться в нескольких приложениях.

Идентификатор действия используется при создании `NSUserActivity` экземпляр для определения типа действия. После возобновления действия на другом устройстве типа действия (а также идентификатор команды приложения) определяет, какое приложение для запуска, чтобы продолжить действия.

Например, мы собираемся создавать пример приложения вызывается **MonkeyBrowser** ([Загрузите здесь](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)). Это приложение будет предоставлять четырех вкладках различные откройте URL-адрес в веб-браузер просмотра. Пользователь будет иметь возможность продолжать любую вкладку для различных операций ввода-вывода устройства на базе приложения.

Чтобы создать необходимые идентификаторы типа действий для поддержки данной возможности, измените **Info.plist** файла и переключитесь в **источника** представления. Добавить `NSUserActivityTypes` ключей и создания следующих идентификаторов:

[![](handoff-images/type01.png "Ключ NSUserActivityTypes и необходимые идентификаторы в редакторе plist")](handoff-images/type01.png#lightbox)

Мы создали четыре новых типа идентификаторы действий, один для каждой из вкладок в примере **MonkeyBrowser** приложения. При создании собственных приложений, замените содержимое `NSUserActivityTypes` массива с идентификаторами тип действия, относящиеся к действия приложения поддерживает.

### <a name="tracking-user-activity-changes"></a>Отслеживание изменений в действие пользователя

Когда мы создаем новый экземпляр `NSUserActivity` класса, будет указана `NSUserActivityDelegate` экземпляра для отслеживания изменений состояния действия. Например следующий код может использоваться для отслеживания изменения состояния:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;

namespace MonkeyBrowse
{
    public class UserActivityDelegate : NSUserActivityDelegate
    {
        #region Constructors
        public UserActivityDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void UserActivityReceivedData (NSUserActivity userActivity, NSInputStream inputStream, NSOutputStream outputStream)
        {
            // Log
            Console.WriteLine ("User Activity Received Data: {0}", userActivity.Title);
        }

        public override void UserActivityWasContinued (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity Was Continued: {0}", userActivity.Title);
        }

        public override void UserActivityWillSave (NSUserActivity userActivity)
        {
            Console.WriteLine ("User Activity will be Saved: {0}", userActivity.Title);
        }
        #endregion
    }
}
```

`UserActivityReceivedData` Метод вызывается, когда поток продолжения получает данные от отправляющего устройства. Дополнительные сведения см. в разделе [потоки продолжения поддерживающие](#Supporting-Continuation-Streams) разделе ниже.

`UserActivityWasContinued` Метод вызывается, когда другое устройство взял на себя действия из текущего устройства. В зависимости от типа действия, такие как добавление нового элемента в список ToDo приложение может требуется прервать действие на отправляющем устройстве.

`UserActivityWillSave` Метод вызывается до сохраняются все изменения в действие и синхронизированные на устройствах доступны локально. Этот метод можно использовать для внесения изменений в последнюю минуту `UserInfo` свойство `NSUserActivity` экземпляра перед его отправкой.

### <a name="creating-a-nsuseractivity-instance"></a>Создание экземпляра NSUserActivity

Все действия, которые приложение желает предоставить возможность продолжить на другом устройстве должен быть инкапсулирован в `NSUserActivity` экземпляра. Можно создавать столько действия при необходимости, приложения и характер этих действий зависит от функций и возможностей приложения в вопросе. Например приложение электронной почты может создать одно действие для создания нового сообщения, а другой для чтения сообщения.

Для нашего примера приложения новый `NSUserActivity` создается каждый раз пользователь вводит новый URL-адрес в одном режиме с вкладками веб-обозревателя. Следующий код сохраняет состояние данной вкладки:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSUserActivity UserActivity { get; set; }
...

UserActivity = new NSUserActivity (UserActivityTab1);
UserActivity.Title = "Weather Tab";
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Он создает новую `NSUserActivity` с помощью одного типа действия пользователя, созданная выше и предоставляет понятное название для действия. Он подключается к экземпляру компонента `NSUserActivityDelegate` созданной выше для наблюдения для состояния изменяется и сообщает операций ввода-вывода, что это действие пользователя-текущее действие.

### <a name="populating-the-userinfo-dictionary"></a>Заполнение в словарь сведений о пользователях

Как было показано выше, `UserInfo` свойство `NSUserActivity` класс `NSDictionary` пар ключ значение, используемое для определения состояния данного действия. Значения, хранящиеся в `UserInfo` должен быть одним из следующих типов: `NSArray`, `NSData`, `NSDate`, `NSDictionary`, `NSNull`, `NSNumber`, `NSSet`, `NSString`, или `NSURL`. `NSURL` значения данных, выберите iCloud документы автоматически корректируется так, чтобы они указывали на устройстве, принимающей один и тот же документ.

В приведенном выше примере мы создали `NSMutableDictionary` объекта и заполняется один ключ, указав URL-адрес, который пользователь просматривает момент на данной вкладке. `AddUserInfoEntries` Метод действия пользователя был использован для обновления указанного действия с данными, который будет использоваться для восстановления действия на принимающем устройстве:

```csharp
// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);
```

Apple рекомендуется указывать данные, передаваемые в barest минимум для обеспечения отправки действия своевременно принимающее устройство. Если больше требуется информация, как изменить изображение, присоединенного к документу требуется отправить, следует использовать потоки для продолжения. В разделе [потоки продолжения поддерживающие](#Supporting-Continuation-Streams) ниже, в разделе для получения дополнительных сведений.

### <a name="continuing-an-activity"></a>Продолжение действия

Перемещение вручную автоматически информировать пользователей локальных устройств iOS и OS X в физической близости от исходного устройства, которые вошли в ту же учетную запись iCloud, доступности продолжено действий пользователей. При выборе для продолжения действий на новое устройство, система запустит соответствующее приложение (с учетом идентификатор команды, а также типа действия) и сведения о его `AppDelegate` том, что продолжение должно произойти.

Во-первых, `WillContinueUserActivityWithType` метод вызывается, чтобы приложение могло бы сообщить, пользователь, что продолжение должно быть запущено. Мы используем следующий код в **AppDelegate.cs** файл наш пример приложения для обработки начальной продолжения:

```csharp
public NSString UserActivityTab1 = new NSString ("com.xamarin.monkeybrowser.tab1");
public NSString UserActivityTab2 = new NSString ("com.xamarin.monkeybrowser.tab2");
public NSString UserActivityTab3 = new NSString ("com.xamarin.monkeybrowser.tab3");
public NSString UserActivityTab4 = new NSString ("com.xamarin.monkeybrowser.tab4");
...

public FirstViewController Tab1 { get; set; }
public SecondViewController Tab2 { get; set;}
public ThirdViewController Tab3 { get; set; }
public FourthViewController Tab4 { get; set; }
...

public override bool WillContinueUserActivity (UIApplication application, string userActivityType)
{
    // Report Activity
    Console.WriteLine ("Will Continue Activity: {0}", userActivityType);

    // Take action based on the user activity type
    switch (userActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Inform view that it's going to be modified
        Tab1.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Inform view that it's going to be modified
        Tab2.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Inform view that it's going to be modified
        Tab3.PreparingToHandoff ();
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Inform view that it's going to be modified
        Tab4.PreparingToHandoff ();
        break;
    }

    // Inform system we handled this
    return true;
}
```

В приведенном выше примере регистрирует каждый контроллер представление `AppDelegate` и имеет открытый `PreparingToHandoff` метод, который отображается индикатор активности и соответствующее сообщение означает, что действие будет передан на текущем устройстве пользователя. Пример

```csharp
private void ShowBusy(string reason) {

    // Display reason
    BusyText.Text = reason;

    //Define Animation
    UIView.BeginAnimations("Show");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0.5f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PreparingToHandoff() {
    // Inform caller
    ShowBusy ("Continuing Activity...");
}
```

`ContinueUserActivity` Из `AppDelegate` будет вызываться фактически продолжить данное действие. Вновь из нашего примера приложения:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

Открытые `PerformHandoff` метод каждого контроллера представления фактически выполняет передачи и восстанавливает действия текущего устройства. В случае примера же URL-адрес отображается на данной вкладке, пользователь страниц на другое устройство. Пример

```csharp
private void HideBusy() {

    //Define Animation
    UIView.BeginAnimations("Hide");
    UIView.SetAnimationDuration(1.0f);

    Handoff.Alpha = 0f;

    //Execute Animation
    UIView.CommitAnimations();
}
...

public void PerformHandoff(NSUserActivity activity) {

    // Hide busy indicator
    HideBusy ();

    // Extract URL from dictionary
    var url = activity.UserInfo ["Url"].ToString ();

    // Display value
    URL.Text = url;

    // Display the give webpage
    WebView.LoadRequest(new NSUrlRequest(NSUrl.FromString(url)));

    // Save activity
    UserActivity = activity;
    UserActivity.BecomeCurrent ();

}
```

`ContinueUserActivity` Включает метод `UIApplicationRestorationHandler` , можно вызвать для документа или респондент на основе возобновления действия. Вам потребуется передать `NSArray` или объекты могут быть восстановлены в обработчик восстановления при вызове. Пример:

```csharp
completionHandler (new NSObject[]{Tab4});
```

Для каждого объекта, переданного его `RestoreUserActivityState` вызываемого метода. Каждый объект можно затем использовать данные в `UserInfo` словарь для восстановления собственное состояние. Пример:

```csharp
public override void RestoreUserActivityState (NSUserActivity activity)
{
    base.RestoreUserActivityState (activity);

    // Log activity
    Console.WriteLine ("Restoring Activity {0}", activity.Title);
}
```

Для приложений на базе документа, если не реализовать `ContinueUserActivity` метода или она возвращает `false`, `UIKit` или `AppKit` автоматически можно возобновить действия. В разделе [поддержка переадресации в приложения на основе документа](#Supporting-Handoff-in-Document-Based-Apps) ниже, в разделе для получения дополнительной информации.

### <a name="failing-handoff-gracefully"></a>Корректное поведение при переадресации

Поскольку перемещение вручную использует для передачи сведений между iOS коллекции слабо подключенных устройств и OS X, процесс передачи могут возникать ошибки. Следует разрабатывать приложения для корректной обработки таких сбоев и оповещения пользователя о ситуации, которые возникают.

В случае сбоя `DidFailToContinueUserActivitiy` метод `AppDelegate` будет вызван. Пример:

```csharp
public override void DidFailToContinueUserActivitiy (UIApplication application, string userActivityType, NSError error)
{
    // Log information about the failure
    Console.WriteLine ("User Activity {0} failed to continue. Error: {1}", userActivityType, error.LocalizedDescription);
}
```

Следует использовать предоставленный `NSError` для предоставления сведений об ошибке пользователю.

## <a name="native-app-to-web-browser-handoff"></a>Собственное приложение для сдачи веб браузера

Пользователь может продолжить действие без необходимости соответствующие собственное приложение, установить на устройство. В некоторых ситуациях веб-интерфейс может предоставить требуемую функциональность, и действия по-прежнему может быть продолжен. Например учетной записи электронной почты может предоставлять веб базовый пользовательский Интерфейс для составления и чтение сообщений.

Если исходный, собственного приложения знает URL-адрес для веб-интерфейса (и синтаксис, необходимый для идентификации данного элемента продолжает) можно закодировать эту информацию в `WebpageURL` свойство `NSUserActivity` экземпляра. Если принимающее устройство не имеет соответствующего собственное приложение, установить для продолжения обработки, можно вызвать предоставленный веб-интерфейса.

## <a name="web-browser-to-native-app-handoff"></a>Веб-браузер для собственного приложения перемещение вручную

Если пользователь использует веб-интерфейса исходного устройства и собственное приложение на принимающем устройстве утверждений доменной части `WebpageURL` свойство, то система будет использовать это приложение маркер продолжения. Получите новое устройство `NSUserActivity` экземпляра, обозначающий тип действия, как `BrowsingWeb` и `WebpageURL` будет содержать URL-адрес, который пользователь посетил, `UserInfo` словарь будет пустым.

Для приложения, чтобы участвовать в этом типе перемещение вручную, должны утверждать домена в `com.apple.developer.associated-domains` правах в формате `<service>:<fully qualified domain name>` (например: `activity continuation:company.com`).

Если указанный домен соответствует `WebpageURL` значение свойства, перемещение вручную загружает список утвержденных приложений идентификаторы с веб-сайта в этом домене. Веб-сайт, необходимо предоставить список утвержденных идентификаторов в файле JSON со знаком, с именем **apple app сайта ассоциации** (например, `https://company.com/apple-app-site-association`).

JSON-файл содержит словарь, содержащий список идентификаторов приложений в виде `<team identifier>.<bundle identifier>`. Пример:

```csharp
{
    "activitycontinuation": {
        "apps": [    "YWBN8XTPBJ.com.company.FirstApp",
            "YWBN8XTPBJ.com.company.SecondApp" ]
    }
}
```

Для подписания файла JSON (так, что он содержит правильные `Content-Type` из `application/pkcs7-mime`), используйте **терминалов** приложение и `openssl` с сертификата и ключа, выданного центром сертификации, доверенным для операций ввода-вывода (см. [ http://support.apple.com/kb/ht5012 ](http://support.apple.com/kb/ht5012) список). Пример:

```csharp
echo '{"activitycontinuation":{"apps":["YWBN8XTPBJ.com.company.FirstApp",
"YWBN8XTPBJ.com.company.SecondApp"]}}' > json.txt

cat json.txt | openssl smime -sign -inkey company.com.key
-signer company.com.pem
-certfile intermediate.pem
-noattr -nodetach
-outform DER > apple-app-site-association
```

`openssl` Команда выводит подписанный файл JSON, поместить на веб-сайте в **apple app сайта ассоциации** URL-адрес. Пример:

```csharp
https://example.com/apple-app-site-association.
```

Приложение получит любое действие, `WebpageURL` домен находится в его `com.apple.developer.associated-domains` права. Только `http` и `https` протоколы поддержки, любой другой протокол создаст исключение.

## <a name="supporting-handoff-in-document-based-apps"></a>Поддержка переадресации в приложения на основе документа

Как упоминалось выше, в iOS и OS X, приложения на основе документа автоматически будет поддерживать получения документов на основе iCloud, если приложения **Info.plist** файл содержит `CFBundleDocumentTypes` ключ `NSUbiquitousDocumentUserActivityType`. Пример:

```xml
<key>CFBundleDocumentTypes</key>
<array>
    <dict>
        <key>CFBundleTypeName</key>
        <string>NSRTFDPboardType</string>
        . . .
        <key>LSItemContentTypes</key>
        <array>
        <string>com.myCompany.rtfd</string>
        </array>
        . . .
        <key>NSUbiquitousDocumentUserActivityType</key>
        <string>com.myCompany.myEditor.editing</string>
    </dict>
</array>
```

В этом примере строка является указатель приложения обратного DNS с именем добавляется действие. Если введено таким образом, операции типа действия не обязательно должны повторяться в `NSUserActivityTypes` массив **Info.plist** файла.

Автоматически создается объект действия пользователя (через документа `UserActivity` свойства) можно ссылаться на другие объекты в приложении и используемый для восстановления состояния на продолжение. Например, для отслеживания элементов выбора и документа позицию. Необходимо указать этот действия `NeedsSave` свойства `true` каждый раз, когда состояние изменения и обновить `UserInfo` словаря в `UpdateUserActivityState` метод.

`UserActivity` Свойство можно использовать из любого потока и соответствует наблюдения протокола (KVO) ключ значение, поэтому он может использоваться для синхронизации документ при его перемещении из iCloud и выхода из системы. `UserActivity` Свойство станут недействительными при закрытии документа.

Дополнительные сведения см. в разделе Apple [поддержки активности пользователя в приложениях на основе документа](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html#//apple_ref/doc/uid/TP40014338-CH3-SW5) документации.

## <a name="supporting-handoff-in-responders"></a>Поддержка переадресации в ответчики.

Можно связать ответчики (наследуется от либо `UIResponder` на iOS или `NSResponder` в OS X) для действий, задав их `UserActivity` свойства. Система автоматически сохраняет `UserActivity` свойство в соответствующее время вызова отвечающего `UpdateUserActivityState` метод для добавления в объект действия пользователей с помощью текущих данных `AddUserInfoEntriesFromDictionary` метод.

## <a name="supporting-continuation-streams"></a>Поддержка потоков продолжения

Могут возникнуть ситуации, когда объем сведений, необходимых для продолжения действия не могут передаваться эффективно с начальные полезные данные перемещение вручную. В таких ситуациях принимающее приложение может установить один или несколько поток между собой и исходного приложения для передачи данных.

Установит исходного приложения `SupportsContinuationStreams` свойство `NSUserActivity` экземпляр `true`. Пример:

```csharp
// Create a new user Activity to support this tab
UserActivity = new NSUserActivity (ThisApp.UserActivityTab1){
    Title = "Weather Tab",
    SupportsContinuationStreams = true
};
UserActivity.Delegate = new UserActivityDelegate ();

// Update the activity when the tab's URL changes
var userInfo = new NSMutableDictionary ();
userInfo.Add (new NSString ("Url"), new NSString (url));
UserActivity.AddUserInfoEntries (userInfo);

// Inform Activity that it has been updated
UserActivity.BecomeCurrent ();
```

Принимающее приложение может вызвать `GetContinuationStreams` метод `NSUserActivity` в его `AppDelegate` для установления потока. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Report Activity
    Console.WriteLine ("Continuing User Activity: {0}", userActivity.ToString());

    // Get input and output streams from the Activity
    userActivity.GetContinuationStreams ((NSInputStream arg1, NSOutputStream arg2, NSError arg3) => {
        // Send required data via the streams
        // ...
    });

    // Take action based on the Activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.monkeybrowser.tab1":
        // Preform handoff
        Tab1.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab1});
        break;
    case "com.xamarin.monkeybrowser.tab2":
        // Preform handoff
        Tab2.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab2});
        break;
    case "com.xamarin.monkeybrowser.tab3":
        // Preform handoff
        Tab3.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab3});
        break;
    case "com.xamarin.monkeybrowser.tab4":
        // Preform handoff
        Tab4.PerformHandoff (userActivity);
        completionHandler (new NSObject[]{Tab4});
        break;
    }

    // Inform system we handled this
    return true;
}
```

На устройстве с исходной делегата действия пользователя получает потоки, вызывая его `DidReceiveInputStream` метод для предоставления данных затребовано продолжение действия пользователя на устройстве возобновления.

Вы воспользуетесь `NSInputStream` для предоставления доступа только для чтения для потоковой передачи данных и `NSOutputStream` предоставляют доступ только для записи. Образом запроса и ответа, где принимающее приложение запрашивает дополнительные данные и предоставляет исходного приложения, его следует использовать потоки. Чтобы данные, записанные в поток вывода на устройстве исходного считывается из входного потока на продолжение устройстве и наоборот.

Даже в случаях, когда поток продолжения не требуется должно быть минимальным назад и вперед взаимодействие между двумя приложениями.

Дополнительные сведения см. в разделе Apple [потоки продолжения с помощью](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/Handoff/AdoptingHandoff/AdoptingHandoff.html#//apple_ref/doc/uid/TP40014338-CH2-SW13) документации.

## <a name="handoff-best-practices"></a>Перемещение вручную советы и рекомендации

Успешной реализации прозрачную продолжение действий пользователей посредством передачи требует тщательного планирования из-за все различные компоненты, участвующие. Apple предлагает внедрение выполните рекомендации для приложений включено перемещение вручную:

- Проектирование действиях пользователя потребуется наименьшее полезных данных можно связать состояние действия будет продолжен. Чем больше размер полезных данных, тем больше времени занимает продолжения для запуска.
- При переносе больших объемов данных, для успешного продолжения учитывают затраты на конфигурации и сетевых издержек.
- Рекомендуется для больших приложений Mac для создания действий пользователей, обрабатываются в нескольких, меньшего размера, определенных задач приложений на устройствах iOS. Различные приложения и версии ОС должны разрабатываться с совместной работы или произойдет сбой.
- При указании типов действий, используйте нотацию обратного DNS во избежание конфликтов. Если действие является специфичным для данного приложения, его имя должен включаться в определение типа (например `com.myCompany.myEditor.editing`). Если действие можно будет работать для нескольких приложений, удалите имя приложения из определения (например `com.myCompany.editing`).
- Если ваше приложение должно обновить состояние активности пользователей (`NSUserActivity`) задайте `NeedsSave` свойства `true`. В соответствующие моменты передача работы вызывает делегата `UserActivityWillSave` метод, поэтому можно обновлять `UserInfo` словарь при необходимости.
- Так как процесс передачи не может мгновенно инициализировать на принимающем устройстве, необходимо реализовать `AppDelegate` `WillContinueUserActivity` и уведомить пользователя, запускается продолжение.

## <a name="example-handoff-app"></a>Перемещение вручную примера приложения

В качестве примера использования переадресации в приложения Xamarin.iOS, которые мы включили [ **MonkeyBrowser** ](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/) пример приложения с помощью этого руководства. Приложение имеет четыре вкладки, которые пользователь может использовать в Интернете, каждый с типом данного действия: погоды, избранного, перерыв и работы.

На любой вкладке, когда пользователь вводит новый URL-адрес и касания **Go** кнопку Новый `NSUserActivity` создается для этого вкладку, содержащую URL-адрес, который пользователь просматривает в настоящее время:

[![](handoff-images/handoff01.png "Перемещение вручную примера приложения")](handoff-images/handoff01.png#lightbox)

Если другой пользователь устройств — **MonkeyBrowser** приложение установлено, вход в iCloud с использованием той же учетной записью, на том же сети и в непосредственной близости от устройства выше, перемещение вручную действие будет отображаться на корневую папку экран (в левом нижнем углу).

[![](handoff-images/handoff02.png "Перемещение вручную действия, отображаемого на начальном экране в левом нижнем углу")](handoff-images/handoff02.png#lightbox)

Если пользователь перетаскивает вверх на значке перемещение вручную, приложение будет запускаться с указанием действий пользователей в `NSUserActivity` будет продолжаться на новое устройство:

[![](handoff-images/handoff03.png "Продолжение действия пользователя на новое устройство")](handoff-images/handoff03.png#lightbox)

Когда действие пользователя было успешно отправлено для другого Apple устройства, отправляющего устройства `NSUserActivity` получит вызов `UserActivityWasContinued` метод для его `NSUserActivityDelegate` сообщая о успешно перенести активности пользователей на другой устройство.

## <a name="summary"></a>Сводка

Эта статья дала вводные сведения о framework перемещение вручную для продолжения действий пользователей между несколькими устройствами Apple пользователя. Далее показано, как включить и реализовать в приложении Xamarin.iOS перемещение вручную. Наконец он рассматривается различные типы доступных продолжений перемещение вручную и сдачи рекомендации.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Образец MonkeyBrowser](https://developer.xamarin.com/samples/monotouch/ios8/MonkeyBrowser/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Руководство по HomeKitDeveloper](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/HomeKitDeveloperGuide/Introduction/Introduction.html)
- [Рекомендации по HomeKit пользовательскому интерфейсу](https://developer.apple.com/homekit/ui-guidelines/)
- [Справочник по HomeKit Framework](https://developer.apple.com/library/ios/home_kit_framework_ref)
