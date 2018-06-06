---
title: Расширенные пользовательские уведомления в Xamarin.iOS
description: Эта статья представляет собой более подробное рассмотрение framework уведомления для пользователей, представленные в iOS 10. Он описывает уведомления для пользователей, уведомления о пользовательском интерфейсе, вложения мультимедиа, настраиваемые пользовательские интерфейсы и многое другое.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/03/2018
ms.openlocfilehash: 09a73ebc3dab90e6342a45c0f1fb5a40184d18a6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34788534"
---
# <a name="advanced-user-notifications-in-xamarinios"></a>Расширенные пользовательские уведомления в Xamarin.iOS

Новые и iOS 10 уведомление пользователя, платформа дает для доставки и обработки локальных и удаленных уведомлений. С помощью этой платформы, приложения или расширения приложения можно запланировать доставки уведомлений, локальный, указав набор условий, таких как расположение или время дня.

## <a name="about-user-notifications"></a>О уведомления для пользователей

Новая инфраструктура уведомление пользователя позволяет доставки и обработки локальных и удаленных уведомлений. С помощью этой платформы, приложения или расширения приложения можно запланировать доставки уведомлений, локальный, указав набор условий, таких как расположение или время дня.

Кроме того, приложения или расширения можно получить (и потенциально изменять) уведомления локальных и удаленных малозаметными устройства iOS.

Новая инфраструктура пользовательского интерфейса для уведомления пользователя позволяет приложения или расширения приложения, чтобы настроить внешний вид уведомления локальных и удаленных, когда они отображаются для пользователя.

Эта платформа предоставляет следующие возможности, приложение может доставить уведомления пользователя:

- **Предупреждения Visual** — где уведомление собирает данные о вниз от верхней части экрана в виде заголовка.
- **Звук и вибрации** -может быть связан с уведомлением.
- **Приложение значок эмблемы** — там, где отображается значок приложения эмблемы, показывающая, что доступно новое содержимое. Например количество непрочитанных сообщений.

Кроме того в зависимости от текущего контекста пользователя, можно различными способами, появляется уведомление:

- Если устройство не будет разблокировано, уведомление будет развертывание из верхней части экрана виде заголовка.
- Если устройство заблокировано, уведомление будет отображаться на экране блокировки пользователя.
- Если пользователь пропустит уведомление, можно открыть центр уведомлений и просматривать доступные, ожидания уведомления, существует.

Приложения Xamarin.iOS имеет два типа, он может отправлять уведомления для пользователей.

- **Локальный уведомления** — они не будут отправлены в приложения, установленные локально на устройстве пользователя.
- **Удаленный уведомления** - отправленных на удаленный сервер, либо отображаются для пользователя или запускает фоновое обновление содержимого приложения.

Дополнительные сведения см. в разделе нашей [усиленной уведомления пользователя](~/ios/platform/user-notifications/enhanced-user-notifications.md) документации.

## <a name="the-new-user-notification-interface"></a>Новый пользовательский интерфейс уведомлений

Уведомления для пользователей в iOS 10 представлены с новой разработки пользовательского интерфейса, который предоставляет больше содержимого, такие как заголовок, подзаголовок и необязательный вложения мультимедиа, могут быть представлены на экране блокировки в виде заголовка в верхней части устройства или в центре уведомлений.

Независимо от того, где отображается уведомление пользователя в iOS 10 оно выводится с же внешний вид и те же функции и функциональные возможности.

В iOS 8 Apple реализовала пригодных к использованию уведомлений, где разработчик может присоединить настраиваемые действия на уведомление и позволяют пользователю предпринять действия на уведомление, не запуская приложение. В iOS 9 Apple увеличилась пригодных к использованию уведомлений с быстрый ответ, который позволяет пользователю получить ответ на уведомление с ввод текста.

Поскольку уведомления пользователя неотъемлемую часть взаимодействие с пользователем в iOS 10, Apple дальнейшей был расширен для поддерживающих 3D Touch, когда пользователь нажимает на уведомление и пользовательский интерфейс отображается предоставляют широкие возможности взаимодействия пригодных к использованию уведомлений с уведомлением.

При отображении пользовательского интерфейса пользователя уведомления, если пользователь взаимодействует с каких-либо действий, присоединенного к уведомлению, для предоставления отзывов о том, что изменилось пользовательского интерфейса могут обновляться сразу же.

Новые для iOS 10 пользовательского интерфейса API уведомления пользователя позволяет приложения Xamarin.iOS, чтобы легко воспользоваться преимуществами этих новых функций пользовательского интерфейса для уведомления пользователя.

## <a name="adding-media-attachments"></a>Добавление вложений мультимедиа

Один из наиболее распространенных элементов, получить совместно использоваться пользователями настолько фотографии, iOS 10 появилась возможность присоединения элемента мультимедиа (например, фотография) непосредственно на уведомление, где будет показана и доступных для пользователя вместе с остальными ко уведомления NT.

