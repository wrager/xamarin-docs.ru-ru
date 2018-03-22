---
title: "Фоновые задачи"
description: "Используйте новый watchOS фоновые задачи 3 Чтобы убедиться, что приложение watch всегда имеет последних данных и моментальные снимки закрепления."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 8fd2b5069e175a68ff7609e75775db1929507582
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="background-tasks"></a>Фоновые задачи

_Используйте новый watchOS фоновые задачи 3 Чтобы убедиться, что приложение watch всегда имеет последних данных и моментальные снимки закрепления._

С watchOS 3 существует, приложение watch можно обновлять сведения о его тремя разными способами: 

- С помощью одного из нескольких новых фоновых задач. 
- Наличие одного из его сложностей циферблате (предоставляя дополнительное время для обновления). 
- Необходимости дока новый ПИН-код пользователя для приложения (где его хранится в памяти и часто обновляемых). 

## <a name="keeping-an-app-up-to-date"></a>Обновление приложения

Прежде чем обсуждать все способы, которые разработчик можно сохранить данные и пользовательский интерфейс приложения watchOS текущего и обновленного, в этом разделе будет взглянем на типичный набор шаблонов использования и как пользователь может перемещать между их iPhone и их Apple Watch течение суток, на основе  время суток и действия в настоящее время выполнения (например, для поездки на машине).

Рассмотрим следующий пример:

