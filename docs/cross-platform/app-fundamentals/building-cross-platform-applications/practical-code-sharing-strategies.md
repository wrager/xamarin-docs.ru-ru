---
title: Часть 5 - практические кода стратегии для управления доступом
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 6f6b88bf29e94a221b2ef58b3299348eb08d33fa
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="part-5---practical-code-sharing-strategies"></a>Часть 5 - практические кода стратегии для управления доступом

В этом разделе приведены примеры способов совместного использования кода для общих сценариев приложений.



## <a name="data-layer"></a>Уровень данных

Уровень данных состоит из подсистемы хранилища, а также методы для чтения и записи данных. Совместимость и производительность, кросс платформенных SQLite для Xamarin кросс платформенных приложений рекомендуется компонента database engine.
Он работает на разнообразных платформах, включая Windows, Android, iOS и Macintosh.


### <a name="sqlite"></a>SQLite

SQLite представляет собой реализацию базы данных с открытым исходным кодом. Источник и документацию можно найти на [SQLite.org](http://www.sqlite.org/). SQLite поддержка доступна в каждой платформы мобильных устройств:

-  **iOS** — встроенной в операционной системе.
- **Android** — встроены в операционную систему с момента Android 2.2 (API уровня 10).
- **Windows** — в разделе [SQLite для универсальной платформы Windows расширения](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).


Даже с компонентом database engine для всех платформ отличаются собственные методы для доступа к базе данных. Оба iOS и Android предоставляют встроенные API для доступа к SQLite, который может использоваться из Xamarin.iOS и Xamarin.Android, однако использование собственных методов SDK предоставляет нет возможность совместного использования кода (кроме, возможно SQL-запросы, при условии, что они хранятся как строки) . Дополнительные сведения о поиске функциональные возможности собственной базы данных для `CoreData` в iOS или Android `SQLiteOpenHelper` класса, так как эти параметры не кросс платформенных они выходят за рамки данного документа.



### <a name="adonet"></a>ADO.NET

Поддержка Xamarin.iOS и Xamarin.Android `System.Data` и `Mono.Data.Sqlite` (в разделе Xamarin.iOS [документации](~/ios/data-cloud/system.data.md) Дополнительные сведения).
Эти пространства имен позволяет писать код ADO.NET, который работает на обеих платформах. Изменение ссылки на проект для включения `System.Data.dll` и `Mono.Data.Sqlite.dll` и добавьте следующие операторы using в коде:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

В следующем примере кода будет работать:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Реализации реальных ADO.NET очевидно, что бы быть разбиты на различные методы и классы (в этом примере — только для демонстрационных целей).



### <a name="sqlite-net--cross-platform-orm"></a>SQLite-NET-ORM кросс платформенных

Объектно-реляционные Преобразователи (или объектно-реляционного сопоставления) пытается упростить хранение данных моделируется в классах. Вместо вручную подготавливая SQL-запросы, создание таблиц или SELECT, INSERT и DELETE данных, которые вручную извлечь из поля и свойства ORM добавляет слой кода, который делает это для вас. Чтобы изучить структуру классов с помощью отражения, ORM может автоматически создавать таблицы и столбцы, которые соответствуют класс и создают запросы на чтение и запись данных. Это позволяет коду приложения просто отправлять и получать ORM, который берет на себя все операции SQL за кулисами экземпляров объектов.

SQLite NET выступает в качестве простого ORM, который позволит вам для сохранения и извлечения классов в SQLite. Он скрывает сложность кросс-доступа к SQLite платформы с помощью директивы компилятора и другие приемы.

Функции SQLite NET:

-  Добавив атрибуты классов модели определяются таблицы.
-  Экземпляр базы данных, представленного подкласс `SQLiteConnection` , основным классом SQLite-Net-library.
-  Данные могут быть вставлены, запрашивать и удалять с помощью объектов. Инструкции SQL не являются обязательными (несмотря на то, что можно написать инструкций SQL, при необходимости).
-  Базовые запросы Linq могут выполняться в коллекциях, возвращенных SQLite NET.


Исходный код и документация для SQLite NET доступна на [SQLite-Net на github](https://github.com/praeclarum/sqlite-net) и была реализована в обоих исследований. Простым примером кода SQLite NET (из *Tasky Pro* практический пример) показано ниже.

Во-первых, `TodoItem` класс использует атрибуты для определения поля первичного ключа базы данных:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

Это позволяет `TodoItem` таблицы для создания на следующую строку кода (и в инструкции SQL не) `SQLiteConnection` экземпляр:

```csharp
CreateTable<TodoItem> ();
```

Данные в таблице можно также управлять с помощью других методов на `SQLiteConnection` (опять же, не требуя инструкций SQL):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

См. исходный код практический пример полные примеры.



## <a name="file-access"></a>Доступ к файлам

Доступ к файлам быть ключевой частью любого приложения. Распространенные примеры файлов, которые могут быть частью включения приложения:

-  Файлы базы данных SQLite.
-  Созданное пользователем данных (текст, изображения, звуковые, видео).
-  Загруженные данные для кэширования (изображения, html или PDF-файлы).




### <a name="systemio-direct-access"></a>System.IO прямой доступ

Xamarin.iOS и Xamarin.Android разрешить доступ к файловой системе, с помощью классов в `System.IO` пространства имен.

Каждой платформы имеют различный уровень доступа ограничения, которые необходимо принимать во внимание.

-  приложения iOS выполняются в изолированной среде с доступа очень ограничен файловой системы. Apple дальнейшей определяет, как следует использовать в файловой системе, указав определенные расположения, резервную копию (и другим пользователям, которые не являются). Ссылаться на [работа с использованием файловой системы в Xamarin.iOS](~/ios/app-fundamentals/file-system.md) руководство для получения дополнительных сведений.
-  Android также ограничивает доступ к некоторые каталоги, связанные с приложением, но оно также поддерживает внешний носитель (например) SD-карт) и доступ к общим данным.
-  Запретить прямой доступ к файлам Windows Phone 8 (Silverlight) — файлов только возможна с помощью `IsolatedStorage`.
-  Проекты WinRT Windows 8.1 и Windows 10 UWP только предлагают асинхронные операции с файлами через `Windows.Storage` API, которые отличаются от других платформ.

#### <a name="example-for-ios-and-android"></a>Пример для iOS и Android

Ниже приведен упрощенного примера, записи и чтения текстового файла.
С помощью `Environment.GetFolderPath` позволяет один и тот же код для выполнения на iOS и Android, каждый из которых возвращать допустимый каталог, исходя из их соглашений файловой системы.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.ReadAllText (filePath));
```

См. в Xamarin.iOS [работа с файловой системой](~/ios/app-fundamentals/file-system.md) Дополнительные сведения о функции файловой системы для конкретных операций ввода-вывода. При написании кода доступа кросс платформенных файл, помните, что некоторые файловые системы чувствительны к регистру и разделителей в другой каталог. Рекомендуется всегда использовать один регистр для имен файлов и `Path.Combine()` метод при создании пути к файлу или каталогу.



### <a name="windowsstorage-for-windows-8-and-windows-10"></a>Windows.Storage для Windows 8 и Windows 10

*Создание мобильных приложений с помощью Xamarin.Forms* [книги](https://developer.xamarin.com/r/xamarin-forms/book/)
[Глава 20. Async и файловый ввод-вывод](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) включает [образцы для Windows 8.1 и Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

С помощью [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) можно прочитать файл. файлы на этих платформах, поддерживаемых API с помощью:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Ссылаться на [глава книги](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) для получения дополнительных сведений.


<a name="Isolated_Storage" />

### <a name="isolated-storage-on-windows-phone-7--8-silverlight"></a>Изолированное хранилище в Windows Phone 7 и 8 (Silverlight)

Изолированное хранилище — общий API для сохранения и загрузки файлов во всех iOS, Android и более старых платформ Windows Phone.

Он представляет собой механизм по умолчанию для доступа к файлам в Windows Phone (Silverlight), которое было реализовано в Xamarin.iOS и Xamarin.Android разрешить общий доступ к файлам код для записи. `System.IO.IsolatedStorage` Ссылка на класс для всех трех платформ в [общий проект](~/cross-platform/app-fundamentals/shared-projects.md).

Ссылаться на [изолированного хранения Обзор для Windows Phone](http://msdn.microsoft.com/library/windowsphone/develop/ff402541(v=vs.105).aspx) для получения дополнительной информации.

API-интерфейсы изолированного хранения не доступны в [переносимой библиотеки классов](~/cross-platform/app-fundamentals/pcl.md). — Альтернатива PCL [PCLStorage NuGet](https://pclstorage.codeplex.com/)



### <a name="cross-platform-file-access-in-pcls"></a>Доступ к файлам кросс платформенных в PCLs

Отсутствует совместимый PCL Nuget — [PCLStorage](https://www.nuget.org/packages/PCLStorage/) — этого средства кросс платформенных доступ к файловой Xamarin поддерживаемые платформы и последнюю API-интерфейсов Windows.


## <a name="network-operations"></a>Сетевые операции

Большинство мобильных приложений будет иметь сетевого компонента, например:

-  Загрузка изображений, видео и аудио (например) эскизы, фотографии, музыка).
-  Загрузка документов (например) HTML, PDF).
-  Передача данных пользователя (например, изображения или текста).
-  Доступ к веб-службы или третьих сторон API-интерфейсы (включая SOAP, XML или JSON).


.NET Framework предоставляет несколько различных классов для доступа к сетевым ресурсам: `HttpClient`, `WebClient`, и `HttpWebRequest`.

### <a name="httpclient"></a>HttpClient

`HttpClient` В класс `System.Net.Http` пространство имен доступно в Xamarin.iOS, Xamarin.Android и большинстве платформ Windows. Отсутствует [Nuget клиентской библиотеки Microsoft HTTP](https://www.nuget.org/packages/Microsoft.Net.Http/) может использоваться для переноса этого API в переносимой библиотеки классов (и Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### <a name="webclient"></a>WebClient

`WebClient` Класс предоставляет простой API-Интерфейс для получения удаленных данных с удаленных серверов.

Универсальных операций Windows Platofrm *должен* быть async, несмотря на то, что Xamarin.iOS и Xamarin.Android поддерживает синхронных операций (что может быть выполнено в фоновых потоках).

Код для простых асинхронное `WebClient` операции:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` также имеет `DownloadFileCompleted` и `DownloadFileAsync` для извлечения двоичных данных.

