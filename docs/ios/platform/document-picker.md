---
title: "Выбор документа"
description: "Выбор документа представление контроллер предоставляет пользователям доступ к файлам вне приложения \"песочницы\". Это простой механизм для общего доступа к документам между приложениями. Он также обеспечивает более сложные рабочие процессы, так как пользователи могут изменять одного документа с несколькими приложениями. В этой статье содержатся вводные сведения с помощью средства выбора документа в приложении Xamarin.iOS и изменения в документах iCloud, необходимые для его поддержки."
ms.topic: article
ms.prod: xamarin
ms.assetid: 89539D79-BC6E-4A3E-AEC6-69D9A6CC6818
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4a8f1632076a12b1737ba8294ac8b2f28f19dc77
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="document-picker"></a>Выбор документа

_Выбор документа представление контроллер предоставляет пользователям доступ к файлам вне приложения "песочницы". Это простой механизм для общего доступа к документам между приложениями. Он также обеспечивает более сложные рабочие процессы, так как пользователи могут изменять одного документа с несколькими приложениями. В этой статье содержатся вводные сведения с помощью средства выбора документа в приложении Xamarin.iOS и изменения в документах iCloud, необходимые для его поддержки._

Выбор документа позволяет документы совместно использоваться приложениями. Эти документы могут храниться в iCloud или в каталоге другое приложение. Через набор общих документов [расширения поставщика документов](~/ios/platform/extensions.md) пользователь установил на своем устройстве. 

Из-за сложности поддержания синхронизируются по приложениям и документы они повышают некоторые необходимые сложности.

## <a name="requirements"></a>Требования

Для завершения действия, описанные в этой статье требуется следующее:

-  **Xcode 7 и iOS 8 или более новой версии** — Apple Xcode 7 и iOS 8 или более новые API необходимо установить и настроить на компьютере разработчика.
-  **Visual Studio или Visual Studio для Mac** — следует установить последнюю версию Visual Studio для Mac.
-  **Устройства iOS** — устройства iOS под управлением iOS 8 или более поздней версии.

## <a name="changes-to-icloud"></a>Изменения в iCloud

Для реализации новых возможностей выбора документа, в компании Apple iCloud службы были внесены следующие изменения:

-  ICloud управляющей программы полностью переписан с помощью CloudKit.
-  Существующие iCloud, компоненты были переименованы iCloud диска.
-  Добавлена поддержка для ОС Microsoft Windows в iCloud.
-  В Mac OS Finder добавлена в папку iCloud.
-  устройства iOS можно открывать содержимое папки iCloud Mac OS.


## <a name="what-is-a-document"></a>Что такое документ?

При обращении к документу в iCloud он один, автономный сущности и восприятия таким образом пользователь. Пользователь может потребоваться изменить документ или совместно с другими пользователями (с помощью электронной почты, например).

Существует несколько типов файлов, пользователь сразу же распознает как документы, например страниц, доклад или числа файлов. Однако iCloud не ограничивается этой концепции. Например состояние игры (например, соответствие шахматной) можно обрабатываются как документ и хранятся в iCloud. Этот файл может передаваться между устройствами пользователя, давая возможность выбора игры, он остановился на другом устройстве.

## <a name="dealing-with-documents"></a>Работа с документами

Прежде чем углубляться в код, необходимый для использования средства выбора документа с помощью Xamarin, в этой статье будет охватывать рекомендации по работе с документами iCloud и некоторые изменения, внесенные в существующие API требуется для поддержки выбора документа.

### <a name="using-file-coordination"></a>С помощью файла координации

