---
title: Проверка подлинности пользователей с базой данных Azure Cosmos DB документа
description: В этой статье объясняется, как для объединения с коллекциями Azure DB Cosmos секционированы, контроля доступа, чтобы пользователь имеет доступ только к своих собственных документов в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 031a48e5e10100b2c57ac067a0dda916c93d20da
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241615"
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Проверка подлинности пользователей с базой данных Azure Cosmos DB документа

_Базы данных Azure Cosmos DB документа поддерживает секционированные коллекции, которые может охватывать несколько серверов и секции, одновременно поддерживая неограниченное хранение и пропускную способность. В этой статье объясняется, как для объединения с секционированных коллекций контроля доступа, чтобы пользователь имеет доступ только к своих собственных документов в приложении Xamarin.Forms._

## <a name="overview"></a>Обзор

Ключ секции должен быть указан при создании секционированной коллекции и документы с одинаковым ключом секции, которые будут храниться в одной секции. Таким образом указав удостоверение пользователя в качестве ключа секции приведет к секционированную коллекцию, которой будут храниться только документы для этого пользователя. Это также гарантирует, что будет выполнено масштабирование базы данных Azure Cosmos DB документа как количество пользователей и увеличить элементов.

Необходимо разрешить доступ к любой коллекции и модель управления доступом SQL API определяет два типа доступа конструкций:

- **Главные ключи** включить полный административный доступ ко всем ресурсам внутри Cosmos DB учетной записи и создается при создании учетной записи Cosmos DB.
- **Маркеры ресурсов** моделируют связь между пользователем базы данных и разрешения, пользователь имеет для определенного ресурса Cosmos DB, например коллекции или документа.

Предоставление доступа к главного ключа открывает учетную запись Cosmos DB вероятность вредоносной или намеренное использование. Тем не менее маркеры ресурсов Azure Cosmos DB предоставляют безопасный механизм позволяя пользователям чтение, запись и удаление конкретных ресурсов в зависимости от разрешений, предоставленных учетной записи Azure Cosmos DB.

Типичный подход к запроса, создания и доставки маркеры ресурсов для мобильного приложения является использование брокера маркера ресурсов. На следующей диаграмме показан общий обзор как в примере приложения используется брокеру маркера ресурсов для управления доступом к данным документа, базы данных:

![](authentication-images/documentdb-authentication.png "Процесс проверки подлинности базы данных документа")

Компонент Service broker маркера ресурсов является служба среднего уровня веб-API, размещенных в службе приложений Azure, может иметь главный ключ базы данных Cosmos учетной записи. В примере приложения используется компонент Service broker маркера ресурсов для управления доступом к данным документа, базы данных следующим образом:

1. При входе в систему Xamarin.Forms приложение обращается к службе приложений Azure, чтобы инициировать процесс проверки подлинности.
1. Служба приложений Azure выполняет поток проверки подлинности OAuth с Facebook. После завершения потока проверки подлинности, Xamarin.Forms приложение получает токен доступа.
1. Xamarin.Forms приложение использует маркер доступа для запроса маркера ресурсов из маркера ресурсов компонент Service broker.
1. Компонент Service broker маркера ресурсов использует маркер доступа для запроса удостоверение пользователя от Facebook. Удостоверение пользователя затем используется для запроса маркера ресурсов из DB Cosmos, который используется для предоставления доступа на чтение и запись секционированную коллекцию прошедшего проверку подлинности пользователя.
1. Xamarin.Forms приложение использует маркер ресурса для прямого доступа к ресурсам Cosmos DB с разрешениями, определяемыми маркер ресурса.

> [!NOTE]
> По истечении срока действия маркера ресурсов документа последующих запросов к базе данных получат 401 создается исключение. На этом этапе Xamarin.Forms приложения следует повторно установить удостоверение и запросите новый маркер ресурса.

