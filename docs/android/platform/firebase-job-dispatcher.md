---
title: "Диспетчер заданий firebase"
description: "В этом руководстве описывается планирование фоновой работы с помощью библиотеки диспетчера заданий Firebase от Google."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/19/2018
ms.openlocfilehash: c542237523b934cb8616fda6cefdcd969b7700bd
ms.sourcegitcommit: cc38757f56aab53bce200e40f873eb8d0e5393c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2018
---
# <a name="firebase-job-dispatcher"></a>Диспетчер заданий firebase

_В этом руководстве описывается планирование фоновой работы с помощью библиотеки диспетчера заданий Firebase от Google._

## <a name="firebase-job-dispatcher-overview"></a>Обзор диспетчера заданий firebase

Один из лучших способов обеспечения быстрого реагирования, пользователю приложения — убедитесь, что сложных или длительных работа выполняется в фоновом режиме. Тем не менее очень важно, что фоновая операция не отрицательно влияет на взаимодействие с пользователем с устройством. 

Например фоновое задание может выполнять опрос веб-сайта каждые пять минут для запроса, для изменения для конкретного набора данных. Это может показаться шагом информационный характер, однако он может иметь катастрофические последствия на устройстве. Приложение будет помещен переводом устройства, повысив уровень ЦП выше состояние питания, включение питания радио выполнения сетевых запросов и затем обработки результатов. Он получает хуже, так как устройство не сразу будет выключить питание и вернуться в состояние бездействия низкого энергопотребления. Плохо расписанию фоновых рабочих случайно могут обеспечить устройства в состоянии без требования к мощности ненужное и излишнее. По сути кажущего безобидно действия (опроса веб-сайт) сделает устройства недоступными в относительно короткий период времени.

Android уже предоставляет несколько API для выполнения работы в фоновом режиме, однако ни один из этих комплексное решение:

* **[Блокировка с намерением службы](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; цель службы прекрасно подходят для выполнения работы, однако они не предоставляют способа планирования работы.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; эти API-интерфейсы только разрешить работу, чтобы запланировать, но не позволяют фактически выполняют работу. Кроме того AlarmManager допускает только ограничения на основе времени, это означает, что создавать аварийный сигнал в определенное время или после истечения определенного периода времени. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; JobSchedule — это отличный API, который работает с операционной системой для планирования заданий. Однако он доступен только для этих приложений для Android, целевой уровень API 21 или более поздней версии. 
* **[Рассылка приемники](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; приложения Android можно настроить широковещательных получателей для выполнения работы в ответ на системные расширенные события или целей. Однако широковещательных получателей не обеспечивают контроля над выполнения задания. Также будет ограничивать изменения в ОС Android при широковещательных получатели будут работать или виды работ, которое может ответить. 
* **Диспетчер сети сообщений Google облака** &ndash; в течение длительного времени это было, вероятно, лучший способ интеллектуально фона расписание работы. Тем не менее GCMNetworkManager, поскольку является устаревшим. 

Существует две основные функции для эффективного выполнения фоновой работы (иногда называют _фоновое задание_ или _задания_):

1. **Интеллектуальное планирования работы** &ndash; важно, когда приложение выполняет работу в фоновом режиме, используется как хорошо угла. В идеальном случае приложения не должен требовать выполнение задания. Вместо этого приложение должно указать условия, которые должны быть выполнены для, когда задание можно запустить и расписание, затем, работа которых позволяет запускать, когда выполняются условия. Это позволяет Android для интеллектуально выполнения работы. Например сетевые запросы могут пакетами для выполнения всех в то же время, чтобы использовать максимально издержки в виде связанных с загрузкой сетевых драйверов.
2. **Инкапсуляция работу** &ndash; код для выполнения фоновой работы должны инкапсулироваться в дискретные компонент, который может выполняться независимо от пользовательского интерфейса и будет относительно легко перенести, если не удается завершить работу по некоторым причинам.

Диспетчер заданий Firebase — это библиотека от Google, предоставляющий fluent API для упрощения планирования фоновой работы. Он предназначен для замены для диспетчера облаков Google. Диспетчер заданий Firebase состоит из следующих API-интерфейсов.

* Объект `Firebase.JobDispatcher.JobService` — абстрактный класс, который необходимо расширить с логикой, которая будет выполняться в фоновом задании.
* Объект `Firebase.JobDispatcher.JobTrigger` объявляет начала задания. Это обычно выражается в виде окна времени, например, подождите по крайней мере за 30 секунд перед запуском задания, но выполнить задание в течение 5 минут.
* Объект `Firebase.JobDispatcher.RetryStrategy` содержит сведения о какие действия должны выполняться, если задание не выполняется должным образом. Стратегия повторов определяет, как долго ждать перед попыткой снова запустите задание. 
* Объект `Firebase.JobDispatcher.Constraint` — необязательное значение, которое описывает условие, которое должны быть выполнены перед запуском заданий, таких как устройство находится в сети, unmetered или заряжается.
* `Firebase.JobDispatcher.Job` API-интерфейс, который унифицирует предыдущих API в для единицы работы, можно запланировать с `JobDispatcher`. `Job.Builder` Класс используется для создания экземпляра `Job`.
* Объект `Firebasee.JobDispatcher.JobDispatcher` использует предыдущие три API-интерфейсы для планирования работы с операционной системой и предоставляет способ отменить задания, при необходимости.

Для планирования работы с диспетчером задания Firebase, приложение Xamarin.Android должен быть инкапсулирован в тип, который расширяет код `JobService` класса. `JobService` имеется три метода жизненный цикл, который может вызываться в течение времени существования задания:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Этот метод является, где будет выполняться работа, а всегда должен быть реализован. Он выполняется в основном потоке. Этот метод будет возвращать `true` при наличии работы осталось, или `false` Если выполняется работа. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Вызывается, когда задание остановлен по некоторым причинам. Этот метод должен возвращать `true` Если задание должно быть перепланировано на более поздний срок.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Этот метод вызывается, когда `JobService` завершения какой-либо асинхронной работы. 

Чтобы запланировать задание, приложение создает экземпляр `JobDispatcher` объекта. Затем инструкция `Job.Builder` используется для создания `Job` объекта, который передается `JobDispatcher` которого будет попробуйте и запланировать запуск задания.

В этом руководстве описывается, как добавить в приложение Xamarin.Android Firebase диспетчер заданий и использовать его для планирования работы в фоновом режиме.

## <a name="requirements"></a>Требования

Диспетчер заданий Firebase требует Android API уровня 9 или более поздней версии. Библиотека диспетчера заданий Firebase использует некоторые компоненты, предоставляемые службы Google Play; устройство должно иметь установлены службы Google Play.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>С помощью диспетчера библиотеки Firebase задания в Xamarin.Android

Чтобы приступить к работе с диспетчером Firebase задания, сначала добавьте [пакет Xamarin.Firebase.JobDispatcher NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher/0.6.0-beta1) Xamarin.Android проект. Поиск диспетчера пакетов NuGet для **Xamarin.Firebase.Jobdispatcher** пакета.  

После добавления в библиотеку диспетчера заданий Firebase, создайте `JobService` класса, а затем запланируйте выполнение с помощью экземпляра `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Создание `JobService`

Все работы, выполненные в библиотеку диспетчера заданий Firebase должно производиться в тип, который расширяет `Firebase.JobDispatcher.JobService` абстрактного класса. Создание `JobService` очень похоже на создание `Service` с платформой Android: 

1. Расширить `JobService` класса
2. Украшение подкласс с `ServiceAttribute`. Несмотря на то, что не обязательно, рекомендуется явно задавать `Name` параметр в процессе отладки `JobService`. 
3. Добавить `IntentFilter` для объявления `JobService` в **AndroidManifest.xml**. Это также позволит библиотеку диспетчера заданий Firebase поиска и вызова неуправляемого кода `JobService`.

Ниже приведен пример простой `JobService` для приложения:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // Note: This runs on the main thread. Anything that takes longer than 16 milliseconds
         // should be run on a seperate thread.
        
        return false; // return false because there is no more work to do.
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Создание `FirebaseJobDispatcher`

Прежде чем можно планировать никаких операций, это необходимо для создания `Firebase.JobDispatcher.FirebaseJobDispatcher` объекта. `FirebaseJobDispatcher` Отвечает за планирование `JobService`. В следующем фрагменте кода является одним из способов создания экземпляра `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

В приведенном выше фрагменте кода `GooglePlayDriver` является классом, который помогает `FirebaseJobDispatcher` взаимодействовать с некоторыми планирования API в службы Google Play на устройстве. Параметр `context` является любой Android `Context`, например, действия. В настоящее время `GooglePlayDriver` является единственным `IDriver` реализацию в библиотеке Firebase диспетчер заданий. 

Привязка Xamarin.Android для задания диспетчера Firebase предоставляет методы расширения для создания `FirebaseJobDispatcher` из `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Один раз `FirebaseJobDispatcher` был создан, его можно создать `Job` и выполнение кода в `JobService` класса. `Job` Создается путем `Job.Builder` объекта и обсуждаются в следующем разделе.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Создание `Firebase.JobDispatcher.Job` с `Job.Builder`

`Firebase.JobDispatcher.Job` Класс отвечает за инкапсуляцию метаданных необходимо запустить `JobService`. Объект`Job` содержит сведения, например ограничений, должны быть выполнены перед запуском заданий, если `Job` повторяется, или какие-либо триггеры, которые вызовут выполнение задания.  Как минимум `Job` должен иметь _тега_ (уникальную строку, которая идентифицирует задание, чтобы `FirebaseJobDispatcher`) и тип `JobService` должен выполняться. Создает экземпляр диспетчера заданий Firebase `JobService` при пришло время для выполнения задания.  Объект `Job` создается с помощью экземпляра `Firebase.JobDispatcher.Job.JobBuilder` класса. 

В следующем фрагменте кода является простейшим примером создание `Job` с использованием привязки Xamarin.Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` Выполняет ряд проверок базовая проверка входных значений для задания. Будет создано исключение, если не возможным `Job.Builder` для создания `Job`.  `Job.Builder` Создаст `Job` с следующие значения по умолчанию:

* Объект `Job` _время существования_ (как долго он будет запланирована для выполнения) — только в том случае, пока устройство перезагрузится &ndash; после перезагрузки устройства `Job` теряется.
* Объект `Job` не повторяется &ndash; он будет выполняться только один раз.
* Объект `Job` будет запланировано выполнение как можно быстрее.
* Стратегия повторов по умолчанию для `Job` заключается в использовании _экспоненциально растущим_ (описано на подробнее рассмотрены ниже в разделе [параметр RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Планирование `Job`

После создания `Job`, он должен быть запланирован с `FirebaseJobDispatcher` перед его запуском. Существует два способа планирования `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Значение, возвращаемое `FirebaseJobDispatcher.Schedule` будет иметь одно из следующих целочисленных значений:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; `Job` Успешно запланировано.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Возникла неизвестная проблема, что помешало `Job` от планирования.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Недопустимый `IDriver` использовался или `IDriver` каким-либо образом был недоступен. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; `Trigger` , Который не поддерживается.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Служба неправильно настроена или недоступна.
 
### <a name="configuring-a-job"></a>Настройка задания

Существует возможность настроить задание. Следующие примеры как можно настроить задание.

* [Передача параметров для задания](#Passing_Parameters_to_a_Job) &ndash; A `Job` может потребоваться дополнительные значения для выполнения работы, например загрузку файла.
* [Задать ограничения](#Setting_Constraints) &ndash; может потребоваться запускать задание только при выполнении определенных условий. Например, запускать только `Job` при заряжается устройства. 
* [Укажите, когда `Job` следует запускать](#Setting_Job_Triggers) &ndash; диспетчера заданий Firebase разрешает приложениям задавать время, когда должно выполняться задание.  
* [Объявите стратегию повторных попыток для неудачно завершившихся заданий](#Setting_a_RetryStrategy) &ndash; A _стратегии повтора_ содержится руководство для `FirebaseJobDispatcher` о том, что делать с `Jobs` , не удастся выполнить. 

В каждом разделе обсуждаются более в следующих разделах.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Передача параметров задания

Параметры передаются в задание, создав `Bundle` , передается вместе с `Job.Builder.SetExtras` метод:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

`Bundle` Осуществляется из `IJobParameters.Extras` свойство `OnStartJob` метод:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Установка ограничений

Ограничения могут помочь сокращает затраты или батареи стока на устройстве. `Firebase.JobDispatcher.Constraint` Класс определяет эти ограничения, как целые значения:

* `Constraint.OnUnmeteredNetwork` &ndash; Запустите задание только в том случае, когда устройство подключено к сети unmetered. Это полезно для предотвращения взимается плата данных пользователя.
* `Constraint.OnAnyNetwork` &ndash; Запустите задание в любой сети устройство подключено. Если указан вместе с `Constraint.OnUnmeteredNetwork`, это значение будет иметь приоритет.
* `Constraint.DeviceCharging` &ndash; Выполняются задания только в том случае, если заряжается устройства.

Ограничения задаются с помощью `Job.Builder.SetConstraint` метод: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

#### <a name="setting-job-triggers"></a>Триггеры задания для параметра

`JobTrigger` Содержится руководство для операционной системы о начала задания. Объект `JobTrigger` имеет _выполнения окно_ , определяющий запланированное время, когда `Job` следует запустить. Окно выполнения имеет _откройте окно_ значение и _окончания окна_ значение. Количество секунд ожидания перед выполнением задания устройства окно запуска, а конечное окно — максимальное количество секунд ожидания перед запуском `Job`. 

Объект `JobTrigger` могут создаваться с `Firebase.Jobdispatcher.Trigger.ExecutionWindow` метод.  Например `Trigger.ExecutionWindow(15,60)` означает, что от 15 до 60 секунд в запланированное время его запуска задания. `Job.Builder.SetTrigger` Метод используется для 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Значение по умолчанию `JobTrigger` для задания, представленный значением `Trigger.Now`, которое указывает, что выполнение задания как можно быстрее после плановой...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Параметр RetryStrategy

`Firebase.JobDispatcher.RetryStrategy` Используется для указания того, сколько задержки устройства следует использовать перед попыткой выполнить невыполненное задание повторно. Объект `RetryStrategy` имеет _политики_, который определяет, какой алгоритм базовое время будет использован для повторного планирования невыполненного задания и появится окно выполнения, задает окно, в котором запланированного задания. Это _перенесения окна_ определяется двумя значениями. Первое значение — количество секунд ожидания перед Перепланирование задания ( _начальной задержки_ значение), а второе — максимальное число секунд, прежде чем задание должно выполняться ( _Максимальная задержка_значение). 
 
Два типа политики повтора идентифицируются этих значений типа int:

* `RetryStrategy.RetryPolicyExponential` &ndash; _Экспоненциально растущим_ политики экспоненциально увеличивает значение начальной задержки после каждой ошибки. При первом сбое задания библиотеки будет ожидать _initial интервала, который указывается перед Перепланирование задания &ndash; примере 30 секунд. Во второй раз задание завершается с ошибкой, библиотеке ожидания менее 60 секунд, прежде чем пытаться выполнить задание. После третьей неудачной попытки, библиотеке ожидания 120 секунд и так далее. Значение по умолчанию `RetryStrategy` Firebase диспетчер заданий библиотеки представляются `RetryStrategy.DefaultExponential` объекта. Она имеет начальной задержки 30 секунд и Максимальная задержка 3600 секунд.
* `RetryStrategy.RetryPolicyLinear` &ndash; Эта стратегия _линейной задержки_ , задание должно быть перепланировано для запуска в задать интервалы времени (до ее успешного выполнения). Линейной задержки наилучшим образом подходит для работы, которая должна быть выполнена как можно быстрее или проблемы, которые быстро устранить самостоятельно. Библиотека диспетчера заданий Firebase определяет `RetryStrategy.DefaultLinear` которого имеет перенесения окна менее 30 секунд до 3600 секунд.

Можно определить пользовательские `RetryStrategy` с `FirebaseJobDispatcher.NewRetryStrategy` метод. Он принимает три параметра:

1. `int policy` &ndash; _Политики_ является одним из предыдущего `RetryStrategy` значения, `RetryStrategy.RetryPolicyLinear`, или `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; _Начальной задержки_ некоторое время, в секундах, необходимое перед попыткой снова запустите задание. Значение по умолчанию составляет 30 секунд. 
3. `int maximumBackoffSeconds` &ndash; _Максимальная задержка_ значение объявляет максимальное число секунд для задержки перед попыткой снова запустите задание. Значение по умолчанию — 3 600 секунд. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Отмена задания

Невозможно отменить всех запланированных заданий или только одно задание с помощью `FirebaseJobDispatcher.CancelAll()` метода или `FirebaseJobDispatcher.Cancel(string)` метод:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Оба этих метода будут возвращать целочисленное значение:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Задание было успешно отменено.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Ошибка не позволила производится Отмена задания.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; `FirebaseJobDispatcher` Не удалось отменить задание, поскольку нет нет допустимых `IDriver` доступны.

## <a name="summary"></a>Сводка

В этом руководстве описывается, как использовать диспетчер заданий Firebase для интеллектуально выполнения работы в фоновом режиме. Обсуждали инкапсулировать работу выполняться как `JobService` и `FirebaseJobDispatcher` для планирования этой работы, задавая критерии с `JobTrigger` и способ обработки ошибок с `RetryStrategy`.


## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Firebase.JobDispatcher в NuGet](https://www.nuget.org/packages/Xamarin.FirebaseJobDispatcher)
- [firebase задания — диспетчер на GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher Binding](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Интеллектуальная планирования заданий](https://developer.android.com/topic/performance/scheduling.html)
- [Android батареи и оптимизации памяти - 2016 Google ввода-вывода (видео)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