<a name="HttpWebRequest" />

### <a name="httpwebrequest"></a>HttpWebRequest

`HttpWebRequest` возможности настройки более чем `WebClient` и в результате требуется дополнительный код для использования.

Код для синхронного простой `HttpWebRequest` операции:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

Приводится пример, в нашем [документации веб-службы](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability" />


### <a name="reachability"></a>Возможности доступа

Мобильные устройства работают в различных условиях сети быстрого Wi-Fi или 4G соединений с низкой приемных и медленными каналами данных EDGE. По этой причине рекомендуется определить, есть ли доступ к сети и если таким образом, какой тип доступна, прежде чем пытаться подключиться к удаленным серверам.

Мобильное приложение может занять в таких ситуациях входят следующие.

-  Если сеть недоступна, следует рекомендовать пользователю. Если они вручную, он был отключен (например) Режим "в самолете" или отключение Wi-Fi) они могут решить проблему.
-  Если подключение является 3G, приложения могут работать иначе (например, Apple не поддерживает приложения больше 20 МБ для загрузки более 3G). Приложения могут использовать эти сведения, чтобы предупредить пользователя о чрезмерной загрузки время при получении больших файлов.
-  Даже если есть доступ к сети, рекомендуется для проверки наличия связи с целевым сервером перед запуском других запросов. Это будет сетевых операций приложения, по истечении времени ожидания несколько раз, а также позволяют более информативные ошибка сообщение, отображаемое для пользователя.


