---
title: "Тренировки приложений"
description: "В этой статье рассматриваются усовершенствования Apple, внесенных в watchOS 3 и способы их реализации в Xamarin тренировки приложения."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 77bad4c31ad0cb11476c656aa495707d2a94aa8f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="workout-apps"></a>Тренировки приложений

_В этой статье рассматриваются усовершенствования Apple, внесенных в watchOS 3 и способы их реализации в Xamarin тренировки приложения._


Новые watchOS 3 тренировки связанных приложения имеют возможность работать в фоновом режиме на Apple Watch и получить доступ к данным HealthKit. IOS 10 на основе их родительского приложения также имеет возможность запускать приложения на основе watchOS 3 без вмешательства пользователя.

Будут подробно рассмотрены следующие темы:

## <a name="about-workout-apps"></a>О приложениях тренировки

Пользователи применимости и тренировки приложений могут быть очень выделенный выделяя несколько часов дня по направлению к их работоспособности и пригодности цели. В результате ожидают, что отвечать на запросы, для использования приложений, которые точно сбора и отображения данных и легко интегрировать работоспособности Apple.

Хорошо спроектированные приложения пригодности или тренировки дает возможность пользователям диаграммы для достижения целей их пригодности их действия. С помощью Apple Watch, применимости и тренировки приложения имеют мгновенный доступ к частота пульса, обнаружение калория записи и действия.

