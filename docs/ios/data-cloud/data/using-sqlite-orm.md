---
title: С помощью SQLite.NET
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2018
ms.openlocfilehash: 8d68df2c29afe828482da7c5747b30dc5d30a5de
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="using-sqlitenet"></a>С помощью SQLite.NET

SQLite.NET библиотеки, в которой Xamarin рекомендует использовать является основные ORM, которая позволяет сохранять и извлекать объекты в локальной базе данных SQLite на устройстве iOS.
Объектно-реляционные Преобразователи означает реляционное сопоставление объекта — API, который позволяет сохранять и извлекать «объекты» из базы данных без написания инструкций SQL.

<a name="Usage"/>

## <a name="usage"></a>Использование

Добавить [пакет SQLite.net PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/), для проекта — он поддерживает различных платформ, включая iOS, Android и Windows.

  [![](using-sqlite-orm-images/image1a-sml.png "Пакет SQLite.NET NuGet")](using-sqlite-orm-images/image1a.png#lightbox)

При наличии доступных библиотеке SQLite.NET, выполните следующие три действия, чтобы использовать его для доступа к базе данных.


1. **Добавить с помощью инструкции** -добавьте следующий оператор для файлов C#, где требуется доступ к данным:

        using SQLite;

1. **Создайте пустую базу данных** -ссылку на базу данных можно создать, передав конструктору класса SQLiteConnection путь к файлу. Необходимо проверить, если файл уже существует, он будет автоматически создан при необходимости, в противном случае существующий файл базы данных будет открыт.

        var db = new SQLiteConnection (dbPath);

    Переменная dbPath, должно определяться в соответствии с правилами, описанных ранее в этом документе.

1. **Сохранить данные** — после создания объекта SQLiteConnection, базы данных, команды выполняются путем вызова его методов, например CreateTable и Insert следующим образом:

        db.CreateTable<Stock> ();
        db.Insert (newStock); // after creating the newStock object

1. **Получить данные** — для извлечения объекта (или список объектов), используйте следующий синтаксис:

        var stock = db.Get<Stock>(5); // primary key id of 5
        var stockList = db.Table<Stock>();

## <a name="basic-data-access-sample"></a>Пример доступа основных данных

*DataAccess_Basic* образцы кода для этого документа выглядит следующим образом, при работе на iOS. Код показывает, как выполнять простые операции SQLite.NET и показывает результаты в виде текста в главном окне приложения.

**iOS**

 ![](using-sqlite-orm-images/image2.png "Образец SQLite.NET iOS")

В следующем образце кода показано взаимодействие всей базы данных с помощью библиотеки SQLite.NET для инкапсуляции базового доступа к базе данных. Оно отображается:

1.  Создание файла базы данных
1.  Вставка данных путем создания объектов и сохранять их
1.  Запрос данных


Необходимо иметь для включения этих пространств имен:

```csharp
using SQLite; // from the github SQLite.cs class
```

Это требуется, SQLite были добавлены в проект, как видно [здесь](#Usage). Обратите внимание, что таблицы базы данных SQLite определен, добавляя атрибуты к классу ( `Stock` класс) вместо команды CREATE TABLE.

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

-  **[PrimaryKey]**  — Этот атрибут может применяться к целочисленное свойство принудительного первичный ключ базовой таблицы. Составных первичных ключей не поддерживаются.
-  **[AutoIncrement]**  — Этот атрибут вызовет значение является целочисленным свойством с автоматическим приращением для каждого нового объекта, вставляемого в базы данных
-  **[Column(name)]**  — Предоставление необязательный `name` переопределит значение по умолчанию имя базового столбца базы данных (который является таким же, как свойство).
-  **[Table(name)]**  — Отмечает класс как хранящиеся в базовой таблице SQLite. Указание необязательное имя параметра переопределит значение по умолчанию имя базовой таблицы базы данных (это то же, что имя класса).
-  **[MaxLength(value)]**  — Ограничения длины свойство текста, при попытке вставки баз данных. Код должен проверить это перед вставкой объекта, как этот атрибут «проверяется только» при вставке базы данных или попытка выполнить операцию обновления.
-  **[Пропустить]**  — SQLite.NET приводит к тому, чтобы не учитывать это свойство. Это особенно полезно для свойств, которые имеют тип, который не может храниться в базе данных или свойства, что модели коллекции, которые не удается разрешить автоматически быть SQLite.
-  **[Unique]**  — Гарантирует уникальность значений в столбец основной базы данных.


Большая часть эти атрибуты являются необязательными, SQLite будет использовать значения по умолчанию для имен таблиц и столбцов. Всегда следует указывать целочисленного первичного ключа, чтобы выбор и удаление запросов может выполняться эффективно на данные.

## <a name="more-complex-queries"></a>Более сложные запросы

Следующие методы `SQLiteConnection` может использоваться для выполнения других операций с данными:

-  **Вставить** — добавляет новый объект в базу данных.
-  **Получить<T>**  — пытается получить объект, с помощью первичного ключа.
-  **Таблица<T>**  — возвращает все объекты в таблице.
-  **Удалить** — удаляет объект с помощью его первичного ключа.
-  **Запрос<T>**  -выполнить SQL-запрос, возвращающий несколько строк (как объекты).
-  **Выполнение** — используйте этот метод (и не `Query` ) при строк назад от SQL (например, инструкции INSERT, UPDATE и DELETE) не нужна.


### <a name="getting-an-object-by-the-primary-key"></a>Получение объекта с первичным ключом

SQLite.Net предоставляет метод Get для извлечения объекта на основе его первичного ключа.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>При выборе объекта с помощью Linq

Методы, которые возвращают коллекции поддерживают IEnumerable<T> поэтому Linq можно использовать для запроса или Сортировать содержимое таблицы. В следующем коде показано использование Linq для фильтрации всех записей, которые начинаются с буквы «A» пример:

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

> [!IMPORTANT]
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


## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [операций ввода-вывода данных рецепты](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