Отсутствует [образец Xamarin.iOS](https://github.com/xamarin/monotouch-samples/tree/master/ReachabilitySample) доступных (основанный на Apple [образце достижимости кода](http://developer.apple.com/library/ios/#samplecode/Reachability/Introduction/Intro.html) ) для обнаружения доступности сети.


## <a name="webservices"></a>Веб-службы

См. в документации по [работа с веб-службами](~/cross-platform/data-cloud/web-services/index.md), который охватывает доступ к REST, конечные точки SOAP и WCF, с помощью Xamarin.iOS. Его можно Самостоятельное создание запросов веб-службы и обработки ответов, однако нет библиотеки сделать это гораздо проще, включая Azure, RestSharp и ServiceStack. Возможен даже основные операции WCF в приложениях Xamarin.

### <a name="azure"></a>Azure

Microsoft Azure — это облачная платформа, которая предоставляет широкий набор служб для мобильных приложений, включая хранение данных и синхронизации и push-уведомлений.

Посетите [azure.microsoft.com](https://azure.microsoft.com/) чтобы опробовать бесплатно.

### <a name="restsharp"></a>RestSharp

RestSharp — это библиотека .NET, которое может быть включено в мобильных приложениях для предоставления клиенту REST, который упрощает доступ к веб-служб. Она помогает, предоставляя простой API для запроса данных и проанализировать Ответ REST. Может быть полезно RestSharp

[Веб-сайт RestSharp](http://restsharp.org/) содержит [документации](https://github.com/restsharp/RestSharp/wiki) о том, как реализовать с помощью RestSharp клиента REST.
RestSharp примеры Xamarin.iOS и Xamarin.Android [github](https://github.com/restsharp/RestSharp/).

Также имеется фрагмент кода Xamarin.iOS в нашем [документации веб-службы](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack" />


### <a name="servicestack"></a>ServiceStack

В отличие от RestSharp ServiceStack — оба серверного решения для размещения веб-службы, а также клиентской библиотеки, который может быть реализован в мобильных приложениях для этих служб.

[ServiceStack веб-сайт](http://servicestack.net/) объясняется назначение проекта и ссылки на примеры документом и кодом. Примеры содержат полную реализацию серверных веб-службы, а также для различных клиентских приложений, которые к нему возможен доступ.

Отсутствует [пример Xamarin.iOS](http://www.servicestack.net/monotouch/remote-info/) на веб-сайт ServiceStack и фрагмента кода в наших [документации веб-службы](~/cross-platform/data-cloud/web-services/index.md).


### <a name="wcf"></a>WCF

Средства Xamarin поможет вам использовать некоторые службы Windows Communication Foundation (WCF). Как правило Xamarin поддерживает тот же поднабор стороны клиента WCF, который поставляется со средой выполнения Silverlight. Сюда входят наиболее распространенные кодировку и протокол реализации WCF: транспорта кодировке текста сообщения SOAP по протоколу HTTP с помощью протокола `BasicHttpBinding`.

Из-за размера и сложности платформы WCF может быть реализации текущих и будущих служб, которые будут выходят за пределы области поддерживается доменом клиента подмножества Xamarin. Кроме того поддержка WCF требует использования средств доступно только в среде Windows для создания прокси-сервера.

 <a name="Threading" />


## <a name="threading"></a>Потоки

Скорость реагирования приложения важно для мобильных приложений — пользователи ожидают, что приложения для загрузки и быстро выполнять. Экран «замораживания», который будет отображаться перестает принимать входные данные пользователя, чтобы указать, что произошло аварийное завершение приложения, поэтому очень важно не блокирует поток пользовательского интерфейса с помощью вызовов блокировки длительное время, например сетевые запросы или медленных локальных операций (например, распаковки файла). В частности процесс запуска не должно содержать длительно выполняемых задач — все платформы мобильных устройств будут kill приложение, которое занимает слишком много времени для загрузки.

Это означает, что пользовательский интерфейс должен реализовывать «индикатор выполнения» или в противном случае «используется» пользовательский Интерфейс, который занимает мало времени для отображения и асинхронные задачи для выполнения фоновой операции. Для выполнения фоновых задач необходимо использование потоков, то есть потребностей фоновой задачи способ передачи обратно в основной поток для отображения хода выполнения или если они завершены.

 <a name="Parallel_Task_Library" />


### <a name="parallel-task-library"></a>Библиотека параллельных задач

Задачи, созданные с помощью библиотеки параллельных задач можно выполнять асинхронно и возвращать их вызывающем потоке, что делает их очень удобно использовать для запуска длительных операций без блокировки пользовательского интерфейса.

Операция простых параллельных задач может выглядеть следующим образом:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

Ключ `TaskScheduler.FromCurrentSynchronizationContext()` которой будут повторно использовать SynchronizationContext.Current потока, вызывая метод (здесь основной поток, на котором выполняется `MainThreadMethod`) — это способ маршалинга обратных вызовов в этот поток. Это означает, что если метод вызывается в потоке пользовательского интерфейса, он будет выполняться `ContinueWith` операцию обратно в потоке пользовательского интерфейса.

Если код запускается задачи из других потоков, используйте следующий шаблон для создания ссылки в поток пользовательского интерфейса и задачи по-прежнему может выполнять обратный вызов к нему:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread" />


### <a name="invoking-on-the-ui-thread"></a>Вызов в потоке пользовательского интерфейса

Для кода, который не использовать библиотеку параллельных задач каждая платформа имеет свой собственный синтаксис для маршалинга операций обратно в поток пользовательского интерфейса.

-  **iOS** — `owner.BeginInvokeOnMainThread(new NSAction(action))`
-  **Android** — `owner.RunOnUiThread(action)`
-  **Xamarin.Forms** — `Device.BeginInvokeOnMainThread(action)`
-  **Windows** — `Deployment.Current.Dispatcher.BeginInvoke(action)`



IOS и Android синтаксис требует класса «контекст» доступен в том, что означает, что код должен передать этот объект в любые методы, требующие обратного вызова в потоке пользовательского интерфейса.

Чтобы сделать поток вызывает метод пользовательского интерфейса в общий код, выполните [пример IDispatchOnUIThread](http://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (от [ @follesoe ](http://jonas.follesoe.no/)). Объявите и программу, чтобы `IDispatchOnUIThread` интерфейс в общий код, а затем реализовать классы от платформы, как показано ниже:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Для использования разработчиками Xamarin.Forms [ `Device.BeginInvokeOnMainThread` ](~/xamarin-forms/platform/device.md#Device_BeginInvokeOnMainThread) в коде (общие проекты или PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation" />


## <a name="platform-and-device-capabilities-and-degradation"></a>Платформы и возможностями устройства и снижение

Дополнительно конкретные примеры задач, связанных с разными возможностями приведены в документации по возможности платформы. Он обрабатывает различные возможности и для постепенного ухудшения качества приложения для обеспечения удобства работы пользователей, даже в том случае, если приложение не может работать в свой потенциал.