Однако из-за размера, задействованных при отправлении даже небольшое изображение, его подключения к удаленной полезные данные уведомления становится непрактичным. Чтобы решить эту проблему, разработчик может использовать новый модуль службы iOS 10, загрузить изображение из другого источника (например, в хранилище данных CloudKit) и присоединить ее к содержимому уведомления перед отображением для пользователя.

Для удаленного уведомления для изменения, расширение службы его полезные данные должны быть отмечены как изменяемые. Пример:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Рассмотрим следующие общие сведения о процессе:

[![](advanced-user-notifications-images/extension02.png "Добавление вложения носителя процесса")](advanced-user-notifications-images/extension02.png#lightbox)

После удаленного уведомление доставляется на устройство (с помощью APN), расширение службы затем можно загрузить необходимые изображения через любыми способами требуемого (такие как `NSURLSession`) и после получения образ, он может изменить содержимое уведомлений и отображения его пользователю.

Ниже приведен пример, как этот процесс может быть обработано в коде:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

При получении уведомления от APN, настраиваемый адрес изображения считывается из содержимого и файл, загружаемый с сервера. Затем `UNNotificationAttachement` создается уникальный идентификатор и локальное расположение образа (как `NSUrl`). Изменяемую копию содержимого уведомлений создается и добавления вложений носителя. Наконец, отображается уведомление о пользователя путем вызова `contentHandler`.

После добавления вложения уведомления системы берет на себя перемещения и управления файла.

Помимо удаленного уведомления, представленные выше, вложения мультимедиа также поддерживаются из локального уведомления, где `UNNotificationAttachement` создается и прикрепляется к уведомлению вместе с его содержимым.

Уведомление в iOS 10 поддерживает мультимедиа вложения изображений (статические и GIF-файлы), аудио или видео и система автоматически отобразит правильный пользовательского интерфейса для каждого из этих типов вложений при появлении уведомления для пользователя.

> [!NOTE]
> Следует соблюдать осторожность, чтобы оптимизировать размер носителя и время, необходимое системе для скачивания носителя от удаленного сервера (или сборка носитель для локального уведомлений) налагает строгого ограничения, как при выполнении расширения службы приложения. Например рассмотрим отправки масштабированной вниз версии изображения или очень мала клип видео представлены в уведомлении.

## <a name="creating-custom-user-interfaces"></a>Создание пользовательских интерфейсов

Чтобы создать собственный пользовательский интерфейс для уведомления для пользователей, разработчику необходимо добавить уведомление содержимого расширение (new для iOS 10) решение для приложения.

Расширение содержимое уведомлений позволяет разработчику добавлять свои собственные представления в Интерфейсе уведомления и отображать все содержимое нужные им. Однако, интерактивный пользовательский Интерфейс мини-приложения (например, кнопок и текстовых полей) **не** поддерживается пользовательским Интерфейсом уведомлений и никогда не должны быть добавлены в пользовательские разработки пользовательского интерфейса.

Для поддержки взаимодействия пользователя с уведомлением пользователя, настраиваемые действия должны быть создается, зарегистрированные в системе и подключается к уведомления до запланированного с системой. Расширение содержимого уведомлений будет вызываться для обработки процессов из этих действий. В разделе [работа с действия уведомления](~/ios/platform/user-notifications/enhanced-user-notifications.md) раздел [усиленной уведомления для пользователей](~/ios/platform/user-notifications/enhanced-user-notifications.md) Дополнительные сведения об настраиваемые действия.

При появлении уведомления пользователя с помощью пользовательского интерфейса для пользователя, он будет иметь следующие элементы:

[![](advanced-user-notifications-images/customui01.png "Уведомление пользователя с элементами пользовательского интерфейса")](advanced-user-notifications-images/customui01.png#lightbox)

Если пользователь взаимодействует с настраиваемые действия (представленные ниже уведомления), пользовательский интерфейс можно обновить для предоставления отзывов пользователей как события при их вызове указанного действия.

### <a name="adding-a-notification-content-extension"></a>Добавление расширения содержимого уведомлений

Для реализации пользовательского интерфейса для уведомления пользователя в приложении Xamarin.iOS, выполните следующие действия.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Откройте решение для приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **Pad решения** и выберите **добавить** > **Добавление нового проекта**.
3. Выберите **iOS** > **расширения** > **содержимого расширения уведомления** и нажмите кнопку **Далее** кнопки: 

    [![](advanced-user-notifications-images/notify01.png "Выбор расширений содержимого уведомлений")](advanced-user-notifications-images/notify01.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **Далее** кнопки: 

    [![](advanced-user-notifications-images/notify02.png "Введите имя для модуля")](advanced-user-notifications-images/notify02.png#lightbox)
5. Настройка **имя проекта** и/или **имя решения** , если требуется и нажмите кнопку **создать** кнопки: 

    [![](advanced-user-notifications-images/notify03.png "Измените имя проекта и/или имя решения")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Откройте решение для приложения в Visual Studio для Mac.
2. Щелкните правой кнопкой мыши имя решения в **обозревателе решений** и выберите **Добавить > Новый проект...** .
3. Выберите **Visual C# > iOS расширения > уведомления содержимого расширения**:

    [![](advanced-user-notifications-images/notify01.w157-sml.png "Выбор расширений содержимого уведомлений")](advanced-user-notifications-images/notify01.w157.png#lightbox)
4. Введите **имя** расширение и нажмите кнопку **ОК** кнопки.

-----

При добавлении в решение расширением содержимого уведомления будут созданы три файла в проекте расширения:

1. `NotificationViewController.cs` — Это контроллер главное представление для расширения содержимого уведомления.
2. `MainInterface.storyboard` -Где разработчик размещает отображается пользовательский Интерфейс для модуля уведомлений, в конструкторе iOS.
3. `Info.plist` — Управляет конфигурации расширения содержимого уведомления.

Значение по умолчанию `NotificationViewController.cs` файла выглядит следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification` Метод вызывается, когда уведомление развернут пользователем, чтобы расширение содержимого уведомлений можно заполнить при помощи пользовательского интерфейса с содержимое `UNNotification`. Для приведенного выше примера метка будет добавлен к представлению, открыт для кода с именем `label` и используется для отображения текста уведомления.

### <a name="setting-the-notification-content-extensions-categories"></a>Настройка категорий расширением содержимого уведомлений

Системы, необходимо знать о том, как найти на конкретные категории, которые она отвечает на основе расширения содержимого для уведомления приложения. Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните расширения `Info.plist` файла в **Pad решения** чтобы открыть его для редактирования.
2. Переключитесь в **источника** представления.
3. Разверните `NSExtension` ключа.
4. Добавить `UNNotificationExtensionCategory` ключа как тип **строка** со значением расширение принадлежит к категории (в этом примере "пригласить событий): 

    [![](advanced-user-notifications-images/customui02.png "Добавить раздел UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02.png#lightbox)
5. Сохраните изменения.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните расширения `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
3. Разверните `NSExtension` ключа.
4. Добавить `UNNotificationExtensionCategory` ключа как тип **строка** со значением расширение принадлежит к категории (в этом примере "пригласить событий): 

    [![](advanced-user-notifications-images/customui02w.png "Добавить раздел UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02w.png#lightbox)
5. Сохраните изменения.

-----

Расширение категории уведомления (`UNNotificationExtensionCategory`) Используйте те же значения категории, используемые для регистрации действий по уведомлениям. В ситуации, где приложение будет использовать тот же интерфейс пользователя для нескольких категорий, переключение `UNNotificationExtensionCategory` типу **массива** и укажите все необходимые категории. Пример:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui03.png "Категории расширения содержимого уведомлений")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui03w.png "Категории расширения содержимого уведомлений")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Скрытие содержимого уведомлений по умолчанию

В ситуации, где пользовательского интерфейса для уведомления будет отображаться содержимое по умолчанию уведомления (заголовок, подзаголовок и текст автоматически отображается в нижней части пользовательского интерфейса уведомлений), эти сведения по умолчанию могут быть скрыты, добавив `UNNotificationExtensionDefaultContentHidden`ключа для `NSExtensionAttributes` ключа как тип **логическое** со значением `YES` в расширении `Info.plist` файла:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui04.png "Поиск сведений по умолчанию")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui04w.png "Поиск сведений по умолчанию")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Проектирование пользовательского интерфейса

Для создания расширения уведомления содержимого пользовательского интерфейса, дважды щелкните `MainInterface.storyboard` файл, чтобы открыть его для редактирования в конструкторе, iOS, перетащив элементы, которые необходимы для создания нужного интерфейса (например, `UILabels` и `UIImageViews`).

> [!NOTE]
> Пользовательский Интерфейс уведомлений _не_ поддерживают интерактивных элементов управления, такие как текстовые поля или кнопки в расширении содержимого уведомления. Пока могут быть добавлены в раскадровку, пользователь не будет иметь возможность взаимодействовать с ними. Чтобы добавить настраиваемый пользовательский Интерфейс уведомлений взаимодействие с пользователем, вместо этого используйте настраиваемые действия.

После макета пользовательского интерфейса и необходимые элементы управления открыт для кода C#, откройте `NotificationViewController.cs` для редактирования и измените `DidReceiveNotification` метод для заполнения пользовательский Интерфейс, когда пользователь разворачивает уведомления. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Установка размера области содержимого

Для настройки размера области содержимого, отображаемых для пользователя, в следующем примере кода выполняется задание `PreferredContentSize` свойства `ViewDidLoad` метод до нужного размера. Этот размер также может настраиваться путем применения ограничений к представлению в конструкторе iOS, он остается разработчику для выбора наиболее подходящего метода.

Поскольку начинаем уведомления системы уже запущена, прежде чем уведомление вызывается расширением содержимого, области содержимого full размера и анимировать до запрошенного размера, если для пользователя.

Чтобы устранить этот эффект, измените `Info.plist` файл для расширения и набор `UNNotificationExtensionInitialContentSizeRatio` ключ `NSExtensionAttributes` ключ к типу **номер** с значение, представляющее нужный коэффициент. Пример:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui05.png "Ключ UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui05w.png "Ключ UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>С помощью Media вложений в настраиваемый пользовательский Интерфейс

Так как вложения носителя (как показано в [Добавление вложений носителя](#Adding-Media-Attachments) выше) являются частью полезной нагрузки уведомления, доступ и отображаться в расширении уведомлений содержимого так же, как они будут находиться в значение по умолчанию Уведомление пользовательского интерфейса.

Например, если включены настраиваемый пользовательский Интерфейс выше `UIImageView` , был открыт для кода C#, приведенный ниже код может использоваться для заполнения его из с вложением мультимедиа:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Так как вложение мультимедиа управляется системой, оно находится вне приложения "песочницы". Расширения необходимо сообщить системе, что ему доступ к файлу путем вызова `StartAccessingSecurityScopedResource` метод. Модуль выполняется с помощью файла, ей необходимо вызвать метод `StopAccessingSecurityScopedResource` для освобождения соединения.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Добавление настраиваемых действий пользовательского интерфейса

Как уже говорилось выше, пользовательский Интерфейс уведомления не поддерживает взаимодействие с пользователем, поэтому при разработке пользовательского интерфейса не поддерживаются элементами управления, такие как текстовые поля или кнопки.

Вместо этого механизм стандартных пользовательских действий используется для добавления интерактивности настраиваемый пользовательский Интерфейс уведомлений. В разделе [работа с действия уведомления](~/ios/platform/user-notifications/enhanced-user-notifications.md) раздел [усиленной уведомления для пользователей](~/ios/platform/user-notifications/enhanced-user-notifications.md) Дополнительные сведения об особых действий.

Помимо пользовательских действий расширение содержимого уведомление может отвечать на следующие встроенные действия также:

- **Действие по умолчанию** -это происходит, когда пользователь касается уведомления, откройте приложение и отображения сведений данного уведомления.
- **Отклонить действие** -это действие, отправляется приложения, когда пользователь закрывает данного уведомления.

Расширения содержимого уведомлений также иметь возможность обновлять их пользовательского интерфейса, когда пользователь вызывает одно из пользовательских действий, например, для отображения даты как принятый, когда пользователь касается **Accept** кнопку пользовательское действие. Кроме того расширения содержимого уведомлений можно сообщить системе задерживать увольнения уведомления пользовательского интерфейса, поэтому пользователь может видеть эффекта их действия, прежде чем закрыть уведомление.

Это делается путем реализации вторая версия `DidReceiveNotification` метод, который содержит обработчик завершения. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

Путем добавления `Server.PostEventResponse` обработчик `DidReceiveNotification` метод уведомления содержимого расширения, расширение *должен* обработки всех пользовательских действий. Модуль может также переслать на приложения, содержащего пользовательские действия, изменив `UNNotificationContentExtensionResponseOption`. Пример:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Работа с операцию ввода текста в настраиваемый пользовательский Интерфейс

В зависимости от структуры приложения и уведомления возможны ситуации, которые пользователь должен вводить текст уведомления (например, при ответе на сообщение). Расширение содержимого уведомления имеет доступ к операцию ввода встроенный текст так же, как и стандартные уведомления.

Пример:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Этот код создает новое действие ввода текста и добавляет его к категории расширения (в `MakeExtensionCategory`) метод. В `DidReceive` Переопределите метод, он обрабатывает пользователь ввода текста с помощью следующего кода:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Если планируются вызовы для добавления пользовательских кнопок с полем ввода текста, добавьте следующий код, чтобы включить их:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

При активации пользователем действие комментария представление-контроллер и настраиваемого текстового поля ввода должны быть активированы:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье были предприняты никакие дополнительные рассматривать использование новой платформы уведомление пользователя в приложении Xamarin.iOS. Он рассматривается добавление вложений носителя для локальных и удаленных уведомления, он рассматривается с помощью пользовательского интерфейса нового пользователя уведомлений для создания специальных интерфейсов уведомления.


## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Справочник по UserNotifications Framework](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Руководство по программированию на локальных и удаленных уведомления](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