[![](background-tasks-images/update00.png "Как пользователь может перемещать между их iPhone и их Apple Watch течение дня")](background-tasks-images/update00.png#lightbox)

1. Ежедневно, время ожидания в очереди за кофе пользователь просматривает текущих новостей на iPhone их на несколько минут.
2. Прежде чем закрывать кафе, они быстро проверять погоды с усложнения на их Циферблат часов.
3. Перед обед они используют Maps приложение для iPhone найти ближайший ресторан и книга резервирования в соответствии с клиентом.
4. Командировках в ресторан, они получают уведомления на их Apple Watch и быстро увидеть, знают, что их встречи обед запущена позднее.
5. По расписанию, они используют Maps приложение для iPhone проверять трафик перед пешком Главная.
6. На способ Домой, получают уведомление iMessage на их Apple Watch, запрашиваются выбирает некоторые Молоко и используют функцию быстрого ответа для отправки ответа «ОК».

Из-за «быстрый обзор» (менее трех секунд) характер как пользователь хотят использовать приложение Apple Watch, что обычно не хватает времени для приложения для извлечения требуемой информации и обновления перед его отображением пользователю его пользовательского интерфейса.

С помощью нового API-интерфейсы Apple включила в watchOS 3, приложение можно запланировать для _фоновое обновление_ и иметь готовности нужные сведения, чтобы пользователь запрашивает ее. Рассмотрим в качестве примера усложнения погоды, описанные выше:

[![](background-tasks-images/update01.png "Примером усложнения погоды")](background-tasks-images/update01.png#lightbox)

1. Расписания приложения, чтобы быть выведены из спящего режима в системе в определенное время. 
2. Приложение получает сведения, потребуется создать обновление.
3. Приложение обновляет пользовательский интерфейс в соответствии с новыми данными.
4. Когда пользователь glances на сложность приложения, он получает актуальные сведения без участия пользователя ожидания для обновления.

Как показано выше, watchOS систему из спящего режима приложения с помощью одной или нескольких задач, в которых он есть очень ограниченный пул доступных:

[![](background-tasks-images/update02.png "Система watchOS из спящего режима приложения с помощью одной или нескольких задач")](background-tasks-images/update02.png#lightbox)

Apple предложить оптимальное этой задачи (поскольку такие ограниченным ресурсом в приложение), удерживает до завершения обновления для самого приложения.

Доставляет системы, эти задачи, вызвав новый `HandleBackgroundTasks` метод `WKExtensionDelegate` делегата. Пример:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

После завершения данной задачи приложения он возвращает его в систему, помечая его выполнить:

[![](background-tasks-images/update03.png "Задача возвращает в систему, помечая его завершения")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Новый фоновые задачи

watchOS 3 предоставляет несколько фоновые задачи, которые приложение может использовать для обновления информации проверку содержимого пользователь должен, прежде чем открыть приложение, например:

- **Фоновые обновления приложения** - [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) задача позволяет приложению обновлять его состояние в фоновом режиме. Обычно это относится и к другой задачи, такие как загрузка новое содержимое из Интернета, используя [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Фоновые обновления моментального снимка** - [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) задач позволяет приложению для обновления пользовательского интерфейса и его содержимое, прежде чем система делает снимок, который будет использоваться для заполнения дока.
- **Фон подключения Контрольные значения** - [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) начала задачи для приложения при получении данных фона с парной iPhone.
- **Фон сеанса URL-адрес** - [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) начала задачи для приложения, когда фоновая передача требует авторизации или завершения (успешно или ошибка).

В следующих разделах подробно рассматриваются эти задачи.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

`WKApplicationRefreshBackgroundTask` Универсального задача, которая может быть запланировано приложение пробуждении в будущем:

[![](background-tasks-images/update04.png "WKApplicationRefreshBackgroundTask, пробуждении в будущем")](background-tasks-images/update04.png#lightbox)

В среде выполнения задачи, приложение можно сделать любого вида локальной обработки, таких как обновление усложнения временной шкалы или извлечь некоторые необходимые данные с `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Система отправит `WKURLSessionRefreshBackgroundTask` после завершения загрузки и готов к обработке приложением данных:

[![](background-tasks-images/update05.png "WKURLSessionRefreshBackgroundTask после завершения загрузки данных")](background-tasks-images/update05.png#lightbox)

Приложение не продолжит выполнение при загрузке данных в фоновом режиме. Вместо этого приложение планирует запрос на данные, а затем она приостановлена и система обрабатывает загрузку данных только reawakening приложения после завершения загрузки.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

В watchOS 3 Apple установил закрепления, где пользователи могут закреплять избранные приложений и быстрого доступа к ним. При нажатии кнопки боковой на Apple Watch, отображается коллекция моментальных снимков закрепленных приложения. Пользователь может проведите влево или вправо, чтобы найти нужное приложение, затем выберите приложения, чтобы запустить его замены моментального снимка интерфейса выполняющегося приложения.

[![](background-tasks-images/update06.png "Замена моментального снимка с интерфейсом выполняющегося приложения")](background-tasks-images/update06.png#lightbox)

Система принимает периодически моментальные снимки пользовательского интерфейса приложения (путем отправки `WKSnapshotRefreshBackgroundTask`) и производит Заполнение дока в таких моментальных снимках. watchOS дает приложению возможность обновить его содержимое и пользовательского интерфейса, прежде чем этот моментальный снимок.

Моментальные снимки очень важны в watchOS 3, так как они работать в качестве изображения предварительного просмотра и запуска приложения. Если в приложении дока сопоставляет пользователя, чтобы развернуть во весь экран, введите переднего плана, для запуска, поэтому важно обеспечить актуальность моментального снимка:

[![](background-tasks-images/update07.png "Если в приложении дока сопоставляет пользователя, его расширение на весь экран")](background-tasks-images/update07.png#lightbox)

Опять же, система выдаст `WKSnapshotRefreshBackgroundTask` , чтобы приложение можно подготовить (путем обновления данных и пользовательский Интерфейс) до моментального снимка:

[![](background-tasks-images/update08.png "Подготовьте приложение, обновление данных и пользовательский Интерфейс до моментального снимка")](background-tasks-images/update08.png#lightbox)

Когда приложение помечает `WKSnapshotRefreshBackgroundTask` завершена, система автоматически переведет моментальный снимок пользовательского интерфейса приложения.

> [!IMPORTANT]
> Очень важно всегда запланировать ` WKSnapshotRefreshBackgroundTask` после получения новых данных и обновить его пользовательский интерфейс приложения или пользователь не увидит измененные сведения.




Кроме того когда пользователь получает уведомление от приложения и нажимает кнопку, чтобы привести приложения отображается на переднем плане, будут обновленными, так как он действует как на экране запуска требуется моментальный снимок:

[![](background-tasks-images/update09.png "Пользователь получает уведомление от приложения и нажимает кнопку, чтобы привести приложения отображается на переднем плане")](background-tasks-images/update09.png#lightbox)

Если прошло больше часа с момента пользователь выполнял с приложением watchOS, смогут вернуть в состояние по умолчанию. Состояние по умолчанию может иметь различные значения для различных приложений и на основании разработки приложения, он может вообще не имеют состояние по умолчанию.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

В watchOS 3 Apple встраивается Контрольные значения подключения с помощью фоновой API обновления через новое `WKWatchConnectivityRefreshBackgroundTask`. С помощью этой новой функции, приложение iPhone можно передать свежими данными Контрольные значения другого домена приложения, пока приложение watchOS работает в фоновом режиме:

[![](background-tasks-images/update10.png "Приложение iPhone можно доставлять свежими данными Контрольные значения другого домена приложения, пока приложение watchOS работает в фоновом режиме")](background-tasks-images/update10.png#lightbox)

Инициирование усложнения принудительной отправки, контексте приложения, отправив файл или обновления сведений о пользователях из приложения iPhone пробуждения приложения Apple Watch в фоновом режиме.

При пробуждении приложение watch через `WKWatchConnectivityRefreshBackgroundTask` его нужно будет использовать стандартные методы API для получения данных из приложения iPhone.

[![](background-tasks-images/update11.png "Поток данных WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png#lightbox)

1. Убедитесь, что сеанс был активирован.
2. Мониторинга новой `HasContentPending` при условии, что значение свойства `true`, приложение все еще имеются данные для обработки. Как и прежде, приложение стоит рассчитывать на задачу до завершения обработки всех данных.
3. Если больше нет данных для обработки (`HasContentPending = false`), пометить задачу завершения, чтобы вернуть его в систему. Если сделать это будет исчерпать фона выделенного приложения среды выполнения, возникающие в отчет о сбоях.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Жизненный цикл API фона

Совместное размещение всех фрагментов нового API фоновой задачи, приведен стандартный набор взаимодействий будет выглядеть следующим образом:

[![](background-tasks-images/update12.png "Жизненный цикл API фона")](background-tasks-images/update12.png#lightbox)

1. Во-первых приложение watchOS планирует фоновой задачи, чтобы быть вернулся в какой-то момент к активности в будущем.
2. Приложение пробуждении системой и отправки задачи.
3. Приложение обрабатывает завершение требовалось любые рабочие задачи.
4. В результате обработки задачи, приложение может потребоваться запланировать дополнительные фоновой задачи для выполнения дополнительной работы в будущем, например загрузкой содержимого с использованием `NSUrlSession`.
5. Приложение помечает задача завершена и возвращает его в систему.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Использование ресурсов без необходимости

Очень важно, что приложение watchOS ведет себя ответственно этой экосистеме, ограничивая его воздействие на общие ресурсы системы.

Рассмотрим следующий сценарий:

[![](background-tasks-images/update13.png "Приложение watchOS ограничивает его пустой тратой ресурсов общей системы")](background-tasks-images/update13.png#lightbox)

1. Пользователь запускает приложение watchOS в 13:00.
2. Приложение планирует задачу для пробуждения и загрузить новое содержимое в течение часа в 14:00.
3. В 13:50 пользователь повторно открывает приложения, что позволяет обновить его данные и пользовательского интерфейса в данный момент.
4. Фиксированного пробуждения задач приложения попытку через 10 минут, приложение должно перепланировать задачу на выполнение в час позже в 14:50.

Хотя все приложения разные, Apple предлагает поиск шаблонов использования, например приведенных выше для экономии ресурсов системы.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Реализация фоновые задачи

Для примера этот документ будет использовать фиктивное MonkeySoccer спортивных приложение, которое сообщает пользователю футбольной оценок. 

Рассмотрим следующий сценарий типичных сценариях использования:

[![](background-tasks-images/update14.png "Типичное использование сценария")](background-tasks-images/update14.png#lightbox)

Избранные футбольной команды пользователя воспроизведения больших совпадение с 7:00 до 9:00, приложение следует ожидать, что он регулярно проверяет оценка и он решает в пределах 30-минутные интервала обновления.

1. Когда пользователь открывает приложение и он планирует задачу для фоновое обновление 30 минут. Фон API позволяет только один тип фоновых задач для выполнения в определенный момент времени.
2. Задача получает и обновляет его данные и пользовательского интерфейса приложения, а затем расписания для другого фоновых задач 30 минут. Очень важно, что разработчик запоминает планирование другой фоновой задачи или приложение будет никогда не будет повторно из спящего режима для получения дополнительных обновлений.
3. Опять же приложение получает задачи и обновляет данные, обновляет пользовательский Интерфейс и планирует другой цвет фона задач 30 минут.
4. Тот же процесс повторяется заново.
5. Последний фон, задача получила приложение попытку обновления и его данные и пользовательского интерфейса. Так как это окончательный показатель он не расписания для новых обновлений в фоновом режиме. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Планирование для фоновое обновление

Если приведенный выше сценарий, MonkeySoccer приложения можно использовать следующий код для планирования для фоновое обновление:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Он создает новую `NSDate` 30 минут в будущем when приложение хочет быть из спящего режима и создает `NSMutableDictionary` для хранения подробных сведений запрашиваемую задачу. `ScheduleBackgroundRefresh` Метод `SharedExtension` используется для запроса, планировать задачи.

Система вернется `NSError` если его не удалось запланировать запрашиваемую задачу.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Обработка обновления

Затем переведите более подробно рассмотрим 5-минутное окно, отображающее шаги, необходимые для обновления оценка:

[![](background-tasks-images/update15.png "5-минутное окно, отображающее шаги, необходимые для обновления оценка")](background-tasks-images/update15.png#lightbox)

1. В 7:30:02 выведены из спящего режима в системе и фон обновление задач приложения. Его приоритетной — получить последние результаты с сервера. В разделе [планирования NSUrlSession](#Scheduling-a-NSUrlSession) ниже.
2. В 7:30:05 приложения исходной задачи, система переводит приложение в спящий режим и продолжает загружать запрошенные данные в фоновом режиме.
3. По завершении загрузки системы, он создает новую задачу для пробуждения приложения, чтобы он мог обрабатывать загруженные сведения. В разделе [фоновых задач обработки](#Handling-Background-Tasks) и [обработка завершения загрузки](#Handling-the-Download-Completing) ниже. 
4. Приложение сохраняет обновленные сведения и помечает завершения задачи. Разработчик может возникнуть желание обновить пользовательский интерфейс приложения в данный момент, однако Apple предлагает планирование задания моментального снимка для обработки этого процесса. В разделе [расписание обновления моментальных снимков](#Scheduling-a-Snapshot-Update) ниже.
5. Приложение получает задаче моментального снимка, обновляет его пользовательский интерфейс и помечает завершения задачи. В разделе [обработка обновления моментального снимка](#Handling-a-Snapshot-Update) ниже.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Планирование NSUrlSession

Для планирования загрузки последней оценки можно использовать следующий код:

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Он настраивает и создает новую `NSUrlSession`, затем использует этот сеанс для создания нового загрузки задач с помощью `CreateDownloadTask` метод. Он вызывает `Resume` метод загрузки задач для начала сеанса.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Обработка фоновых задач

Путем переопределения `HandleBackgroundTasks` метод `WKExtensionDelegate`, приложение может обрабатывать входящие фоновых задач:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

`HandleBackgroundTasks` Метод выполняет циклический переход по все задачи, что система отправила приложения (в `backgroundTasks`) для поиска `WKUrlSessionRefreshBackgroundTask`. Если он найден, он подключается к сеансу и присоединяет `NSUrlSessionDownloadDelegate` для обработки завершения загрузки (в разделе [обработка завершения загрузки](#Handling-the-Download-Completing) ниже):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Дескриптор он сохраняется в задаче до ее завершения, добавьте его в коллекцию:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Все задачи, отправленные в приложение необходимо выполнять для любой задачи в настоящее время не обрабатываются, пометить ее как завершенную:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Обработка завершения загрузки

MonkeySoccer приложение использует следующие `NSUrlSessionDownloadDelegate` делегата для обработки завершения загрузки и обработать запрошенные данные:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

При инициализации, что позволяет сохранять дескриптор как `ExtensionDelegate` и `WKRefreshBackgroundTask` , порожденных его. Он переопределяет `DidFinishDownloading` метод для обработки завершения загрузки. Затем используется `CompleteTask` метод `ExtensionDelegate` для информирования задача что он завершения и удалить его из коллекции задач в очереди. В разделе [фоновых задач обработки](#Handling-Background-Tasks) выше.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Расписание обновления моментальных снимков

Для планирования задач для обновления пользовательского интерфейса с последней оценки моментальных снимков можно использовать следующий код:

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Так же, как `ScheduleURLUpdateSession` описанный выше метод, он создает новую `NSDate` хочет быть из спящего режима и создает приложение `NSMutableDictionary` для хранения подробных сведений запрашиваемую задачу. `ScheduleSnapshotRefresh` Метод `SharedExtension` используется для запроса, планировать задачи.

Система вернется `NSError` если его не удалось запланировать запрашиваемую задачу.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Обработка обновления моментальных снимков

Для обработки задача моментального снимка, `HandleBackgroundTasks` метод (см. [фоновых задач обработки](#Handling-Background-Tasks) выше) изменяются так, чтобы выглядеть следующим образом:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

Метод проверяет тип обработки задачи. Если это `WKSnapshotRefreshBackgroundTask` он получает доступ к задачи:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Метод обновляет пользовательский интерфейс, а затем создается `NSDate` сообщить системе при устареть моментального снимка. Он создает `NSMutableDictionary` с сведения о пользователе для описания нового моментального снимка и метки моментальных снимков задача завершилась со следующими параметрами:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Кроме того она также предписывает задаче моментального снимка, приложение не возвращает к состоянию по умолчанию (в качестве первого параметра). Приложения, которые не имеют концепции состояние по умолчанию всегда следует присвойте этому свойству значение `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Работает неэффективно

Как показано в приведенном выше примере окна пять минут, MonkeySoccer приложение, которое потребовалось для обновить его оценки эффективности работы с новой watchOS 3 фоновых задач, это приложение было active составляет 15 секунд: 

[![](background-tasks-images/update16.png "Это приложение было только active составляет 15 секунд")](background-tasks-images/update16.png#lightbox)

Это сокращает влияние, которое будет иметь приложение на доступные ресурсы Apple Watch и от батареи и также позволяет приложению улучшают работу других приложений, выполняющихся на часах.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Как работает планирования

Хотя приложение watchOS 3 на переднем плане, всегда планируется для выполнения и может сделать любой тип обработки таких как обновление данных или перерисовывает его пользовательского интерфейса. Когда приложение перемещает в фоновом режиме, обычно она приостановлена в системе и приостановлено все время выполнения операции. 

Пока приложение находится в фоновом режиме, он может применяться системой, чтобы быстро выполнить конкретную задачу. Поэтому в watchOS 2, система может временно пробуждения приложении фона к примеру, обработка уведомления long вид или для обновления приложения усложнения. В watchOS 3 существует несколько новых способов, которые приложения могут выполняться в фоновом режиме.

Пока приложение находится в фоновом режиме, система накладывает некоторые ограничения на его:

- Для выполнения любой задачи будет присвоено только через несколько секунд. Система учитывает не только время передано, но также объем ресурсов ЦП приложение использует для получения этого предела.
- Любое приложение, которое превысит пределы будет завершен с следующие коды ошибок:
    - **CPU** - 0xc51bad01
    - **Время** -0xc51bad02
- Система будет применять различные ограничения в зависимости от типа приложения для выполнения запрошенных фоновой задачи. Например `WKApplicationRefreshBackgroundTask` и `WKURLSessionRefreshBackgroundTask` задачи, получают немного больше времени сред выполнения над другими типами фоновых задач.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Сложности и обновления приложений

Помимо новых фоновых задач, который добавлен Apple watchOS 3 приложении watchOS сложности может оказывать влияние на том, как и когда приложение получает обновления в фоновом режиме.

Сложностей, небольшие визуальные элементы, которые предоставляют полезные сведения, с одного взгляда. В зависимости от выбранного Циферблат часов пользователь имеет возможность настраивать Циферблат часов с одной или нескольких сложностью, которая может быть предоставленный приложение watch в watchOS 3.

Если пользователь включает один сложности приложения на их Циферблат часов, он предоставляет приложения обновлены следующие преимущества:

- Система помнить все готово для запуска приложения состояние, когда он пытается запустить приложение в фоновом режиме, сохраняется в памяти и обеспечивает дополнительное время для обновления.
- Сложностей гарантируется по меньшей мере 50 принудительной обновлений в день.

Разработчик всегда следует стремиться к создавать привлекательные сложностей для своих приложений, чтобы заставить пользователя добавлением их Циферблат часов по причинам, перечисленных выше.

В watchOS 2 сложностей были основным способом, что приложение получено среды выполнения в фоновом режиме. В watchOS 3 приложения усложнения по-прежнему быть гарантирована получать несколько обновлений в час, однако он может использовать `WKExtensions` запросить дополнительные среды выполнения для обновления его сложностей.

Рассмотрим следующий код, который используется для обновления усложнения из приложения iPhone подключенных:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Она использует `RemainingComplicationUserInfoTransfers` свойство `WCSession` чтобы увидеть, сколько высокий приоритет передает приложения вышел за день, а затем принимает действие на основании этого числа. Если приложение начинает нехватке передачи, он не торопитесь отправки незначительные изменения и отправлять сведения только при наличии значительное изменение.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Планирование и дока

В watchOS 3 Apple установил закрепления, где пользователи могут закреплять избранные приложений и быстрого доступа к ним. При нажатии кнопки боковой на Apple Watch, отображается коллекция моментальных снимков закрепленных приложения. Пользователь может проведите влево или вправо, чтобы найти нужное приложение, затем выберите приложения, чтобы запустить его замены моментального снимка интерфейса выполняющегося приложения.

[![](background-tasks-images/dock01.png "Дока")](background-tasks-images/dock01.png#lightbox)

Система периодически принимает пользовательского интерфейса приложения моментальных снимков и использует эти моментальные снимки для заполнения документы. watchOS дает приложению возможность обновить его содержимое и пользовательского интерфейса, прежде чем этот моментальный снимок.

Приложения, которые были закреплены дока можно ожидать, что следующее:

- Они получат как минимум одно обновление в час. Это включает в себя задачу обновления приложения и задачи моментальных снимков.
- Обновление бюджета распределяется между всех приложений в приемки. Поэтому меньше приложения, которые пользователь закреплен, повышают вероятность каждого приложение будет получать обновления.
- Приложения будут храниться в памяти, приложение будет быстрее возобновить при выборе из приемки.

Последний пользователь выполнил приложения будут считаться _недавно использовавшиеся_ приложения и будет занимать последнюю ячейку в приемки. Отсюда следует, что пользователь может выбрать закрепите его окончательно дока. Недавно использовавшиеся будет рассматриваться как любое другое избранные приложение пользователь уже закреплены дока.

> [!IMPORTANT]
> Приложения, которые были добавлены только для начального экрана не будет иметь регулярного расписания. Для получения регулярного расписания и фона обновлений, приложение _должен_ добавить дока.

Как уже говорилось ранее в этом документе, моментальные снимки являются очень важно для watchOS 3, поскольку они работать в качестве изображения предварительного просмотра и запуска приложения. Если в приложении дока сопоставляет пользователя, он разворачивается во весь экран, введите переднего плана и запущен, поэтому важно обеспечить актуальность моментального снимка.

Возможны ситуации, когда система решает, что требуется новый моментальный снимок пользовательского интерфейса приложения. В этом случае запрос моментального снимка будет не подпадают бюджета среды выполнения приложения. Ниже будет активировать системы запрос моментального снимка:

- Обновление с усложнения временной шкалы.
- Взаимодействие с пользователем с помощью приложения уведомления.
- Переключение на переднем плане состояние фона.
- Через 1 час состояния фона таким образом приложение может возвращать в состояние по умолчанию.
- Когда watchOS первой загрузки.

<a name="Best-Practices" />

## <a name="best-practices"></a>Рекомендации 

При работе с фоновыми задачами, Apple предлагает следующие рекомендации:

- Запланируйте так часто, как приложения должен быть обновлен. Каждый раз при запуске приложения необходимо заново определить ее будущие потребности и при необходимости изменить.
- Если приложение не требует обновления система отправляет обновления фоновой задачи, отложите до обновления требуется фактически.
- Рассмотрим все возможности среды выполнения, доступные для приложения.
    - Активация закрепления и переднего плана.
    - Уведомления.
    - Усложнения обновления.
    - Фоновое обновление.
- Используйте `ScheduleBackgroundRefresh` для среды выполнения фоновой общего назначения, такие как:
    - Опрос системы сведения.
    - Планировать будущие `NSURLSessions` фона запрашивать данные. 
    - Известные время переходов.
    - Запуск обновления усложнения.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Рекомендации по обеспечению моментальных снимков

При работе с обновлений моментальных снимков, Apple вносит следующие рекомендации:

- Сделать недействительным моментальные снимки только при необходимости, например, если имеется значительное изменение содержимого.
- Избегайте высокочастотные недействительности моментального снимка. Например приложение таймера не следует обновления моментального снимка каждую секунду, это следует делать только после окончания таймера.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>Поток данных приложения

Apple предлагаются следующие действия для работы с потоком данных.

[![](background-tasks-images/update17.png "Схема потока данных приложения")](background-tasks-images/update17.png#lightbox)

Внешнее событие (например, подключение контрольных значений) из спящего режима приложения. Это заставляет приложение для обновления своей модели данных (который представляет текущее состояние приложения). В результате изменения модели данных приложения необходимо будет обновить его сложностей запросить новый моментальный снимок, возможно запустить фон `NSURLSession` для извлечения дополнительных данных и дальнейшей запланировать фоновое обновление.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Жизненный цикл приложения

Из-за дока и возможность закрепить избранных приложений ему, что пользователи переходят между гораздо более приложения Apple гораздо чаще, затем они поддерживались с watchOS 2. В результате приложения должны быть готовы обработать это изменение быстрого и перехода между состояниями переднего плана и фона.

Apple имеет следующие рекомендации:

- Убедитесь, что приложение завершения любой фоновой задачи, как можно быстрее после входа активации переднего плана.
- Убедитесь, что вовремя выполнить всю работу на переднем плане перед входом в фоновом режиме путем вызова `NSProcessInfo.PerformExpiringActivity`.
- При тестировании приложения в watchOS симулятор, ни один из бюджетов задач будут применены, можно обновить приложения так, для полноценного тестирования компонент.
- Всегда тестируйте на реальном оборудовании Apple Watch, чтобы убедиться, что приложения не запущена после его бюджеты перед публикацией в iTunes Connect.
- Apple предлагает сохранением Apple Watch к зарядному устройству во время тестирования и отладки.
- Убедитесь, throughly протестированных холодный запуск и возобновление работы приложения.
- Убедитесь, что завершения всех задач приложения.
- Изменение числа приложений, которые закреплены Dock для тестирования лучших и наихудшей сценарии вариантов.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован усовершенствования сделанные Apple watchOS и описание их использования для поддержания актуального состояния приложения контрольных значений. Во-первых, все нового охваченных Apple фоновая задача добавлена в watchOS 3. Затем рассматривается жизненного цикла API фона и способ реализации фоновых задач в приложении watchOS Xamarin. Наконец он рассматривается как планирования работы и предлагает некоторые рекомендации.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
