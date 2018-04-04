---
title: EventKit
description: В этом руководстве Общие сведения о том, как работать с календари, CalendarEvents и напоминаний данные, хранящиеся в базе данных календаря как через EventKit. Рассматриваются основные классы и их роли в EventKit программирования, а также количество общие задачи, связанные с платформой EventKit.
ms.prod: xamarin
ms.assetid: 00E88629-357D-1FCD-4FCE-1330D5D9D32C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a8439586ac92f8139cf9341611125352c85706e5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="eventkit"></a>EventKit

_В этом руководстве Общие сведения о том, как работать с календари, CalendarEvents и напоминаний данные, хранящиеся в базе данных календаря как через EventKit. Рассматриваются основные классы и их роли в EventKit программирования, а также количество общие задачи, связанные с платформой EventKit._

iOS имеет два календаря приложения, связанные с встроенной: приложение календарь и напоминания приложения. Это просто понять, как приложение календаря управляет данных календаря, но приложение напоминания очевидно. Напоминания, могут иметь даты, связанные с ними на основе, когда они из-за, они время завершения, и т. д. Таким образом, операций ввода-вывода сохраняет все данные календаря, будь события календаря или напоминания в одном месте, вызывается *календаря базы данных*.

Платформа EventKit предоставляет способ доступа к *календари*, *события календаря*, и *напоминания* данные, хранящиеся в базе данных календаря. Доступ к календарям и календарь событий был доступен с iOS 4, но доступ к напоминания новые возможности iOS 6.

В этом руководстве мы собираемся охватывают:

-   **Основы EventKit** — Это вынуждает основных частей EventKit через основные классы и дает представление их использования. В этом разделе требуется чтения перед выполняемой в следующей части документа. 
-   **Общие задачи** — общие задачи раздел предназначен для использования краткий справочник о том, как выполнять общие действия, такие как; перечисление календари, создание, сохранение и извлечение календарь событий и оповещения, а также с помощью встроенных контроллеров для Создание и изменение события календаря. В этом разделе требуется не удалось прочитать передней, как означало ссылку для конкретных задач. 


