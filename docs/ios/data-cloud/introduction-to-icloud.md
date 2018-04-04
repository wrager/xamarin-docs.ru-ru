---
title: iCloud
description: Apple реализовала iCloud в iOS 5 в качестве службы позволяет приложениям хранить данные на серверах компании Apple и их синхронизации на всех устройствах, используемые одним лицом (через их идентификатор Apple). Она также содержит компонент резервного копирования, где данные на устройстве, резервное копирование на серверах компании Apple. В этом документе описывается использование некоторых iCloud API-интерфейсов, предоставляемых компанией Apple для хранения и извлечения данных из их серверах с примеры C# для хранения пар ключ значение мелких и хранения документов. Также описывается, как влияют на проект приложения для резервного копирования iCloud.
ms.prod: xamarin
ms.assetid: C6F3B87C-C195-4434-EF14-D66E63894F09
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/09/2016
ms.openlocfilehash: c9e7c920855d2002f52d05e28c5225f301cd62b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="icloud"></a>iCloud

_Apple реализовала iCloud в iOS 5 в качестве службы позволяет приложениям хранить данные на серверах компании Apple и их синхронизации на всех устройствах, используемые одним лицом (через их идентификатор Apple). Она также содержит компонент резервного копирования, где данные на устройстве, резервное копирование на серверах компании Apple. В этом документе описывается использование некоторых iCloud API-интерфейсов, предоставляемых компанией Apple для хранения и извлечения данных из их серверах с примеры C# для хранения пар ключ значение мелких и хранения документов. Также описывается, как влияют на проект приложения для резервного копирования iCloud._

API-Интерфейс хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложения в центральном расположении и доступа к указанным элементам из всех пользовательских устройств.

Существует четыре типа хранения.

- **Значение ключа хранилища** — для совместного использования небольших объемов данных с помощью приложения на пользователя устройства.

- **Хранилище UIDocument** — для хранения документов и другие данные в учетной записи пользователя iCloud, с помощью подкласс UIDocument.

- **Coredata в методе** -хранилища базы данных SQLite.

- **Отдельные файлы и каталоги** — для управления большое количество различных файлов непосредственно в файловой системе.

В этом документе описание первых двух типов — пары "ключ-значение" и подклассов UIDocument — и способы использования этих возможностей в Xamarin.iOS.

## <a name="requirements"></a>Требования

- Последняя стабильная версия Xamarin.iOS
- Xcode 8 или более поздней версии
- Visual Studio для Mac или Visual Studio 2015 и более поздних.

## <a name="preparing-for-icloud-development"></a>Подготовка для разработки iCloud