Дополнительные сведения о секционировании Cosmos DB см. в разделе [как секции и масштабирования в Azure Cosmos DB](/azure/cosmos-db/partition-data/). Дополнительные сведения об управлении доступом Cosmos DB см. в разделе [защита доступа к данным Cosmos DB](/azure/cosmos-db/secure-access-to-data/) и [управление доступом в API-Интерфейсы SQL](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Установка

Интеграция broker маркера ресурсов в приложении Xamarin.Forms выполняется следующим образом:

1. Создайте учетную запись Cosmos DB, которая будет использовать контроль доступа. Дополнительные сведения см. в разделе [Cosmos DB конфигурации](#cosmosdb_configuration).
1. Создание службы приложения Azure для размещения ресурсов компонент Service broker токена. Дополнительные сведения см. в разделе [конфигурации службы приложения Azure](#app_service_configuration).
1. Создайте приложение Facebook для проверки подлинности. Дополнительные сведения см. в разделе [Конфигурация приложения Facebook](#facebook_configuration).
1. Настройка службы приложений Azure для выполнения простой проверки подлинности с Facebook. Дополнительные сведения см. в разделе [конфигурация проверки подлинности службы приложений Azure](#app_service_authentication_configuration).
1. Настройка примера приложения Xamarin.Forms для взаимодействия с службе приложений Azure и Cosmos DB. Дополнительные сведения см. в разделе [конфигурации приложения Xamarin.Forms](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Конфигурация Azure Cosmos DB

Процесс создания Cosmos DB учетной записи, которую будет использовать контроль доступа выглядит следующим образом:

1. Создайте учетную запись Cosmos DB. Дополнительные сведения см. в разделе [создать учетную запись Azure Cosmos DB](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. В учетной записи Cosmos DB, создайте новую коллекцию с именем `UserItems`, указав ключ раздела `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Конфигурация службы приложений Azure

Размещение брокер маркера ресурсов в службе приложений Azure выполняется следующим образом:

1. На портале Azure создайте новое веб-приложение служб приложений. Дополнительные сведения см. в разделе [Создание веб-приложения в среде службы приложений](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. На портале Azure откройте колонку параметров приложения для веб-приложения и добавьте следующие параметры:
    - `accountUrl` — Это значение должно быть URL-адрес Cosmos DB учетной записи из колонки ключи учетной записи Cosmos DB.
    - `accountKey` — Это значение должно быть Cosmos DB главного ключа (основная или вторичная) из колонки ключи учетной записи Cosmos DB.
    - `databaseId` – значение должно быть имя базы данных, Cosmos DB.
    - `collectionId` – значение должно быть имя коллекции Cosmos БД (в данном случае `UserItems`).
    - `hostUrl` — Это значение должно быть URL-адрес веб-приложения в колонке Общие сведения об учетной записи службы приложения.

    Следующий снимок экрана демонстрирует эту конфигурацию:

    [![](authentication-images/azure-web-app-settings.png "Параметры веб-приложения служб приложений")](authentication-images/azure-web-app-settings-large.png#lightbox "параметры веб-приложения служб приложений")

1. Публикация решения broker маркера ресурсов веб-приложения службы приложений Azure.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Конфигурация приложения Facebook

Процесс создания приложения Facebook для проверки подлинности выглядит следующим образом:

1. Создайте приложение Facebook. Дополнительные сведения см. в разделе [регистрация и настройка приложения](https://developers.facebook.com/docs/apps/register) в центре разработчика Facebook.
1. Добавьте имя входа Facebook продукт в приложение. Дополнительные сведения см. в разделе [добавить имя входа Facebook для приложения или веб-сайт](https://developers.facebook.com/docs/facebook-login) в центре разработчика Facebook.
1. Настройка имени входа Facebook следующим образом:
  - Включите имя входа клиента OAuth.
  - Включение имени входа OAuth Web.
  - Набор перенаправления OAuth допустимый URI, URI-адрес веб-приложения служб приложений, с `/.auth/login/facebook/callback` добавляется.

  Следующий снимок экрана демонстрирует эту конфигурацию:

  ![](authentication-images/facebook-oauth-settings.png "Параметры входа Facebook OAuth")

Дополнительные сведения см. в разделе [зарегистрировать приложение в Facebook](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Настройки проверки подлинности службы приложений Azure

Настройка приложения службы простой проверки подлинности выполняется следующим образом:

1. На портале Azure перейдите в веб-приложение служб приложений.
1. На портале Azure откройте проверки подлинности и авторизации колонки и выполните следующие действия:
  - Проверка подлинности службы приложений должно быть включено.
  - Действие, выполняемое, если запрос не прошел проверку подлинности должно быть присвоено **входа в систему с Facebook**.

  Следующий снимок экрана демонстрирует эту конфигурацию:

  [![](authentication-images/app-service-authentication-settings.png "Параметры проверки подлинности для веб-приложения служб приложений")](authentication-images/app-service-authentication-settings-large.png#lightbox "параметры проверки подлинности для веб-приложения служб приложений")

Службы приложений веб-приложения также должны быть настроены для обмена данными с помощью приложения Facebook, чтобы включить поток проверки подлинности. Это можно сделать, выбрав поставщика удостоверений Facebook и введя **идентификатор приложения** и **секрет приложения** значения из настроек приложения Facebook в центре разработчика Facebook. Дополнительные сведения см. в разделе [Facebook, добавьте сведения в приложение](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Настройка приложения Xamarin.Forms

Настройка примера приложения Xamarin.Forms выполняется следующим образом:

1. Откройте решение Xamarin.Forms.
1. Откройте `Constants.cs` и обновите значения из следующих констант:
  - `EndpointUri` — Это значение должно быть URL-адрес Cosmos DB учетной записи из колонки ключи учетной записи Cosmos DB.
  - `DatabaseName` – значение должно быть имя базы данных документа.
  - `CollectionName` – значение должно быть имя коллекции документов базы данных (в данном случае `UserItems`).
  - `ResourceTokenBrokerUrl` — Это значение должно быть URL-адрес веб-приложения broker маркера ресурсов из колонки Общие сведения об учетной записи службы приложения.

## <a name="initiating-login"></a>Инициирование входа

Образец приложения инициирует процесс входа в систему с помощью Xamarin.Auth для перенаправления браузера по URL-поставщик удостоверений, как показано в следующем примере кода:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

В результате поток проверки подлинности OAuth инициировать между службой приложений Azure и Facebook, который отображает страницу входа в Facebook:

![](authentication-images/login.png "Имя входа Facebook")

Имя входа можно отменить, нажав клавишу **отменить** кнопки на iOS или клавишу **обратно** кнопки на Android, в этом случае пользователь останется без проверки подлинности и пользовательский интерфейс поставщика удостоверений является удалить с экрана.

Дополнительные сведения о Xamarin.Auth см. в разделе [проверки подлинности пользователей с помощью поставщика удостоверений](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Получение маркера ресурсов

После успешной проверки подлинности `WebRedirectAuthenticator.Completed` вызывается событие. В следующем примере кода демонстрируется обработка этого события:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Результатом успешной проверки подлинности является маркер доступа, который доступен `AuthenticatorCompletedEventArgs.Account` свойство. Маркер доступа извлекаются и используются в запросе GET посреднику маркера ресурсов `resourcetoken` API.

`resourcetoken` API использует маркер доступа для запроса удостоверение пользователя от Facebook, который в свою очередь используется для запроса маркера ресурсов из базы данных Cosmos. Если документ допустимое разрешение уже существует для пользователя в базе данных документа, он извлекается и Xamarin.Forms приложению возвращается документ JSON, содержащий маркер ресурса. Если допустимое разрешение документа не существует для пользователя, пользователей и разрешений создается в базе данных документа, и маркер ресурса извлекается из документа разрешение и возвращается приложению Xamarin.Forms в документе JSON.

> [!NOTE]
> Документ является пользователь базы данных ресурсов, связанных с базой данных документа, а каждая база данных может содержать ноль или более пользователей. Разрешение базы данных документа является ресурс, связанный с пользователем базы данных документа и каждого пользователя может содержать ноль или несколько разрешений. Ресурс разрешение предоставляет доступ к маркера безопасности, который требуется пользователю при попытке получить доступ к ресурсу, например документ.

Если `resourcetoken` API успешного завершения, он отправляет код состояния HTTP 200 (ОК) в ответе, вместе с документом JSON, содержащий токен ресурсов. Приведенные ниже данные JSON показано сообщение обычно успешный ответ:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` Обработчик событий считывает ответ от `resourcetoken` API и извлекает маркер ресурсов и идентификатор пользователя. Маркер ресурса затем передается в качестве аргумента для `DocumentClient` конструктор, который инкапсулирует конечной точки, учетные данные и политики подключение для доступа к Cosmos DB и используется для настройки и выполнения запросов к БД Cosmos. Маркер ресурса отправляются с каждым запросом для прямого доступа к ресурсу и указывает, что чтение и запись секционированную коллекцию прошедших проверку пользователей доступа к.

## <a name="retrieving-documents"></a>Получение документов

Получение документов, относящихся только к прошедший проверку пользователь может осуществляться путем создания запроса документов, включая идентификатор пользователя в качестве ключа секции и демонстрируется в следующем примере кода:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Запрос асинхронно извлекает все документы, принадлежащих авторизованного пользователя из указанной коллекции и помещает их в `List<TodoItem>` коллекции для отображения.

`CreateDocumentQuery<T>` Указывает метод `Uri` аргумент, который представляет коллекцию, необходимо запросить для документов, и `FeedOptions` объекта. `FeedOptions` Объекта указывает на возвращение неограниченное число элементов, запроса и идентификатор пользователя в качестве ключа секции. Это гарантирует, что в результате возвращаются только документы в секционированную коллекцию пользователя.

> [!NOTE]
> Обратите внимание, что разрешение документов, которые создаются компонентом Service broker маркера ресурсов, хранятся в той же коллекцией документа, что документы, созданные приложением Xamarin.Forms. Таким образом, запрос документа содержит `Where` предложение, которое применяет предикат фильтрации в запросе к коллекции документов. Это предложение гарантирует, что разрешение документы не возвращен из коллекции документов.

Дополнительные сведения о получении документов из коллекции документов см. в разделе [получение коллекции документов документа](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Вставка документов

Перед вставкой документа в виде коллекции документа `TodoItem.UserId` следует обновить свойство со значением, используемого в качестве ключа секции, как показано в следующем примере кода:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Это гарантирует, что документ будет вставлен в секционированную коллекцию пользователя.

Дополнительные сведения о вставке документа в коллекции документов см. в разделе [Вставка документа в коллекции документов](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Удаление документов

Необходимо указать значение ключа секции, при удалении документа из секционированных коллекций, как показано в следующем примере кода:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Это гарантирует, что Cosmos DB знает, который секционированы удалить документ из коллекции.

Дополнительные сведения об удалении документа из коллекции документов см. в разделе [удаление документа из документа коллекции](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Сводка

В этой статье описано для объединения с секционированных коллекций контроля доступа, чтобы пользователь имеет доступ только к своих собственных документов базы данных документа в приложении Xamarin.Forms. Указание удостоверения пользователя в качестве ключа секции гарантирует, что секционированную коллекцию можно сохранить только документы для этого пользователя.


## <a name="related-links"></a>Связанные ссылки

- [TODO Azure Cosmos DB Auth (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Использование базы данных документов Azure Cosmos DB](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Защита доступа к данным Azure Cosmos DB](/azure/cosmos-db/secure-access-to-data/)
- [Управление доступом в API-Интерфейсы SQL](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Создание разделов и шкалы в базе данных Azure Cosmos](/azure/cosmos-db/partition-data/)
- [Клиентская библиотека Azure Cosmos DB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [API Azure Cosmos DB](https://msdn.microsoft.com/library/azure/dn948556.aspx)
