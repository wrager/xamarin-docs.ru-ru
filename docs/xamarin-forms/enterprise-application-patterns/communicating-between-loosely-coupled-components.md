---
title: "Взаимодействие между слабо связанные компоненты"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: e05cd0ec7d03a033e24dcbfb8124cfc2ccfa438e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="communicating-between-loosely-coupled-components"></a>Взаимодействие между слабо связанные компоненты

Публикация-подписка является шаблон обмена сообщениями в котором издателей отправлять сообщения без необходимости знания о любой приемников, известный как подписчики. Аналогично подписчики прослушивать определенных сообщений без необходимости знания ни одного издателя.

Публикация реализации событий в .NET-шаблон подписки и являются наиболее простым и понятным подход для уровня связи между компонентами, если слабой связи не требуется, например элемент управления и содержащая его страница. Тем не менее время существования издатель и подписчик являются связанными, ссылки на объекты друг с другом, и тип подписчика должно иметь ссылку на тип издателя. Это может создать памяти управления проблемы, особенно при наличии количества кратковременных объектов, которые подписаться на событие статическими или долгоживущих объектов. Если не удалить обработчик событий, подписчик будет поддерживать их существование ссылку на него на издателе, и будут предотвратить или задержке сборки мусора подписчика.

## <a name="introduction-to-messagingcenter"></a>Общие сведения о MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) публикации реализует класс-шаблон, позволяя на основе сообщения обмена данными между компонентами, которые неудобны для связывания с ссылки на объект и тип подписки. Этот механизм позволяет издателями и подписчиками для обмена данными без наличия ссылки друг на друга, помогая уменьшить количество зависимостей между компонентами, предоставляя компонентов для разработки и тестирования независимо друг от друга.

[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) Класс предоставляет многоадресной рассылки публикации подписки функциональные возможности. Это означает, что может быть несколько издателей, которые публикуют одно сообщение, которое может быть несколько подписчиков, прослушивание одного сообщения. Данная связь показана на рис. 4-1:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Многоадресная рассылка публикации подписки функциональные возможности")

**Рис. 4-1:** многоадресной рассылки публикации подписки функциональные возможности

Издатели отправлять сообщения с помощью [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) метод, пока подписчики прослушивания сообщений с помощью [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) метод. Кроме того, подписчики также отказаться от получения сообщений подписок, при необходимости, с [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) метод.

На внутреннем уровне [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) класс использует слабые ссылки. Это означает, что он не будет хранить объекты проверки активности, которое позволит им быть собраны как мусор. Таким образом она необходима только отменить подписку на сообщения, когда класс больше не хочет получать сообщения.

Использование мобильного приложения eShopOnContainers [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) класса для обмена данными между слабо связанные компоненты. Приложение определяет три сообщения:

-   `AddProduct` Публикуемое сообщение `CatalogViewModel` класс при добавлении элемента в корзину. В ответ `BasketViewModel` класс подписывается на сообщения и увеличивает количество элементов в список покупок в ответ. Кроме того `BasketViewModel` класса также отменяет подписку данное сообщение.
-   `Filter` Публикуемое сообщение `CatalogViewModel` класса, когда пользователь применяет фильтр фирменной символики или тип отображаемых из каталога. В ответ `CatalogView` класс подписывается на сообщения и обновляет пользовательский Интерфейс, чтобы отображались только те элементы, которые соответствуют критериям фильтра.
-   `ChangeTab` Публикуемое сообщение `MainViewModel` класса, если `CheckoutViewModel` переходит к `MainViewModel` после успешного создания и отправки нового заказа. В ответ `MainView` класс подписывается сообщение и обновления пользовательского интерфейса таким образом, **Мой профиль** вкладка активна, чтобы показать заказы на пользователя.

> [!NOTE]
> Хотя [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) позволяет поддерживать связь между слабо связанными классами в классе, он не предлагает только архитектурные решения этой проблемы. Например связь между модели представления и представления также достигается модулем привязки, так и через уведомления об изменении свойств. Кроме того связи между двумя моделями представления также может осуществляться путем передачи данных во время перехода.

