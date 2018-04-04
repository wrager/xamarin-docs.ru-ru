---
title: Использование веб-службы Windows Communication Foundation (WCF)
description: WCF является единой framework корпорации Майкрософт для построения сервисноориентированных приложений. Она позволяет разработчикам создавать распределенные приложения безопасную, надежную, транзакций и с возможностью взаимодействия. В этой статье показано, как использовать службы WCF Simple Object Access Protocol (SOAP) в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 5696FF04-EF21-4B7A-8C8B-26DE28B5C0AD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: c626008012ccdab2f8ed2c719b34a45471598d47
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="consuming-a-windows-communication-foundation-wcf-web-service"></a>Использование веб-службы Windows Communication Foundation (WCF)

_WCF является единой framework корпорации Майкрософт для построения сервисноориентированных приложений. Она позволяет разработчикам создавать распределенные приложения безопасную, надежную, транзакций и с возможностью взаимодействия. В этой статье показано, как использовать службы WCF Simple Object Access Protocol (SOAP) в приложении Xamarin.Forms._

WCF описание службы с различными разные контракты, в том числе следующие:

- **Контракты данных** — структуры данных, которые образуют основу для содержимого сообщения.
- **Контрактами сообщений** — создания сообщений с существующие контракты данных.
- **Контракты сбоя** — разрешить пользовательских ошибок SOAP, должен быть задан.
- **Контракты службы** — указать операции, которые поддерживают службы и сообщения, необходимые для взаимодействия с каждой операцией. Они также определяют любое поведение пользовательской ошибки, могут быть связаны с операциями для каждой службы.

