---
title: "Локальные базы данных"
description: "Xamarin.Forms поддерживает приложения на основе базы данных, используя компонент database engine SQLite, что делает возможным для загрузки и сохранения объектов в общем коде. В этой статье описывается, как Xamarin.Forms приложения могут читать и записывать данные к локальной базе данных SQLite, с помощью SQLite.Net."
ms.topic: article
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2017
ms.openlocfilehash: 29686b29a18fe409a1f778d54266cbeedea40eda
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="local-databases"></a>Локальные базы данных

_Xamarin.Forms поддерживает приложения на основе базы данных, используя компонент database engine SQLite, что делает возможным для загрузки и сохранения объектов в общем коде. В этой статье описывается, как Xamarin.Forms приложения могут читать и записывать данные к локальной базе данных SQLite, с помощью SQLite.Net._

## <a name="overview"></a>Обзор

Xamarin.Forms приложения могут использовать [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) пакета для включения операций базы данных в общий код, ссылаясь на `SQLite` классов, поставляемых в NuGet. Операции с базой данных можно определить в проекте переносимой библиотеки классов (PCL) решения Xamarin.Forms с проекты под конкретные платформы, возвращая путь для хранения базы данных.

Сопутствующий [образец приложения](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) это простое приложение Todo list. Следующих снимках экрана показано, как образец отображается на каждой платформе.

[![Снимки экрана примера базы данных Xamarin.Forms](databases-images/todo-list-sml.png "TodoList первой страницы, снимки экрана")](databases-images/todo-list.png#lightbox "TodoList первой страницы, снимки экрана") [ ![ Снимки экрана примера базы данных Xamarin.Forms](databases-images/todo-list-sml.png "TodoList первой страницы, снимки экрана")](databases-images/todo-list.png#lightbox "TodoList первой страницы, снимки экрана")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>С помощью SQLite

В этом разделе показано, как добавить пакеты SQLite.Net NuGet решение Xamarin.Forms, написать методы для выполнения операций с базой данных и использовать [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) для определения местоположения для хранения базы данных для каждой платформы.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-pcl-project"></a>Проект Xamarins.Forms PCL

Чтобы добавить поддержку SQLite в проект Xamarin.Forms PCL, с помощью функции поиска NuGet найти **sqlite-net-pcl** и установить пакет:

![](databases-images/vs2017-sqlite-pcl-nuget.png "Добавьте пакет NuGet SQLite.NET PCL")

Существует несколько пакетов NuGet с одинаковыми именами, правильный пакет имеет следующие атрибуты:

- **Созданные:** Krueger Михаил A.
- **Id:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

После добавления ссылки писать интерфейс абстрактными функции специфический для платформы, которые необходимо определить расположение файла базы данных. Интерфейс, используемый в данном примере определяет единственный метод:

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

После определения интерфейса используйте [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) получить реализацию и получить путь к локальному файлу (Обратите внимание, что этот интерфейс еще не реализована). Следующий код возвращает реализацию в `App.Database` свойство:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Конструктор показан ниже:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Этот подход создает подключение одной базы данных, сохраняется открытым во время выполнения приложения, таким образом избежать затрат на открытие и закрытие файла базы данных при каждом выполнении операции базы данных.

В оставшейся части `TodoItemDatabase` класс содержит SQLite запросы, которые выполняются между различными платформами. Ниже приведен пример запроса (Дополнительные сведения о синтаксисе можно найти в [с помощью SQLite.NET](~/cross-platform/app-fundamentals/index.md) статьи):

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Преимуществом использования асинхронный интерфейс API SQLite.Net является базы данных операций, перемещаются в фоновых потоках. Кроме того нет необходимости для записи дополнительных параллелизма, код обработки, так как он обеспечивает API-интерфейса.

Все записи кода доступа к данным в проект PCL для общего для всех платформ. Только начало путь к локальному файлу базы данных требует кода под конкретную платформу, как описано в следующих разделах.

<a name="PCL_iOS" />

### <a name="ios-project"></a>Проект iOS

Чтобы настроить приложение iOS, добавьте один и тот же пакет NuGet для проекта iOS с помощью *NuGet* окна:

![](databases-images/vsmac-sqlite-nuget.png "Добавьте пакет NuGet SQLite.NET PCL")

— Только код, необходимый `IFileHelper` реализацию, которая определяет путь к файлу данных. Следующий код помещает файл базы данных SQLite **библиотеки или баз данных** папки в изолированной среде приложения. В разделе [iOS работа с файловой системой](~/ios/app-fundamentals/file-system.md) Дополнительные сведения в разных каталогах, которые доступны для хранилища.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

            if (!Directory.Exists(libFolder))
            {
                Directory.CreateDirectory(libFolder);
            }

            return Path.Combine(libFolder, filename);
        }
    }
}
```

Обратите внимание, что код включает `assembly:Dependency` таким образом, эта реализация обнаружить `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Проект Android

Чтобы настроить приложение Android, добавьте один и тот же пакет NuGet для проекта Android с помощью *NuGet* окна:

![](databases-images/vsmac-sqlite-nuget.png "Добавьте пакет NuGet SQLite.NET PCL")

После добавления этой ссылки — только код, необходимый `IFileHelper` реализацию, которая определяет путь к файлу данных.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            return Path.Combine(path, filename);
        }
    }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP) для Windows 10

Чтобы настроить приложения UWP, добавьте один и тот же пакет NuGet для проекта UWP с помощью *NuGet* окна:

![](databases-images/vs2017-sqlite-uwp-nuget.png "Добавьте пакет NuGet SQLite.NET PCL")

После добавления ссылки реализовать `IFileHelper` интерфейса с помощью платформы `Windows.Storage` API, чтобы определить путь к файлу данных.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
        }
    }
}

```

## <a name="summary"></a>Сводка

Xamarin.Forms поддерживает приложения на основе базы данных, используя компонент database engine SQLite, что делает возможным для загрузки и сохранения объектов в общем коде.

В этой статье основное внимание уделено **доступ к** в базу данных SQLite, с помощью Xamarin.Forms. Дополнительные сведения о работе с самой SQLite.Net посвящены [доступа к данным: с помощью SQLite.NET](~/cross-platform/app-fundamentals/index.md) документации. Большая часть кода SQLite.Net общий для всех платформ; Простая настройка расположение файла базы данных SQLite требуется функциональность конкретную платформу.


## <a name="related-links"></a>Связанные ссылки

- [Образец TODO](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [База данных книги](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