В мобильном приложении eShopOnContainers[ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) используется для обновления в пользовательском Интерфейсе в ответ на действие, которое выполняется в другом классе. Таким образом сообщения публикуются в потоке пользовательского интерфейса с подписчиками, получения сообщения в том же потоке.

> [!TIP]
> Маршалинг в поток пользовательского интерфейса при выполнении пользовательского интерфейса обновлений. Если сообщение, отправляемое из фонового потока требуется обновление пользовательского интерфейса, обработки сообщения в потоке пользовательского интерфейса в подписчике с помощью вызова [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) метод.

Дополнительные сведения о [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), в разделе [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## <a name="defining-a-message"></a>Определение сообщения

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) сообщения — это строки, которые используются для идентификации сообщений. В следующем примере кода показаны сообщения, определенные в мобильном приложении eShopOnContainers:

```csharp
public class MessengerKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

В этом примере сообщения определяются с помощью константы. Преимущество такого подхода является, что она обеспечивает безопасность типа во время компиляции и оптимизации кода.

## <a name="publishing-a-message"></a>Публикация сообщения

Издатели уведомления подписчиков сообщения с одним из [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) перегрузки. В следующем примере кода демонстрируется публикация `AddProduct` сообщение:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

В этом примере [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) метод задает три аргумента:

-   Первый аргумент указывает класс отправителя. Класс отправитель должен быть указан с любой из подписчиков, которые хотят получать сообщения.
-   Второй аргумент задает сообщение.
-   Третий аргумент задает полезные данные для отправки ее подписчику. В этом случае является полезные данные `CatalogItem` экземпляра.

[ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Метод будет публиковать сообщения и его полезные данные, используя подход выстрелил и забыл. Следовательно сообщение отправляется, даже если существуют подписчики, не зарегистрирован для получения сообщения. В этом случае учитывается отправленного сообщения.

> [!NOTE]
> [ `MessagingCenter.Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) Метод можно использовать универсальные параметры для управления как доставки сообщений. Таким образом разные подписчики могут быть получены несколько сообщений, которые используют удостоверение сообщения, но отправлять различными типами полезной нагрузки данных.

## <a name="subscribing-to-a-message"></a>Подписка на сообщение

Подписчики могут зарегистрироваться для получения сообщения с помощью одного из [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) перегрузки. В следующем примере кода показано, как подписывается на eShopOnContainers мобильного приложения и процессы, `AddProduct` сообщение:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

В этом примере [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) метод подписывается `AddProduct` сообщений и выполняет делегат обратного вызова в ответ на получение сообщения. Этот делегат обратного вызова, указанный как лямбда-выражение выполняет код, который обновляет пользовательский Интерфейс.

> [!TIP]
> Рассмотрите возможность использования неизменяемый полезные данные. Не пытайтесь изменить полезные данные в делегат обратного вызова, так как несколько потоков может осуществлять доступ к полученным данным одновременно. В этом сценарии полезные данные должны быть неизменными во избежание ошибок параллелизма.

Подписчик не может понадобиться обработка каждый экземпляр опубликованное сообщение и это может управляться аргументов универсального типа, которые определены на [ `Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) метод. В этом примере, подписчик получит только `AddProduct` сообщений, отправляемых из `CatalogViewModel` класса, данные которого полезные данные `CatalogItem` экземпляра.

## <a name="unsubscribing-from-a-message"></a>Отмена подписки на сообщение

Подписчики отказаться от получения сообщений, которые больше не хотите получать. Это достигается с помощью одного из [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) перегрузки метода, как показано в следующем примере кода:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessengerKeys.AddProduct);
```

В этом примере [ `Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) синтаксис метода отражает аргументы типа, указанный при подписке на получение `AddProduct` сообщения.

## <a name="summary"></a>Сводка

Xamarin.Forms [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) публикации реализует класс-шаблон, позволяя на основе сообщения обмена данными между компонентами, которые неудобны для связывания с ссылки на объект и тип подписки. Этот механизм позволяет издателями и подписчиками для обмена данными без наличия ссылки друг на друга, помогая уменьшить количество зависимостей между компонентами, предоставляя компонентов для разработки и тестирования независимо друг от друга.


## <a name="related-links"></a>Связанные ссылки

- [Загрузить электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
