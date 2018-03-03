---
title: "Использование службы Amazon SimpleDB"
description: "Amazon SimpleDB является веб-службы, которая обеспечивает возможность хранения и запроса данных в облаке Amazon. В этой статье объясняется, как использовать для запроса, создание, замена и удаление данные, хранящиеся в службе SimpleDB AWS пакета SDK для .NET."
ms.topic: article
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 590e39deb7972df9e45064bb1a96e533a1fc9856
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Использование службы Amazon SimpleDB

_Amazon SimpleDB является веб-службы, которая обеспечивает возможность хранения и запроса данных в облаке Amazon. В этой статье объясняется, как использовать для запроса, создание, замена и удаление данные, хранящиеся в службе SimpleDB AWS пакета SDK для .NET._

SimpleDB службы используется модель запроса и ответа, знакомые пользователям служб REST. Вызываются операции службы SimpleDB, отправив запрос, который может содержать данные. После обработки запроса, SimpleDB служба возвращает ответ, содержащий все результаты. Служба SimpleDB должно быть создано программным способом и не может быть создана с помощью [консоли AWS](https://aws.amazon.com). Тем не менее AWS учетной записи требуется для создания и доступа к любой веб-службы Amazon.

В службе SimpleDB данные организованы в домены, в течение которых можно разместить данные и выполнять запросы к данным. Доменов состоят из элементов, которые описаны пар имя значение атрибута. Домены может рассматриваться как аналог таблицы, с атрибутами аналог столбцов и строк аналог элементов. Дополнительные сведения о модели данных SimpleDB см. в разделе [модели данных](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) Amazon веб-сайта.

Инструкции по настройке необходимые службы Amazon можно найти в файле readme, сопровождающий пример приложения. При запуске образца приложения подключатся пул идентификаторов Amazon Cognito для авторизации доступа к службе SimpleDB, как показано на следующем снимке экрана:

![](aws-images/portal.png "Образец приложения")

> [!NOTE]
> В iOS 9 и более поздней версии безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения, чтобы предотвратить случайное раскрытие конфиденциальных сведений. Поскольку ATS включена по умолчанию в приложениях, разработанных для iOS 9, все соединения будут применяться ATS требования безопасности. Если соединения не удовлетворяют этим требованиям, произойдет сбой с исключением.
> Если это не позволяет использовать ATS могут быть присоединены из `HTTPS` протокола и безопасную передачу Интернет-ресурсов. Это можно сделать путем обновления приложения **Info.plist** файла. Дополнительные сведения см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>Использование службы SimpleDB

Удостоверение Cognito Amazon позволяет AWS служб, таких как SimpleDB, вызываемый из приложения без жестко AWS учетных данных в приложение. Вместо этого создается уникальный идентификатор пула в [Cognito консоли Amazon](https://console.aws.amazon.com/cognito/home). Пул идентификаторов содержит удостоверений, которые используют роли, чтобы указать ресурсы, такие как SimpleDB, имеет доступ удостоверение.

[AWS пакет SDK для .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) предоставляет `CognitoAWSCredentials` и `AmazonSimpleDBClient` классов, которые используются приложением Xamarin.Forms для доступа к службе SimpleDB, как показано в следующем примере кода:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Новый экземпляр `CognitoAWSCredentials` , указав идентификатор пула уникальный идентификатор и регион учетной записи удостоверения Cognito создается класс. Во время записи удостоверения Cognito доступна только в регионах USEast1 и EUWest1. Тем не менее может обмениваться данными с помощью службы Amazon вне области.

Когда `AmazonSimpleDBClient` , создается экземпляр `CognitoAWSCredentials` экземпляр должен быть предоставлен, вместе с `AmazonSimpleDBConfig` , который определяет географический регион, где находится служба SimpleDB. `CognitoAWSCredentials` Экземпляр гарантирует, что служба SimpleDB, к которому осуществляется один связанный с учетной записью AWS, в которой был создан пул идентификаторов, избегая необходимость внедрять клавиши доступа AWS и секретный ключ в приложении.

SimpleDB домена службы создается путем вызова `AmazonSimpleDBClient.CreateDomainAsync` метода, как показано в следующем примере кода:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync` Метод требует `CreateDomainRequest` экземпляр в качестве параметра. `CreateDomainRequest` Экземпляра инициализирует `DomainName` свойство в значение, которое используется для определения домена. Чтобы создать домен, это значение должно быть уникальным среди домены, связанные с учетной записью AWS. В противном случае домена не будет создана и указывает, что будут отправляться без ответа с ошибкой. Все операции с именем домена для существующего домена, а не вновь созданного домена затем произойдет.

### <a name="creating-simpledb-objects"></a>Создание объектов SimpleDB

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` экземпляра в его необходимо сначала преобразовать в службе SimpleDB `List` из `ReplaceableAttribute` объектов. Это достигается путем `ToSimpleDBReplaceableAttributes` метода, как показано в следующем примере кода:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Этот метод создает `List` из нового `ReplaceableAttribute` экземпляров, с `List` представляет один `TodoItem` экземпляра. Каждый `ReplaceableAttribute` экземпляр представляет одно свойство из `TodoItem` экземпляра. Дополнительные сведения о `ReplaceableAttribute` см. в описании [ReplaceableAttribute класса](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) Amazon веб-сайта.

Аналогичным образом, при получении данных от службы SimpleDB, оно должно быть преобразовано из `List` из `Attribute` экземпляры `TodoItem` экземпляра. Это осуществляется с помощью `FromSimpleDBAttributes` метода, как показано в следующем примере кода:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Этот метод просто получает каждый `Attribute` экземпляра из `List` и задает его в только что созданный `TodoItem` экземпляра.

Дополнительные сведения о `Attribute` см. в описании [класс атрибута](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) Amazon веб-сайта.

### <a name="querying-data"></a>Запросы к данным

Содержимое домена можно получить, вызвав `AmazonSimpleDBClient.SelectAsync` метода, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync` Метод принимает `SelectRequest` экземпляр в качестве параметра, который указывает `Select` выражение в его запроса `SelectExpression` свойство. Выражение запроса имеет аналогичен формату стандартного SQL `SELECT` инструкции. Дополнительные сведения о выражении запроса см. в разделе [использование инструкции Select для создания запросов SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon веб-сайта.

> [!NOTE]
> **Примечание**: Следуйте правилам кавычек при построении выражения запроса. Дополнительные сведения см. в разделе [выберите правила для заключения в кавычки](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon веб-сайта.

`SelectAsync` Метод возвращает ответ, содержащий коллекцию элементов и связанных атрибутов, соответствующих выражения запроса. Эта коллекция преобразуется в `List` из `TodoItem` экземпляров для отображения.

### <a name="creating-and-replacing-data"></a>Создание и замены данных

`AmazonSimpleDBClient.PutAttributesAsync` Метод используется для создания и заменять данные в домене службы SimpleDB, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync` Метод принимает `PutAttributesRequest` экземпляр в качестве параметра. `PutAttributesRequest` Экземпляр задает пары имя значение атрибута, которые должны быть созданы как новый элемент или заменить существующий элемент. `List` Из `ReplaceableAttribute` созданных экземпляров `ToSimpleDBReplaceableAttributes` метод. Этот метод также задает `Replace` каждого экземпляра `ReplaceableAttribute` для `true`. Это вызовет новое значение атрибута для замены существующего значения атрибута, если заменяемый данных. Однако попытка заменить значения атрибутов, которые не существуют не приведет к в ответ на ошибку.

Значение `PutAttributesRequest.ItemName` свойство управляет ли к домену, в котором будет добавлен новый элемент или ли будет заменить существующий элемент. Когда приложение создает новый элемент, он задает `TodoItem.ID` для нового `Guid`. Это гарантирует, что каждый `TodoItem` экземпляр имеет уникальный идентификатор. Таким образом Если `PutAttributesRequest.ItemName` свойству присвоено значение, которое не существует в домене, служба SimpleDB создает новый элемент, содержащий пары имя значение указанного атрибута. Если `PutAttributesRequest.ItemName` свойству присвоено значение, которое уже существует в домене, служба SimpleDB обновит элемент пары имя значение указанного атрибута.

### <a name="deleting-data"></a>Удаление данных

`AmazonSimpleDBClient.DeleteAttributesAsync` Метод используется для удаления данных из домена службы SimpleDB, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync` Метод принимает `DeleteAttributesRequest` экземпляр в качестве параметра.  `DeleteAttributesRequest` Экземпляр указывает атрибуты, которые должны быть удалены из элемента, с `List` из `Attribute` экземпляры должны удаляться построенных по `ToSimpleDBAttributes` метод. Элемент будет удален при условии, что удаляются все атрибуты элемента.

## <a name="summary"></a>Сводка

В этой статье описано, как использовать для запроса, создания и замените AWS пакета SDK для .NET и удалять данные, хранящиеся в службе SimpleDB. Этот пакет SDK предоставляет `CognitoAWSCredentials` и `AmazonSimpleDBClient` классы, которые используются для доступа к службе SimpleDB приложением Xamarin.Forms.


## <a name="related-links"></a>Связанные ссылки

- [TodoAWS (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon Web Services руководстве для разработчиков Xamarin SDK](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Удостоверение Cognito Amazon](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Документация для разработчиков SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Класс AmazonSimpleDBClient](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [Amazon Web Services SDK для .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
