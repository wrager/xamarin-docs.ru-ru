---
title: "С помощью данных в приложении"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2CB8150E-CD2C-4E97-8605-1EE8CBACFEEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: aefec1992a5f061f9d50d499b3594601a2651b17
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="using-data-in-an-app"></a>С помощью данных в приложении

**DataAccess_Adv** приведен пример рабочее приложение, которое допускает ввод данных пользователем и *CRUD* функциональность базы данных (Создание, чтение, обновление и удаление). Приложение состоит из двух экранах: список и форму для ввода данных. Все код доступа к данным многократно используемые в iOS и Android без изменений.

После добавления данных приложение экраны, iOS выглядеть следующим образом:

 ![](using-data-in-an-app-images/image9.png "Список примеров операций ввода-вывода")

 ![](using-data-in-an-app-images/image10.png "Детализация iOS")

IOS проекта ниже показан — код, приведенные в этом разделе содержится в **Orm** каталога:

 ![](using-data-in-an-app-images/image13.png "Дерево проектов iOS")

Машинный код пользовательского интерфейса для ViewControllers в iOS находится вне области рассмотрения этого документа.
Ссылаться на [iOS работа с таблицами и ячейками](~/ios/user-interface/controls/tables/index.md) Дополнительные сведения об элементах управления пользовательского интерфейса.

## <a name="read"></a>Чтение

Существует несколько операций чтения в образце:

-  При чтении списка
-  Чтение отдельных записей


Два метода в `StockDatabase` класса:

```csharp
public IEnumerable<Stock> GetStocks ()
{
    lock (locker) {
        return (from i in Table<Stock> () select i).ToList ();
    }
}
public Stock GetStock (int id)
{
    lock (locker) {
        return Table<Stock>().FirstOrDefault(x => x.Id == id);
    }
}
```

iOS интерпретирует данные по-разному как `UITableView`.

## <a name="create-and-update"></a>Создание и обновление

Чтобы упростить код приложения, одним методом save — предоставленный, который выполняет инструкции Insert или Update, в зависимости от того, была ли установлена PrimaryKey. Поскольку `Id` свойство помечено `[PrimaryKey]` атрибута, его не следует задавать в коде.
Этот метод проверяет ли это значение было предыдущего сохранены (проверив свойство первичного ключа) и вставить или обновить объект соответствующим образом:

```csharp
public int SaveStock (Stock item)
{
    lock (locker) {
        if (item.Id != 0) {
            Update (item);
            return item.Id;
    } else {
            return Insert (item);
        }
    }
}
```



Некоторые проверки (например, обязательные поля, минимальная длина или другие бизнес-правила) обычно требуют реальных приложений.
Хороший кросс платформенных приложений реализовать столько проверки логической максимально общего кода, передавая ошибки проверки, создать резервную копию пользовательский Интерфейс для отображения в соответствии с возможностями платформы.

## <a name="delete"></a>Удаление

В отличие от `Insert` и `Update` методы, `Delete<T>` метод может принимать только значения первичного ключа, а не полный `Stock` объекта.
В этом примере `Stock` объект передается в метод, но только свойство идентификатора передается в `Delete<T>` метод.

```csharp
public int DeleteStock(Stock stock)
{
    lock (locker) {
        return Delete<Stock> (stock.Id);
    }
}
```

## <a name="using-a-pre-populated-sqlite-database-file"></a>С помощью предварительно заполненных SQLite файла базы данных

Некоторые приложения поставляются вместе с базой данных, уже заполненной данными.
Вы можете легко этого в мобильное приложение доставки существующий файл базы данных SQLite вместе с приложением и скопировать его для записи каталога перед доступом к нему. Поскольку SQLite стандартные формате, который используется на многих платформах, существует ряд средств, доступных для создания файла базы данных SQLite.

-  **Расширения диспетчера SQLite Firefox** — работает на Mac и Windows и создает файлы, совместимые с iOS и Android.
-  **Команда** — в разделе [www.sqlite.org/sqlite.html](http://www.sqlite.org/sqlite.html) .


При создании файла базы данных для распространения вместе с приложением, будьте внимательны с именования таблиц и столбцов, чтобы они соответствуют ожиданиям кода, особенно в том случае, если вы используете SQLite.NET, который будет ожидать, что имена для сопоставления C# классы и свойства (или связанные пользовательские атрибуты).

Для iOS, включите файл sqlite в приложении и убедитесь, он помечен с помощью **действие при построении: содержимого**. Поместите код в `FinishedLaunching` для копирования файла в каталог, доступный для записи *перед* вызывать методы данных. Следующий код будет копировать базы данных называется **data.sqlite**, только в том случае, если он еще не существует.

```csharp
// Copy the database across (if it doesn't exist)
var appdir = NSBundle.MainBundle.ResourcePath;
var seedFile = Path.Combine (appdir, "data.sqlite");
if (!File.Exists (Database.DatabaseFilePath))
{
  File.Copy (seedFile, Database.DatabaseFilePath);
}
```

Любой код доступа к данным (ли ADO.NET или с помощью SQLite.NET), которое выполняется после завершения будет иметь доступ к данным, предварительно заполненных это.


## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [операций ввода-вывода данных рецепты](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
