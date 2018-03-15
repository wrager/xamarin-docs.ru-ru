---
title: "Использование веб-службы ASP.NET (ASMX)"
description: "ASMX предоставляет возможность создания веб-служб, используемых при отправке сообщений с помощью Simple Object Access Protocol (SOAP). SOAP — это независимая от платформы и независимые от языка протокол для создания и доступа к веб-служб. Потребителям службы ASMX, никакая о платформе, объектной модели или язык программирования, используемый для реализации службы не требуется. Они должны понимать, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать службы ASMX SOAP в приложении Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: D5533964-5528-4D35-9C2B-FAFB632472AC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: a095dbbb78ad1517791356ae0b7cbeaa94d1336f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="consuming-an-aspnet-web-service-asmx"></a>Использование веб-службы ASP.NET (ASMX)

_ASMX предоставляет возможность создания веб-служб, используемых при отправке сообщений с помощью Simple Object Access Protocol (SOAP). SOAP — это независимая от платформы и независимые от языка протокол для создания и доступа к веб-служб. Потребителям службы ASMX, никакая о платформе, объектной модели или язык программирования, используемый для реализации службы не требуется. Они должны понимать, как отправлять и получать сообщения SOAP. В этой статье показано, как использовать службы ASMX SOAP в приложении Xamarin.Forms._

Сообщение SOAP — это документ XML, содержащую следующие элементы:

- Корневой элемент *конверт* , идентифицирующий XML-документа, как сообщение SOAP.
- Необязательный *заголовок* элемент, который содержит сведения о приложении, например, данные проверки подлинности. Если *заголовок* присутствует элемент он должен быть первым дочерним элементом *конверт* элемента.
- Обязательный *текст* элемент, содержащий сообщение SOAP, предназначенное для получателя.
- Необязательный *ошибки* элемент, который используется для указания сообщения об ошибках. Если *ошибки* элемент присутствует, он должен быть дочерним для элемента *текст* элемента.

SOAP может работать через несколько транспортных протоколов, включая HTTP, SMTP, TCP и UDP. Однако службы ASMX может работать только по протоколу HTTP. Платформа Xamarin поддерживает стандартные реализации SOAP 1.1 по протоколу HTTP, и она включает поддержку для многих стандартных конфигураций службы ASMX.

Инструкции по настройке службы ASMX можно найти в файле readme, сопровождающий пример приложения. Тем не менее при запуске примера приложения подключится к службе ASMX, размещенных в Xamarin, которая предоставляет доступ только для чтения к данным, как показано на следующем снимке экрана:

![](asmx-images/portal.png "Образец приложения")