Существуют различия между службами ASP.NET Web (ASMX) и WCF, но важно понимать, что WCF поддерживает те же возможности, предоставляемые ASMX-сообщений SOAP по протоколу HTTP. Дополнительные сведения об использовании службы ASMX см. в разделе [использования ASP.NET Web Services (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md).

Как правило платформа Xamarin поддерживает тот же поднабор стороны клиента WCF, который поставляется со средой выполнения Silverlight. Сюда входят наиболее распространенные кодировку и протокол реализации WCF — транспорта кодировке текста сообщения SOAP по протоколу HTTP с помощью протокола `BasicHttpBinding` класса. Кроме того поддержка WCF требует использования средств доступно только в среде Windows для создания прокси-сервера.

Инструкции по настройке службы WCF можно найти в файле readme, сопровождающий пример приложения. Тем не менее при запуске образца приложения он будет подключиться к службе WCF, размещенной в Xamarin, которая предоставляет доступ только для чтения к данным, как показано на следующем снимке экрана:

![](wcf-images/portal.png "Образец приложения")

> [!NOTE]
> В iOS 9 и более поздней версии безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения, чтобы предотвратить случайное раскрытие конфиденциальных сведений. Поскольку ATS включена по умолчанию в приложениях, разработанных для iOS 9, все соединения будут применяться ATS требования безопасности. Если соединения не удовлетворяют этим требованиям, произойдет сбой с исключением.
> Если это не позволяет использовать ATS могут быть присоединены из `HTTPS` протокола и безопасную передачу Интернет-ресурсов. Это можно сделать путем обновления приложения **Info.plist** файла. Дополнительные сведения см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Веб-служба

Служба WCF предоставляет следующие операции:

|Операция|Описание|Параметры|
|--- |--- |--- |
|GetTodoItems|Получить список заданий для выполнения|
|CreateTodoItem|Создать новый элемент задачи|Сериализованное XML TodoItem|
|EditTodoItem|Задание обновления|Сериализованное XML TodoItem|
|DeleteTodoItem|Удалить задание|Сериализованное XML TodoItem|

Дополнительные сведения о модели данных, используемые в приложении см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Образец приложения использует службу WCF, размещенной в Xamarin, которая обеспечивает доступ только для чтения к веб-службе. Таким образом операции, которые создание, обновление и удаление данных не изменится данным в приложении. Однако доступна в размещаемом версии службы ASMX **TodoWCFService** папки в соответствующем примере приложения. Эта версия размещаемом разрешение службы WCF на полный создания, обновления, чтения и удаления доступ к данным.

Объект *прокси* должен быть создан для использования службы WCF, которая позволяет подключиться к службе в приложении. Прокси-сервер создается путем много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания веб-служб (WSDL), который создается веб-службой. Прокси-сервера можно создать с помощью поставщика ссылочных службы Microsoft WCF Web в Visual Studio 2017 г. для добавления ссылки на службу для веб-службы для стандартной библиотеки .NET. Вместо создания прокси-сервера, используя Microsoft WCF Web ссылку поставщик службы в Visual Studio 2017 г. является использование ServiceModel Metadata Utility Tool (svcutil.exe). Дополнительные сведения см. в разделе [ServiceModel Metadata Utility Tool (Svcutil.exe)](/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe/).

Созданный прокси-классы предоставляют методы для использования веб-служб, использующих шаблон разработки для асинхронной модели программирования (APM). В этом шаблоне асинхронную операцию, реализуется в виде двух методов с именами *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий интерфейс `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение должно также вызывать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый метод синхронной веб-службы. Например `EndGetTodoItems` метод возвращает коллекцию `TodoItem` экземпляров. *EndOperationName* также включает `IAsyncResult` параметр, который следует задать экземпляр, возвращаемый соответствующим вызовом метода *BeginOperationName* метод.

Библиотека параллельных задач (TPL) может упростить процесс использует пары методов begin и end APM, инкапсулируя асинхронных операций в том же `Task` объекта. Эта инкапсуляция предоставляется несколько перегрузок `TaskFactory.FromAsync` метод.

Дополнительные сведения о APM в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [библиотека параллельных задач и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

### <a name="creating-the-todoserviceclient-object"></a>Создание объекта TodoServiceClient

Созданный класс прокси предоставляет `TodoServiceClient` класс, который используется для связи со службой WCF через HTTP. Он предоставляет функциональные возможности для вызова методов веб-службы, экземпляр службы определенные асинхронных операций от URI. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке Async](~/cross-platform/platform/async.md).

`TodoServiceClient` Экземпляр объявлен на уровне класса, чтобы для находятся объекты, при условии, что приложение должно использовать службу WCF, как показано в следующем примере кода:

```csharp
public class SoapService : ISoapService
{
  ITodoService todoService;
  ...

  public SoapService ()
  {
    todoService = new TodoServiceClient (
      new BasicHttpBinding (),
      new EndpointAddress (Constants.SoapUrl));
  }
  ...
}
```

`TodoServiceClient` Экземпляр настроен с помощью привязки, сведения и адрес конечной точки. Привязка используется для указания транспорта, кодировки и данных протокола, требуемых для приложений и служб для взаимодействия друг с другом. `BasicHttpBinding` Указывает отправки сообщения SOAP с кодировкой текста по транспортному протоколу HTTP. Задание адреса конечной точки позволяет приложению подключаться к различным экземплярам службы WCF, при условии, что несколько опубликованных экземпляров.

Дополнительные сведения о настройке ссылки на службу см. в разделе [Настройка ссылки на службу](~/cross-platform/data-cloud/web-services/index.md#wcf).

### <a name="creating-data-transfer-objects"></a>Создание объектов для передачи данных

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` элемента в веб-службе, его необходимо сначала преобразовать в прокси, созданный `TodoItem` типа. Это достигается путем `ToWCFServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
TodoWCFService.TodoItem ToWCFServiceTodoItem (TodoItem item)
{
  return new TodoWCFService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Этот метод просто создает новый `TodoWCFService.TodoItem` экземпляром и задает для каждого свойства одинаковые свойства из `TodoItem` экземпляра.

Аналогичным образом, при получении данных из веб-службы, оно должно быть преобразовано из прокси, созданный `TodoItem` тип `TodoItem` экземпляра. Это осуществляется с помощью `FromWCFServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
static TodoItem FromWCFServiceTodoItem (TodoWCFService.TodoItem item)
{
  return new TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}

```

Этот метод просто получает данные из прокси, созданный `TodoItem` тип и задает его в только что созданный `TodoItem` экземпляра.

### <a name="retrieving-data"></a>Получение данных

`TodoServiceClient.BeginGetTodoItems` И `TodoServiceClient.EndGetTodoItems` методы используются для вызова `GetTodoItems` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync <ObservableCollection<TodoWCFService.TodoItem>> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromWCFServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndGetTodoItems` метод один раз `TodoServiceClient.BeginGetTodoItems` метод завершения, с `null` параметр, указывающий, что данные не передается в `BeginGetTodoItems` делегата. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

`TodoServiceClient.EndGetTodoItems` Возвращает `ObservableCollection` из `TodoWCFService.TodoItem` экземпляров, которые затем преобразуется в `List` из `TodoItem` экземпляров для отображения.

### <a name="creating-data"></a>Создание данных

`TodoServiceClient.BeginCreateTodoItem` И `TodoServiceClient.EndCreateTodoItem` методы используются для вызова `CreateTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndCreateTodoItem` метод один раз `TodoServiceClient.BeginCreateTodoItem` метод завершения, с `todoItem` данным, которое передается параметр `BeginCreateTodoItem` делегат для указания `TodoItem` создаваемого веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` если ей не удается создать `TodoItem`, который обрабатывается приложением.

### <a name="updating-data"></a>Обновление данных

`TodoServiceClient.BeginEditTodoItem` И `TodoServiceClient.EndEditTodoItem` методы используются для вызова `EditTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToWCFServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndEditTodoItem` метод один раз `TodoServiceClient.BeginCreateTodoItem` метод завершения, с `todoItem` данным, которое передается параметр `BeginEditTodoItem` делегат для указания `TodoItem` обновляемого веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` если ей не удается найти или обновить `TodoItem`, который обрабатывается приложением.

### <a name="deleting-data"></a>Удаление данных

`TodoServiceClient.BeginDeleteTodoItem` И `TodoServiceClient.EndDeleteTodoItem` методы используются для вызова `DeleteTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task DeleteTodoItemAsync (string id)
{
  ...
  await Task.Factory.FromAsync (
    todoService.BeginDeleteTodoItem,
    todoService.EndDeleteTodoItem,
    id,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoServiceClient.EndDeleteTodoItem` метод один раз `TodoServiceClient.BeginDeleteTodoItem` метод завершения, с `id` данным, которое передается параметр `BeginDeleteTodoItem` делегат для указания `TodoItem` должны быть удалены с веб-службы. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `FaultException` если ей не удается найти или удалить `TodoItem`, который обрабатывается приложением.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать службы WCF SOAP в приложении Xamarin.Forms. Как правило платформа Xamarin поддерживает тот же поднабор стороны клиента WCF, который поставляется со средой выполнения Silverlight. Сюда входят наиболее распространенные кодировку и протокол реализации WCF — транспорта кодировке текста сообщения SOAP по протоколу HTTP с помощью протокола `BasicHttpBinding` класса. Кроме того поддержка WCF требует использования средств доступно только в среде Windows для создания прокси-сервера.


## <a name="related-links"></a>Связанные ссылки

- [TodoWCF (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
