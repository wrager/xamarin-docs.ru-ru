---
title: С помощью ADO.NET с помощью Xamarin.iOS
description: В этом документе описывается использование ADO.NET как метод для доступа к SQLite приложения Xamarin.iOS. Он описывает, ссылки на сборки, Mono.Data.Sqlite и BasicDataAccess образца.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8240e3052b4deb4bfdf0ec94e67fbd6827a34dab
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784833"
---
# <a name="using-adonet-with-xamarinios"></a>С помощью ADO.NET с помощью Xamarin.iOS

Xamarin имеет встроенную поддержку базу данных SQLite, которая доступна на iOS, предоставляемых через знакомый синтаксис ADO.NET. С помощью этих API требует написания инструкций SQL, которые обрабатываются SQLite, такие как `CREATE TABLE`, `INSERT` и `SELECT` инструкции.

## <a name="assembly-references"></a>Ссылки на сборки

Использование доступа SQLite через ADO.NET, необходимо добавить `System.Data` и `Mono.Data.Sqlite` ссылки на проект iOS, как показано ниже (для выборки в Visual Studio для Mac и Visual Studio):

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

 ![](using-adonet-images/image4.png "Ссылки на сборки в Visual Studio для Mac")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Ссылки на сборки в Visual Studio")

-----

Щелкните правой кнопкой мыши **References > Изменить ссылки...**  а затем установите необходимые сборки.

## <a name="about-monodatasqlite"></a>О Mono.Data.Sqlite

Мы будем использовать `Mono.Data.Sqlite.SqliteConnection` класс, чтобы создать пустую базу данных и затем для создания экземпляра `SqliteCommand` объектов, мы можно использовать для выполнения инструкций SQL в базе данных.


1. **Создание пустой базы данных** -вызова `CreateFile` метод с недопустимым (т. е. для записи) путь к файлу. Стоит ли перед вызовом этого метода уже существует файл, иначе будут созданы новые базы данных (пусто) поверх старого и данные в существующем файле будут потеряны:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Переменной должно определяться в соответствии с правилами, описанных ранее в этом документе.

2. **Создание подключения к базе данных** — после создания файла базы данных SQLite можно создать объект подключения для доступа к данным. Подключение создается со строкой соединения, которая принимает форму `Data Source=file_path`, как показано ниже:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Как упоминалось ранее, соединение никогда не следует использовать повторно в разных потоках. Если вы сомневаетесь, создания соединения при необходимости и закрыть его, после завершения; Однако следует учитывать это более часто требуется слишком.
    
3. **Создание и выполнение команды базы данных** — после подключения, можно выполнить произвольные команды SQL для нее. В следующем примере кода показано, при выполнении инструкции CREATE TABLE.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

При выполнении SQL непосредственно с базой данных, необходимо выполнить обычные меры предосторожности не делать недопустимых запросов, например Попытка создать таблицу, которая уже существует. Отслеживайте от структуры базы данных, чтобы не вызвать SqliteException, такие как «SQLite ошибки [элементы] уже существует таблица».

## <a name="basic-data-access"></a>Доступ к данным, основные

*DataAccess_Basic* образцы кода для этого документа выглядит следующим образом, при работе на iOS:

 ![](using-adonet-images/image9.png "Образец ADO.NET iOS")

В следующем примере кода показано, как выполнять простые операции SQLite и показывает результаты в виде текста в главном окне приложения.

Необходимо иметь для включения этих пространств имен:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

В следующем образце кода показано взаимодействие всей базы данных.

1.  Создание файла базы данных
2.  Вставка данных
3.  Запрос данных

Эти операции обычно отобразятся в нескольких местах по всему коду, например можно создать файл базы данных и таблиц, при первом запуске приложения и выполнять операции чтения и записи в отдельных экранов в приложении. В следующем примере были сгруппированы в один метод в этом примере:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>Более сложные запросы

Поскольку SQLite допускает произвольные команды SQL, которую следует выполнить для данных, можно выполнять независимо от СОЗДАНИЯ, вставки, обновления, удаления или ВЫБЕРИТЕ инструкции, как вам удобно. Вы найдете сведения о поддерживаемых SQLite на веб-сайте Sqlite команды SQL. Инструкции SQL выполняются с помощью одного из трех методов объекта SqliteCommand:

-  **ExecuteNonQuery** — обычно используется для создания или данных вставить таблицы. Для некоторых операций возвращается число обработанных строк, в противном случае — значение -1.
-  **ExecuteReader** — используется при коллекцию строк, будут возвращены в качестве `SqlDataReader` .
-  **ExecuteScalar** — возвращает одно значение (например агрегатную функцию).


### <a name="executenonquery"></a>EXECUTENONQUERY

Инструкции INSERT, UPDATE и DELETE возвращает количество затронутых строк. Все инструкции SQL возвращает -1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

В следующем методе показано предложение WHERE в инструкции SELECT. Так как код, дополнительно полную инструкцию SQL его должны уделить особое внимание escape-зарезервированные символы, такие как кавычки (') вокруг строки.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

Метод ExecuteReader возвращает объект SqliteDataReader. Помимо метода Read, показано в примере других полезных свойств, включают:

-  **RowsAffected** — количество строк, затронутых запросом.
-  **HasRows** — ли все строки были возвращены.


### <a name="executescalar"></a>EXECUTESCALAR

Это используется для инструкции SELECT, которые возвращают одиночное значение (например, статистическое).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Имеет возвращаемый тип метода `object` – следует приведения результата в зависимости от запроса к базе данных. Результат может быть целое число от запроса число или строку из одного столбца запроса SELECT. Обратите внимание, что это отличается в другие методы Execute, возвращающие объект считывания или показатель числа обработанных строк.


## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [операций ввода-вывода данных рецепты](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
