---
title: "Пошаговое руководство. Использование фоновая служба передачи и NSURLSession"
description: "В этом пошаговом руководстве мы используем фоновой службы передачи данных и NSURLSession API с началом загрузки большое изображение, которое продолжает загружать во время работы приложения в фоновом режиме."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4ab11239caf5986bba52f080945d90a91ea9453e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="walkthrough---using-background-transfer-service-and-nsurlsession"></a>Пошаговое руководство. Использование фоновая служба передачи и NSURLSession

_В этом пошаговом руководстве мы используем фоновой службы передачи данных и NSURLSession API с началом загрузки большое изображение, которое продолжает загружать во время работы приложения в фоновом режиме._

Фоновая передача инициируется путем настройки фона `NSURLSession` и устанавливая передачи или загрузки задачи. Если во время backgrounded, приостановки или завершения приложения выполнения задач, операций ввода-вывода будет уведомлять приложение, вызывая обработчик завершения приложения *AppDelegate*. Следующая диаграмма демонстрирует это в действии:

 [![](background-transfer-walkthrough-images/transfer.png "Фоновая передача инициируется путем настройки фона NSURLSession и устанавливая отправка или загрузка задачи")](background-transfer-walkthrough-images/transfer.png#lightbox)

Давайте посмотрим, как это выглядит в коде.

## <a name="configuring-a-background-session"></a>Настройка в фоновом сеансе

Чтобы добавить в фоновом сеансе, создайте новый `NSUrlSession` и настройте его с помощью `NSUrlSessionConfiguration` объекта.

Определяет объект конфигурации сеанса, которые можно сделать и типы задач, которые оно может работать.
Сеансов, настроенных с помощью `CreateBackgroundSessionConfiguration` метод будет выполняться в отдельном процессе и выполнять избирательные (WiFi) передач для сохранения данных и срок автономной работы.
В следующем образце кода демонстрируется правильной настройки использования сеанса фоновой передачи `CreateBackgroundSessionConfiguration` метод и уникальный строковый идентификатор:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate, new NSOperationQueue());

}
```

Помимо объект конфигурации сеанса необходимо также делегат сеанса и очереди.
Очереди определяет порядок, в котором выполняются задачи. Делегат сеанса chaperones процесс передачи маркеров проверки подлинности, кэширование и других проблем, связанных с сеансами.

## <a name="working-with-tasks-and-delegates"></a>Работа с задачами и делегаты

Теперь, когда мы указали в фоновом сеансе, давайте начнем задач для обработки переноса. Мы можно хранить список этих задач с помощью `NSUrlSessionDelegate` экземпляр называется делегат сеанса. Делегат сеанса несет ответственность за пробуждение прерванных или приостановленных приложений в фоновом режиме для проверки подлинности маркера, ошибки или завершения передачи.

`NSUrlSessionDelegate` Предоставляет следующие основные методы для проверки состояния передачи:

-  *DidFinishEventsForBackgroundSession* -этот метод вызывается, когда после завершения всех задач и передача завершена.
-  *DidReceiveChallenge* — вызывается для запроса учетных данных, когда требуется авторизация.
-  *DidBecomeInvalidWithError* -вызываемой при `NSURLSession` станет недействительным.


Сеанс фона требуется более специализированные делегаты в зависимости от типов задач, работающих под управлением. Сеансы фона ограничены двух типов задач:

-  *Отправить задачи* -задачи типа `NSUrlSessionUploadTask` использовать `NSUrlSessionTaskDelegate` , который наследует от `NSUrlSessionDelegate` . Этот делегат предоставляет Отправка дополнительных методов для отслеживания хода выполнения, дескриптор перенаправление HTTP и многое другое.
-  *Загрузите задачи* -задачи типа `NSUrlSessionDownloadTask` использовать `NSUrlSessionDownloadDelegate` , который наследует от `NSUrlSessionTaskDelegate` . Этот делегат предоставляет методы для отправки задачи, а также методы загрузки конкретного ход загрузки и определение задачи загрузки возобновить или завершить.


Следующий код определяет задачу, которая может использоваться для загрузки изображения с URL-адреса. Мы начнем задачи путем вызова `CreateDownloadTask` на нашем фоновом сеансе и передачи запроса URL-адрес:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Далее мы создадим новый сеанс делегат загрузки для отслеживания всех задач загрузки в этом сеансе:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Если мы хотим узнать ход выполнения задачи загрузки, можно переопределить `DidWriteData` способ отслеживания хода выполнения и даже обновление пользовательского интерфейса. Обновления пользовательского интерфейса немедленно появится, если приложение находится на переднем плане, или будет ожидать пользователь при очередном открытии приложения.

API делегат сеанса предоставляет широкий набор инструментов для взаимодействия с задачами. Полный список сеанса делегировать методам, ознакомьтесь с `NSUrlSessionDelegate` документации по API.

> [!IMPORTANT]
> Фон сеансы запускаются в фоновом потоке, поэтому все вызовы для обновления пользовательского интерфейса необходимо явно выполняться в потоке пользовательского интерфейса путем вызова `InvokeOnMainThread` во избежание iOS завершением работы приложения. 


## <a name="handling-transfer-completion"></a>Обработка завершения передачи

Последним шагом является для уведомления приложения о после завершения всех задач, связанных с сеансом и обрабатывать новое содержимое.

В *AppDelegate*, Подпишитесь на `HandleEventsForBackgroundUrl` событий. Когда приложение входит в фоновом режиме и запускается сеанса передачи, этот метод вызывается, и система передает нам обработчик завершения:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Мы будем использовать обработчик завершения, чтобы позволить знать после завершения нашего приложения iOS обработки.

Помните, что сеанс может создать несколько задач для обработки передачи. После завершения последней задачи, приостановлена или завершенных приложения повторно, запущенное в фоновом режиме. Приложение снова подключается к `NSURLSession` с помощью уникальный идентификатор сеанса и вызовы `DidFinishEventsForBackgroundSession` в делегате сеанса. Этот метод является возможность обработки новое содержимое, включая обновление пользовательского интерфейса в соответствии с результатами передачи приложения:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

После обработки новое содержимое Готово, мы называем обработчик завершения, чтобы сообщить системе о том, что можно сделать снимок приложение и вернитесь в спящий режим:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

В этом пошаговом руководстве были рассмотрены основные шаги по реализации фоновой службы передачи данных в iOS 7.



## <a name="related-links"></a>Связанные ссылки

- [Простой фоновой передачи (пример)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
