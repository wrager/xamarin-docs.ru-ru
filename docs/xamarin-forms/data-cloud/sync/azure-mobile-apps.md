---
title: "Синхронизация данных вне сети с помощью Azure мобильных приложений"
description: "Автономная синхронизация позволяет пользователям взаимодействовать с мобильного приложения, просмотр, добавление или изменение данных, даже в том случае, где нет сетевого подключения. Изменения сохраняются в локальной базе данных, и когда устройство подключено к сети, можно синхронизировать изменения с экземпляром мобильных приложений Azure. В этой статье описывается добавление автономной синхронизации функциональные возможности в Xamarin.Forms приложения."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: b7756c63901d3b4fbfea70587b3fdf8e5cf9df72
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Синхронизация данных вне сети с помощью Azure мобильных приложений

_Автономная синхронизация позволяет пользователям взаимодействовать с мобильного приложения, просмотр, добавление или изменение данных, даже в том случае, где нет сетевого подключения. Изменения сохраняются в локальной базе данных, и когда устройство подключено к сети, можно синхронизировать изменения с экземпляром мобильных приложений Azure. В этой статье описывается добавление автономной синхронизации функциональные возможности в Xamarin.Forms приложения._

## <a name="overview"></a>Обзор

[Пакет SDK Azure Mobile клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) предоставляет `IMobileServiceTable` интерфейс, который представляет операции, которые могут быть выполнены для таблиц, сохраненных в экземпляре мобильных приложений Azure. Эти операции напрямую подключаться к экземпляру мобильных приложений Azure и завершится ошибкой, если мобильное устройство не имеет сетевого подключения.

Для поддержки синхронизации в автономном режиме, мобильный клиент Azure SDK поддерживает *синхронизировать таблицы*, которой предоставляются `IMobileServiceSyncTable` интерфейса. Этот интерфейс предоставляет же создание, чтение, обновление, удаление (CRUD) операций как `IMobileServiceTable` интерфейс, но операций чтения или записи в локальном хранилище. Локальное хранилище не заполнено новыми данными из мобильных приложений Azure экземпляр до вызова *запросу* данных. Аналогичным образом, данные не отправляются в экземпляр мобильных приложений Azure до вызова *принудительной* локальные изменения.

Автономная синхронизация также поддерживает обнаружение конфликтов при изменении одной записи в локальном хранилище и в экземпляре мобильных приложений Azure, а пользовательское разрешение конфликтов. Конфликты могут быть обработаны, либо в локальном хранилище или в экземпляре мобильных приложений Azure.

