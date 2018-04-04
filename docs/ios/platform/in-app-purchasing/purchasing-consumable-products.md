---
title: Приобретение к использованию продуктов
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5c2c84c044ff41cced2c97e414502faff45341ec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-consumable-products"></a>Приобретение к использованию продуктов

Использовать продукты, проще всего реализовать, так как нет необходимости «восстановить». Они полезны для устройства, например валюты в игре или элемент однократного использования функциональности. Пользователи повторно можно приобрести к использованию продуктов over и over еще раз.

## <a name="built-in-product-delivery"></a>Встроенные продукта доставки

В образце кода, сопровождающие в этом документе демонстрируется встроенной продуктами — коды продуктов жестко запрограммированы в приложение, так как они тесно связаны в код, который разблокирует функцию после выплаты. Процесс приобретения можно представить следующим образом:   
   
[![Визуализация процесса покупки](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Ниже приведен простой рабочий процесс.   
   
   
   
 1. Добавляет приложение `SKPayment` в очередь. При необходимости пользователь будет запрашиваться их идентификатора Apple ID и запрос на подтверждение платеж.   
   
 2. StoreKit отправляет запрос на сервер для обработки.   
   
 3. После завершения транзакции сервер в ответ отправляет уведомление о транзакции.   
   
 4. `SKPaymentTransactionObserver` Подкласс получению получает и обрабатывает его.   
   
 5. Приложение включает продукта (путем обновления `NSUserDefaults` или другой механизм) и затем вызывает его StoreKit `FinishTransaction`.

Другой тип рабочего процесса — *продуктов Server-Delivered* — то есть обсуждается далее в документе (см. в разделе *проверки поступления и продуктов Server-Delivered*).

## <a name="consumable-products-example"></a>Использовать пример продуктов

[Кода InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) содержит проект с именем *материалы* , реализует базовый «Валюта в игре» (называемые «повозиться кредиты»). Образец показан способ реализации двух продуктов покупок в приложении пользователь приобрести как много «monkey кредитов» нужно — в реальном приложении также будет какой-либо способ оплаты, их!   
   
   
   
 Приложение отображается на этих снимках экрана — каждой покупкой добавляет дополнительные «monkey кредиты» баланс пользователя:   
   
   
   
 [![Каждой покупкой добавляет дополнительные кредиты monkey баланс пользователей](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Взаимодействие между пользовательских классов, StoreKit и магазине приложений будет выглядеть следующим образом:   
   
   
   
 [![Взаимодействие между пользовательских классов, StoreKit и магазин приложений](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>Методы ViewController

Помимо свойства и методы, необходимые для получения сведений о продукте представление-контроллер требует дополнительных уведомлений наблюдателей для прослушивания уведомлений, связанных с покупки. Это просто `NSObjects` будет регистрироваться и удалена в `ViewWillAppear` и `ViewWillDisappear` соответственно.

```csharp
NSObject succeededObserver, failedObserver;
```

Конструктор также создает `SKProductsRequestDelegate` подкласс ( `InAppPurchaseManager`), в свою очередь создает и регистрирует `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 В первой части обработки транзакции покупки из приложений является обработки нажатие кнопки, если пользователь хочет приобрести что-то, как показано в следующем коде из примера приложения:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Во второй части пользовательского интерфейса обрабатывает уведомление о том, что транзакция выполнена успешно, в этом случае обновив показанного сальдо:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Последняя часть пользовательского интерфейса отображается сообщение, если транзакция отменяется для какой-либо причине. В примере кода просто записи сообщения в окне вывода:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Помимо перечисленных выше методов на представление-контроллер, транзакции покупки к использованию продукта требуется код на `SKProductsRequestDelegate` и `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>Методы InAppPurchaseManager

Образец кода реализует число приобрести связанных методов класса InAppPurchaseManager, включая `PurchaseProduct` метод, который создает `SKPayment` экземпляра и добавляет его в очередь для обработки:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Добавление платеж в очередь является асинхронной операцией. Приложение получает управление, пока StoreKit обрабатывает транзакции и отправляет его на серверах компании Apple. Именно на этом этапе проверит, iOS, пользователь вошел в магазине приложений и запрашивать ей Apple ID и пароль при необходимости.   
   
   
   
 Предполагая, что пользователь успешно проходит аутентификацию в магазине приложений и согласен с транзакцией `SKPaymentTransactionObserver` будет получен ответ StoreKit и вызвать следующий метод для выполнения транзакции и завершает его.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Последний шаг — убедитесь, что уведомления StoreKit, транзакции, успешно выполнен, вызвав `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

После поставки продукта `SKPaymentQueue.DefaultQueue.FinishTransaction` должен вызываться для удаления транзакции из очереди оплаты.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Методы SKPaymentTransactionObserver (CustomPaymentObserver)

Вызовы StoreKit `UpdatedTransactions` метод при получении ответа от серверов Apple и передает массив `SKPaymentTransaction` объекты для проверки кода. Метод проход по каждой транзакции и выполняет другую функцию, исходя из состояния транзакции (как показано ниже):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`CompleteTransaction` Метод был ранее описанных в этом разделе — она сохраняет сведения о покупке для `NSUserDefaults`, завершает транзакцию с помощью StoreKit и наконец уведомляет пользовательский Интерфейс для обновления.

### <a name="purchasing-multiple-products"></a>Приобретение несколько продуктов

Если это имеет смысл в приложении на покупку нескольких продуктов, используйте `SKMutablePayment` и присвоить поле «количество»:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Код обработки завершенной транзакции также необходимо запросить свойство количество для удовлетворения покупки правильно:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Когда пользователь покупает несколько экземпляров, предупреждения подтверждение StoreKit будет отражать количество, цену за единицу и общая стоимость, они будет списана сумма, как показано на следующем снимке экрана:

[![Подтверждение покупки](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Обработка сбоев сети

Покупки из приложений требуется рабочее сетевое подключение для StoreKit для взаимодействия с серверами Apple. Если сетевое подключение недоступен, затем покупки из приложений будет недоступна.

### <a name="product-requests"></a>Запросы продукта

Если сеть недоступна, делая `SKProductRequest`, `RequestFailed` метод `SKProductsRequestDelegate` подкласс ( `InAppPurchaseManager`) будет вызываться, как показано ниже:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

Затем ViewController осуществляет прослушивание уведомлений и отображает сообщение в приобретении кнопок:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Так как сетевое подключение может быть временной на мобильных устройствах, приложения могут требуется вести наблюдение с помощью платформы SystemConfiguration состояния сети и повторите попытку при доступности сетевого подключения. Обратитесь к компании Apple или что он использует.

### <a name="purchase-transactions"></a>Транзакции покупки

Сохранит очереди StoreKit оплаты и прямой покупки запросы по возможности, так что влияние сбоя сети зависят от сети, сбой при оформлении покупки.   
   
   
   
 Если ошибка возникает во время транзакции, `SKPaymentTransactionObserver` подкласс ( `CustomPaymentObserver`) будет иметь `UpdatedTransactions` метод с именем и `SKPaymentTransaction` класс будет в состоянии сбоя.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

`FailedTransaction` Метод обнаруживает ли ошибка из-за отмены пользователя, как показано ниже:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Даже если транзакция дает сбой, `FinishTransaction` метод должен вызываться для удаления транзакции из очереди оплаты:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

Пример кода затем отправляет уведомление, чтобы ViewController может отобразить сообщение. Приложения не должно появляться дополнительное сообщение, если пользователь отменил транзакции. Другие коды ошибок, которые могут возникнуть следующие.

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Обработка ограничений

**Параметры > Общие > ограничения** функции iOS позволяет заблокировать некоторые возможности устройства.   
   
   
   
 Можно выполнить запрос, разрешено ли пользователю совершать покупки из приложений через `SKPaymentQueue.CanMakePayments` метод. Если он возвращает значение false затем пользователь не имеет доступа в приложении приобретения. StoreKit автоматически отобразит сообщение об ошибке для пользователя при попытке покупки. Это значение приложения можно вместо этого скрыть кнопки покупки, либо выполнение некоторых действий, чтобы помочь пользователю.   
   
   
   
 В `InAppPurchaseManager.cs` файл `CanMakePayments` метод создает оболочку для функции StoreKit следующим образом:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Чтобы протестировать этот метод, используйте **ограничения** функции iOS, чтобы отключить **покупки из приложений**:   
   
   
   
 [![Использовать функцию ограничения операций ввода-вывода для отключения покупки из приложений](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Этот пример кода из `ConsumableViewController` реагирует на `CanMakePayments` возвращение значения false, отображая **AppStore отключена** текст на кнопках отключено.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Оно выглядит этот формат, если **покупки из приложений** функция ограничен — кнопки покупки отключены.   
   
   
   
 [![Приложение выглядит следующим образом, при покупки в приложение, компонент является ограниченной покупки кнопки отключены](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Сведения о продукте по-прежнему может быть когда запрашивается `CanMakePayments` имеет значение false, чтобы приложение по-прежнему можно извлекать и отображать цены. Это означает, что если мы удалили `CanMakePayments` проверки из кода, кнопок покупку будет по-прежнему быть активным, но при попытке выполнить покупку будет отображено сообщение, **покупки из приложений не допускаются** (созданных в ходе StoreKit Если очередь оплаты обращается к):   
   
   
   
 [![Покупки из приложений не допускаются.](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Реальные приложения могут использовать другой подход к обработке ограниченного использования программ, таких как скрытие кнопки полностью и возможно предложения более подробные сообщения, чем оповещение, которое StoreKit показано автоматически.

