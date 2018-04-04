---
title: Поиск данных с поиском Azure
description: Поиск Azure — облачной службы, которая предоставляет индексирования и запросов возможности загруженные данные. Требования к инфраструктуре и сложности алгоритма поиска, обычно связанные с реализации функциональности поиска в приложении будут удалены. В этой статье демонстрируется использование библиотеки поиска Microsoft Azure для интеграции поиска Azure в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: b0542b330e54a41a0cbe6ffe364def78ab6386b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="searching-data-with-azure-search"></a>Поиск данных с поиском Azure

_Поиск Azure — облачной службы, которая предоставляет индексирования и запросов возможности загруженные данные. Требования к инфраструктуре и сложности алгоритма поиска, обычно связанные с реализации функциональности поиска в приложении будут удалены. В этой статье демонстрируется использование библиотеки поиска Microsoft Azure для интеграции поиска Azure в приложении Xamarin.Forms._

## <a name="overview"></a>Обзор

Данные хранятся в службе поиска Azure, как индексы и документы. *Индекс* является хранилищем данных, которые можно искать службой поиска Azure и принципиально подобен таблицу базы данных. Объект *документа* представляет собой единое средство для поиска данных в индексе и концептуально похож на строку базы данных. При отправке документов и отправки запросов поиска для поиска Azure, запросы выполняются по указанному индексу в службе поиска.

Каждый запрос, выполненный для поиска Azure должны включать имя службы и ключ API. Существует два типа ключей API.

- *Ключи администратора* предоставить полные права для всех операций. Это включает управление службой, создание и удаление индексов и источников данных.
- *Ключи запроса* предоставить доступ только для чтения к индексам и документам и должен использоваться приложениями, которые создают запросы поиска.

Наиболее общими запросами к поиску Azure заключается в выполнении запроса. Существует два типа запроса, который может быть передан.

