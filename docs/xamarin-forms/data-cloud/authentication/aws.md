---
title: "Проверка подлинности пользователей с помощью службы Amazon SimpleDB"
description: "Amazon SimpleDB не предоставляет собственную систему разрешения на основе ресурсов. Вместо этого можно использовать проверки подлинности поставщика удостоверений, чтобы убедиться, что пользователи имеют только доступ к собственным данным в домене SimpleDB. В этой статье объясняется, как ограничить доступ пользователей к их собственным данным SimpleDB."
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 85413cf223e794ad2fda093601f9221d0261af39
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Проверка подлинности пользователей с помощью службы Amazon SimpleDB

_Amazon SimpleDB не предоставляет собственную систему разрешения на основе ресурсов. Вместо этого можно использовать проверки подлинности поставщика удостоверений, чтобы убедиться, что пользователи имеют только доступ к собственным данным в домене SimpleDB. В этой статье объясняется, как ограничить доступ пользователей к их собственным данным SimpleDB._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) используется в примере приложения для управления процессом проверки подлинности пользователя и сохраните учетные данные пользователей на устройстве. Дополнительные сведения см. [проверки подлинности пользователей с помощью поставщика удостоверений](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>Позволяя прошедшего проверку подлинности пользователя доступ к данным SimpleDB домена

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` экземпляра в его необходимо сначала преобразовать в службе SimpleDB `List` из `ReplaceableAttribute` объектов. Дополнительные сведения см. [SimpleDB создание объектов](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> В iOS 9 и более поздней версии безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения, чтобы предотвратить случайное раскрытие конфиденциальных сведений. Поскольку ATS включена по умолчанию в приложениях, разработанных для iOS 9, все соединения будут применяться ATS требования безопасности. Если соединения не удовлетворяют этим требованиям, произойдет сбой с исключением.
> Если это не позволяет использовать ATS могут быть присоединены из `HTTPS` протокола и безопасную передачу Интернет-ресурсов. Это можно сделать путем обновления приложения **Info.plist** файла. Дополнительные сведения см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

Чтобы убедиться, что пользователи имеют доступ к только свои собственные данные в домене SimpleDB `ToSimpleDBReplaceableAttributes` метод сохраняет дополнительный атрибут для `TodoItem` экземпляра, как показано в следующем примере кода:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Этот атрибут обеспечивает каждого элемента, хранящегося в домене SimpleDB связанный адрес электронной почты для пользователя, который используется для идентификации пользователя, которому принадлежит данные. Если содержимое домена возвращаются путем вызова `AmazonSimpleDBClient.SelectAsync` метод, выражения запроса гарантирует, что извлекаются только элементы для проверенного пользователя, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync` Метод возвращает ответ, содержащий коллекцию элементов и связанных атрибутов, соответствующих выражения запроса. Выражение запроса гарантирует будут извлекаться только те элементы, которые соответствуют адреса электронной почты пользователя. Дополнительные сведения о выражениях запроса см. в разделе [использование инструкции Select для создания запросов SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon веб-сайта.

> [!NOTE]
> **Примечание**: Следуйте правилам кавычек при построении выражения запроса. Дополнительные сведения см. в разделе [выберите правила для заключения в кавычки](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon веб-сайта.

## <a name="summary"></a>Сводка

В этой статье описано, как ограничить доступ пользователей к их собственным данным SimpleDB. Amazon SimpleDB не предоставляет собственную систему разрешения на основе ресурсов. Вместо этого можно использовать проверки подлинности поставщика удостоверений, чтобы убедиться, что пользователи имеют доступ к только свои собственные данные в домене SimpleDB.


## <a name="related-links"></a>Связанные ссылки

- [TodoAWSAuth (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Использование службы Amazon SimpleDB](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Проверка подлинности пользователей с помощью поставщика удостоверений](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Удостоверение Cognito Amazon](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Документация для разработчиков SimpleDB Amazon](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK для .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