> [!NOTE]
> В iOS 9 и более поздней версии безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения, чтобы предотвратить случайное раскрытие конфиденциальных сведений. Поскольку ATS включена по умолчанию в приложениях, разработанных для iOS 9, все соединения будут применяться ATS требования безопасности. Если соединения не удовлетворяют этим требованиям, произойдет сбой с исключением.
> Если это не позволяет использовать ATS могут быть присоединены из `HTTPS` протокола и безопасную передачу Интернет-ресурсов. Это можно сделать путем обновления приложения **Info.plist** файла. Дополнительные сведения см. [безопасность транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="consuming-the-web-service"></a>Веб-служба

Служба ASMX предоставляет следующие операции:

|Операция|Описание:|Параметры|
|--- |--- |--- |
|GetTodoItems|Получить список заданий для выполнения|
|CreateTodoItem|Создать новый элемент задачи|Сериализованное XML TodoItem|
|EditTodoItem|Задание обновления|Сериализованное XML TodoItem|
|DeleteTodoItem|Удалить задание|Сериализованное XML TodoItem|

Дополнительные сведения о модели данных, используемые в приложении см. в разделе [моделирования данных](~/xamarin-forms/data-cloud/walkthrough.md).

> [!NOTE]
> Образец приложения использует размещенное Xamarin ASMX службу, которая предоставляет доступ только для чтения к веб-службе. Таким образом операции, которые создание, обновление и удаление данных не изменится данным в приложении. Однако доступна в размещаемом версии службы ASMX **TodoASMXService** папки в соответствующем примере приложения. Эта версия размещаемом ASMX службы разрешает полный создания, обновления, чтения и удаления доступ к данным.

Объект *прокси* должен быть создан для работы со службой ASMX, которая позволяет подключиться к службе в приложении. Прокси-сервер создается путем много метаданных службы, определяющий методы и связанные службы конфигурации. Эти метаданные предоставляются в виде документа языка описания веб-служб (WSDL), который создается веб-службой. Прокси-сервер создается путем добавления веб-ссылки на веб-службу в проекты под конкретные платформы.

Созданный прокси-классы предоставляют методы для использования веб-службы, использующие шаблон разработки для асинхронной модели программирования (APM). В этом шаблоне асинхронную операцию, реализуется в виде двух методов с именами *BeginOperationName* и *EndOperationName*, который начинают и завершают асинхронную операцию.

*BeginOperationName* метод начинает асинхронную операцию и возвращает объект, реализующий интерфейс `IAsyncResult` интерфейс. После вызова метода *BeginOperationName*, приложение может продолжить выполнение инструкций в вызывающем потоке, пока асинхронная операция выполняется в потоке пула потоков.

Для каждого вызова *BeginOperationName*, приложение должно также вызывать *EndOperationName* для получения результатов операции. Возвращаемое значение *EndOperationName* имеет тот же тип, возвращаемый метод синхронной веб-службы. Например `EndGetTodoItems` метод возвращает коллекцию `TodoItem` экземпляров. *EndOperationName* также включает `IAsyncResult` параметр, который следует задать экземпляр, возвращаемый соответствующим вызовом метода *BeginOperationName* метод.

Библиотека параллельных задач (TPL) может упростить процесс использует пары методов begin и end APM, инкапсулируя асинхронных операций в том же `Task` объекта. Эта инкапсуляция предоставляется несколько перегрузок `TaskFactory.FromAsync` метод.

Дополнительные сведения о APM в разделе [модели асинхронного программирования](https://msdn.microsoft.com/library/ms228963(v=vs.110).aspx) и [библиотека параллельных задач и традиционное .NET Framework асинхронное программирование](https://msdn.microsoft.com/library/dd997423(v=vs.110).aspx) на сайте MSDN.

### <a name="creating-the-todoservice-object"></a>Создание объекта TodoService

Созданный класс прокси предоставляет `TodoService` класс, который используется для взаимодействия со службой ASMX по протоколу HTTP. Он предоставляет функциональные возможности для вызова методов веб-службы, экземпляр службы определенные асинхронных операций от URI. Дополнительные сведения об асинхронных операциях см. в разделе [Общие сведения о поддержке Async](~/cross-platform/platform/async.md).

`TodoService` Экземпляр объявлен на уровне класса, чтобы для находятся объекты, при условии, что приложение должно использовать службу ASMX, как показано в следующем примере кода:

```csharp
public class SoapService : ISoapService
{
  ASMXService.TodoService asmxService;
  ...

  public SoapService ()
  {
    asmxService = new ASMXService.TodoService (Constants.SoapUrl);
  }
  ...
}
```

`TodoService` Конструктор принимает необязательный строковый параметр, указывает URL-адрес экземпляра службы ASMX. Это позволяет приложению подключаться к различным экземплярам службы ASMX, при условии, что несколько опубликованных экземпляров.

### <a name="creating-data-transfer-objects"></a>Создание объектов для передачи данных

В примере приложения используется `TodoItem` для модели данных. Для хранения `TodoItem` элемента в веб-службе, его необходимо сначала преобразовать в прокси, созданный `TodoItem` типа. Это достигается путем `ToASMXServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
ASMXService.TodoItem ToASMXServiceTodoItem (TodoItem item)
{
  return new ASMXService.TodoItem {
    ID = item.ID,
    Name = item.Name,
    Notes = item.Notes,
    Done = item.Done
  };
}
```

Этот метод просто создает новый `ASMService.TodoItem` экземпляром и задает для каждого свойства одинаковые свойства из `TodoItem` экземпляра.

Аналогичным образом, при получении данных из веб-службы, оно должно быть преобразовано из прокси, созданный `TodoItem` тип `TodoItem` экземпляра. Это осуществляется с помощью `FromASMXServiceTodoItem` метода, как показано в следующем примере кода:

```csharp
static TodoItem FromASMXServiceTodoItem (ASMXService.TodoItem item)
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

`TodoService.BeginGetTodoItems` И `TodoService.EndGetTodoItems` методы используются для вызова `GetTodoItems` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var todoItems = await Task.Factory.FromAsync<ASMXService.TodoItem[]> (
    todoService.BeginGetTodoItems,
    todoService.EndGetTodoItems,
    null,
    TaskCreationOptions.None);

  foreach (var item in todoItems) {
    Items.Add (FromASMXServiceTodoItem (item));
  }
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoService.EndGetTodoItems` метод один раз `TodoService.BeginGetTodoItems` метод завершения, с `null` параметр, указывающий, что данные не передается в `BeginGetTodoItems` делегата. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

`TodoService.EndGetTodoItems` Метод возвращает массив `ASMXService.TodoItem` экземпляров, которые затем преобразуется в `List` из `TodoItem` экземпляров для отображения.

### <a name="creating-data"></a>Создание данных

`TodoService.BeginCreateTodoItem` И `TodoService.EndCreateTodoItem` методы используются для вызова `CreateTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginCreateTodoItem,
    todoService.EndCreateTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoService.EndCreateTodoItem` метод один раз `TodoService.BeginCreateTodoItem` метод завершения, с `todoItem` данным, которое передается параметр `BeginCreateTodoItem` делегат для указания `TodoItem` создаваемого веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `SoapException` если ей не удается создать `TodoItem`, который обрабатывается приложением.

### <a name="updating-data"></a>Обновление данных

`TodoService.BeginEditTodoItem` И `TodoService.EndEditTodoItem` методы используются для вызова `EditTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

```csharp
public async Task SaveTodoItemAsync (TodoItem item, bool isNewItem = false)
{
  ...
  var todoItem = ToASMXServiceTodoItem (item);
  ...
  await Task.Factory.FromAsync (
    todoService.BeginEditTodoItem,
    todoService.EndEditTodoItem,
    todoItem,
    TaskCreationOptions.None);
  ...
}
```

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoService.EndEditTodoItem` метод один раз `TodoService.BeginCreateTodoItem` метод завершения, с `todoItem` данным, которое передается параметр `BeginEditTodoItem` делегат для указания `TodoItem` обновляемого веб-службой. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `SoapException` если ей не удается найти или обновить `TodoItem`, который обрабатывается приложением.

### <a name="deleting-data"></a>Удаление данных

`TodoService.BeginDeleteTodoItem` И `TodoService.EndDeleteTodoItem` методы используются для вызова `DeleteTodoItem` операции, предоставляемые веб-службой. Эти асинхронные методы, содержащийся в `Task` объекта, как показано в следующем примере кода:

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

`Task.Factory.FromAsync` Метод создает `Task` , выполняющего `TodoService.EndDeleteTodoItem` метод один раз `TodoService.BeginDeleteTodoItem` метод завершения, с `id` данным, которое передается параметр `BeginDeleteTodoItem` делегат для указания `TodoItem` должны быть удалены с веб-службы. Наконец, значение `TaskCreationOptions` перечисление указывает, что следует использовать поведение по умолчанию для создания и выполнения задач.

Служба вызывает исключение web `SoapException` если ей не удается найти или удалить `TodoItem`, который обрабатывается приложением.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать к веб-службе ASMX из приложения Xamarin.Forms. ASMX предоставляет возможность создания веб-служб, используемых при отправке сообщений по протоколу HTTP с помощью протокола SOAP. Потребителям службы ASMX, никакая о платформе, объектной модели или язык программирования, используемый для реализации службы не требуется. Они должны понимать, как отправлять и получать сообщения SOAP.


## <a name="related-links"></a>Связанные ссылки

- [TodoASMX (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX/)
- [IAsyncResult](https://msdn.microsoft.com/library/system.iasyncresult(v=vs.110).aspx)