[![](workout-apps-images/workout01.png "Пример приложения применимости и тренировки")](workout-apps-images/workout01.png#lightbox)

Новые watchOS 3, _запущен фоновый_ дает тренировки связанных приложений возможность работать в фоновом режиме на Apple Watch и получить доступ к данным HealthKit.

В этом документе будет появился компонент запущен в фоновом режиме, охватывает жизненный цикл приложения тренировки и показать, как приложение тренировки могут влиять на пользователя _действия колец_ на Apple Watch.

## <a name="about-workout-sessions"></a>О сеансах тренировки

— Лежит в основе каждого приложения, тренировки _сеанс тренировки_ (`HKWorkoutSession`), пользователь может запустить и остановить. API сеанс тренировки легко реализовать и предоставляет несколько преимуществ для тренировки приложения, такие как:

- Перемещение и калория записи обнаружения на основе типа действия.
- Автоматическое вклад колец действия пользователя.
- В сеансе, приложение будет автоматически отображаться каждый раз, когда пользователь выходит из режима устройства (либо путем вызова их манжеты или взаимодействия с Apple Watch).

## <a name="about-background-running"></a>О запуске фона

Как уже говорилось выше, с watchOS 3 приложения тренировки можно задать выполнение в фоновом режиме. С помощью фоновой работы с приложением тренировки могут обрабатывать данные от датчиков Apple Watch во время выполнения в фоновом режиме. Например приложение для дальнейшего мониторинга пользователя пульс, несмотря на то, что он больше не отображается на экране.

Выполнение фонового также предоставляет возможность предлагать пользователю динамической обратной связи в любое время во время активного сеанса тренировки, например, отправку haptic оповещение, чтобы информировать пользователей об их текущее состояние.

Кроме того выполнение фонового позволяет приложению быстро обновить свой пользовательский интерфейс, поэтому пользователь имеет самые последние данные, если они быстро ознакомьтесь с их Apple Watch.

Для поддержания высокого уровня производительности на Apple Watch, приложение watch, с помощью выполнения фоновой следует ограничить объем фоновой работы в целях экономии батареи. Если приложение использует ЦП, избыточное в фоновом режиме, он может получить приостановлено watchOS.

### <a name="enabling-background-running"></a>Включение фона под управлением

Для выполнения фоновой, выполните следующие действия.

1. В **обозревателе решений**, дважды щелкните модуль наблюдения за дополнительное iPhone приложения `Info.plist` файл, чтобы открыть его для редактирования.
2. Переключитесь в **источника** представления: 

    [![](workout-apps-images/plist01.png "В представлении источника")](workout-apps-images/plist01.png#lightbox)
3. Добавьте новый раздел с именем `WKBackgroundModes` и задайте **тип** для `Array`: 

    [![](workout-apps-images/plist02.png "Добавьте новый раздел с именем WKBackgroundModes")](workout-apps-images/plist02.png#lightbox)
4. Добавление нового элемента в массиве, с **тип** из `String` и значение `workout-processing`: 

    [![](workout-apps-images/plist03.png "Добавление нового элемента в массиве типа String и значением тренировки обработки")](workout-apps-images/plist03.png#lightbox)
5. Сохраните изменения в файле.

## <a name="starting-a-workout-session"></a>Запуск сеанса тренировки

Существует три основных действия для запуска сеанса тренировки:

[![](workout-apps-images/workout02.png "Три основных этапа запуска сеанса тренировки")](workout-apps-images/workout02.png#lightbox)

1. Приложение должно запросить авторизацию для доступа к данным в HealthKit.
2. Создайте объект конфигурации тренировки для типа тренировки которой началось.
3. Создание и запуск тренировки сеанс, используя только что созданный тренировки конфигурации.

### <a name="requesting-authorization"></a>Запрос авторизации

Приложение может получить доступ к HealthKit данных пользователя, необходимо запросить и получить авторизацию от пользователя. В зависимости от характера приложения тренировки может быть следующие типы запросов:

- Разрешение на запись данных:
    - Тренировки
- Разрешение на чтение данных:
    - Записать энергии
    - расстояние
    - Частота пульса  

Прежде чем приложение сможет запрашивать авторизации, она должна быть настроена для доступа к HealthKit.

Выполните следующие действия:

1. В **обозревателе решений** дважды щелкните файл `Entitlements.plist`, чтобы открыть его для редактирования.
2. Прокрутите вниз и проверьте **включить HealthKit**: 

    [![](workout-apps-images/auth01.png "Проверка включения HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Сохраните изменения в файле.
4. Следуйте инструкциям в [явный идентификатор приложения и профиль подготовки](~/ios/platform/healthkit.md) и [связывание идентификатор приложения и подготовки профиля с помощью Xamarin.iOS приложения](~/ios/platform/healthkit.md) разделы [введение в HealthKit](~/ios/platform/healthkit.md) статьи для правильной подготовки приложения.
5. Наконец, используйте инструкции в [работоспособности комплект средств для программирования](~/ios/platform/healthkit.md) и [запрашивает разрешение от пользователя](~/ios/platform/healthkit.md) разделы [введение в HealthKit](~/ios/platform/healthkit.md) статью для запроса Авторизация для доступа к хранилищу данных HealthKit пользователя.

### <a name="setting-the-workout-configuration"></a>Настройка конфигурации тренировки

Тренировки сеансов, создаваемых с помощью объекта конфигурации тренировки (`HKWorkoutConfiguration`), указывающий тип тренировки (такие как `HKWorkoutActivityType.Running`) и расположение тренировки (такие как `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Создание сеанса делегат тренировки 

Для обработки событий, которые могут возникнуть во время сеанса тренировки, приложению потребуется для создания экземпляра делегата тренировки сеанса. Добавьте новый класс в проект и создать ее выйти из системы `HKWorkoutSessionDelegate` класса. Пример стационарные выполнения он может выглядеть следующим образом:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Этот класс создает несколько событий, которые будет вызываться как состояние сеанса тренировки изменений (`DidChangeToState`) и в случае сеанс тренировки (`DidFail`). 

### <a name="creating-a-workout-session"></a>Создание сеанса тренировки

С помощью конфигурации тренировки и делегат сеанс тренировки созданной ранее для создания нового сеанса тренировки и запустите ее хранилище HealthKit пользователя по умолчанию:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Если приложение запускает этот сеанс тренировки и переключении обратно в их Циферблат часов, очень мала зеленый значок «выполнение man» будет отображаться над лицевой стороны:

[![](workout-apps-images/workout03.png "Мало места зеленый выполняющегося man значок наверху начертание шрифта")](workout-apps-images/workout03.png#lightbox)

Если пользователь касается этот значок, они будет выполнен переход к приложению.

## <a name="data-collection-and-control"></a>Сбор данных и управления

После сеанса тренировки был настроен и запущен, приложение необходимо собирать данные о сеансе (например, частота пульса пользователя) и контролировать состояние сеанса:

[![](workout-apps-images/workout04.png "Сбор данных и схема управления")](workout-apps-images/workout04.png#lightbox)

1. **Наблюдение за образцы** -приложения будет необходимо получать сведения из HealthKit, которая будет оказано и отображается для пользователя.
2. **Наблюдение за событиями** -приложения будет необходимо реагировать на события, создаваемые по HealthKit или из пользовательского интерфейса приложения (например, пользователь, приостановка тренировки).
3. **Введите состояние выполнения** -сеанс был запущен и выполняется в данный момент.
4. **Введите приостановлено** -пользователь приостановлена в текущем сеансе тренировки и перезапустить его позже. Пользователь может переключаться между запущен и приостановленного состояния несколько раз в одном сеансе тренировки.
5. **Завершение сеанса тренировки** - в любой момент пользователь может завершить сеанс тренировки или может завершить сам по себе, если он был отслеживаемой тренировки (например, запуска двух миль) и срок действия.

Последним шагом является сохранение результатов сеанса тренировки datastore HealthKit пользователя.

### <a name="observing-healthkit-samples"></a>Наблюдение за HealthKit образцы

Приложения потребуется открыть _привязки запроса объектов_ для каждого из HealthKit точек интересующие его, таких как частота пульса или энергии активной записи. Для каждой точки данных просматривается обработчика обновлений должны создаваться для записи новых данных при передаче в приложение.

Из этих точек данных приложения можно накапливать итоги (например, общее расстояние выполнения) и обновить его пользовательский интерфейс при необходимости. Кроме того приложение может уведомлять пользователей о переходе определенную задачу или достижение, такие как завершение Далее миль запуска.

Рассмотрим следующий пример кода:

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Он создает предикат, который необходимо задать дату начала, ему нужно получить данные по использованию `GetPredicateForSamples` метод. Он создает набор устройств, чтобы получить сведения о HealthKit с помощью `GetPredicateForObjectsFromDevices` метода, в данном случае только локальные Apple Watch (`HKDevice.LocalDevice`). Два предиката объединяются в составной предиката (`NSCompoundPredicate`) с помощью `CreateAndPredicate` метод.

Новый `HKAnchoredObjectQuery` создается для требуемого точки данных (в этом случае `HKQuantityTypeIdentifier.ActiveEnergyBurned` активной записи энергии точки данных), отсутствие ограничений на объем данных, возвращаемых (`HKSampleQuery.NoLimit`) и обработчика обновлений, определенных для обработки данных выполняется возврат к приложению из HealthKit. 

Обработчик обновлений будет вызываться каждый раз, когда новые данные доставляется в приложение для точки данных. Если ошибка не возвращается, приложение можно считывать данные, внести все необходимые вычисления и обновить его пользовательского интерфейса при необходимости.

Код выполняет цикл по всем образцов (`HKSample`) возвращается в `addedObjects` массива и приводит их количество выборке (`HKQuantitySample`). Затем он возвращает значение типа double образца как joule (`HKUnit.Joule`) и аккумулирует его в промежуточных итогов по active энергии, потребляемой для тренировки и обновляет пользовательский интерфейс.

### <a name="achieved-goal-notification"></a>Цель достигнутую уведомления

Как сказано выше Если пользователь достигает цели в приложении тренировки (например, первый миль запуска выполнения), он может отправить haptic отзыв пользователя через обработчик Taptic. Приложения должны также обновить его пользовательского интерфейса на этом этапе, так как пользователь скорее вызовет их манжеты, чтобы увидеть события, которое запрос отзыва.

Чтобы воспроизвести haptic обратной связи, используйте следующий код:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Наблюдение за событиями

События, отметки времени, приложение можно использовать для выделения определенных точках во время тренировки пользователя. Некоторые события будут непосредственно с помощью приложений и сохранить его в тренировки, и некоторые события будет автоматически создан HealthKit.

Чтобы увидеть события, созданные HealthKit, переопределяют приложения `DidGenerateEvent` метод `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple были добавлены следующие новые типы событий в watchOS 3:

- `HKWorkoutEventType.Lap` -Предназначены для событий, которые разбиение тренировки на одинаковом расстоянии частей. Например для пометки один Знакомство с функциями отслеживания во время выполнения.
- `HKWorkoutEventType.Marker` -Для произвольного Достопримечательности находятся в пределах тренировки. Например достижение конкретную точку в маршруте стационарные выполнения.

Эти новые типы можно создать приложение и хранятся в тренировки для дальнейшего использования при создании диаграмм и статистические данные.

Создание маркера события, выполните следующие действия.

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Этот код создает новый экземпляр события маркера (`HKWorkoutEvent`) и сохраняет его в частной коллекции событий (которые будут записаны позже в сеанс тренировки) и уведомляет пользователя через haptics события.

### <a name="pausing-and-resuming-workouts"></a>Приостановка и возобновление тренировки

На любом этапе сеанса тренировки пользователь может временно приостановить тренировки и возобновить его позже. Например они могут приостановить выполнение внутренний вызов важные и возобновление выполнения после завершения вызова.

Пользовательский Интерфейс приложения должен предоставить возможность приостанавливать и возобновлять тренировки (путем вызова HealthKit), чтобы Apple Watch больше места питания и данных пользователя приостанавливаются их действия. Кроме того приложения должны игнорировать все новые точки данных, которые могут быть получены при тренировки сеанс находится в приостановленном состоянии.

Для приостановки и возобновления вызовы сформировав Приостановка и возобновление событий будет отвечать HealthKit. Во время приостановки сеанса тренировки новых событий или нет данных будут отправляться в приложение, HealthKit до возобновления сеанса.

Чтобы приостанавливать и возобновлять сеанс тренировки, используйте следующий код:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Приостановка и возобновление события, которые будут созданы из HealthKit можно обрабатывать путем переопределения `DidGenerateEvent` метод `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>События перемещения

Кроме того, новые watchOS 3 являются приостановки движения (`HKWorkoutEventType.MotionPaused`) и возобновление перемещения (`HKWorkoutEventType.MotionResumed`) события. Эти события вызываются автоматически HealthKit во время выполнения тренировки когда пользователь запускает и останавливает перемещение.

Когда приложение получает событие приостановки движения, его следует остановить сбор данных, пока пользователь возобновляет движение и получении события возобновляет движения. Приложение приложений не следует приостановить сеанс тренировки в ответ на событие приостановки движения.

> [!IMPORTANT]
> **Примечание:** движения приостановки и возобновления движения события поддерживаются только для типа действия RunningWorkout (`HKWorkoutActivityType.Running`).

Опять же, эти события могут обрабатываться путем переопределения `DidGenerateEvent` метод `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Завершение и сохранение сеанса тренировки

После завершения их производительным пользователь приложения необходимо завершить текущий сеанс тренировки и сохраните его в базу данных HealthKit. Тренировки, сохраняется в HealthKit будет автоматически отображаются в списке тренировки действие.

Новые для iOS 10 включает список на iPhone пользователя, а также список тренировки действия. Поэтому даже если Apple Watch нет рядом, тренировки откроется на телефоне.

Тренировки, которые включают образцы энергии обновит кольцо перемещения пользователя в приложении действия, сторонние приложения теперь могут способствовать ежедневной целей перемещения пользователя.

До завершения и сохранить сеанс тренировки необходимы следующие действия:

[![](workout-apps-images/workout05.png "Завершение и сохранение диаграммы тренировки сеанса")](workout-apps-images/workout05.png#lightbox)

1. Во-первых приложению потребуется для завершения сеанса тренировки.
2. Сеанс тренировки сохраняется HealthKit.
3. Добавьте сохраненный сеанс тренировки всех образцов (например энергии, потребляемой или расстояния).

### <a name="ending-the-session"></a>Завершение сеанса

Чтобы завершить сеанс тренировки, вызовите `EndWorkoutSession` метод `HKHealthStore` передав `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Это приведет к сбросу устройства датчики их нормальный режим. По завершении HealthKit конца тренировки, он получит обратный вызов `DidChangeToState` метод `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>Сохранение сеанса

После завершения сеанса тренировки приложения его необходимо создать тренировки (`HKWorkout`) и сохраните его в хранилище данных HealthKit (вместе с событиями) (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Этот код создает требуется общий объем энергии, потребляемой и расстояния для тренировки как `HKQuantity` объектов. Словарь метаданные, определяющие тренировки создается и расположение тренировки указывается:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Новый `HKWorkout` создан объект с тем же `HKWorkoutActivityType` как `HKWorkoutSession`, даты начала и окончания, список событий, (собранные из вышеперечисленных), энергия записи общее расстояние и словарь метаданных. Этот объект сохраняется в базе данных Health Store и обработки ошибок.  

### <a name="adding-samples"></a>Добавив образцы

Когда приложение сохраняет набор образцов для тренировки, HealthKit создает соединение между примерами и тренировки, сам таким образом, чтобы приложение может запросить HealthKit позднее для всех образцов, связанный с данной тренировки. Используя эту информацию, приложение может создания диаграмм из данных, тренировки и отобразить их на временной шкале тренировки.

Приложения, чтобы способствовать кольцо переместить приложения Activity он должен содержать образцы энергии с сохраненной тренировки. Кроме того итоги для расстояния и энергии должно соответствовать сумма всех образцов, приложение связывает с сохраненной тренировки.

Чтобы добавить образцы сохраненного тренировки, выполните следующее:

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

При необходимости приложение можно вычислить и создать подмножество образцы или только один мегабайт образец (охвата всего диапазона тренировки), затем возвращает связанные с сохраненной тренировки.

## <a name="workouts-and-ios-10"></a>Тренировки и iOS 10

Каждое приложение тренировки watchOS 3 имеет приложения на основе тренировки iOS 10 родительского и опыта работы с iOS 10, это приложение iOS может использоваться для запуска тренировки, который будет размещать Apple Watch в режиме тренировки (без вмешательства пользователя) и запустите приложение, watchOS в режиме выполнения фоновой (см. [инастроеко запущен фоновый](#About-Background-Running) выше для получения дополнительных сведений).

Во время watchOS приложения, его можно использовать WatchConnectivity для обмена сообщениями и обмена данными с родительского приложения iOS.

Рассмотрим, как работает этот процесс:

[![](workout-apps-images/workout06.png "iPhone и схемы обмена данными Apple Watch")](workout-apps-images/workout06.png#lightbox)

1. Создает приложение для iPhone `HKWorkoutConfiguration` и устанавливает тренировки тип и расположение.
2. `HKWorkoutConfiguration` Объект отправляется версия приложения для Apple Watch и, если он еще не запущен, запускается системой.
3. С помощью переданного в конфигурации тренировки, watchOS 3 приложение начинает новый сеанс тренировки (`HKWorkoutSession`).

> [!IMPORTANT]
> **Примечание:** для родительского приложения iPhone запуск тренировки на Apple Watch, приложение watchOS 3 должен иметь фона под управлением включена. См. в разделе [Включение запущен фоновый](#Enabling-Background-Running) выше для получения дополнительных сведений.

Этот процесс очень похож на процесс запуска сеанса тренировки в приложении watchOS 3 напрямую. На iPhone используйте следующий код:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Этот код гарантирует, что установлена версия watchOS приложения и версии iPhone можно сначала подключиться к нему:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Затем он создает `HKWorkoutConfiguration` обычным образом и использует `StartWatchApp` метод `HKHealthStore` для отправки его Apple Watch и запустите приложение и тренировки сеанса.

И в приложении Контрольные значения операционной системы, используйте следующий код в `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Он принимает `HKWorkoutConfiguration` и создает новый `HKWorkoutSession` и присоединяет экземпляр пользовательской `HKWorkoutSessionDelegate`. Запускается сеанс тренировки хранилище работоспособности HealthKit пользователя.

## <a name="bringing-all-the-pieces-together"></a>Объединяя все фрагменты

Принимая все сведения, представленные в этом документе, приложения на основе тренировки watchOS 3 и его родительского приложения на основе тренировки iOS 10 может включать следующие части:

1. **iOS 10 `ViewController.cs`**  -обрабатывает запуск сеанса подключения контрольных значений и тренировки на Apple Watch.
2. **watchOS 3 `ExtensionDelegate.cs`**  -обрабатывает версии watchOS 3 тренировки приложения.
3. **watchOS 3 `OutdoorRunDelegate.cs`**  -пользовательский `HKWorkoutSessionDelegate` для обработки событий для тренировки.

> [!IMPORTANT]
> **Примечание:** код, показанный в следующих разделах включает только компоненты, необходимые для реализации новой, улучшенной функции, предоставляемые в watchOS 3 тренировки приложения. Все вспомогательные код и код для представления и обновление пользовательского интерфейса не включено, но можно легко создать, выполнив watchOS документацию.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

`ViewController.cs` Файл в основной версии iOS 10 тренировки приложения будет содержать следующий код:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

`ExtensionDelegate.cs` Файл в версии watchOS 3 тренировки приложения будет содержать следующий код:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

`OutdoorRunDelegate.cs` Файл в версии watchOS 3 тренировки приложения будет содержать следующий код:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>Рекомендации

Apple предлагает использовать следующие рекомендации при разработке и реализации приложений тренировки в watchOS 3 и iOS 10:

- Убедитесь, что watchOS 3 тренировки приложение продолжает работать даже в том случае, если не удается подключиться к iPhone и версия приложения для iOS 10.
- Используйте HealthKit расстояние, когда GPS недоступен, так как это может создать образцы расстояний без GPS.
- Разрешить пользователю запускать производительным из Apple Watch или iPhone.
- Разрешить приложение для отображения тренировки из других источников (например, других сторонних приложений) в представлениях его исторических данных.
- Убедитесь, что приложение выполняет не удалены отображения тренировки в данных с предысторией.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован усовершенствования Apple, внесенных в watchOS 3 и способы их реализации в Xamarin тренировки приложения.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Общие сведения о HealthKit](~/ios/platform/healthkit.md)
