---
title: "С помощью SQLite.NET"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: b9523d76c04dae97b74744fbe2bd6bc7022c3194
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="using-sqlitenet"></a>С помощью SQLite.NET

SQLite.NET библиотеки, в которой Xamarin рекомендует использовать является очень простым ORM, который позволяет легко хранить и извлекать объекты в локальной базе данных SQLite на устройстве с Android. Объектно-реляционные Преобразователи расшифровывается реляционное сопоставление объекта &ndash; API, который позволяет сохранять и извлекать «объекты» из базы данных без написания инструкций SQL.

## <a name="using-sqlitenet"></a>С помощью SQLite.NET

Для включения библиотеки SQLite.NET в приложение Xamarin [пакет SQLite.net PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) в проект с помощью **SQLite net PCL** пакет NuGet:

[![Пакет SQLite.NET NuGet](using-sqlite-orm-images/image1a-sml.png "пакет SQLite.NET NuGet")](using-sqlite-orm-images/image1a.png#lightbox)

При наличии доступных библиотеке SQLite.NET, выполните следующие три действия, чтобы использовать его для доступа к базе данных.


1.  **Добавить с помощью инструкции** &ndash; добавьте следующий оператор для файлов C#, где требуется доступ к данным: 

    ```csharp
    using SQLite;
    ```

2.  **Создайте пустую базу данных** &ndash; можно создать ссылку на базу данных, передав конструктору класса SQLiteConnection путь к файлу. Необходимо проверить, существует ли файл &ndash; он будет создан автоматически при необходимости, в противном случае открывается существующий файл базы данных. `dbPath` Переменной должно определяться в соответствии с правилами, описанных ранее в этом документе:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Сохранить данные** &ndash; выполняются после создания объекта SQLiteConnection команд базы данных, вызвав его методы, например CreateTable и Insert следующим образом:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Получить данные** &ndash; для извлечения объекта (или список объектов) используйте следующий синтаксис:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Пример доступа основных данных

*DataAccess_Basic* образцы кода для этого документа выглядит следующим образом, при работе в Android. Код показывает, как выполнять простые операции SQLite.NET и показывает результаты в виде текста в главном окне приложения.


**Android**

![Пример Android SQLite.NET](using-sqlite-orm-images/image3.png "Android SQLite.NET образца")

В следующем образце кода показано взаимодействие всей базы данных с помощью библиотеки SQLite.NET для инкапсуляции базового доступа к базе данных.
Оно отображается:

1.  Создание файла базы данных

2.  Вставка данных путем создания объектов и сохранять их

3.  Запрос данных

Необходимо иметь для включения этих пространств имен:

```csharp
using SQLite; // from the github SQLite.cs class
```

Последнее из них требуется, SQLite были добавлены в проект. Обратите внимание, что таблицы базы данных SQLite определен, добавляя атрибуты к классу ( `Stock` класс) вместо команды CREATE TABLE.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

С помощью `[Table]` атрибута без указания параметра имя таблицы приведет к базовой таблицы базы данных с тем же именем, что и класс (в данном случае «Stock»). Имя существующей таблицы важно, если написать запросы SQL непосредственно в базе данных, а не использовать методов доступа к данным ORM. Аналогичным образом `[Column("_id")]` атрибут является необязательным и если отсутствует столбец добавляется к таблице с тем же именем, как свойство в классе.

## <a name="sqlite-attributes"></a>SQLite атрибуты

Перечислены общие атрибуты, которые можно применять к классам для управления, как они хранятся в базе данных.

-   **[PrimaryKey]**  &ndash; Этот атрибут может применяться к целочисленное свойство принудительного первичный ключ базовой таблицы. Составных первичных ключей не поддерживаются.

-   **[AutoIncrement]**  &ndash; Этот атрибут вызовет значение является целочисленным свойством с автоматическим приращением для каждого нового объекта, вставляемого в базы данных

-   **[Column(name)]**  &ndash; Указания необязательный `name` переопределит значение по умолчанию имя базового столбца базы данных (который является таким же, как свойство).

-   **[Table(name)]**  &ndash; Помечает класс как хранящиеся в базовой таблице SQLite. Указание необязательное имя параметра переопределит значение по умолчанию имя базовой таблицы базы данных (это то же, что имя класса).

-   **[MaxLength(value)]**  &ndash; Ограничения длины свойство текста, при попытке вставки баз данных. Код должен проверить это перед вставкой объекта, как этот атрибут «проверяется только» при вставке базы данных или попытка выполнить операцию обновления.

-   **[Пропустить]**  &ndash; SQLite.NET приводит к тому, чтобы не учитывать это свойство.
    Это особенно полезно для свойств, которые имеют тип, который не может храниться в базе данных или свойства, что модели коллекции, которые не удается разрешить автоматически быть SQLite.

-   **[Unique]**  &ndash; Гарантирует уникальность значений в столбец основной базы данных.


Большая часть эти атрибуты являются необязательными, SQLite будет использовать значения по умолчанию для имен таблиц и столбцов. Всегда следует указывать целочисленного первичного ключа, чтобы выбор и удаление запросов может выполняться эффективно на данные.

## <a name="more-complex-queries"></a>Более сложные запросы

Следующие методы `SQLiteConnection` может использоваться для выполнения других операций с данными:

-   **Вставить** &ndash; добавляет новый объект в базу данных.

-   **Получить&lt;T&gt;**  &ndash; пытается получить объект, с помощью первичного ключа.

-   **Таблица&lt;T&gt;**  &ndash; возвращает все объекты в таблице.

-   **Удалить** &ndash; удаляет объект с помощью его первичного ключа.

-   **Запрос&lt;T&gt;**  &ndash; выполнить SQL-запрос, возвращающий несколько строк (как объекты).

-   **Выполнение** &ndash; используйте этот метод (и не `Query`) при строк назад от SQL (например, инструкции INSERT, UPDATE и DELETE) не нужна.


### <a name="getting-an-object-by-the-primary-key"></a>Получение объекта с первичным ключом

SQLite.Net предоставляет метод Get для извлечения объекта на основе его первичного ключа.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>При выборе объекта с помощью Linq

Поддерживает методы, которые возвращают коллекции `IEnumerable<T>` поэтому Linq можно использовать для запроса или Сортировать содержимое таблицы. В следующем коде показано использование Linq для фильтрации всех записей, которые начинаются с буквы «A» пример:

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>При выборе объекта с помощью SQL

Несмотря на то, что SQLite.Net позволяют объектно ориентированного осуществлять доступ к данным, иногда может потребоваться выполнить более сложный запрос, чем позволяет Linq (или может потребоваться более высокую производительность). С помощью метода запроса, можно использовать команды SQL, как показано ниже.

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!NOTE]
> При написании инструкций SQL непосредственно зависимость создается на именах таблиц и столбцов в базе данных, которые были созданы из классов и их атрибуты. Изменить эти имена в коде необходимо обновить все написанному вручную инструкции SQL.