В образце приложения, сопровождающий доступны все задачи в данном руководстве.

 [![](eventkit-images/01.png "Экраны сопровождающий пример приложения")](eventkit-images/01.png#lightbox)

## <a name="requirements"></a>Требования

EventKit впервые появился в iOS 4.0, но доступ к данным напоминания появилась в iOS 6.0. Таким образом, чтобы сделать общей разработки EventKit, необходимо по крайней мере целевой версии 4.0 и 6.0 для напоминания.

Кроме того приложение напоминания недоступно в имитаторе, это означает, что данные напоминания будет также недоступен, если не были добавлены сначала. Кроме того запросы на доступ только отображаются для пользователя на реальном устройстве. Таким образом разработка EventKit лучше всего тестировать на устройстве.

## <a name="event-kit-basics"></a>Общие сведения о событиях пакета

При работе с EventKit, важно иметь представление о часто используемые классы и их использования. Все эти классы можно найти в `EventKit` и `EventKitUI` (для `EKEventEditController`).

### <a name="eventstore"></a>EventStore

*EventStore* класс является наиболее важным в EventKit так, как это необходимо для выполнения любых операций в EventKit. Он может рассматриваться как постоянное хранилище, то компонент database engine для всех данных EventKit. Из `EventStore` у вас есть доступ к календарям и событий календаря в календаре, а также напоминания в приложении напоминания.

Поскольку `EventStore` — как компонента database engine, он должен быть долгоживущие, это означает, что он должен создаваться и уничтожаться как можно реже в течение времени существования экземпляра приложения. На самом деле, рекомендуется после создания одного экземпляра `EventStore` в приложении, обеспечили вокруг этой ссылки в течение всего времени существования приложения, только если уверены, он не нужен еще раз. Кроме того, все вызовы должны проходить в один `EventStore` экземпляра. По этой причине единый шаблон рекомендуется для сохранения доступен один экземпляр.

#### <a name="creating-an-event-store"></a>Создание хранилища событий

Следующий код иллюстрирует эффективный способ создания одного экземпляра `EventStore` класса и сделать ее доступной статически из приложения:

```csharp
public class App
{
    public static App Current {
            get { return current; }
    }
    private static App current;

    public EKEventStore EventStore {
            get { return eventStore; }
    }
    protected EKEventStore eventStore;

    static App ()
    {
            current = new App();
    }
    protected App () 
    {
            eventStore = new EKEventStore ( );
    }
}
```

Приведенный выше код использует единый шаблон для создания экземпляра `EventStore` при загрузке приложения. `EventStore` Может осуществляться глобально из приложения следующим образом:

```csharp
App.Current.EventStore;
```

Обратите внимание, что все примеры здесь используют этот шаблон, поэтому они ссылаются на `EventStore` через `App.Current.EventStore`.

#### <a name="requesting-access-to-calendar-and-reminder-data"></a>Запрашивает доступ к данным напоминание и календарь

Чтобы получить разрешение на доступ к данным через EventStore, приложения должен сначала запросить доступ к напоминания данные, в зависимости от того, какой из них нужно или данных события календаря. Чтобы упростить эту задачу, `EventStore` предоставляет метод, называемый `RequestAccess` которого — при вызове — будет показано представление "предупреждения" для пользователя о том, что приложение запрашивает доступ к данным календаря или напоминание данных, в зависимости от того, какие `EKEntityType`передается в него. Так как он выдает представление предупреждений, вызов выполняется в асинхронном режиме и вызывает обработчик завершения, переданный в качестве `NSAction` (или лямбда-выражение) к нему, для которого будут получены два параметры; логическое значение из того, является ли доступ предоставлен и `NSError`, который, если не равно null содержать все сведения об ошибках в запросе. Например следующие закодированных потребует доступа к данных события календаря и Показать просмотреть оповещение, если запрос не был предоставлен.

```csharp
App.Current.EventStore.RequestAccess (EKEntityType.Event, 
    (bool granted, NSError e) => {
            if (granted)
                    //do something here
            else
                    new UIAlertView ( "Access Denied", 
"User Denied Access to Calendar Data", null,
"ok", null).Show ();
            } );
```

После запроса он использовавшегося при условии, что приложение установлено на устройстве и не появится предупреждение для пользователя.
Тем не менее доступ предоставляется только для типа ресурса, событий календаря или напоминания предоставлены. Если приложению требуется доступ к обеим версиям, следует запросить оба варианта.

Поскольку разрешение запоминается, требующий относительно малых затрат на выполнение запроса каждый раз, поэтому рекомендуется всегда запрашивать доступ до выполнения операции.

Кроме того, поскольку в потоке отдельно (без пользовательского интерфейса), вызывается обработчик завершения, все обновления в пользовательском Интерфейсе в обработчик завершения должен вызываться через `InvokeOnMainThread`, в противном случае будет создано исключение, и если не обработано, произойдет сбой в приложении.

### <a name="ekentitytype"></a>EKEntityType

`EKEntityType` представляет собой перечисление, описывающее тип `EventKit` элемента или данных. Он имеет два значения: `Event` и напоминания. Он используется в ряд методов, включая `EventStore.RequestAccess` сообщить `EventKit` вида данных, чтобы получить доступ к или получить.

### <a name="ekcalendar"></a>EKCalendar

 *EKCalendar* представляет календарь, который содержит группу события календаря. Календари могут храниться во множестве различных мест, таких как локально, в *iCloud*в третьей стороне расположение поставщика, таких как *Exchange Server* или *Google*и т. д. Сколько раз `EKCalendar` сообщает `EventKit` где можно искать события и их расположение.

### <a name="ekeventeditcontroller"></a>EKEventEditController

 *EKEventEditController* можно найти в `EventKitUI` пространства имен и — это встроенный контроллер, который можно использовать для изменения или создавать события календаря. Как встроенные в контроллерах камеры `EKEventEditController` выполняет всю тяжелую работу автоматически в отображения пользовательского интерфейса и умение работать с сохранением.

### <a name="ekevent"></a>EKEvent

 *EKEvent* представляет события календаря. Оба `EKEvent` и `EKReminder` наследовать от `EKCalendarItem` и иметь поля, такие как `Title`, `Notes`, и т. д.

### <a name="ekreminder"></a>EKReminder

 *EKReminder* представляет элемент напоминание.

### <a name="ekspan"></a>EKSpan

*EKSpan* представляет собой перечисление, описывающее область события при изменении события, которые могут повторяться и имеет два значения: *этот* и *FutureEvents*. `ThisEvent` означает, что все изменения будут встречаться только для определенного события в модуле, на который ссылается, в то время как `FutureEvents` повлияет на это событие и все будущие повторения.

## <a name="tasks"></a>Задачи

Для удобства использования EventKit был разбит на общие задачи, описанные в следующих разделах.

### <a name="enumerate-calendars"></a>Перечислить календарей

Чтобы перечислить календари, которые пользователь настроил на устройстве, вызовите `GetCalendars` на `EventStore` и передать этот тип календарей (напоминания или события), которые вы хотите получать:

```csharp
EKCalendar[] calendars = 
App.Current.EventStore.GetCalendars ( EKEntityType.Event );
```

### <a name="add-or-modify-an-event-using-the-built-in-controller"></a>Добавление или изменение события с помощью встроенного контроллера

*EKEventEditViewController* выполняет множество часть тяжелой работы для вас, если требуется создать или изменить событие с помощью же пользовательского интерфейса, которые отображаются для пользователя при работе с приложением календаря:

 [![](eventkit-images/02.png "Пользовательского интерфейса, которые отображаются для пользователя при работе с приложением календаря")](eventkit-images/02.png#lightbox)

Чтобы использовать его, необходимо объявить его как переменной уровня класса, чтобы оно не получать сбора мусора, если он объявлен внутри метода:

```csharp
public class HomeController : DialogViewController
{
        protected CreateEventEditViewDelegate eventControllerDelegate;
        ...
}
```

Нажмите, чтобы запустить его: создать его экземпляр, присвойте ему ссылку на `EventStore`, подключите *EKEventEditViewDelegate* делегата к нему, а затем отобразить ее с помощью `PresentViewController`:

```csharp
EventKitUI.EKEventEditViewController eventController = 
        new EventKitUI.EKEventEditViewController ();

// set the controller's event store - it needs to know where/how to save the event
eventController.EventStore = App.Current.EventStore;

// wire up a delegate to handle events from the controller
eventControllerDelegate = new CreateEventEditViewDelegate ( eventController );
eventController.EditViewDelegate = eventControllerDelegate;

// show the event controller
PresentViewController ( eventController, true, null );
```

При необходимости, если вы хотите предварительно заполнить события, можно создать новые события (как показано ниже) или можно извлечь сохраненные события:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and exercise!";
newEvent.Notes = "This is your reminder to go and exercise for 30 minutes.”;
```

Если вы хотите предварительно заполнить пользовательского интерфейса, убедитесь, что свойства события на контроллере:

```csharp
eventController.Event = newEvent;
```

Для использования существующего события, в разделе *получение событий по Идентификатору* статьи позже.

Делегат должен переопределить `Completed` метод, который вызывается при завершении пользователем с диалоговым окном контроллера:

```csharp
protected class CreateEventEditViewDelegate : EventKitUI.EKEventEditViewDelegate
{
        // we need to keep a reference to the controller so we can dismiss it
        protected EventKitUI.EKEventEditViewController eventController;

        public CreateEventEditViewDelegate (EventKitUI.EKEventEditViewController eventController)
        {
                // save our controller reference
                this.eventController = eventController;
        }

        // completed is called when a user eith
        public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
        {
                eventController.DismissViewController (true, null);
                }
        }
}
```

При необходимости в делегате, можно проверить *действия* в `Completed` метод для изменения событий и заново или выполнения других действий в том случае, если она отменена, etcetera:

```csharp
public override void Completed (EventKitUI.EKEventEditViewController controller, EKEventEditViewAction action)
{
        eventController.DismissViewController (true, null);

        switch ( action ) {

        case EKEventEditViewAction.Canceled:
                break;
        case EKEventEditViewAction.Deleted:
                break;
        case EKEventEditViewAction.Saved:
                // if you wanted to modify the event you could do so here,
// and then save:
                //App.Current.EventStore.SaveEvent ( controller.Event, )
                break;
        }
}
```

### <a name="creating-an-event-programmatically"></a>Создание события программным способом

Чтобы создать событие в коде, используйте *FromStore* фабричный метод на `EKEvent` и укажите все данные на нем:

```csharp
EKEvent newEvent = EKEvent.FromStore ( App.Current.EventStore );
// set the alarm for 10 minutes from now
newEvent.AddAlarm ( EKAlarm.FromDate ( DateTime.Now.AddMinutes ( 10 ) ) );
// make the event start 20 minutes from now and last 30 minutes
newEvent.StartDate = DateTime.Now.AddMinutes ( 20 );
newEvent.EndDate = DateTime.Now.AddMinutes ( 50 );
newEvent.Title = "Get outside and do some exercise!";
newEvent.Notes = "This is your motivational event to go and do 30 minutes of exercise. Super important. Do this.";
```

Необходимо задать календарь, в котором сохранен в события, но если предпочтение отсутствует, можно использовать значение по умолчанию:

```csharp
newEvent.Calendar = App.Current.EventStore.DefaultCalendarForNewEvents;
```

Чтобы сохранить события, вызовите *SaveEvent* метод `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveEvent ( newEvent, EKSpan.ThisEvent, out e );
```

После сохранения, *EventIdentifier* свойство будет обновляться с уникальным идентификатором, который может использоваться для извлечения события позднее:

```csharp
Console.WriteLine ("Event Saved, ID: " + newEvent.CalendarItemIdentifier);
```

 `EventIdentifier` — Строка в формате GUID.

### <a name="create-a-reminder-programmatically"></a>Создавать оповещение программным способом

Напоминание в коде создается так же, как создание события календаря:

```csharp
EKReminder reminder = EKReminder.Create ( App.Current.EventStore );
reminder.Title = "Do something awesome!";
reminder.Calendar = App.Current.EventStore.DefaultCalendarForNewReminders;
```

Чтобы сохранить, вызовите *SaveReminder* метод `EventStore`:

```csharp
NSError e;
App.Current.EventStore.SaveReminder ( reminder, true, out e );
```

### <a name="retrieving-an-event-by-id"></a>Получение события по Идентификатору

Для получения события, он является Идентификатором, используйте *EventFromIdentifier* метод `EventStore` и передать его `EventIdentifier` , запрошенная из события:

```csharp
EKEvent mySavedEvent = App.Current.EventStore.EventFromIdentifier ( newEvent.EventIdentifier );
```

Для событий, имеется два свойства идентификатора, но `EventIdentifier` является единственным, подходящим для этого.

### <a name="retrieving-a-reminder-by-id"></a>Получение оповещение по Идентификатору

Чтобы получить напоминание, используйте *GetCalendarItem* метод `EventStore` и передать его *CalendarItemIdentifier*:

```csharp
EKCalendarItem myReminder = App.Current.EventStore.GetCalendarItem ( reminder.CalendarItemIdentifier );
```

Поскольку `GetCalendarItem` возвращает `EKCalendarItem`, он должен быть приведен к `EKReminder` требуется доступ к данным напоминание или использовать экземпляр в качестве `EKReminder` позже.

Не используйте `GetCalendarItem` событий календаря, поскольку на момент написания статьи, он не работает.

### <a name="deleting-an-event"></a>Удаление события

Чтобы удалить события календаря, вызовите *RemoveEvent* на ваш `EventStore` и передать ссылку на событие и соответствующие `EKSpan`:

```csharp
NSError e;
App.Current.EventStore.RemoveEvent ( mySavedEvent, EKSpan.ThisEvent, true, out e);
```

Примечание Однако после удаления события ссылку событий будет `null`.

### <a name="deleting-a-reminder"></a>Удаляя оповещения

Чтобы удалить оповещение, вызовите *RemoveReminder* на `EventStore` и передать ссылку на оповещение:

```csharp
NSError e;
App.Current.EventStore.RemoveReminder ( myReminder as EKReminder, true, out e);
```

Обратите внимание, что в приведенном выше примере приведение к `EKReminder`, так как `GetCalendarItem` был использован для его извлечения

### <a name="searching-for-events"></a>Поиск событий

Чтобы найти события календаря, необходимо создать *NSPredicate* объекта через *PredicateForEvents* метод `EventStore`. `NSPredicate` — Это запрос, операций ввода-вывода использует для поиска совпадений объект данных:

```csharp
DateTime startDate = DateTime.Now.AddDays ( -7 );
DateTime endDate = DateTime.Now;
// the third parameter is calendars we want to look in, to use all calendars, we pass null
NSPredicate query = App.Current.EventStore.PredicateForEvents ( startDate, endDate, null );
```

После создания `NSPredicate`, используйте *EventsMatching* метод `EventStore`:

```csharp
// execute the query
EKCalendarItem[] events = App.Current.EventStore.EventsMatching ( query );
```

Обратите внимание, что запросы, синхронный (блокировка) может занять длительное время, в зависимости от запроса, поэтому может потребоваться запустить новый поток или задачу, чтобы сделать это.

### <a name="searching-for-reminders"></a>Поиск напоминания

Поиск напоминания похож на события; он требует предикат, но вызов уже является асинхронным, поэтому можно не волноваться по поводу блокировки потока:

```csharp
// create our NSPredicate which we'll use for the query
NSPredicate query = App.Current.EventStore.PredicateForReminders ( null );

// execute the query
App.Current.EventStore.FetchReminders (
        query, ( EKReminder[] items ) => {
                // do someting with the items
        } );
```

## <a name="summary"></a>Сводка

В этом документе предлагает Обзор важных элементов EventKit framework и ряд распространенных задач. Однако EventKit framework очень большой и мощные и включает в себя функции, которые еще не были введены, такие как: пакета обновления, Настройка сигналы, Настройка повторения о событиях, регистрации и прослушивание изменений в базе данных календаря Настройка GeoFences и многое другое.  Дополнительные сведения см в компании Apple [календаря и руководство по программированию напоминания](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html).


## <a name="related-links"></a>Связанные ссылки

- [Календари (пример)](https://developer.xamarin.com/samples/monotouch/Calendars/)
- [Введение в iOS 6](~/ios/platform/introduction-to-ios6/index.md)
- [Общие сведения о календарях и напоминания](https://developer.apple.com/library/prerelease/ios/#documentation/DataManagement/Conceptual/EventKitProgGuide/Introduction/Introduction.html)