Приложения должен быть настроен на использование iCloud оба в [Подготовка портала Apple](https://developer.apple.com/account/ios/overview.action) и сам проект. Перед разработкой iCloud (или ознакомлении образцы) выполните следующие действия.

Для правильной настройки приложения для доступа к iCloud:

-   **Найти ваш TeamID** -имя входа для [developer.apple.com](http://developer.apple.com) , а также **центр > учетной записи > Сводка учетной записи разработчика** получить идентификатор команды (или отдельный идентификатор для одного разработчиков ). Он представляет собой 10-значный строку ( **A93A5CM278** например)-это входит в состав «идентификатор контейнера».

-   **Создать новый идентификатор приложения** : чтобы создать идентификатор приложения, выполните шаги, описанные в [инициализации для технологии хранилища раздела руководства подготовки устройства](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md)и не забудьте проверить **iCloud** как Разрешенные службы:

 [![](introduction-to-icloud-images/icloud-sml.png "Проверьте iCloud как разрешенные службы")](introduction-to-icloud-images/icloud.png#lightbox)

- **Создать новый профиль подготовки** : чтобы создать профиль подготовки, выполните шаги, описанные в [подготовки устройства руководства](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) .

- **Добавьте идентификатор контейнера для Entitlements.plist** -формат идентификатор контейнера `TeamID.BundleID`. Дополнительные сведения см. в [работа с данными](~/ios/deploy-test/provisioning/entitlements.md) руководства.

- **Настройка свойств проекта** — в файле Info.plist, убедитесь файл **идентификатор пакета** соответствует **идентификатор пакета** устанавливается, когда [Создание идентификатор приложения ](~/ios/deploy-test/provisioning/capabilities/index.md); Подписывание пакета использует iOS **профиль подготовки** , которые содержат идентификатор приложения с iCloud службы приложений и **пользовательские права** выбранного файла. Все этого в Visual Studio в панели свойств проекта.

- **Включить iCloud на вашем устройстве** — последовательно выберите пункты **параметры > iCloud** и убедитесь, что устройство регистрируется.
Установите и включите **документы и данные** параметр.

- **Необходимо использовать устройство для тестирования iCloud** -он не будет работать в симуляторе.
На самом деле действительно требуется несколько устройств всех вход выполнен с использованием того же идентификатора Apple, чтобы увидеть iCloud в действии.


## <a name="key-value-storage"></a>Значение ключа хранилища

Значение ключа хранилища предназначены для небольших объемов данных, пользователю может быть материализованный через устройств — например страницы, на котором они просмотрели в книги или журнала. Значение ключа хранилища не должен использоваться для резервных копий данных.

Существуют некоторые ограничения, которые следует учитывать при использовании хранилища ключ значение:

- **Максимальный размер ключа** -имена разделов не может превышать 64 байт.

- **Максимальное значение размера** -не удается сохранить более 64 килобайт в одно значение.

- **Размер максимального хранилищу ключей и значений для приложения** -всего приложения можно сохранить только до 64 килобайт данных ключ значение. Попытки задать разделы за рамками этого ограничения будут завершаться ошибкой и предыдущее значение будет сохраняться.

- **Типы данных** — только базовых типов, таких как строки, числа и логические значения можно сохранить.

**ICloudKeyValue** примере показано, как это работает. В примере кода создается раздел с именем для каждого из устройств: можно задать этот ключ на одном устройстве и наблюдайте за значением должны быть переданы другим пользователям. Он также создает ключ с именем «Общий», который можно изменять на любом устройстве - Если изменить сразу на несколько устройствах, iCloud будет решить, какие значения «wins» (с помощью метки времени изменения) и возвращает распространяются.

На этом снимке экрана показан пример используется. При получении уведомления об изменении от iCloud их печати в представлении текста прокрутки в нижней части экрана и обновляются в поля ввода.



 [![](introduction-to-icloud-images/icloud-kv-arrows.png "Поток сообщений между устройствами")](introduction-to-icloud-images/icloud-kv-arrows.png#lightbox)

### <a name="setting-and-retrieving-data"></a>Установка и получение данных

Этот код показано, как задать строковое значение.

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.SetString("testkey", "VALUE IN THE CLOUD");  // key and value
store.Synchronize();
```

Вызов Synchronize гарантирует, что значение сохраняется в локальном диске только в хранилище. Синхронизация с iCloud происходит в фоновом режиме и не удалось «принудительно» с помощью кода приложения. С сетевого подключения синхронизации часто происходит в течение 5 секунд, однако если у сети низкая (или отключено) обновление может занять больше времени.

Можно извлечь значение следующим кодом:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
display.Text = store.GetString("testkey");
```

Значение извлекается из локального хранилища данных — этот метод не пытается связаться с серверами iCloud необходимо получить значение «последние». iCloud обновит локальное хранилище данных в соответствии с его расписание.

### <a name="deleting-data"></a>Удаление данных

Чтобы полностью удалить пару ключ значение, используйте метод Remove следующим образом:

```csharp
var store = NSUbiquitousKeyValueStore.DefaultStore;
store.Remove("testkey");
store.Synchronize();
```

### <a name="observing-changes"></a>Просмотра изменений

Приложение также может получать уведомления при изменении значения с iCloud путем добавления наблюдатель `NSNotificationCenter.DefaultCenter`.
В следующем примере кода из **KeyValueViewController.cs** `ViewWillAppear` метод показано, как для прослушивания этих уведомлений и создать список, из которых были изменены ключи:

```csharp
keyValueNotification =
NSNotificationCenter.DefaultCenter.AddObserver (
    NSUbiquitousKeyValueStore.DidChangeExternallyNotification, notification => {
    Console.WriteLine ("Cloud notification received");
    NSDictionary userInfo = notification.UserInfo;

    var reasonNumber = (NSNumber)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangeReasonKey);
    nint reason = reasonNumber.NIntValue;

    var changedKeys = (NSArray)userInfo.ObjectForKey (NSUbiquitousKeyValueStore.ChangedKeysKey);
    var changedKeysList = new List<string> ();
    for (uint i = 0; i < changedKeys.Count; i++) {
        var key = changedKeys.GetItem<NSString> (i); // resolve key to a string
        changedKeysList.Add (key);
    }
    // now do something with the list...
});
```

Затем, код может выполнять некоторые действия со списком измененных ключей, например обновление локальной копии их или обновление пользовательского интерфейса с новыми значениями.

Изменение возможные причины: ServerChange (0), InitialSyncChange (1) или QuotaViolationChange (2). Вы можете обращаться по причине и выполнения различных обработки, при необходимости (например, может потребоваться удалить некоторые разделы, в результате использования *QuotaViolationChange*).

## <a name="document-storage"></a>Хранения документов

iCloud хранения документов предназначена для управления данными, важную для приложения (и для пользователя). Он может использоваться для управления файлами и другие данные, которые понадобятся вашему приложению для выполнения, в то же время предоставляя резервное копирование на основе iCloud и функциональные возможности для управления доступом на устройствах пользователей.

Эта диаграмма показывает, как все сочетается друг с другом. Каждое устройство имеет данные, сохраненные на локальное хранилище (UbiquityContainer) и iCloud операционной системы, которую управляющая программа отвечает за отправку и получение данных в облаке. Файл доступа к UbiquityContainer должно осуществляться через FilePresenter/FileCoordinator, чтобы предотвратить одновременный доступ. `UIDocument` Класс реализует те автоматически; в этом примере показано, как использовать UIDocument.

 [![](introduction-to-icloud-images/icloud-overview.png "Общие сведения о хранилище документа")](introduction-to-icloud-images/icloud-overview.png#lightbox)

В примере iCloudUIDoc реализуется простой `UIDocument` подкласс, в котором содержится одно текстовое поле. Текст отображается в `UITextView` и изменения распространяются с iCloud на других устройствах с сообщение уведомления, показаны красным цветом. В образце кода не работают с более сложных функций iCloud как разрешение конфликтов.

На этом снимке экрана показан пример приложения — после изменения текста и нажав клавишу **UpdateChangeCount** документа синхронизируется через iCloud к другим устройствам.

 [![](introduction-to-icloud-images/iclouduidoc.png "На этом снимке экрана показан пример приложения после изменения текста и нажав клавишу UpdateChangeCount")](introduction-to-icloud-images/iclouduidoc.png#lightbox)

Состоит из пяти частей примере iCloudUIDoc:

1. **Доступ к UbiquityContainer** -определить, включена iCloud и в этом случае путь к области хранения iCloud приложения.

1. **Создание подкласса UIDocument** — создание класса для посредничества между iCloud хранилища и объектов модели.

1. **Поиск и открытие документов с iCloud** -использовать `NSFileManager` и `NSPredicate` для поиска документов в iCloud и открыть их.

1. **Отображение документов в iCloud** -предоставляют свойства из вашей `UIDocument` , чтобы можно было взаимодействовать с элементами управления пользовательского интерфейса.

1. **Сохранение документов с iCloud** -убедитесь, что изменения, внесенные в пользовательском Интерфейсе, сохраняются на диск и в iCloud.

Все операции iCloud запуска (или следует запускать) асинхронно, чтобы они не блокировали при ожидании к выполнению определенного действия. Вы увидите три различных способа решения этой проблемы в образце:

 **Потоки** — в `AppDelegate.FinishedLaunching` первый вызов `GetUrlForUbiquityContainer` выполняется в другом потоке, чтобы предотвратить блокировку основной поток.

 **NotificationCenter** - регистрации для уведомлений, когда асинхронных операций, таких как `NSMetadataQuery.StartQuery` завершения.

 **Обработчики завершения** - передачей методы для выполнения по завершении асинхронной операции, как `UIDocument.Open`.

### <a name="accessing-the-ubiquitycontainer"></a>Доступ к UbiquityContainer

Сначала с помощью iCloud хранения документов — определить iCloud включен и в этом случае расположение контейнера распространенности «» (каталога для хранения файлов, доступных в iCloud на устройстве).

Этот код находится в `AppDelegate.FinishedLaunching` метод выборки.

```csharp
// GetUrlForUbiquityContainer is blocking, Apple recommends background thread or your UI will freeze
ThreadPool.QueueUserWorkItem (_ => {
    CheckingForiCloud = true;
    Console.WriteLine ("Checking for iCloud");
    var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer (null);
    // OR instead of null you can specify "TEAMID.com.your-company.ApplicationName"

    if (uburl == null) {
        HasiCloud = false;
        Console.WriteLine ("Can't find iCloud container, check your provisioning profile and entitlements");

        InvokeOnMainThread (() => {
            var alertController = UIAlertController.Create ("No \uE049 available",
            "Check your Entitlements.plist, BundleId, TeamId and Provisioning Profile!", UIAlertControllerStyle.Alert);
            alertController.AddAction (UIAlertAction.Create ("OK", UIAlertActionStyle.Destructive, null));
            viewController.PresentViewController (alertController, false, null);
        });
    } else { // iCloud enabled, store the NSURL for later use
        HasiCloud = true;
        iCloudUrl = uburl;
        Console.WriteLine ("yyy Yes iCloud! {0}", uburl.AbsoluteUrl);
    }
    CheckingForiCloud = false;
});
```

Несмотря на то, что образец сделать это, Apple рекомендует вызов GetUrlForUbiquityContainer всякий раз, когда приложение отображается на переднем плане.

### <a name="creating-a-uidocument-subclass"></a>Создание подкласса UIDocument

Все iCloud файлы и каталоги (т. е. все, хранящимся в каталоге UbiquityContainer) должно осуществляться с помощью методов NSFileManager, реализация протокола NSFilePresenter и записи через NSFileCoordinator.
Самый простой способ сделать все это является не записывают ее самостоятельно, а также подкласс UIDocument, который делает это все за вас.

Существует только два метода, которые необходимо реализовать в подклассе UIDocument для работы с iCloud.

- **LoadFromContents** -передает NSData содержимое файла можно распаковать в модели класс/es.

- **ContentsForType** -запрос для указания NSData представление модели класса и es для сохранения на диск (и в облаке).

Этот пример кода из **iCloudUIDoc\MonkeyDocument.cs** показано, как реализовать UIDocument.

```csharp
public class MonkeyDocument : UIDocument
{
    // the 'model', just a chunk of text in this case; must easily convert to NSData
    NSString dataModel;
    // model is wrapped in a nice .NET-friendly property
    public string DocumentString {
        get {
            return dataModel.ToString ();
        }
        set {
            dataModel = new NSString (value);
        }
    }
    public MonkeyDocument (NSUrl url) : base (url)
    {
        DocumentString = "(default text)";
    }
    // contents supplied by iCloud to display, update local model and display (via notification)
    public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("LoadFromContents({0})", typeName);

        if (contents != null)
            dataModel = NSString.FromData ((NSData)contents, NSStringEncoding.UTF8);

        // LoadFromContents called when an update occurs
        NSNotificationCenter.DefaultCenter.PostNotificationName ("monkeyDocumentModified", this);
        return true;
    }
    // return contents for iCloud to save (from the local model)
    public override NSObject ContentsForType (string typeName, out NSError outError)
    {
        outError = null;

        Console.WriteLine ("ContentsForType({0})", typeName);
        Console.WriteLine ("DocumentText:{0}",dataModel);

        NSData docData = dataModel.Encode (NSStringEncoding.UTF8);
        return docData;
    }
}
```

В этом случае модель данных очень простой - одно текстовое поле. Модель данных может быть сложными, при необходимости, например Xml-документ или двоичные данные. Основная роль реализацию UIDocument — для преобразования между классами модели и представлением NSData, могут быть сохранены или загружены на диске.

### <a name="finding-and-opening-icloud-documents"></a>Поиск и открытие документов iCloud

Пример приложения обрабатывает только один файл - test.txt - поэтому код в **AppDelegate.cs** создает `NSPredicate` и `NSMetadataQuery` для искать именно это имя файла. `NSMetadataQuery` Выполняется асинхронно и отправляет уведомление при завершении. `DidFinishGathering` вызывается для уведомления наблюдателя, Остановка запроса и вызывает LoadDocument, который использует `UIDocument.Open` метод с помощью обработчика завершения попытки загрузить его и отобразит в `MonkeyDocumentViewController`.

```csharp
string monkeyDocFilename = "test.txt";
void FindDocument ()
{
    Console.WriteLine ("FindDocument");
    query = new NSMetadataQuery {
        SearchScopes = new NSObject [] { NSMetadataQuery.UbiquitousDocumentsScope }
    };

    var pred = NSPredicate.FromFormat ("%K == %@", new NSObject[] {
        NSMetadataQuery.ItemFSNameKey, new NSString (MonkeyDocFilename)
    });

    Console.WriteLine ("Predicate:{0}", pred.PredicateFormat);
    query.Predicate = pred;

    NSNotificationCenter.DefaultCenter.AddObserver (
        this,
        new Selector ("queryDidFinishGathering:"),
        NSMetadataQuery.DidFinishGatheringNotification,
        query
    );

    query.StartQuery ();
}

[Export ("queryDidFinishGathering:")]
void DidFinishGathering (NSNotification notification)
{
    Console.WriteLine ("DidFinishGathering");
    var metadataQuery = (NSMetadataQuery)notification.Object;
    metadataQuery.DisableUpdates ();
    metadataQuery.StopQuery ();

    NSNotificationCenter.DefaultCenter.RemoveObserver (this, NSMetadataQuery.DidFinishGatheringNotification, metadataQuery);
    LoadDocument (metadataQuery);
}

void LoadDocument (NSMetadataQuery metadataQuery)
{
    Console.WriteLine ("LoadDocument");

    if (metadataQuery.ResultCount == 1) {
        var item = (NSMetadataItem)metadataQuery.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);
        doc = new MonkeyDocument (url);

        doc.Open (success => {
            if (success) {
                Console.WriteLine ("iCloud document opened");
                Console.WriteLine (" -- {0}", doc.DocumentString);
                viewController.DisplayDocument (doc);
            } else {
                Console.WriteLine ("failed to open iCloud document");
            }
        });
    } // TODO: if no document, we need to create one
}
```

### <a name="displaying-icloud-documents"></a>Отображение документов в iCloud

Отображение UIDocument должен быть не отличается от других любому другому классу модели
- свойства отображаются в элементах управления пользовательского интерфейса возможно изменена пользователем, а затем обратно в модель.

В примере **iCloudUIDoc\MonkeyDocumentViewController.cs** отображает текст MonkeyDocument в `UITextView`. `ViewDidLoad` осуществляет прослушивание уведомлений, отправляемых `MonkeyDocument.LoadFromContents` метод. `LoadFromContents` вызывается, когда iCloud имеет новые данные для файла, чтобы уведомление указывает, что документ был обновлен.

```csharp
NSNotificationCenter.DefaultCenter.AddObserver (this,
    new Selector ("dataReloaded:"),
    new NSString ("monkeyDocumentModified"),
    null
);
```

Пример обработчика уведомлений код вызывает метод для обновления пользовательского интерфейса — в этом случае без обнаружения конфликтов и разрешения.

```csharp
[Export ("dataReloaded:")]
void DataReloaded (NSNotification notification)
{
    doc = (MonkeyDocument)notification.Object;
    // we just overwrite whatever was being typed, no conflict resolution for now
    docText.Text = doc.DocumentString;
}
```

### <a name="saving-icloud-documents"></a>Сохранение документов с iCloud

Добавление iCloud, можно вызвать UIDocument `UIDocument.Save` напрямую (для новых документов только) или перемещения существующего файла с помощью `NSFileManager.DefaultManager.SetUbiquitious`. В примере кода создается новый документ непосредственно в контейнере распространенности с этим кодом (существует два завершения обработчика, один для `Save` операции, а другая — для открытия):

```csharp
var docsFolder = Path.Combine (iCloudUrl.Path, "Documents"); // NOTE: Documents folder is user-accessible in Settings
var docPath = Path.Combine (docsFolder, MonkeyDocFilename);
var ubiq = new NSUrl (docPath, false);
var monkeyDoc = new MonkeyDocument (ubiq);
monkeyDoc.Save (monkeyDoc.FileUrl, UIDocumentSaveOperation.ForCreating, saveSuccess => {
Console.WriteLine ("Save completion:" + saveSuccess);
if (saveSuccess) {
    monkeyDoc.Open (openSuccess => {
        Console.WriteLine ("Open completion:" + openSuccess);
        if (openSuccess) {
            Console.WriteLine ("new document for iCloud");
            Console.WriteLine (" == " + monkeyDoc.DocumentString);
            viewController.DisplayDocument (monkeyDoc);
        } else {
            Console.WriteLine ("couldn't open");
        }
    });
} else {
    Console.WriteLine ("couldn't save");
}
```

Последующие изменения в документе не «сохраняются» напрямую, вместо этого мы сообщаем `UIDocument` , изменившиеся с `UpdateChangeCount`, и он будет автоматически расписание сохранения операции диск:

```csharp
doc.UpdateChangeCount (UIDocumentChangeKind.Done);
```

### <a name="managing-icloud-documents"></a>Управление документами iCloud

Пользователи могут управлять документами iCloud в **документов** каталог «распространенности контейнера» вне вашего приложения с помощью параметров; они могут просматривать список файлов и проведите для удаления. Код приложения должен быть способен обрабатывать ситуации, где удаленные документы пользователя. Не храните данные внутреннее приложение в **документов** каталога.

 [![](introduction-to-icloud-images/icloudstorage.png "Управление рабочим процессом документов в iCloud")](introduction-to-icloud-images/icloudstorage.png#lightbox)



Пользователи также будут получать различные предупреждения при попытке удалить приложение включен iCloud со своего устройства, сообщите ему о состоянии iCloud документы, относящиеся к данному приложению.

 [![](introduction-to-icloud-images/icloud-delete1.png "Пример диалогового окна, при попытке пользователя удалить приложение включен iCloud со своего устройства")](introduction-to-icloud-images/icloud-delete1.png#lightbox)

 [![](introduction-to-icloud-images/icloud-delete2.png "Пример диалогового окна, при попытке пользователя удалить приложение включен iCloud со своего устройства")](introduction-to-icloud-images/icloud-delete2.png#lightbox)

## <a name="icloud-backup"></a>iCloud резервной копии

Хотя резервное копирование iCloud не является функцией, которая осуществляется непосредственно с разработчиками, на способ разработки приложения может повлиять на взаимодействие с пользователем.
Компания Apple предоставляет [iOS правила хранения данных](http://developer.apple.com/icloud/documentation/data-storage/) для разработчиков в приложениях iOS.

Наиболее важным является ли ваше приложение хранит большие файлы, не созданное пользователем (например, приложение читатель журнала, в котором хранится содержимое на решение проблемы hundred-plus мегабайт). Apple отдает предпочтение не храните эту сортировку данных, где она будет иметь резервную копию в iCloud и без необходимости заполнения квоты пользователя iCloud.

Приложения, которые хранить большие объемы данных следующим образом должен быть либо хранить в одном из каталогов пользователя, не является резервной копии (например) Кэш-память или tmp) или использовать `NSFileManager.SetSkipBackupAttribute` для применения к этим файлам флаг, чтобы их игнорирует iCloud во время резервного копирования.

## <a name="summary"></a>Сводка

В этой статье представлена новая функция iCloud, включенных в iOS 5. Он проверяет шаги, необходимые для настройки проекта для использования iCloud и затем приводятся примеры реализации возможностей iCloud.

В примере ключ значение показано, как iCloud может использоваться для хранения небольшой объем данных, аналогично тому, как хранятся NSUserPreferences. В примере UIDocument, показано, как более сложные данные могут храниться и синхронизации на нескольких устройствах через iCloud.

Наконец он включен краткое на как добавление iCloud резервной копии следует влияют на дизайн вашего приложения.



## <a name="related-links"></a>Связанные ссылки

- [Введение в iCloud (пример)](https://developer.xamarin.com/samples/monotouch/IntroductionToiCloud)
- [Образец кода семинар iCloud](https://github.com/xamarin/Seminars/tree/master/2012-03-22-iCloud)
- [Слайды семинар iCloud](http://www.slideshare.net/Xamarin/using-icloud-with-monotouch)
- [iCloud NSUbiquitousKeyValueStore](https://developer.apple.com/library/prerelease/ios/)
- [Хранилище iCloud](http://support.apple.com/kb/HT4847)
