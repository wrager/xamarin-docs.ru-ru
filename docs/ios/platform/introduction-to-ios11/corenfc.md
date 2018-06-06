---
title: Ядро NFC Xamarin.iOS
description: В этом документе описывается чтение около поля связь тегов в Xamarin.iOS, с помощью интерфейсов API, представленные в iOS 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2016
ms.openlocfilehash: c42048f9c00238fb73e354ea86322c3d19bae601
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787381"
---
# <a name="core-nfc-in-xamarinios"></a>Ядро NFC Xamarin.iOS

_Теги чтения рядом с полем связи действия (NFC) с помощью iOS 11_

CoreNFC — это новая платформа в iOS 11, который предоставляет доступ к _рядом связь_ переключателей (NFC) для чтения тегов из приложения. Он работает на iPhone 7, плюс 7, 8, 8, а также X.

Средство чтения тег NFC на устройствах iOS поддерживает все типы тег NFC с 1 по 5, который содержит _формат обмена данными NFC_ сведения (NDEF).

Существуют некоторые ограничения, которые следует учитывать:

- CoreNFC поддерживает только чтение (а не записи или форматирование) тега.
- Просмотров тег должен быть инициированной пользователем и время ожидания через 60 секунд.
- Приложения должны быть видимыми на переднем плане для сканирования.
- CoreNFC можно проверить только на реальном устройстве (но не в симуляторе).

На этой странице описаны конфигурации, необходимые для использования CoreNFC и показано, как использовать API с помощью [«TFCTagReader» пример кода](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/).

## <a name="configuration"></a>Конфигурация

Чтобы включить CoreNFC, необходимо настроить три элемента в проекте:

- **Info.plist** ключ конфиденциальности.
- **Entitlements.plist** входа.
- Профиль подготовки с **считывании тегов NFC** возможностей.

### <a name="infoplist"></a>Info.plist

Добавить **NFCReaderUsageDescription** ключ конфиденциальности и текст, который отображается для пользователя во время сканирования. С помощью сообщения, подходящий для вашего приложения (например, пояснения сканирования):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Приложение должно запросить **рядом считывании тегов поля связи** пара возможностью с помощью следующих ключ значение в вашей **Entitlements.plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Профиль подготовки

Создайте новый **идентификатор приложения** и убедитесь, что **считывании тегов NFC** службы установлен:

[![Страница портала новый идентификатор приложения разработчика с считывании тегов NFC выбран](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Необходимо затем создать новый профиль подготовки для этого код приложения, а затем загрузить и установить его на разработку Mac.

## <a name="reading-a-tag"></a>Чтение тег

После настройки проекта добавить `using CoreNFC;` в верхнюю часть файла и выполните следующие три действия, чтобы реализовать NFC тег функциональные возможности чтения:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Реализуйте `INFCNdefReaderSessionDelegate`

Интерфейс имеет два метода для реализации:

- `DidDetect` — Вызывается при успешном считывании тег.
- `DidInvalidate` — Вызывается, когда происходит ошибка или 60 второй время ожидания истекло.

#### <a name="diddetect"></a>DidDetect

В образце кода каждого сообщение добавляется к представлению таблицы:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Этот метод может вызываться несколько раз (и массив сообщений может быть передан в) Если сеанса позволяет нескольким тега операций чтения. Это задается с помощью третий параметр `Start` метод (объясняется в [шаг 2](#step2)).

#### <a name="didinvalidate"></a>DidInvalidate

Недействительность может возникать по ряду причин.

- Произошла ошибка при проверке.
- Приложение перестало быть на переднем плане.
- Выбор пользователя для проверки отмены.
- Проверка отменена приложением.

В следующем примере кода показано, как обработать ошибку:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

После сеанса является недействительным, необходимо создать нового объекта сеанса для повторной проверки.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Запуск `NFCNdefReaderSession`

Сканирование должен начинаться с запрос пользователя, например нажатие кнопки.
Следующий код создает и запускает сеанс сканирования:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Параметры для `NFCNdefReaderSession` конструктора, как показано ниже:

- `delegate` — Реализация `INFCNdefReaderSessionDelegate`. В образце кода делегата реализована в контроллер представление таблицы, поэтому `this` используется в качестве параметра делегата.
- `queue` — Очереди, для обработки обратных вызовов. Это может быть `null`, в этом случае необходимо использовать `DispatchQueue.MainQueue` при обновлении элементов управления пользовательского интерфейса (как показано в образце).
- `invalidateAfterFirstRead` — Если `true`, сканирование останавливается после первого успешного сканирования; при `false` сканирование будет продолжен и возвращается несколько результатов до отмены сканирования или 60 второй время ожидания истекло.


### <a name="3-cancel-the-scanning-session"></a>3. Отмена сканирования сеанса

Пользователь может отменить сеанс сканирования по нажатию кнопки системных в пользовательском интерфейсе:

![Кнопка «Отмена» при сканировании](corenfc-images/scan-cancel-sml.png)

Приложение может отменить сканирование программным образом путем вызова `InvalidateSession` метод:

```csharp
Session.InvalidateSession();
```

В обоих случаях делегата `DidInvalidate` будет вызван метод.

## <a name="summary"></a>Сводка

CoreNFC позволяет приложению считывать данные из NFC-метки. Поддерживает чтение различные форматы тег (типы NDEF от 1 до 5), но не поддерживает запись или форматирования.


## <a name="related-links"></a>Связанные ссылки

- [NFCTagReader (пример)](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
- [Введение в основные NFC (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/718/)