Дополнительные сведения об автономной синхронизации см. в разделе [автономной синхронизации данных в мобильных приложениях Azure](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) и [включить автономной синхронизации для мобильного приложения Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Установка

Интеграция автономной синхронизации между Xamarin.Forms приложением и экземпляром мобильных приложений Azure выполняется следующим образом:

1. Создайте экземпляр мобильных приложений Azure. Дополнительные сведения см. в разделе [использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Добавить [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) пакет NuGet для всех проектов в решении Xamarin.Forms.
1. (Необязательно) Включите проверку подлинности в экземпляре Azure мобильные приложения и приложения Xamarin.Forms. Дополнительные сведения см. в разделе [проверки подлинности пользователей с помощью мобильных приложений Azure](~/xamarin-forms/data-cloud/authentication/azure.md).

Следующий раздел содержит дополнительные инструкции по установке Настройка проектов универсальной платформы Windows (UWP) для использования пакета Microsoft.Azure.Mobile.Client.SQLiteStore NuGet. Дополнительные настройки не обязательно использовать пакет Microsoft.Azure.Mobile.Client.SQLiteStore NuGet в iOS и Android.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows 

Чтобы использовать SQLite на универсальной платформы Windows (UWP), выполните следующие действия.

1. Установка [SQLite для универсальной платформы Windows](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) расширение Visual Studio в среде разработки.
1. В проекте UWP в Visual Studio, щелкните правой кнопкой мыши **References > Добавить ссылку**, перейдите к **расширения** и добавьте **SQLite для универсальной платформы Windows** и  **Среда выполнения 2015 Visual C++ для универсальных приложений для платформы Windows** расширения для проекта UWP.

## <a name="initializing-the-local-store"></a>Инициализация локальное хранилище

Перед выполнением любых операций синхронизации таблицы необходимо инициализировать локальное хранилище. Это можно сделать в проекте переносимой библиотеки классов (PCL) решения Xamarin.Forms:

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Созданные новую локальную базу данных SQLite `MobileServiceSQLiteStore` класса при условии, что он еще не существует. Затем `DefineTable<T>` метод создает таблицу в локальном хранилище, совпадающие с полями в `TodoItem` тип, при условии, что он еще не существует.

Объект *контекст синхронизации* связанные с `MobileServiceClient` экземпляр и отслеживает изменения, внесенные с помощью таблицы синхронизации. Контекст синхронизации поддерживает очередь, сохраняет упорядоченный список операций создания, обновления и удаления (CUD), будут отправляться на экземпляр мобильных приложений Azure позже. `IMobileServiceSyncContext.InitializeAsync()` Метод используется для связывания с контекстом синхронизации локального хранилища.

`todoTable` Поле является `IMobileServiceSyncTable`, и поэтому все операции CRUD использовать локальное хранилище.

## <a name="performing-synchronization"></a>Выполнение синхронизации

Локальное хранилище синхронизируется с мобильных приложений Azure экземпляра при `SyncAsync` вызова метода:

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync` Метод работает контекст синхронизации, а не конкретной таблицы и отправляет все изменения CUD с момента последнего push.

Запросу осуществляется `IMobileServiceSyncTable.PullAsync` метода на одной таблице. Первый параметр `PullAsync` метод является имя запроса, который используется только на мобильном устройстве. Результатами запроса непустое имя при выполнении клиента SDK Azure Mobile *добавочной синхронизации*, где каждый раз операцию извлечения возвращает результат, что самые последние `updatedAt` отметки времени из результатов хранятся в локальном системные таблицы. Операции извлечения последующих затем получить только записи после этой метки времени. Кроме того *полная синхронизация* может осуществляться путем передачи `null` как имя запроса, что приводит к всех записей, извлекаемых в каждой операции извлечения. Вслед за все операции синхронизации полученных данных вставляется в локальном хранилище.

> [!NOTE]
> **Примечание**: при выполнении запросу к таблице с ожидающими локального обновления опрашивающего сначала выполнить push в контекст синхронизации. Это сводит к минимуму конфликтов между изменениями, которые уже поставлены в очередь и новые данные из экземпляра мобильных приложений Azure.

`SyncAsync` Также включает базовую реализацию для обработки конфликтов при изменении одной записи в локальном хранилище и в экземпляре мобильных приложений Azure. При конфликт, данных был обновлен в локальном хранилище и в экземпляре мобильных приложений Azure `SyncAsync` метод обновляет данные в локальном хранилище из данных, хранящихся в экземпляре мобильных приложений Azure. При возникновении любого конфликта других `SyncAsync` метод отменяет локальные изменения. Это обрабатывает сценарий, где существует локальное изменение данных, удаляется из экземпляра мобильных приложений Azure.

В реальном приложении, разработчикам следует писать пользовательский `IMobileServiceSyncHandler` обработки конфликтов реализацию, которая подходит к их варианту использования. Дополнительные сведения см. в разделе [использовать оптимистическую блокировку для разрешения конфликтов](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) на портале Azure и [Подробнее о поддержке в автономном режиме в управляемый клиент SDK](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) блоги MSDN.

## <a name="purging-data"></a>Очистка данных

Можно очистить таблицы в локальном хранилище данных с `IMobileServiceSyncTable.PurgeAsync` метод. Этот метод поддерживает сценарии, такие как удаление устаревших данных, приложения больше не требуется. Например, образец приложения отображаются только `TodoItem` экземпляров, которые не выполнены. Таким образом завершенные элементы больше не нужно хранить локально. Очистка завершенного элементов из локального хранилища можно следующим образом:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Вызов `PurgeAsync` также запускает принудительной отправки. Таким образом любые элементы, которые помечены как завершенные локально будут отправляться на экземпляр мобильных приложений Azure до удаления из локального хранилища. Тем не менее, если операции синхронизации с экземпляром мобильных приложений Azure, очистка вызовет `InvalidOperationException` Если `force` параметра равным `true`. Альтернативным вариантом является проверка `IMobileServiceSyncContext.PendingOperations` свойство, которое возвращает количество ожидающих операций, которые еще не был занесен в стек экземпляр мобильных приложений Azure и выполнять Очистка, только если свойство равно нулю.

> [!NOTE]
> **Примечание**: вызов `PurgeAsync` с `force` равным `true` будут потеряны все ожидающие изменения.

## <a name="initiating-synchronization"></a>Запуск синхронизации

В примере приложения `SyncAsync` метод вызывается с помощью `TodoList.OnAppearing` метод:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Это означает, что приложение попытается синхронизации с экземпляром мобильных приложений Azure, при запуске.

Кроме того, синхронизация может быть инициировано в iOS и Android с помощью запросу для обновления на список данных, а также на платформах Windows с помощью **синхронизации** кнопку в пользовательском интерфейсе. Дополнительные сведения см. в разделе [обновления по запросу](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Сводка

В этой статье описывается добавление функциональность автономной синхронизации приложения Xamarin.Forms. Автономная синхронизация позволяет пользователям взаимодействовать с мобильного приложения, просмотр, добавление или изменение данных, даже в том случае, где нет сетевого подключения. Изменения сохраняются в локальной базе данных, и когда устройство подключено к сети, можно синхронизировать изменения с экземпляром мобильных приложений Azure.


## <a name="related-links"></a>Связанные ссылки

- [TodoAzureAuthOfflineSync (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Использование мобильного приложения Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Проверка подлинности пользователей с помощью Azure мобильных приложений](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Пакет SDK для Azure мобильного клиента](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