Поскольку можно изменять файл из нескольких мест, координации должна использоваться для предотвращения потери данных.

 [![](document-picker-images/image1.png "С помощью файла координации")](document-picker-images/image1.png#lightbox)

Давайте рассмотрим выше рисунке:

1.  Устройства iOS с помощью файла координации создает новый документ и сохраняет его в папку icloud.
2.  iCloud сохраняет измененный файл в облако для распространения каждому устройству.
3.  Вложенные Mac видит измененный файл в iCloud папки и использует файл координации скопировать изменения в файл.
4.  Устройство не с помощью файла координации вносит изменения в файл и сохраняет его в папку icloud. Мгновенно эти изменения реплицируются на другие устройства.

Предположим, исходное устройство iOS или MAC-адрес редактирования файла, теперь потеряно и заменены версию файла с несогласованных устройства свои изменения. Чтобы предотвратить потерю данных, координации файла является обязательным при работе с документами на основе облака.

### <a name="using-uidocument"></a>С помощью UIDocument

 `UIDocument` упрощает действия (или `NSDocument` на macOS), выполнив все часть тяжелой работы для разработчиков. Он предоставляет созданного файла совместно с помощью фоновой очереди для предотвращения блокировки пользовательского интерфейса приложения.

 `UIDocument` предоставляет доступ к нескольким, требует высокого уровня API, которые облегчают усилия разработчиков приложения Xamarin с любой целью разработчика.

В следующем коде создается подкласс `UIDocument` реализовать универсальный текстовый документ, который может использоваться для хранения и извлечения текста из iCloud:

```csharp
using System;
using Foundation;
using UIKit;

namespace DocPicker
{
    public class GenericTextDocument : UIDocument
    {
        #region Private Variable Storage
        private NSString _dataModel;
        #endregion

        #region Computed Properties
        public string Contents {
            get { return _dataModel.ToString (); }
            set { _dataModel = new NSString(value); }
        }
        #endregion

        #region Constructors
        public GenericTextDocument (NSUrl url) : base (url)
        {
            // Set the default document text
            this.Contents = "";
        }

        public GenericTextDocument (NSUrl url, string contents) : base (url)
        {
            // Set the default document text
            this.Contents = contents;
        }
        #endregion

        #region Override Methods
        public override bool LoadFromContents (NSObject contents, string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Were any contents passed to the document?
            if (contents != null) {
                _dataModel = NSString.FromData( (NSData)contents, NSStringEncoding.UTF8 );
            }

            // Inform caller that the document has been modified
            RaiseDocumentModified (this);

            // Return success
            return true;
        }

        public override NSObject ContentsForType (string typeName, out NSError outError)
        {
            // Clear the error state
            outError = null;

            // Convert the contents to a NSData object and return it
            NSData docData = _dataModel.Encode(NSStringEncoding.UTF8);
            return docData;
        }
        #endregion

        #region Events
        public delegate void DocumentModifiedDelegate(GenericTextDocument document);
        public event DocumentModifiedDelegate DocumentModified;

        internal void RaiseDocumentModified(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentModified != null) {
                this.DocumentModified (document);
            }
        }
        #endregion
    }
}
```

`GenericTextDocument` Представленные выше класс будет использоваться в этой статье, при работе с выбора документа и внешних документов в приложении Xamarin.iOS 8.

## <a name="asynchronous-file-coordination"></a>Асинхронный файловый координации

iOS 8 представлено несколько новых функций координации асинхронной файл через новые интерфейсы API координации файла. Перед iOS 8 все существующие API координации файла были полностью синхронной. Это означает, что разработчик отвечал за фон очередь для предотвращения блокировки пользовательского интерфейса приложения координации файл реализации.

Новый `NSFileAccessIntent` класс содержит URL-адрес, указывающий на файл и несколько параметров для управления типом координацию, запрашиваемую. В следующем коде показано перемещение файла из одного расположения в другое с помощью целей:

```csharp
// Get source options
var srcURL = NSUrl.FromFilename ("FromFile.txt");
var srcIntent = NSFileAccessIntent.CreateReadingIntent (srcURL, NSFileCoordinatorReadingOptions.ForUploading);

// Get destination options
var dstURL = NSUrl.FromFilename ("ToFile.txt");
var dstIntent = NSFileAccessIntent.CreateReadingIntent (dstURL, NSFileCoordinatorReadingOptions.ForUploading);

// Create an array
var intents = new NSFileAccessIntent[] {
    srcIntent,
    dstIntent
};

// Initialize a file coordination with intents
var queue = new NSOperationQueue ();
var fileCoordinator = new NSFileCoordinator ();
fileCoordinator.CoordinateAccess (intents, queue, (err) => {
    // Was there an error?
    if (err!=null) {
        Console.WriteLine("Error: {0}",err.LocalizedDescription);
    }
});
```

## <a name="discovering-and-listing-documents"></a>Обнаружение и списка документов

Способ обнаружения и перечисление документов — с помощью существующего `NSMetadataQuery` API-интерфейсы. В этом разделе рассматриваются новые возможности, добавленные к `NSMetadataQuery` , упрощают работу с документами еще проще, чем до.

### <a name="existing-behavior"></a>Поведение существующих

До 8, iOS `NSMetadataQuery` замедлялось для изменения локального файла раскладки например: удаление, создание и изменение имени.

 [![](document-picker-images/image2.png "Обзор изменения локального файла NSMetadataQuery")](document-picker-images/image2.png#lightbox)

На схеме выше:

1.  Для файлов, которые уже существуют в контейнере приложения `NSMetadataQuery` имеющую существующие `NSMetadata` записи уже создана и таким образом, они будут мгновенно доступны для приложения в очереди.
1.  Приложение создает новый файл в контейнере приложения.
1.  Задержка перед `NSMetadataQuery` видит изменения к контейнеру приложения и создает необходимые `NSMetadata` записи.


Из-за задержки при создании `NSMetadata` записи, приложения должны были открыть источники данных: один для изменения локального файла и один для облака на основе изменений.

### <a name="stitching"></a>Объединение

В iOS 8 `NSMetadataQuery` проще в использовании непосредственно с новая функция, называемая объединение:

 [![](document-picker-images/image3.png "Объединение вызывается NSMetadataQuery с помощью новой функции")](document-picker-images/image3.png#lightbox)

С помощью объединение на схеме выше:

1.  Как и ранее, для файлов, которые уже существуют в контейнере приложения `NSMetadataQuery` имеющую существующие `NSMetadata` записи уже создана и помещаются в очередь.
1.  Приложение создает новый файл в контейнере приложения, с помощью файла координации.
1.  Обработчик в контейнере приложения видит изменения и вызовы `NSMetadataQuery` для создания необходимых `NSMetadata` записи.
1.  `NSMetadata` Запись создается непосредственно после файл и становится доступным для приложения.


С помощью объединение приложение больше не нужно открыть источник данных для наблюдения за локальной и облаков, изменения в файле. Теперь приложение можно положиться на `NSMetadataQuery` напрямую.

> [!IMPORTANT]
> **Примечание**: объединение работает, только если приложение использует файл координации представленный в предыдущем разделе. Если файл координации не используется, API-интерфейсы по умолчанию поведение операционных систем iOS 8.




### <a name="new-ios-8-metadata-features"></a>Новые возможности метаданных iOS 8

Были добавлены следующие новые функции `NSMetadataQuery` в iOS 8:

-   `NSMetatadataQuery` Теперь можно перечислить нелокальных документы, хранящиеся в облаке.
-  Были добавлены новые интерфейсы API для доступа к информации метаданных для документов на основе облака. 
-  Новая `NSUrl_PromisedItems` API, который будет получить доступ к файлу атрибуты файлов, которые могут не иметь их содержимое доступно локально.
-  Используйте `GetPromisedItemResourceValue` метод, чтобы получить сведения о данного файла, либо используйте `GetPromisedItemResourceValues` метод, чтобы получить информацию о более одного файла за раз.


Для работы с метаданными были добавлены два новых флагов координации файла:

-   `NSFileCoordinatorReadImmediatelyAvailableMetadataOnly` 
-   `NSFileCoordinatorWriteContentIndependentMetadataOnly` 


Флаги, то выше содержимое файла документа не обязательно должны быть доступны локально для использования.

Следующий фрагмент кода показывает использование `NSMetadataQuery` для запроса наличия определенного файла и создать файл, если он не существует:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;


#region Static Properties
public const string TestFilename = "test.txt"; 
#endregion

#region Computed Properties
public bool HasiCloud { get; set; }
public bool CheckingForiCloud { get; set; }
public NSUrl iCloudUrl { get; set; }

public GenericTextDocument Document { get; set; }
public NSMetadataQuery Query { get; set; }
#endregion

#region Private Methods
private void FindDocument () {
    Console.WriteLine ("Finding Document...");

    // Create a new query and set it's scope
    Query = new NSMetadataQuery();
    Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

    // Build a predicate to locate the file by name and attach it to the query
    var pred = NSPredicate.FromFormat ("%K == %@"
        , new NSObject[] {
            NSMetadataQuery.ItemFSNameKey
            , new NSString(TestFilename)});
    Query.Predicate = pred;

    // Register a notification for when the query returns
    NSNotificationCenter.DefaultCenter.AddObserver (this,
            new Selector("queryDidFinishGathering:"),           NSMetadataQuery.DidFinishGatheringNotification,
            Query);

    // Start looking for the file
    Query.StartQuery ();
    Console.WriteLine ("Querying: {0}", Query.IsGathering);
}

[Export("queryDidFinishGathering:")]
public void DidFinishGathering (NSNotification notification) {
    Console.WriteLine ("Finish Gathering Documents.");

    // Access the query and stop it from running
    var query = (NSMetadataQuery)notification.Object;
    query.DisableUpdates();
    query.StopQuery();

    // Release the notification
    NSNotificationCenter.DefaultCenter.RemoveObserver (this
        , NSMetadataQuery.DidFinishGatheringNotification
        , query);

    // Load the document that the query returned
    LoadDocument(query);
}

private void LoadDocument (NSMetadataQuery query) {
    Console.WriteLine ("Loading Document...");    

    // Take action based on the returned record count
    switch (query.ResultCount) {
    case 0:
        // Create a new document
        CreateNewDocument ();
        break;
    case 1:
        // Gain access to the url and create a new document from
        // that instance
        NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
        var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

        // Load the document
        OpenDocument (url);
        break;
    default:
        // There has been an issue
        Console.WriteLine ("Issue: More than one document found...");
        break;
    }
}
#endregion

#region Public Methods
public void OpenDocument(NSUrl url) {

    Console.WriteLine ("Attempting to open: {0}", url);
    Document = new GenericTextDocument (url);

    // Open the document
    Document.Open ( (success) => {
        if (success) {
            Console.WriteLine ("Document Opened");
        } else
            Console.WriteLine ("Failed to Open Document");
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public void CreateNewDocument() {
    // Create path to new file
    // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
    var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
    var docPath = Path.Combine (docsFolder, TestFilename);
    var ubiq = new NSUrl (docPath, false);

    // Create new document at path 
    Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
    Document = new GenericTextDocument (ubiq);

    // Set the default value
    Document.Contents = "(default value)";

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
        Console.WriteLine ("Save completion:" + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
        } else {
            Console.WriteLine ("Unable to Save Document");
        }
    });

    // Inform caller
    RaiseDocumentLoaded (Document);
}

public bool SaveDocument() {
    bool successful = false;

    // Save document to path
    Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
        Console.WriteLine ("Save completion: " + saveSuccess);
        if (saveSuccess) {
            Console.WriteLine ("Document Saved");
            successful = true;
        } else {
            Console.WriteLine ("Unable to Save Document");
            successful=false;
        }
    });

    // Return results
    return successful;
}
#endregion

#region Events
public delegate void DocumentLoadedDelegate(GenericTextDocument document);
public event DocumentLoadedDelegate DocumentLoaded;

internal void RaiseDocumentLoaded(GenericTextDocument document) {
    // Inform caller
    if (this.DocumentLoaded != null) {
        this.DocumentLoaded (document);
    }
}
#endregion
```

### <a name="document-thumbnails"></a>Работать с эскизами документов

Apple выбраны оптимального взаимодействия с пользователем при перечислении документов для приложения для использования предварительных версий. Это обеспечивает контекст конечных пользователей, их можно легко находить документ, который хочет работать с.

До iOS 8 показывающая эскизы документов требуется пользовательскую реализацию. Опыта работы с iOS 8, атрибутов файловой системы, которые позволяют разработчику быстро работать с эскизами документов.

#### <a name="retrieving-document-thumbnails"></a>Получение работать с эскизами документов 

Путем вызова `GetPromisedItemResourceValue` или `GetPromisedItemResourceValues` методы, `NSUrl_PromisedItems` API, `NSUrlThumbnailDictionary`, возвращается. Только в настоящее время в этом словаре лежат `NSThumbnial1024X1024SizeKey` , а также его `UIImage`.

#### <a name="saving-document-thumbnails"></a>Сохранение работать с эскизами документов

Самый простой способ сохранить эскиз — с помощью `UIDocument`. Путем вызова `GetFileAttributesToWrite` метод `UIDocument` и установка эскиза, будут автоматически сохранены при открывании файла документа. ICloud управляющая программа будет видит это изменение и передать их в iCloud. В Mac OS X эскизы автоматически создаваемых разработчика подключаемый модуль быстрого поиска.

С основами работы с документами на основе iCloud на месте, а также изменения в существующие интерфейсы API, мы готовы к реализации View Controller выбора документа в Xamarin iOS 8 мобильного приложения.


## <a name="enabling-icloud-in-xamarin"></a>Включение iCloud в Xamarin

Прежде чем средство выбора документа можно использовать в приложении Xamarin.iOS, поддержка iCloud необходимо включить в приложение и через Apple. 

Следующий пример действия в процессе подготовки для iCloud.

1. Создайте контейнер iCloud.
2. Создайте идентификатор приложения, содержащий iCloud службы приложений.
3. Создать профиль подготовки, который содержит этот код приложения.

[Работа с возможностями](~/ios/deploy-test/provisioning/capabilities/icloud-capabilities.md) руководстве рассматриваются первых двух шагов. Чтобы создать профиль подготовки, выполните действия, описанные в [профиль подготовки](~/ios/get-started/installation/device-provisioning/index.md#Provisioning_Profile) руководства.



Следующий пример шаги процесса настройки приложения для iCloud:

Выполните следующие действия:

1.  Откройте проект в Visual Studio для Mac или Visual Studio.
2.  В **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите параметры.
3.  В диалоговое окно «Параметры» выберите **приложение iOS**, убедитесь, что **идентификатор пакета** совпадает со структурой, которое было определено в **идентификатор приложения** созданной выше для приложения. 
4.  Выберите **подписывание пакета iOS**выберите **удостоверение разработчика** и **профиль подготовки** созданной выше.
5.  Нажмите кнопку **ОК** кнопку, чтобы сохранить изменения и закрыть диалоговое окно.
6.  Щелкните правой кнопкой мыши `Entitlements.plist` в **обозревателе решений** чтобы открыть его в редакторе.

    > [!IMPORTANT]
> **Примечание**: В Visual Studio может потребоваться открыть редактор прав, щелкнув его, выбрав **открыть с помощью...** и выбрав редактор списка свойств

7.  Проверьте **включить iCloud** , **документов с iCloud** , **значение ключа хранилища** и **CloudKit** .
8.  Убедитесь, **контейнера** существует для приложения (как созданных ранее). Пример: `iCloud.com.your-company.AppName`
9.  Сохраните изменения в файле.

Дополнительные сведения о правах посвящены [работа с данными](~/ios/deploy-test/provisioning/entitlements.md) руководства.

С указанными параметрами в месте приложение теперь можно использовать документов на основе облака и новый контроллер представление выбора документа.

## <a name="common-setup-code"></a>Общий код установки

Перед началом работы с контроллером выбора представления документа отсутствует код стандартной установки требуется. Начать с изменения приложения `AppDelegate.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading;
using Foundation;
using UIKit;
using ObjCRuntime;
using System.IO;

namespace DocPicker
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        #region Static Properties
        public const string TestFilename = "test.txt"; 
        #endregion

        #region Computed Properties
        public override UIWindow Window { get; set; }
        public bool HasiCloud { get; set; }
        public bool CheckingForiCloud { get; set; }
        public NSUrl iCloudUrl { get; set; }

        public GenericTextDocument Document { get; set; }
        public NSMetadataQuery Query { get; set; }
        public NSData Bookmark { get; set; }
        #endregion

        #region Private Methods
        private void FindDocument () {
            Console.WriteLine ("Finding Document...");

            // Create a new query and set it's scope
            Query = new NSMetadataQuery();
            Query.SearchScopes = new NSObject [] {
                NSMetadataQuery.UbiquitousDocumentsScope,
                NSMetadataQuery.UbiquitousDataScope,
                NSMetadataQuery.AccessibleUbiquitousExternalDocumentsScope
            };

            // Build a predicate to locate the file by name and attach it to the query
            var pred = NSPredicate.FromFormat ("%K == %@",
                new NSObject[] {NSMetadataQuery.ItemFSNameKey
                , new NSString(TestFilename)});
            Query.Predicate = pred;

            // Register a notification for when the query returns
            NSNotificationCenter.DefaultCenter.AddObserver (this
                , new Selector("queryDidFinishGathering:")
                , NSMetadataQuery.DidFinishGatheringNotification
                , Query);

            // Start looking for the file
            Query.StartQuery ();
            Console.WriteLine ("Querying: {0}", Query.IsGathering);
        }


        [Export("queryDidFinishGathering:")]
        public void DidFinishGathering (NSNotification notification) {
            Console.WriteLine ("Finish Gathering Documents.");

            // Access the query and stop it from running
            var query = (NSMetadataQuery)notification.Object;
            query.DisableUpdates();
            query.StopQuery();

            // Release the notification
            NSNotificationCenter.DefaultCenter.RemoveObserver (this
                , NSMetadataQuery.DidFinishGatheringNotification
                , query);

            // Load the document that the query returned
            LoadDocument(query);
        }

        private void LoadDocument (NSMetadataQuery query) {
            Console.WriteLine ("Loading Document...");  

            // Take action based on the returned record count
            switch (query.ResultCount) {
            case 0:
                // Create a new document
                CreateNewDocument ();
                break;
            case 1:
                // Gain access to the url and create a new document from
                // that instance
                NSMetadataItem item = (NSMetadataItem)query.ResultAtIndex (0);
                var url = (NSUrl)item.ValueForAttribute (NSMetadataQuery.ItemURLKey);

                // Load the document
                OpenDocument (url);
                break;
            default:
                // There has been an issue
                Console.WriteLine ("Issue: More than one document found...");
                break;
            }
        }
        #endregion

        #region Public Methods

        public void OpenDocument(NSUrl url) {

            Console.WriteLine ("Attempting to open: {0}", url);
            Document = new GenericTextDocument (url);

            // Open the document
            Document.Open ( (success) => {
                if (success) {
                    Console.WriteLine ("Document Opened");
                } else
                    Console.WriteLine ("Failed to Open Document");
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        public void CreateNewDocument() {
            // Create path to new file
            // var docsFolder = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var docsFolder = Path.Combine(iCloudUrl.Path, "Documents");
            var docPath = Path.Combine (docsFolder, TestFilename);
            var ubiq = new NSUrl (docPath, false);

            // Create new document at path 
            Console.WriteLine ("Creating Document at:" + ubiq.AbsoluteString);
            Document = new GenericTextDocument (ubiq);

            // Set the default value
            Document.Contents = "(default value)";

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForCreating, (saveSuccess) => {
                Console.WriteLine ("Save completion:" + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                } else {
                    Console.WriteLine ("Unable to Save Document");
                }
            });

            // Inform caller
            RaiseDocumentLoaded (Document);
        }

        /// <summary>
        /// Saves the document.
        /// </summary>
        /// <returns><c>true</c>, if document was saved, <c>false</c> otherwise.</returns>
        public bool SaveDocument() {
            bool successful = false;

            // Save document to path
            Document.Save (Document.FileUrl, UIDocumentSaveOperation.ForOverwriting, (saveSuccess) => {
                Console.WriteLine ("Save completion: " + saveSuccess);
                if (saveSuccess) {
                    Console.WriteLine ("Document Saved");
                    successful = true;
                } else {
                    Console.WriteLine ("Unable to Save Document");
                    successful=false;
                }
            });

            // Return results
            return successful;
        }
        #endregion

        #region Override Methods
        public override void FinishedLaunching (UIApplication application)
        {

            // Start a new thread to check and see if the user has iCloud
            // enabled.
            new Thread(new ThreadStart(() => {
                // Inform caller that we are checking for iCloud
                CheckingForiCloud = true;

                // Checks to see if the user of this device has iCloud
                // enabled
                var uburl = NSFileManager.DefaultManager.GetUrlForUbiquityContainer(null);

                // Connected to iCloud?
                if (uburl == null)
                {
                    // No, inform caller
                    HasiCloud = false;
                    iCloudUrl =null;
                    Console.WriteLine("Unable to connect to iCloud");
                    InvokeOnMainThread(()=>{
                        var okAlertController = UIAlertController.Create ("iCloud Not Available", "Developer, please check your Entitlements.plist, Bundle ID and Provisioning Profiles.", UIAlertControllerStyle.Alert);
                        okAlertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        Window.RootViewController.PresentViewController (okAlertController, true, null);
                    });
                }
                else
                {   
                    // Yes, inform caller and save location the the Application Container
                    HasiCloud = true;
                    iCloudUrl = uburl;
                    Console.WriteLine("Connected to iCloud");

                    // If we have made the connection with iCloud, start looking for documents
                    InvokeOnMainThread(()=>{
                        // Search for the default document
                        FindDocument ();
                    });
                }

                // Inform caller that we are no longer looking for iCloud
                CheckingForiCloud = false;

            })).Start();
                
        }
        
        // This method is invoked when the application is about to move from active to inactive state.
        // OpenGL applications should use this method to pause.
        public override void OnResignActivation (UIApplication application)
        {
        }
        
        // This method should be used to release shared resources and it should store the application state.
        // If your application supports background execution this method is called instead of WillTerminate
        // when the user quits.
        public override void DidEnterBackground (UIApplication application)
        {
            // Trap all errors
            try {
                // Values to include in the bookmark packet
                var resources = new string[] {
                    NSUrl.FileSecurityKey,
                    NSUrl.ContentModificationDateKey,
                    NSUrl.FileResourceIdentifierKey,
                    NSUrl.FileResourceTypeKey,
                    NSUrl.LocalizedNameKey
                };

                // Create the bookmark
                NSError err;
                Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

                // Was there an error?
                if (err != null) {
                    // Yes, report it
                    Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
                }
            }
            catch (Exception e) {
                // Report error
                Console.WriteLine ("Error: {0}", e.Message);
            }
        }
        
        // This method is called as part of the transition from background to active state.
        public override void WillEnterForeground (UIApplication application)
        {
            // Is there any bookmark data?
            if (Bookmark != null) {
                // Trap all errors
                try {
                    // Yes, attempt to restore it
                    bool isBookmarkStale;
                    NSError err;
                    var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

                    // Was there an error?
                    if (err != null) {
                        // Yes, report it
                        Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
                    } else {
                        // Load document from bookmark
                        OpenDocument (srcUrl);
                    }
                }
                catch (Exception e) {
                    // Report error
                    Console.WriteLine ("Error: {0}", e.Message);
                }
            }

        }
        
        // This method is called when the application is about to terminate. Save data, if needed.
        public override void WillTerminate (UIApplication application)
        {
        }
        #endregion

        #region Events
        public delegate void DocumentLoadedDelegate(GenericTextDocument document);
        public event DocumentLoadedDelegate DocumentLoaded;

        internal void RaiseDocumentLoaded(GenericTextDocument document) {
            // Inform caller
            if (this.DocumentLoaded != null) {
                this.DocumentLoaded (document);
            }
        }
        #endregion
    }
}

```

> [!IMPORTANT]
> **Примечание**: приведенный выше код включает код из раздела Discovering и список документов выше. Оно выводится здесь целиком, так же, как в настоящем приложении. Для простоты в этом примере работает с один файл жестко (`test.txt`) только.

Приведенный выше код предоставляет несколько клавиш iCloud диска для упрощения их работы в остальной части приложения.

Добавьте следующий код в любое представление или контейнер представления, который будет с помощью палитры документа или работа с документами на основе облака.

```csharp
using CloudKit;
...

#region Computed Properties
/// <summary>
/// Returns the delegate of the current running application
/// </summary>
/// <value>The this app.</value>
public AppDelegate ThisApp {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
#endregion
```

При этом добавляется ярлык для получения `AppDelegate` и получить доступ к iCloud клавиш, созданной ранее.

Этот код в месте давайте рассмотрим реализация View Controller выбора документа в приложении iOS 8 Xamarin.

## <a name="using-the-document-picker-view-controller"></a>С помощью контроллера выбора представления документа

До появления iOS 8 было очень сложно получить доступ к документам из другого приложения, так как не было возможности обнаруживать документы вне приложения из приложения.

### <a name="existing-behavior"></a>Поведение существующих

 [![](document-picker-images/image31.png "Общие сведения о существующих поведение")](document-picker-images/image31.png#lightbox)

Давайте рассмотрим доступ к внешнему документу до iOS 8:

1.  Сначала пользователю необходимо открыть приложение, который первоначально создал документ.
1.  Выбран документа и `UIDocumentInteractionController` используется для отправки документа для нового приложения.
1.  Наконец копии исходного документа помещается в контейнер нового приложения.


Оттуда документ доступен для второго приложения для открытия и изменения.

### <a name="discovering-documents-outside-of-an-apps-container"></a>Обнаружение документы вне контейнера приложения

В iOS 8 приложению доступ к документам вне контейнера приложения с легкостью:

 [![](document-picker-images/image32.png "Обнаружение документы вне контейнера приложения")](document-picker-images/image32.png#lightbox)

С помощью нового iCloud выбора документа ( `UIDocumentPickerViewController`), приложение iOS могут непосредственно обнаружить и доступ вне контейнера приложения. `UIDocumentPickerViewController` Предоставляет механизм для пользователя для предоставления доступа и изменить их обнаружения документы с помощью разрешений.

Приложения должны согласиться на его документы отображаться в iCloud документа выбора и быть доступны для других приложений для обнаружения и работать с ними. Чтобы приложение Xamarin iOS 8 совместно использовать приложения контейнера, изменить его `Info.plist` в стандартном текстовом редакторе и добавьте следующие две строки в конец словаря (между `<dict>...</dict>` теги):

```xml
<key>NSUbiquitousContainerIsDocumentScopePublic</key>
<true/>
```

`UIDocumentPickerViewController` Предоставляет отличный новый пользовательский Интерфейс, который позволяет пользователю выбирать документов. Чтобы отобразить представление контроллер выбора документа в приложении Xamarin iOS 8, выполните следующие действия:

```csharp
using MobileCoreServices;
...

// Allow the Document picker to select a range of document types
        var allowedUTIs = new string[] {
            UTType.UTF8PlainText,
            UTType.PlainText,
            UTType.RTF,
            UTType.PNG,
            UTType.Text,
            UTType.PDF,
            UTType.Image
        };

        // Display the picker
        //var picker = new UIDocumentPickerViewController (allowedUTIs, UIDocumentPickerMode.Open);
        var pickerMenu = new UIDocumentMenuViewController(allowedUTIs, UIDocumentPickerMode.Open);
        pickerMenu.DidPickDocumentPicker += (sender, args) => {

            // Wireup Document Picker
            args.DocumentPicker.DidPickDocument += (sndr, pArgs) => {

                // IMPORTANT! You must lock the security scope before you can
                // access this file
                var securityEnabled = pArgs.Url.StartAccessingSecurityScopedResource();

                // Open the document
                ThisApp.OpenDocument(pArgs.Url);

                // IMPORTANT! You must release the security lock established
                // above.
                pArgs.Url.StopAccessingSecurityScopedResource();
            };

            // Display the document picker
            PresentViewController(args.DocumentPicker,true,null);
        };

pickerMenu.ModalPresentationStyle = UIModalPresentationStyle.Popover;
PresentViewController(pickerMenu,true,null);
UIPopoverPresentationController presentationPopover = pickerMenu.PopoverPresentationController;
if (presentationPopover!=null) {
    presentationPopover.SourceView = this.View;
    presentationPopover.PermittedArrowDirections = UIPopoverArrowDirection.Down;
    presentationPopover.SourceRect = ((UIButton)s).Frame;
}
```

> [!IMPORTANT]
> **Примечание**: разработчик должен вызвать `StartAccessingSecurityScopedResource` метод `NSUrl` можно получить доступ к внешнему документу. `StopAccessingSecurityScopedResource` Метод должен вызываться для снятия блокировки безопасности сразу после загрузки этого документа.

### <a name="sample-output"></a>Пример результатов выполнения

Ниже приведен пример, как приведенный выше код отобразит выбора документа, когда запускается на устройстве iPhone:

1.  Пользователь запускает приложение, и выводится это основной интерфейс:   
 
    [![](document-picker-images/image33.png "Это основной интерфейс отображается")](document-picker-images/image33.png#lightbox)
1.  Касания пользователь **действия** кнопку в верхней части экрана и будет предложено выбрать **документа поставщика** из списка доступных поставщиков:   
 
    [![](document-picker-images/image34.png "Выберите из списка доступных поставщиков поставщик документа")](document-picker-images/image34.png#lightbox)
1.  **Документа выбора View Controller** отображается для выбранного **документа поставщика**:   
 
    [![](document-picker-images/image35.png "Отображается представление контроллер выбора документа")](document-picker-images/image35.png#lightbox)
1.  Пользователь нажимает на **папку документов** для отображения содержимого:   
 
    [![](document-picker-images/image36.png "Содержимое папки документов")](document-picker-images/image36.png#lightbox)
1.  Пользователь выбирает **документа** и **выбора документа** закрыт.
1.  Это основной интерфейс отобразится, **документа** загружается из внешнего контейнера и отображения его содержимого.


Фактическое отображение контроллера выбора представления документа зависит от поставщиков документа, что пользователь установил на устройстве и режима выбора документа было реализовать. Приведенный выше пример использует режим открытия, другие типы режим будет подробно ниже.

## <a name="managing-external-documents"></a>Управление документами на внешних

Как отмечалось выше, перед iOS 8, приложение может доступ только к документы, которые были частью приложения контейнера. В iOS 8. приложение может получить доступ к документам из внешних источников:

 [![](document-picker-images/image37.png "Общие сведения об управлении внешних документов")](document-picker-images/image37.png#lightbox)

Когда пользователь выбирает документа из внешнего источника, ссылочного документа записывается в приложение контейнера, который указывает на исходный документ.

Чтобы упростить добавление этой новой возможности в существующие приложения, были добавлены несколько новых возможностей для `NSMetadataQuery` API. Как правило приложение использует универсальной области документа в список документов, находящихся в контейнере приложения. С помощью этой области, документы только в контейнере приложения будет продолжать отображаться.

С помощью новой универсальной внешней области документа будут возвращать документы, live вне контейнера приложения и возвращать метаданные для них. `NSMetadataItemUrlKey` Будет указывать на URL-адрес, где фактически находится документ.

Иногда приложение не хочет работать с документами, которые указывает ссылка th. Вместо этого приложение хочет работать непосредственно с справочный документ. Например приложение может потребоваться отобразить документ в папке приложения в пользовательском Интерфейсе или разрешить пользователю перемещать ссылки внутри папки.

В iOS 8 новый `NSMetadataItemUrlInLocalContainerKey` предоставляется доступ к справочный документ напрямую. Этот ключ указывает на фактическое ссылку на внешний документ в контейнере приложения.

`NSMetadataUbiquitousItemIsExternalDocumentKey` Используется для проверки ли документ находится за пределами приложения контейнера. `NSMetadataUbiquitousItemContainerDisplayNameKey` Используется для доступа к имени контейнера, который исходной копии внешнем документе, где размещены.

### <a name="why-document-references-are-required"></a>Почему требуются ссылки на документ

Основной причиной этого iOS 8, используемый для доступа к документам, внешние ссылки является безопасности. Приложение не получает доступ к контейнеру любого другого приложения. Только средства выбора документа можно сделать, поскольку выполнение out of process и имеют доступ на уровне системы.

Единственный способ получить документ вне контейнера приложения с помощью функции выбора документа, который Если URL-адрес, возвращаемый средства выбора уровня безопасности. URL-адрес области безопасности содержит достаточно информации для выбранного документа вместе с заданной областью права, необходимые для предоставления приложению доступа к документу.

Очень важно Обратите внимание, что если URL-адрес области безопасности был сериализован в строку и затем десериализованный, сведения о безопасности будут потеряны файл станет недоступным по URL-адресу. Компонент ссылки на документ предоставляет механизм для восстановления файлов, на который указывает URL-адресов.

Таким образом, если приложение получает `NSUrl` из одного из документов, он уже присоединен с областью безопасности и может использоваться для доступа к файлу. По этой причине настоятельно рекомендуется использовать `UIDocument` , так как он обрабатывает все эти сведения и процессов для них.

### <a name="using-bookmarks"></a>Использование закладок

Не всегда возможно для перечисления документы приложения, чтобы вернуться к конкретным документом, например, при выполнении восстановления состояния. iOS 8 предоставляет механизм для создания закладки, которые ориентированы данного документа.

Следующий код создает закладку из `UIDocument`в `FileUrl` свойство:

```csharp
// Trap all errors
try {
    // Values to include in the bookmark packet
    var resources = new string[] {
        NSUrl.FileSecurityKey,
        NSUrl.ContentModificationDateKey,
        NSUrl.FileResourceIdentifierKey,
        NSUrl.FileResourceTypeKey,
        NSUrl.LocalizedNameKey
    };

    // Create the bookmark
    NSError err;
    Bookmark = Document.FileUrl.CreateBookmarkData (NSUrlBookmarkCreationOptions.WithSecurityScope, resources, iCloudUrl, out err);

    // Was there an error?
    if (err != null) {
        // Yes, report it
        Console.WriteLine ("Error Creating Bookmark: {0}", err.LocalizedDescription);
    }
}
catch (Exception e) {
    // Report error
    Console.WriteLine ("Error: {0}", e.Message);
}
```

Существующие интерфейсы API закладки используется для создания закладки для существующего `NSUrl` , можно сохранять и загружать для предоставления прямого доступа во внешний файл. Следующий код будет восстановить закладка, которая была создана выше:

```csharp
if (Bookmark != null) {
    // Trap all errors
    try {
        // Yes, attempt to restore it
        bool isBookmarkStale;
        NSError err;
        var srcUrl = new NSUrl (Bookmark, NSUrlBookmarkResolutionOptions.WithSecurityScope, iCloudUrl, out isBookmarkStale, out err);

        // Was there an error?
        if (err != null) {
            // Yes, report it
            Console.WriteLine ("Error Loading Bookmark: {0}", err.LocalizedDescription);
        } else {
            // Load document from bookmark
            OpenDocument (srcUrl);
        }
    }
    catch (Exception e) {
        // Report error
        Console.WriteLine ("Error: {0}", e.Message);
    }
}
```

## <a name="open-vs-import-mode-and-the-document-picker"></a>Откройте vs. Режим import и выбора документа

Выбор представление документа контроллер функций два разных режима работы:

1.  **Режим открытия** — в этом режиме, когда пользователь выбирает и внешнего документа выбора документа будет создать закладку области безопасности в контейнере приложения.   
 
    [![](document-picker-images/image37.png "Закладки в контейнере приложения области безопасности")](document-picker-images/image37.png#lightbox)
1.  **Режим импорта** — в этом режиме, когда пользователь выбирает и внешнего документа, Выбор документа будет не создать закладку, но вместо этого скопируйте файл во временное местоположение и предоставить приложению доступ к документу в этом месте:   
 
    [![](document-picker-images/image38.png "Выбор документа будет скопировать файл во временное местоположение и предоставить приложению доступ в документ в этом расположении")](document-picker-images/image38.png#lightbox)   
 Когда приложение завершает работу по любой причине, очищается во временную папку и файл удален. Если приложение должно поддерживать доступ к файлу, необходимо создать копию и поместите его в приложение контейнера.


Откройте режим полезен в тех случаях, когда приложения, которые будут работать совместно с другим приложением и совместно использовать любые изменения, внесенные в документ с этим приложением. Режим импорта используется в том случае, если приложение необходимо передать его изменения в документ с другими приложениями.

## <a name="making-a-document-external"></a>Создание внешнего документа

Как указано выше, приложение iOS 8 не имеет доступа к контейнерам вне контейнера приложения. Приложение можно написать свой собственный контейнер локально или во временное местоположение, а затем позволяет переносить результирующем документе вне контейнера приложения пользователю выбрать местоположение режим специальных документов.

Для перемещения документа на внешний носитель, выполните следующие действия.

1.  Создайте новый документ в локальном, так и временное расположение.
1.  Создание `NSUrl` , указывающий, в новый документ.
1.  Откройте новый контроллер представление выбора документа и передать его `NSUrl` с режимом `MoveToService` . 
1.  Когда пользователь выбирает новое расположение, документ перемещается из ее текущего расположения в новое расположение.
1.  Справочный документ должны записываться в контейнер приложения приложения, чтобы файл по-прежнему можно осуществлять создание приложения.


Для перемещения документа на внешний носитель, можно использовать следующий код: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.MoveToService);`

Справочный документ, возвращаемый описанный выше процесс именно в том же созданные откройте режим выбора документа. Однако бывают случаи, которые приложение может потребоваться переместить документ без сохранения ссылку на него.

Чтобы переместить документ без генерации ссылки, используйте `ExportToService` режим. Пример: `var picker = new UIDocumentPickerViewController (srcURL, UIDocumentPickerMode.ExportToService);`

При использовании `ExportToService` режим, он копируется в внешний контейнер и имеющейся копии остается в исходном расположении.

## <a name="document-provider-extensions"></a>Расширения поставщика документов

С iOS 8 Apple хочет пользователь может иметь доступ к их документов на основе облака, независимо от того, где они действительно существуют. Для достижения этой цели, iOS 8 предоставляет новый механизм расширения поставщика документа.

### <a name="what-is-a-document-provider-extension"></a>Что такое расширение поставщика документа?

Проще говоря, расширение поставщика документа — это способ разработчик или сторонних разработчиков, чтобы обеспечить хранения альтернативных документа приложения, можно получить в точно так же, как место хранения существующих iCloud.

Пользователь может выбрать один из этих расположений альтернативные хранилища из палитры документа, и они могут использовать точное и теми же режимами доступа (Открытие, импорт, перемещения или экспорт) для работы с файлами в этом расположении.

Это реализуется с помощью двух разных модулей:

-  **Выбор расширения документов** — предоставляет `UIViewController` подкласс, в котором предоставляет графический интерфейс пользователя для выбора документа альтернативное расположение хранения. Этот подкласс будет отображаться как часть View Controller выбора документа.
-  **Укажите расширение файла** — это модуль без пользовательского интерфейса, который обрабатывает фактически предоставлять содержимое файлов. Эти расширения, предоставляются через файл координации ( `NSFileCoordinator` ). Это другой важный случай, когда необходим файл координации.


В примере ниже показан поток данных обычно при работе с поставщика расширения документов:

 [![](document-picker-images/image39.png "Этой схеме показан поток данных обычно, при работе с поставщика расширения документов")](document-picker-images/image39.png#lightbox)

Происходит следующий процесс.

1.  Приложение представляет контроллер выбора документа позволяет пользователю выбрать файл для работы с.
1.  Пользователь выбирает альтернативное расположение и файла пользовательского `UIViewController` расширение вызывается для отображения пользовательского интерфейса.
1.  Пользователь выбирает файл из этого расположения и URL-адрес передаются обратно в средство выбора документа.
1.  Выбор документа выбирает URL-адрес файла и возвращает его в приложение для пользователя, для работы с.
1.  URL-адрес, передаваемый координатора файл для возврата файлов содержимого в приложение.
1.  Координатор файла вызывает пользовательского поставщика расширение файла для получения файла.
1.  Содержимое файла возвращаются к координатору файла.
1.  Содержимое файла возвращается в приложение.


### <a name="security-and-bookmarks"></a>Безопасность и закладки

В этом разделе займет кратко рассмотрим, как безопасности и постоянный доступ к файлам через works закладки документа поставщика расширений. В отличие от iCloud поставщика документа, который автоматически сохраняет безопасности и закладки в контейнере приложения, документа поставщика расширения не так, как они не являются частью документа эталонной системы.

Например: на предприятии, предоставляющий свой собственный безопасное хранилище данных для всей компании, администраторы не хотите конфиденциальной корпоративной информации доступ или обрабатываемых открытый iCloud серверов. Таким образом нельзя использовать встроенную систему ссылки документа.

По-прежнему используется система закладки и отвечает поставщика расширения файла для правильной обработки закладкой URL-адрес и возвращает содержимое документа, на который указывает его.

В целях безопасности iOS 8 имеет уровень изоляции, сохраняет сведения о том, какие приложения имеют доступ к какой идентификатор внутри какой поставщик файл. Следует отметить, что все доступ к файлам управляется этот уровень изоляции.

В примере ниже показан поток данных при работе с закладками и расширение поставщика документа:

 [![](document-picker-images/image40.png "На этой схеме показан поток данных при работе с закладками и расширение поставщика документа")](document-picker-images/image40.png#lightbox)

Происходит следующий процесс.

1.  Приложение собирается перейти в фоновом режиме и необходимо сохранять свое состояние. Он вызывает `NSUrl` для создания закладки в файл в альтернативное хранилище.
1.  `NSUrl` вызывает поставщика расширение файла, чтобы получить постоянное URL-адрес для документа. 
1.  Расширение файла поставщик возвращает URL-адрес в виде строки для `NSUrl` .
1.  `NSUrl` Объединяет URL-адрес в закладки и возвращает его в приложение.
1.  Когда awakes из процесса в фоновом режиме и необходимо восстановить состояние приложения, он передает закладку `NSUrl` .
1.  `NSUrl` вызывает поставщик расширение файла с URL-адрес файла.
1.  Поставщик расширений файла обращается к файлу и возвращает расположение файла для `NSUrl` .
1.  Расположение файла вместе с сведения о безопасности и возвращаются в приложение.


Здесь приложение доступ к файлу и работать с ними в обычном режиме.

### <a name="writing-files"></a>Запись файлов

В этом разделе займет кратко рассмотрим, как записи файлов в альтернативное расположение с works документа поставщика расширения. Приложение iOS будет использовать координации файла для сохранения информации на диск в контейнере приложения. Вскоре после успешно записан файл изменения будут отправляться поставщика расширение файла.

На этом этапе поставщик расширение файла можно начать отправки файла в альтернативное расположение (или пометить файл как "грязные" и требовать передачи).

### <a name="creating-new-document-provider-extensions"></a>Создание нового документа поставщика расширений

Создание нового документа поставщика расширений находится за пределами этой вводной статье. Эта информация предоставляется здесь чтобы показать, что, в зависимости от расширения, которые пользователь загрузил в своем устройстве iOS, приложения могут иметь доступ к документа места хранения за пределами Apple указано расположение iCloud.

Разработчик должен быть этот факт при использовании средства выбора документа и работа с внешних документов. Они не предполагают размещенных этих документов в iCloud.

Дополнительные сведения о создании поставщика хранилища или расширение выбора документа см. в разделе [введение расширения приложения](~/ios/platform/extensions.md) документа.

## <a name="migrating-to-icloud-drive"></a>Миграция с iCloud диска

IOS 8 пользователи могут выбирать для продолжения с помощью iCloud существующие документы системы применяется для iOS 7 (и предыдущих систем) или можно выполнить миграцию существующих документов на новый диск механизм iCloud.

В Mac OS X Yosemite, Apple не обеспечивает обратную совместимость так, чтобы все документы, которые необходимо перенести в iCloud диска или больше не будет обновляться на устройствах.

После миграции учетной записи пользователя iCloud диск только устройства с помощью iCloud диск будет иметь возможность распространить изменения документов на этих устройствах.

> [!IMPORTANT]
> **Примечание**: разработчикам следует иметь в виду, что новые функции, описанные в данной статье будут доступны только если учетной записи пользователя был перенесен в iCloud диска. 

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован изменения в существующие iCloud API-интерфейсы требуется для поддержки iCloud диск и новый контроллер представление выбора документа. Он был проиллюстрирован координации файла и поэтому очень важно при работе с документами на основе облака. Он охваченных установки, необходимые для включения облачных документов в приложении Xamarin.iOS и получает познакомимся работа с документами вне контейнера приложения приложения с помощью контроллера выбора представления документа.

Кроме того в этой статье кратко рассматриваются расширения поставщика документов и почему разработчику необходимо учитывать их при написании приложений, которые могут обрабатывать документы на основе облака.

## <a name="related-links"></a>Связанные ссылки

- [DocPicker (пример)](https://developer.xamarin.com/samples/monotouch/ios8/DocPicker/)
- [Введение в iOS 8](~/ios/platform/introduction-to-ios8.md)
- [Введение в расширения приложения](~/ios/platform/extensions.md)