### <a name="deleting-an-object"></a>Удаление объекта

Первичный ключ используется для удаления строки, как показано ниже:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Вы можете проверить `rowcount` для подтверждения, количество строк, затронутых (в этом случае удаляются).

## <a name="using-sqlitenet-with-multiple-threads"></a>С помощью SQLite.NET с несколькими потоками

SQLite поддерживает три различных режима потоков: *одним потоком*, *многопоточную*, и *сериализованные*. Если вы хотите получить доступ к базе данных из нескольких потоков без каких-либо ограничений, можно настроить SQLite для использования **сериализованные** threading режиме. Очень важно для режима в начале приложения (например, в начале `OnCreate` метода).

Чтобы изменить режим потоков, вызовите `SqliteConnection.SetConfig`. Например, следующая строка кода настраивает SQLite для **сериализованные** режим: 

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```

Android версии SQLite имеет ограничение, требуется еще несколько шагов. Если вызов `SqliteConnection.SetConfig` создает исключение SQLite, такие как `library used incorrectly`, необходимо использовать следующий обходной путь: 

1.  Ссылка на собственный **libsqlite.so** библиотеки, чтобы `sqlite3_shutdown` и `sqlite3_initialize` API-интерфейсы, становятся доступными для приложения:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  В самом начале `OnCreate` метод, добавьте следующий код для завершения работы SQLite, настройте его для **сериализованные** режиме и повторная инициализация SQLite:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Этот способ также работает для `Mono.Data.Sqlite` библиотеки. Дополнительные сведения о SQLite и многопоточность см. в разделе [SQLite и несколько потоков](https://www.sqlite.org/threadsafe.html). 



## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Рецепты данных для Android](https://developer.xamarin.com/recipes/android/data/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