- Объект *поиска* запрос ищет один или несколько элементов во всех полях для поиска в индексе. Поисковые запросы создаются с помощью упрощенного синтаксиса или Lucene синтаксис запроса. Дополнительные сведения см. в разделе [синтаксис простого запроса в поиске Azure](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), и [Lucene синтаксис запроса в поиске Azure](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- Объект *фильтра* запрос вычисляет логическое выражение над всех фильтруемых полях в индексе. Фильтрации запросов строятся с использованием подмножества языка фильтра OData. Дополнительные сведения см. в разделе [синтаксис выражений OData для поиска Azure](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Запросы поиска и фильтрации запросов могут использоваться отдельно или вместе. При совместном использовании фильтра запроса сначала применяется ко всему индексу, а затем выполняется запрос поиска на результаты запроса фильтра.

Поиск Azure также поддерживает получение предложения на основании ввода условия поиска. Дополнительные сведения см. в разделе [предложений запросов](#suggestions).

## <a name="setup"></a>Установка

Интеграция поиска Azure в приложении Xamarin.Forms выполняется следующим образом:

1. Создание службы поиска Azure. Дополнительные сведения см. в разделе [Создание службы поиска Azure с помощью портала Azure](/azure/search/search-create-service-portal/).
1. Удаление Silverlight в качестве целевой платформы с решения Xamarin.Forms Переносимая библиотека классов (PCL). Это можно сделать, изменив профиль PCL ни к одному профилю, поддерживающего кросс платформенной разработки, но не поддерживает Silverlight, таким как профиль 151 или 92.
1. Добавить [библиотеки поиска Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search) пакета NuGet в проект PCL решения Xamarin.Forms.

После выполнения этих действий, API библиотеки поиска Microsoft может использоваться для управления поиска индексов и источниками данных, передача и управление документами и выполнения запросов.

## <a name="creating-the-azure-search-index"></a>Создание индекса поиска Azure

Схема индекса должны быть определены, сопоставляется структуры данных, в котором производится поиск. Это может быть выполнено на портале Azure или программным путем, используя `SearchServiceClient` класса. Этот класс управляет соединениями для поиска Azure и может использоваться для создания индекса. В следующем примере кода показано, как создавать экземпляры этого класса:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` Перегрузку конструктора принимает имя службы поиска и `SearchCredentials` объекта в качестве аргументов, с `SearchCredentials` упаковки объекта *ключ администратора* для службы поиска Azure. *Ключ администратора* необходим для создания индекса.

> [!NOTE]
>  Один `SearchServiceClient` экземпляр должен использоваться в приложении избегать открытия слишком много подключений для службы поиска Azure.

Индекс определяется `Index` объекта, как показано в следующем примере кода:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name` Свойство должно быть присвоено имя индекса и `Index.Fields` свойство должно быть установлено на массив `Field` объектов. Каждый `Field` экземпляр указывает имя, тип и любые свойства, определяющие, как используется поле. Эти свойства включают:

- `IsKey` — Указывает, является ли поле ключа индекса. Только одно поле в индексе типа `DataType.String`, необходимо назначить в качестве ключевого поля.
- `IsFacetable` — Указывает, является ли можно выполнять фасетная навигация для этого поля. Значение по умолчанию — `false`.
- `IsFilterable` — Указывает, может ли поле использоваться в фильтрации запросов. Значение по умолчанию — `false`.
- `IsRetrievable` — Указывает, может ли поле быть извлечен в результатах поиска. Значение по умолчанию — `true`.
- `IsSearchable` — Указывает, включены ли поле в полнотекстовом поиске. Значение по умолчанию — `false`.
- `IsSortable` — Указывает, может ли поле использоваться в `OrderBy` выражения. Значение по умолчанию — `false`.

> [!NOTE]
> Изменение индекса после его развертывания включает в себя перестроения и загрузка данных.

`Index` Объекта можно указать `Suggesters` свойство, которое определяет поля в индекс, чтобы использовать для поддержки автозавершения или предложений запросов поиска. `Suggesters` Свойство должно быть установлено на массив `Suggester` объектами, которые определяют поля, которые используются для построения поиска результаты предложения.

После создания `Index` объекта, индекс создается путем вызова `Indexes.Create` на `SearchServiceClient` экземпляра.

> [!NOTE]
> При создании индекса из приложения должен храниться отвечать на запросы, используйте `Indexes.CreateAsync` метод.

Дополнительные сведения см. в разделе [создать индекс поиска Azure с помощью пакета SDK .NET](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Удаление индекса поиска Azure

Индекс можно удалить путем вызова `Indexes.Delete` на `SearchServiceClient` экземпляр:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Передача данных в индекс поиска Azure

После определения индекса, данные могут передаваться с помощью одного из двух моделей:

- **Модель с извлечением** — данных является периодически полученный из Azure Cosmos DB, база данных SQL Azure, хранилище больших двоичных объектов Azure или SQL Server, размещенных в виртуальной машине Azure.
- **Принудительная модель** — программными средствами данные отправляются в индекс. Это модель, что существует в этой статье.

Объект `SearchIndexClient` должен создать экземпляр для импорта данных в индекс. Это можно сделать путем вызова `SearchServiceClient.Indexes.GetClient` метода, как показано в следующем примере кода:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Данные для импорта в индекс упаковывается в виде `IndexBatch` объекта, который инкапсулирует коллекцию `IndexAction` объектов. Каждый `IndexAction` экземпляр содержит документ и свойство, определяющее, какое действие следует выполнить в документе службы поиска Azure. В приведенном выше примере кода `IndexAction.Upload` действие указано, какие результаты в документе, вставляемого в индекс, если он является новым, или заменен, если он уже существует. `IndexBatch` Объект затем передается в индекс с помощью `Documents.Index` метод `SearchIndexClient` объекта. Сведения о других действиях индексирования см. в разделе [решить индексирования действие, которое следует использовать](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Только 1 000 документов могут быть включены в один запрос индексирования.

Обратите внимание, что в приведенном выше примере кода `monkeyList` коллекция создается как анонимный объект из коллекции `Monkey` объектов. Это создает данные для `id` , а также разрешает сопоставление случая Pascal `Monkey` имена полей индекса поиска имен свойств в верхний регистр. Кроме того, это сопоставление можно также путем добавления `[SerializePropertyNamesAsCamelCase]` атрибут `Monkey` класса.

Дополнительные сведения см. в разделе [передавать данные в поиске Azure, используя пакет SDK для .NET](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Запросы к индексу поиска Azure

Объект `SearchIndexClient` должен создать экземпляр для запроса индекса. Когда приложение выполняет запросы, то желательно следуйте принципу наименьших прав доступа и создания `SearchIndexClient` напрямую, передав *ключа запроса* в качестве аргумента. Это гарантирует, что пользователи имеют доступ только для чтения к индексам и документам. Этот подход демонстрируется в следующем примере кода:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` Перегрузку конструктора принимает имя службы поиска, имя индекса и `SearchCredentials` объекта в качестве аргументов, с `SearchCredentials` упаковки объекта *ключа запроса* для службы поиска Azure.

### <a name="search-queries"></a>Запросы поиска

Индекс можно запрашивать путем вызова `Documents.SearchAsync` метод `SearchIndexClient` экземпляра, как показано в следующем примере кода:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync` Метод принимает аргумент поиска текста и необязательный `SearchParameters` объект, который можно использовать для дальнейшего уточнения запроса. Запрос поиска указан в качестве аргумента поиска текста во время запроса фильтра можно задать, присвоив `Filter` свойство `SearchParameters` аргумент. В следующем примере кода показано, что запрашивать типов:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Этот запрос фильтр применяется ко всему индексу и удаляет документы из результатов где `location` поле не равно Китае и не равно Вьетнам. После фильтрации поисковый запрос выполняется на результаты запроса фильтра.

> [!NOTE]
> Чтобы отфильтровать без поиска, передайте `*` в качестве аргумента поиска текста.

`SearchAsync` Возвращает метод `DocumentSearchResult` , содержащий результаты запроса. Этот объект перечисляется, с каждым `Document` объекта, которые создаются в `Monkey` объекта и добавлены `Monkeys` `ObservableCollection` для отображения. Следующие снимки экрана Показать поиска вернуть результаты запроса из поиска Azure:

![](azure-search-images/search.png "Результаты поиска")

Дополнительные сведения о поиске и фильтрации см. в разделе [запросить индекс поиска Azure с помощью пакета SDK .NET](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Запросы предложений

Поиск Azure позволяет предложения запрашиваться основана на запросе поиска, путем вызова `Documents.SuggestAsync` метод `SearchIndexClient` экземпляра. Это показано в следующем примере кода:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync` Метод принимает аргумент поиска текста, имя средства подбора для использования (который определен в индекс), и необязательный `SuggestParameters` объект, который можно использовать для дальнейшего уточнения запроса. `SuggestParameters` Экземпляр задает следующие свойства:

- `UseFuzzyMatching` — Если задано значение `true`, поиск Azure будет находить предложения даже при наличии замененного или отсутствующего символа в текст для поиска.
- `HighlightPreTag` — тег, который добавляется в предложение попаданий.
- `HighlightPostTag` — тег, который добавляется к попаданий предложений.
- `MinimumCoverage` — представляет процент индекса, который необходимо рассмотреть по предложенному запросу, запрос может быть сообщила об успешном завершении. Значение по умолчанию — 80.
- `Top` — Количество извлекаемых предложений. Он должен быть целым числом от 1 до 100, значение по умолчанию 5.

Общий эффект — 10 лучших результатов от индекса, которые будет возвращаться с, попадания в выделение, и результаты будут включать документы, содержащие аналогично написания условия поиска.

`SuggestAsync` Возвращает метод `DocumentSuggestResult` , содержащий результаты запроса. Этот объект перечисляется, с каждым `Document` объекта, которые создаются в `Monkey` объекта и добавлены `Monkeys` `ObservableCollection` для отображения. Следующих снимках экрана показано предложение результаты, возвращаемые из поиска Azure.

![](azure-search-images/suggest.png "Результаты предложения")

Обратите внимание, что в образце приложения `SuggestAsync` метод вызывается только в том случае, когда пользователь завершает ввод условие поиска. Тем не менее он может использоваться также для поддержки автозавершения поисковые запросы, выполнив на каждого нажатия клавиши.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать библиотеку поиска Microsoft Azure для интеграции поиска Azure в приложении Xamarin.Forms. Поиск Azure — облачной службы, которая предоставляет индексирования и запросов возможности загруженные данные. Требования к инфраструктуре и сложности алгоритма поиска, обычно связанные с реализации функциональности поиска в приложении будут удалены.


## <a name="related-links"></a>Связанные ссылки

- [Поиск Azure (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Поиск Azure документации](/azure/search/)
- [Библиотека поиска Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Search/)
