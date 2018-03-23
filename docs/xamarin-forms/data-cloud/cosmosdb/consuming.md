---
title: Использование базы данных служб Azure Cosmos DB документа
description: База данных документа Azure Cosmos DB является NoSQL базы данных, которая предоставляет высокоскоростной доступ к документам JSON, предоставляющие доступ к службе быстрое высокодоступные, масштабируемые базы данных для приложений, требующих удобный масштабирования и глобальные репликации. В этой статье описывается использование клиентской библиотеке Azure Cosmos DB .NET Standard интегрировать базе данных Azure Cosmos DB документа в приложении Xamarin.Forms.
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: e2fa5ae12531069e1ad1bc19e110e4dcffe23a02
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2018
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Использование базы данных служб Azure Cosmos DB документа

_База данных документа Azure Cosmos DB является NoSQL базы данных, которая предоставляет высокоскоростной доступ к документам JSON, предоставляющие доступ к службе быстрое высокодоступные, масштабируемые базы данных для приложений, требующих удобный масштабирования и глобальные репликации. В этой статье описывается использование клиентской библиотеке Azure Cosmos DB .NET Standard интегрировать базе данных Azure Cosmos DB документа в приложении Xamarin.Forms._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB, по [университета Xamarin](https://university.xamarin.com/)**

Учетная запись базы данных Azure Cosmos DB документа может предоставляться с помощью подписки Azure. Каждая учетная запись базы данных может иметь ноль или несколько баз данных. Документ базы данных в базу данных Cosmos Azure — это логический контейнер для коллекциями документов и пользователей.

База данных документа Azure Cosmos DB может содержать ноль или более коллекциями документов. Каждая коллекция документов может иметь на разных уровнях производительности, позволяя более высокая пропускная способность задается для часто используемых коллекций и меньше пропускной способности для редко используемых коллекций.

Каждая коллекция документ состоит из ноль или несколько документов JSON. Документы в коллекции без схемы и поэтому нет необходимости используют одинаковую структуру или поля. Когда документы добавляются в коллекцию документов, Cosmos DB автоматически индексирует их и они становятся доступными для запросов.

В целях разработки базы данных документа также может быть востребован эмулятор. С помощью эмулятора, приложения можно разработаны и протестированы локально, без создания подписки Azure и расходов на любой. Дополнительные сведения об эмуляторе см. в разделе [разработки локально в эмуляторе Azure Cosmos DB](/azure/cosmos-db/local-emulator/).

В этой статье, а также пример приложения, сопровождающие демонстрирует приложения списка задач, где задачи хранятся в базе данных Azure Cosmos DB документа. Дополнительные сведения о примере приложения см. в разделе [понимание образца](~/xamarin-forms/data-cloud/walkthrough.md).

Дополнительные сведения о базе данных Azure Cosmos см. в разделе [документации DB Cosmos Azure](/azure/cosmos-db/).

## <a name="setup"></a>Установка

Интеграция базы данных служб Azure Cosmos DB документа в приложении Xamarin.Forms выполняется следующим образом:

1. Создайте учетную запись Cosmos DB. Дополнительные сведения см. в разделе [создать учетную запись Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Добавить [Azure Cosmos DB .NET Standard клиентская библиотека](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) пакет NuGet для платформы проектов в решение Xamarin.Forms.
1. Добавить `using` директивы для `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, и `Microsoft.Azure.Documents.Linq` пространства имен, классы, которые получат доступ к учетной записи Cosmos DB.

После выполнения этих действий, клиентской библиотеке Azure Cosmos DB .NET Standard можно использовать для настройки и выполнения запросов к базе данных документа.

> [!NOTE]
> Клиентская библиотека Azure Cosmos DB .NET Standard можно установить только в проектах платформы, а не в проект переносимой библиотеки классов (PCL). Таким образом приложение представляет общий доступ проекта (SAP) для исключения повтора кода. Тем не менее `DependencyService` класс может использоваться в проект переносимой библиотеки Классов для вызова Azure Cosmos DB .NET Standard кода библиотеки клиента, содержащихся в проекты под конкретные платформы.

## <a name="consuming-the-azure-cosmos-db-account"></a>Использование учетной записи Azure Cosmos DB

`DocumentClient` Тип инкапсулирует конечной точки, учетные данные и политики подключение для доступа к учетной записи Azure Cosmos DB и используется для настройки и выполнения запросов к учетной записи. В следующем примере кода показано, как создавать экземпляры этого класса:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Cosmos DB Uri и первичного ключа должно быть представлено в `DocumentClient` конструктора. Их можно получить на портале Azure. Дополнительные сведения см. в разделе [подключиться к учетной записи Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Создание базы данных

Документ базы данных — это логический контейнер для коллекциями документов и пользователей и может быть создан в портале Azure или программным путем, используя `DocumentClient.CreateDatabaseIfNotExistsAsync` метод:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync` Указывает метод `Database` объекта в качестве аргумента, с `Database` указывать имя базы данных в виде объекта его `Id` свойство. `CreateDatabaseIfNotExistsAsync` Метод создает базу данных, если она не существует, или возвращает базу данных, если он уже существует. Тем не менее, образец приложения игнорирует все данные, возвращенные `CreateDatabaseIfNotExistsAsync` метод.

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync` Возвращает метод `Task<ResourceResponse<Database>>` объекта и код состояния ответа может проверяться для определения базы данных был создан, или существующую базу данных был возвращен.

### <a name="creating-a-document-collection"></a>Создание коллекции документов

Коллекция документов — это контейнер для документов JSON, а также может быть создан в портале Azure или программным путем, используя `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` метод:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync` Метод требует принудительного два аргумента — имя базы данных, согласно `Uri`и `DocumentCollection` объекта. `DocumentCollection` Представляет коллекцию документа с заданным именем `Id` свойство. `CreateDocumentCollectionIfNotExistsAsync` Метод создает коллекцию документов, если она не существует, или возвращает коллекцию документов, если он уже существует. Тем не менее, образец приложения игнорирует все данные, возвращенные `CreateDocumentCollectionIfNotExistsAsync` метод.

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync` Возвращает метод `Task<ResourceResponse<DocumentCollection>>` объекта и код состояния ответа может проверяться для определения коллекции документов был создан, или существующую коллекцию документ был возвращен.

При необходимости `CreateDocumentCollectionIfNotExistsAsync` также может указать метод `RequestOptions` объекта, который инкапсулирует параметры, которые могут быть указаны для запросов, выданных Cosmos DB учетной записи. `RequestOptions.OfferThroughput` Свойство используется для определения уровня производительности коллекции документов и в образце приложения, равно 400 единиц запросов в секунду. Это значение должно быть увеличивается или уменьшается в зависимости от того, является ли коллекция часто и редко осуществляется.

> [!IMPORTANT]
> Обратите внимание, что `CreateDocumentCollectionIfNotExistsAsync` метод создает новую коллекцию с зарезервированной пропускной способностью, имеющая цены последствия.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Получение документа коллекции документов

Содержимое коллекции документов можно получить, создав и выполнив запрос к документу. Запрос документа создается с `DocumentClient.CreateDocumentQuery` метод:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Этот запрос асинхронно извлекает все документы из указанной коллекции куда помещается в документах `List<TodoItem>` коллекции для отображения.

`CreateDocumentQuery<T>` Указывает метод `Uri` аргумент, который представляет коллекцию, необходимо запросить для документов. В этом примере `collectionLink` переменная является полем уровня класса, которое указывает `Uri` , представляющий коллекцию документов, для извлечения документов из:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>` Метод создает запрос, который выполняется синхронно и возвращает `IQueryable<T>` объекта. Тем не менее `AsDocumentQuery` метод преобразует `IQueryable<T>` объект `IDocumentQuery<T>` объект, который может выполняться асинхронно. Асинхронный запрос выполняется с `IDocumentQuery<T>.ExecuteNextAsync` метод, который возвращает следующую страницу результатов из базы данных документа, с `IDocumentQuery<T>.HasMoreResults` свойство, указывающее, есть ли дополнительные результаты, возвращаемые из запроса.

Документы могут быть отфильтрованы стороне сервера, включая `Where` предложение в запросе, который применяет предикат фильтрации в запросе к коллекции документов:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Этот запрос извлекает все документы из коллекции, `Done` свойства равен `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Вставка документа в коллекции документов

Документы содержимое JSON в определяемых пользователем и могут быть вставлены в коллекции документов с `DocumentClient.CreateDocumentAsync` метод:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync` Указывает метод `Uri` аргумент, который представляет документ должен быть вставлен, коллекции и `object` аргумент, представляющий документ, который нужно вставить.

### <a name="replacing-a-document-in-a-document-collection"></a>Замена документа в коллекции документов

Можно заменить документы в коллекции документов с `DocumentClient.ReplaceDocumentAsync` метод:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync` Указывает метод `Uri` аргумент, который представляет документ в коллекции, которую необходимо заменить, и `object` аргумент, который представляет данные, обновленный документ.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Удаление документа из коллекции документов

Документ может быть удалена из коллекции документов с `DocumentClient.DeleteDocumentAsync` метод:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync` Указывает метод `Uri` аргумент, который представляет документ в коллекции, должны быть удалены.

### <a name="deleting-a-document-collection"></a>Удаление коллекции документов

Коллекция документов могут быть удалены из базы данных с `DocumentClient.DeleteDocumentCollectionAsync` метод:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync` Указывает метод `Uri` аргумент, который представляет коллекцию документов, для удаления. Обратите внимание, что вызов этого метода приведет к удалению документов, хранящихся в коллекции.

### <a name="deleting-a-database"></a>Удаление базы данных

Базы данных могут быть удалены из учетной записи базы данных Cosmos DB с `DocumentClient.DeleteDatabaesAsync` метод:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync` Указывает метод `Uri` аргумента, представляющего базу данных для удаления. Обратите внимание, что вызов этого метода также удалит коллекциями документов, хранящихся в базе данных и документов, хранящихся в коллекциях документа.

## <a name="summary"></a>Сводка

В этой статье описываются способы использования клиентской библиотеке Azure Cosmos DB .NET Standard интегрировать базе данных Azure Cosmos DB документа в приложении Xamarin.Forms. База данных документа Azure Cosmos DB является NoSQL базы данных, которая предоставляет высокоскоростной доступ к документам JSON, предоставляющие доступ к службе быстрое высокодоступные, масштабируемые базы данных для приложений, требующих удобный масштабирования и глобальные репликации.


## <a name="related-links"></a>Связанные ссылки

- [DB Cosmos Azure TODO (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Документация по Azure Cosmos DB](/azure/cosmos-db/)
- [Клиентская библиотека Azure Cosmos DB .NET Standard](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API Azure Cosmos DB](https://docs.microsoft.com/en-us/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
