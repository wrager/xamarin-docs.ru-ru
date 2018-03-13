---
title: "Реализация SiriKit"
description: "В этой статье рассматриваются действия, необходимые для реализации поддержки SiriKit в приложениях Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 20FFB981-EB10-48BA-BF79-40F37F0291EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0e271fb78cfd225f9ccdae9a515685e89bfd7ac2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-sirikit"></a>Реализация SiriKit

_В этой статье рассматриваются действия, необходимые для реализации поддержки SiriKit в приложениях Xamarin.iOS._



Новые для iOS 10 SiriKit позволяет приложения Xamarin.iOS для предоставления служб, которые доступны пользователю, с помощью Siri и Maps приложения на устройстве iOS. В этой статье рассматриваются действия, необходимые для реализации поддержки SiriKit в Xamarin.iOS приложений, добавив необходимые расширения целей, способы расширения пользовательского интерфейса и словаря.

Siri работает с понятием **домены**, групп знать действия для связанных задач. Каждый диалог, приложение будет установлено с Siri необходимо относятся к одной известной службы доменов следующим образом:

- Аудио или видео вызова.
- Резервирования расстояния.
- Управление тренировки.
- Обмен сообщениями.
- Поиск фотографий.
- Отправки или приема платежей.

Когда пользователь выполняет запрос Siri, связанных с одним расширения приложения служб, SiriKit отправляет расширение **намерение** , описывающий запроса пользователя, а также все дополнительные данные. Расширение приложения создает соответствующий **ответ** объекта для заданного **намерение**, с подробными сведениями о том, как расширение сможет обработать запрос.

В этом руководстве будет представлять краткий пример, в том числе поддержка SiriKit в существующее приложение. Для данного примера мы будем использовать фиктивное MonkeyChat приложения:

[![](implementing-sirikit-images/monkeychat01.png "Значок MonkeyChat")](implementing-sirikit-images/monkeychat01.png#lightbox)

MonkeyChat сохраняет свой собственный контактов книга друзей пользователя, каждый связанный с именем экрана (например, Bobo, например) и пользователь может отправить текст разговоры каждый друг по его отображаемому имени.

## <a name="extending-the-app-with-sirikit"></a>Расширение приложения с SiriKit

Как показано в [SiriKit основные понятия](~/ios/platform/sirikit/understanding-sirikit.md) руководство, в расширении возможностей приложения с SiriKit участвуют три основные части:

[![](implementing-sirikit-images/elements01.png "Расширение приложения со схемой SiriKit")](implementing-sirikit-images/elements01.png#lightbox)

Сюда входит следующее.

1. **Способы расширения** -проверяет ответы пользователей проверка того, приложение сможет обработать запрос и фактически выполняет задачу, для удовлетворения запроса пользователя.
2. **Способы расширения пользовательского интерфейса** - *необязательно*, предоставляет пользовательский Интерфейс для ответов в среде Siri можно добавить приложения пользовательского интерфейса и фирменной символики в Siri, чтобы расширить возможности пользователя.
3. **Приложение** -предоставляет словари конкретного пользователя для помощи в работе с ним Siri приложения. 

В следующих разделах подробно рассматриваются все эти элементы и действия, чтобы включить их в приложении.


## <a name="preparing-the-app"></a>Подготовка приложения

SiriKit построена на расширения, тем не менее, перед добавлением расширения приложения, есть несколько моментов, которые разработчик должен выполнить для справки по внедрению SiriKit.

### <a name="moving-common-shared-code"></a>Перемещение общий код 

Во-первых, разработчик может переместить некоторые из общий код, который будет использоваться между приложением и расширениях в один _общих проектов_, _переносимой библиотеки классов (PCLs)_ или _собственного Библиотеки_.

Расширения, необходимо иметь возможность выполнять все задачи, которые выполняет приложение. Пример приложения MonkeyChat таких вещей, как поиск контактов, добавление новых контактов для условий отправки сообщений и получения сообщения из журнала.

Перемещая этот общий код в общий проект, PCL или собственная библиотека он позволяет легко поддерживать этот код в одном месте общих и гарантирует, что расширения и родительского приложения предоставляют универсальный интерфейсом и функциональные возможности для пользователя.

В случае использования примера приложения MonkeyChat модели данных и обработки кода, такие как доступ к сети и базы данных будут перенесены в собственной библиотеки.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и откройте приложение MonkeyChat.
2. Щелкните правой кнопкой мыши имя решения в **Pad решения** и выберите **добавить** > **новый проект...** : 

    [![](implementing-sirikit-images/prep01.png "Добавление нового проекта")](implementing-sirikit-images/prep01.png#lightbox)
3. Выберите **iOS** > **библиотеки** > **библиотеки классов** и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/prep02.png "Выбор библиотеки классов")](implementing-sirikit-images/prep02.png#lightbox)
4. Введите `MonkeyChatCommon` для **имя** и нажмите кнопку **создать** кнопки: 

    [![](implementing-sirikit-images/prep03.png "Введите MonkeyChatCommon имени")](implementing-sirikit-images/prep03.png#lightbox)
5. Щелкните правой кнопкой мыши **ссылки** папку основного приложения в **обозревателе решений** и выберите **изменить ссылки...** . Проверьте **MonkeyChatCommon** проекта и нажмите кнопку **ОК** кнопки: 

    [![](implementing-sirikit-images/prep05.png "Проверка проекта MonkeyChatCommon")](implementing-sirikit-images/prep05.png#lightbox)
6. В **обозревателе решений**, перетащите общий код из основного приложения в собственной библиотеки.
7. В случае MonkeyChat, перетащите **DataModels** и **процессоров** папки из основного приложения в собственной библиотеки: 

    [![](implementing-sirikit-images/prep06.png "Процессоры и DataModels папки в обозревателе решений")](implementing-sirikit-images/prep06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и откройте приложение MonkeyChat.
2. Щелкните правой кнопкой мыши имя решения в **обозревателе решений** и выберите **добавить** > **новый проект...** .
3. Выберите **Visual C#** > **общий проект** и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/prep02w.png "Выбор библиотеки классов")](implementing-sirikit-images/prep02w.png#lightbox)
4. Введите `MonkeyChatCommon` для **имя** и нажмите кнопку **создать** кнопки.
5. Щелкните правой кнопкой мыши **ссылки** папку основного приложения в **обозревателе решений** и выберите **изменить ссылки...** . Проверьте **MonkeyChatCommon** проекта и нажмите кнопку **ОК** кнопки: 

    [![](implementing-sirikit-images/prep05w.png "Проверка проекта MonkeyChatCommon")](implementing-sirikit-images/prep05w.png#lightbox)
6. В **обозревателе решений**, перетащите общий код из основного приложения в общий проект.
7. В случае MonkeyChat, перетащите **DataModels** и **процессоров** папки из основного приложения в собственной библиотеки.

-----

Измените любой из файлов, которые были перемещены в собственной библиотеки и пространства имен, чтобы он соответствовал библиотеки. Например, изменение `MonkeyChat` для `MonkeyChatCommon`:

```csharp
using System;
namespace MonkeyChatCommon
{
    /// <summary>
    /// A message sent from one user to another within a conversation.
    /// </summary>
    public class MonkeyMessage
    {
        public MonkeyMessage ()
        {
        }
        ...
    }
}
```

После этого вернитесь к основным приложением и добавить `using` инструкции для собственной библиотеки пространства имен в любом приложение использует один из классов, которые были перенесены:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using MonkeyChatCommon;

namespace MonkeyChat
{
    public partial class MasterViewController : UITableViewController
    {
        public DetailViewController DetailViewController { get; set; }

        DataSource dataSource;
        ...
    }
}
```

### <a name="architecting-the-app-for-extensions"></a>Проектирование архитектуры приложения для расширения

Как правило приложение будет зарегистрироваться для нескольких целей и разработчику необходимо убедиться, что приложение спроектирован для соответствующего числа намерение расширения.

В ситуации, когда приложению требуется более чем одной целью разработчик может размещение всех их обработку, блокировка с намерением в одно назначение расширение или создание отдельных намерение расширения для каждой цели.

Если вы решили создать отдельное назначение расширение для каждой цели, разработчик может повторять большой объем стандартный код в каждом расширении и создавать большой объем процессор и память.

Чтобы выбрать один из двух параметров, см. Если любой из целей естественным образом связаны друг с другом. Например приложение, аудио и видео вызовы могут быть включены оба этих целей в одном расширении цель, как они обрабатываются других подобных задач, что может привести большинство повторное использование кода.

Назначение или группа целей, которые не помещаются в существующую группу создайте новое расширение намерением в приложение для их хранения.


### <a name="setting-the-required-entitlements"></a>Установка необходимых прав

Любое приложение Xamarin.iOS, включающий SiriKit интеграции, должны содержать правильные права группы. Если разработчик не установлены правильно данные необходимые права, он не сможет установить или тестировать приложение на оборудовании реальные iOS 10 (или более поздней), который также является требованием, начиная с iOS 10 симулятор не поддерживает SiriKit.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Entitlements.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
2. Переключитесь в **источника** вкладки.
3. Добавить `com.apple.developer.siri` **свойство**, задайте **тип** для `Boolean` и **значение** для `Yes`: 

    [![](implementing-sirikit-images/setup01.png "Добавить свойство com.apple.developer.siri")](implementing-sirikit-images/setup01.png#lightbox)
4. Сохраните изменения в файле.
5. Дважды щелкните **файл проекта** в **обозревателе решений** чтобы открыть его для редактирования.
6. Выберите **подписывание пакета iOS** и убедитесь, что `Entitlements.plist` в выбран файл **пользовательские права** поля: 

    [![](implementing-sirikit-images/setup02.png "Выберите файл Entitlements.plist в поле пользовательские права")](implementing-sirikit-images/setup02.png#lightbox)
7. Нажмите кнопку **ОК**, чтобы сохранить изменения.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните `Entitlements.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
3. Добавить `com.apple.developer.siri` **свойство**, задайте **тип** для `Boolean` и **значение** для `Yes`: 

    [![](implementing-sirikit-images/setup01w.png "Добавить свойство com.apple.developer.siri")](implementing-sirikit-images/setup01w.png#lightbox)
4. Сохраните изменения в файле.
5. Дважды щелкните **файл проекта** в **обозревателе решений** чтобы открыть его для редактирования.
6. Выберите **подписывание пакета iOS** и убедитесь, что `Entitlements.plist` в выбран файл **пользовательские права** поля.

-----

После завершения приложения `Entitlements.plist` файл должен выглядеть следующим образом (в открыть внешний редактор):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.developer.siri</key>
    <true/>
</dict>
</plist>
```

### <a name="correctly-provisioning-the-app"></a>Правильно подготовки приложения

Из-за строгой безопасности, которое помещено Apple вокруг платформа SiriKit любого приложения Xamarin.iOS, реализующий SiriKit _должен_ иметь правильный идентификатор приложения и права (см. выше) и должны быть подписаны строгим Профиль подготовки.

Выполните следующие действия на компьютере Mac:

1. В веб-браузер, перейдите к [http://developer.apple.com](http://developer.apple.com) и журналов в учетную запись.
2. Щелкните **сертификаты**, **идентификаторы** и **профилей**.
3. Выберите **профили подготовки** и выберите **идентификаторы приложений**, нажмите кнопку  **+**  кнопки.
4. Введите **имя** нового профиля.
5. Введите **идентификатор пакета** следующие Apple именования элемента рекомендации.
6. Прокрутите вниз до **службы приложений** выберите **SiriKit** и нажмите кнопку **Продолжить** кнопки: 

    [![](implementing-sirikit-images/setup03.png "Выберите SiriKit")](implementing-sirikit-images/setup03.png#lightbox)
7. Проверьте все параметры, затем **отправить** идентификатор приложения.
8. Выберите **профили подготовки** > **разработки**, нажмите кнопку  **+**  кнопку, выберите **Apple ID**, Нажмите кнопку **Продолжить**.
9. Выберите **все**, нажмите кнопку **Продолжить**.
10. Нажмите кнопку **выделить все** еще раз, нажмите кнопку **Продолжить**.
11. Введите **имя профиля** с помощью Apple именования элемента предложения, нажмите кнопку **Продолжить**.
12. Запустите Xcode.
13. В меню Xcode **предпочтения...**
14. Выберите **учетные записи**, нажмите кнопку **подробности...** кнопки: 

    [![](implementing-sirikit-images/setup04.png "Выберите учетные записи")](implementing-sirikit-images/setup04.png#lightbox)
15. Нажмите кнопку **загрузить все профили** кнопки в левом нижнем углу: 

    [![](implementing-sirikit-images/setup05.png "Загрузка всех профилей")](implementing-sirikit-images/setup05.png#lightbox)
16. Убедитесь, что **профиль подготовки** создана более поздней версии установлена в Xcode.
17. Откройте проект для добавления поддержки SiriKit, чтобы в Visual Studio для Mac.
18. Дважды щелкните `Info.plist` файла в **обозревателе решений**.
18. Убедитесь, что **идентификатор пакета** совпадает со структурой, созданные на портале разработчиков Apple выше: 

    [![](implementing-sirikit-images/setup06.png "Идентификатор пакета")](implementing-sirikit-images/setup06.png#lightbox)
18. В **обозревателе решений**выберите **проекта**.
19. Щелкните правой кнопкой мыши проект и выберите **параметры**.
21. Выберите **подписывание пакета iOS**выберите **удостоверение подписи** и **профиль подготовки** созданной выше: 

    [![](implementing-sirikit-images/setup07.png "Выберите удостоверение подписи и подготовительный профиль")](implementing-sirikit-images/setup07.png#lightbox)
22. Нажмите кнопку **ОК**, чтобы сохранить изменения.

> [!IMPORTANT]
> **Примечание:** SiriKit тестирования работает только с реальными iOS 10 аппаратного устройства, а не в iOS 10 симулятора. Если возникли проблемы при установке SiriKit включен приложения Xamarin.iOS на реальном оборудовании, убедитесь, что необходимые права, идентификатор приложения, идентификатор подписи и профиль подготовки были правильно настроены в Apple портал разработчиков и Visual Studio для Mac.

### <a name="requesting-siri-authorization"></a>Запрос авторизации Siri

Прежде чем приложение добавляет все словаря конкретного пользователя или расширения целей подключается к Siri, его необходимо запрашивает авторизацию от пользователя для доступа к Siri.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Изменение приложения `Info.plist` файл, перейдите в **источника** просмотра и добавления `NSSiriUsageDescription` ключа со строковым значением, описывающий, как приложение будет использовать Siri и что типы данных будут отправляться. MonkeyChat приложение может например, «контакты MonkeyChat будет отправляться Siri»:

[![](implementing-sirikit-images/request01.png "NSSiriUsageDescription в редакторе Info.plist")](implementing-sirikit-images/request01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Изменение приложения `Info.plist` и добавьте `NSSiriUsageDescription` ключа со строковым значением, описывающий, как приложение будет использовать Siri и что типы данных будут отправляться. MonkeyChat приложение может например, «контакты MonkeyChat будет отправляться Siri»:

[![](implementing-sirikit-images/request01w.png "NSSiriUsageDescription в редакторе Info.plist")](implementing-sirikit-images/request01w.png#lightbox)

-----

Вызовите `RequestSiriAuthorization` метод `INPreferences` класса при первом запуске приложения. Изменить `AppDelegate.cs` класса и сделать `FinishedLaunching` внешний метод следующим образом:


```csharp
using Intents;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{

    // Request access to Siri
    INPreferences.RequestSiriAuthorization ((INSiriAuthorizationStatus status) => {
        // Respond to returned status
        switch (status) {
        case INSiriAuthorizationStatus.Authorized:
            break;
        case INSiriAuthorizationStatus.Denied:
            break;
        case INSiriAuthorizationStatus.NotDetermined:
            break;
        case INSiriAuthorizationStatus.Restricted:
            break;
        }
    });


    return true;
}
```

Этот метод вызывается при первом запуске выводится оповещение подтверждения от пользователя, чтобы разрешить приложению доступ к Siri. Сообщение, которое разработчик добавлены `NSSiriUsageDescription` выше, будет отображаться в это предупреждение. Если пользователь сначала запрещает доступ, они могут использовать **параметры** приложения для предоставления доступа к приложению.

В любое время, приложение можно проверить возможность приложения к Siri путем вызова `SiriAuthorizationStatus` метод `INPreferences` класса.

### <a name="localization-and-siri"></a>Локализация и Siri

На устройстве iOS пользователю возможность выбрать язык для Siri отличается, то в системе по умолчанию. При работе с данными локализованные приложения нужно будет использовать `SiriLanguageCode` метод `INPreferences` класса, чтобы получить код языка из Siri. Пример:

```csharp
var language = INPreferences.SiriLanguageCode();

// Take action based on language
if (language == "en-US") {
    // Do something...
}
```

### <a name="adding-user-specific-vocabulary"></a>Добавление словаря определенного пользователя

Словарь определенных пользователем должен предоставить слов или фраз, которые являются уникальными для отдельных пользователей приложения. Они будут представлены во время выполнения из основного приложения (не расширения приложения) в виде упорядоченный набор терминов, упорядоченные в наиболее значимых приоритет использования для пользователей с наиболее важные термины в начале списка.

Словарь определенных пользователем должны принадлежать к одному из следующих категорий:

- Контактных имен (которые не управляются framework контактов).
- Теги фото.
- Названия альбомов фото.
- Имена тренировки.

Только при выборе терминология для регистрации в качестве пользовательского словаря, выберите условия, могут быть неправильно поняты кем-то не знакомы с приложением. Никогда не регистра основных терминах, такие как «Мои тренировки» или «Мои альбома». Например приложение MonkeyChat зарегистрирует псевдонимы, связанные с обращением к каждому в адресной книге пользователя.

Приложение предоставляет словарь конкретного пользователя путем вызова `SetVocabularyStrings` метод `INVocabulary` класса и передав `NSOrderedSet` коллекции из основного приложения. Следует всегда вызывать приложение `RemoveAllVocabularyStrings` метод первый, чтобы удалить любые существующие условия перед добавлением новых. Пример:

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Foundation;
using Intents;

namespace MonkeyChatCommon
{
    public class MonkeyAddressBook : NSObject
    {
        #region Computed Properties
        public List<MonkeyContact> Contacts { get; set; } = new List<MonkeyContact> ();
        #endregion

        #region Constructors
        public MonkeyAddressBook ()
        {
        }
        #endregion

        #region Public Methods
        public NSOrderedSet<NSString> ContactNicknames ()
        {
            var nicknames = new NSMutableOrderedSet<NSString> ();

            // Sort contacts by the last time used
            var query = Contacts.OrderBy (contact => contact.LastCalledOn);

            // Assemble ordered list of nicknames by most used to least
            foreach (MonkeyContact contact in query) {
                nicknames.Add (new NSString (contact.ScreenName));
            }

            // Return names
            return new NSOrderedSet<NSString> (nicknames.AsSet ());
        }

        // This method MUST only be called on a background thread!
        public void UpdateUserSpecificVocabulary ()
        {
            // Clear any existing vocabulary
            INVocabulary.SharedVocabulary.RemoveAllVocabularyStrings ();

            // Register new vocabulary
            INVocabulary.SharedVocabulary.SetVocabularyStrings (ContactNicknames (), INVocabularyStringType.ContactName);
        }
        #endregion
    }
}
```

Этот код в месте он может вызываться следующим образом:

```csharp
using System;
using System.Threading;
using UIKit;
using MonkeyChatCommon;
using Intents;

namespace MonkeyChat
{
    public partial class ViewController : UIViewController
    {
        #region AppDelegate Access
        public AppDelegate ThisApp {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do we have access to Siri?
            if (INPreferences.SiriAuthorizationStatus == INSiriAuthorizationStatus.Authorized) {
                // Yes, update Siri's vocabulary
                new Thread (() => {
                    Thread.CurrentThread.IsBackground = true;
                    ThisApp.AddressBook.UpdateUserSpecificVocabulary ();
                }).Start ();
            }
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

> [!IMPORTANT]
> **Примечание:** Siri считает пользовательского словаря, подсказки и включают в себя все термины, как можно. Однако пространства для пользовательского словаря ограничено, сделав его необходимо зарегистрировать _только_ терминологии, могут вызвать путаницу, поэтому сводя к минимуму общее число зарегистрированных слов.

Дополнительные сведения см. в разделе нашей [справочную словаря пользователя](~/ios/platform/sirikit/understanding-sirikit.md) и Apple [Указание пользовательского словаря ссылка](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

### <a name="adding-app-specific-vocabulary"></a>Добавление определенного приложения словаря

Терминологию конкретного приложения определяет конкретные слова и фразы, которые будет доступно для всех пользователей приложения, например типов транспортных средств или тренировки имена. Так как они являются частью приложения, они определены в `AppIntentVocabulary.plist` файле как часть пакета основным приложением. Кроме того эти слова и фразы должно быть локализовано.

Словарь терминов определенного приложения должны принадлежать к одному из следующих категорий:

- Воспользуйтесь преимуществами параметры.
- Имена тренировки.

Файл словаря конкретного приложения содержит два ключей корневого уровня:

- `ParameterVocabularies` **Требуется** -определяет настраиваемые условия приложения и назначение параметров, они применяются.
- `IntentPhrases` **Необязательный** -содержит пример фраз с использованием пользовательского условия, определенные в `ParameterVocabularies`.

Каждая запись в `ParameterVocabularies` необходимо указать строку идентификатора, термин и назначение, к которому применяется термин. Кроме того один термин может применяться несколько целей.

Полный список допустимых значений и структуры необходимый файл, см. в разделе Apple [ссылка на формат файла словаря приложения](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CustomVocabularyKeys.html#//apple_ref/doc/uid/TP40016875-CH10-SW1).

Чтобы добавить `AppIntentVocabulary.plist` файл в проект приложения, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **добавить** > **новый файл...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01.png "Добавьте список свойств")](implementing-sirikit-images/plist01.png#lightbox) 
2. Дважды щелкните `AppIntentVocabulary.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
3. Нажмите кнопку  **+**  для добавления раздела, установите **имя** для `ParameterVocabularies` и **тип** для `Array`:

    [![](implementing-sirikit-images/plist02.png "Задайте имя ParameterVocabularies и тип массива")](implementing-sirikit-images/plist02.png#lightbox)
4. Разверните `ParameterVocabularies` и нажмите кнопку  **+**  кнопку и задайте **тип** для `Dictionary`:

    [![](implementing-sirikit-images/plist03.png "Задайте тип словаря")](implementing-sirikit-images/plist03.png#lightbox)
5. Нажмите кнопку  **+**  Чтобы добавить новый раздел, задайте **имя** для `ParameterNames` и **тип** для `Array`:

    [![](implementing-sirikit-images/plist04.png "Задайте имя ParameterNames и тип массива")](implementing-sirikit-images/plist04.png#lightbox)
6. Нажмите кнопку  **+**  Чтобы добавить новый раздел со **тип** из `String` и значение как один из доступных имен параметров. Например `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05.png "Ключ INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05.png#lightbox)
7. Добавить `ParameterVocabulary` ключа для `ParameterVocabularies` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist06.png "Добавьте раздел ParameterVocabulary ParameterVocabularies ключ с типом массива")](implementing-sirikit-images/plist06.png#lightbox)
8. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist07.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist07.png#lightbox)
9. Добавить `VocabularyItemIdentifier` ключа с **тип** из `String` и укажите уникальный ИД для термина:

    [![](implementing-sirikit-images/plist08.png "Добавить ключ VocabularyItemIdentifier с типа String и укажите уникальный ИД")](implementing-sirikit-images/plist08.png#lightbox)
10. Добавить `VocabularyItemSynonyms` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist09.png "Добавить ключ VocabularyItemSynonyms с типом массива")](implementing-sirikit-images/plist09.png#lightbox)
11. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist10.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist10.png#lightbox)
12. Добавить `VocabularyItemPhrase` ключа с **тип** из `String` и термин определении приложения:

    [![](implementing-sirikit-images/plist11.png "Добавить ключ VocabularyItemPhrase с типа String с термином определении приложения")](implementing-sirikit-images/plist11.png#lightbox)
13. Добавить `VocabularyItemPronunciation` ключа с **тип** из `String` и фонетическое произношение термина:

    [![](implementing-sirikit-images/plist12.png "Добавить раздел VocabularyItemPronunciation типа String и фонетическое произношение термина")](implementing-sirikit-images/plist12.png#lightbox)
14. Добавить `VocabularyItemExamples` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist13.png "Добавить ключ VocabularyItemExamples с типом массива")](implementing-sirikit-images/plist13.png#lightbox)
15. Добавить несколько `String` ключей с примерами использования термина:

    [![](implementing-sirikit-images/plist14.png "Добавить несколько ключей String с примерами использования термин")](implementing-sirikit-images/plist14.png#lightbox)
16. Повторите описанные выше шаги для других пользовательских выражений необходимо определить приложения.
17. Свернуть `ParameterVocabularies` ключа.
18. Добавить `IntentPhrases` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist15.png "Добавить ключ IntentPhrases с типом массива")](implementing-sirikit-images/plist15.png#lightbox)
19. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist16.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist16.png#lightbox)
20. Добавить `IntentName` ключа с **тип** из `String` и намерением для примера:

    [![](implementing-sirikit-images/plist17.png "Добавьте раздел IntentName со строкового типа и цели для примера")](implementing-sirikit-images/plist17.png#lightbox)
21. Добавить `IntentExamples` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist18.png "Добавить ключ IntentExamples с типом массива")](implementing-sirikit-images/plist18.png#lightbox)
22. Добавить несколько `String` ключей с примерами использования термина:

    [![](implementing-sirikit-images/plist19.png "Добавить несколько ключей String с примерами использования термин")](implementing-sirikit-images/plist19.png#lightbox)
23. Повторите описанные выше действия для любого намерения приложения нужно предоставлять примера использования.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **добавить** > **новый файл...**   >  **iOS**:

    [![](implementing-sirikit-images/plist01w.png "Добавить новый Info.plist")](implementing-sirikit-images/plist01w.png#lightbox) 
2. Дважды щелкните `AppIntentVocabulary.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
3. Нажмите кнопку  **+**  для добавления раздела, установите **имя** для `ParameterVocabularies` и **тип** для `Array`:

    [![](implementing-sirikit-images/plist02w.png "Задайте имя ParameterVocabularies и тип массива")](implementing-sirikit-images/plist02w.png#lightbox)
4. Разверните `ParameterVocabularies` и нажмите кнопку  **+**  кнопку и задайте **тип** для `Dictionary`:

    [![](implementing-sirikit-images/plist03w.png "Задайте тип словаря")](implementing-sirikit-images/plist03w.png#lightbox)
5. Нажмите кнопку  **+**  Чтобы добавить новый раздел, задайте **имя** для `ParameterNames` и **тип** для `Array`:

    [![](implementing-sirikit-images/plist04w.png "Задайте имя ParameterNames и тип массива")](implementing-sirikit-images/plist04w.png#lightbox)
6. Нажмите кнопку  **+**  Чтобы добавить новый раздел со **тип** из `String` и значение как один из доступных имен параметров. Например `INStartWorkoutIntent.workoutName`:

    [![](implementing-sirikit-images/plist05w.png "Ключ INStartWorkoutIntent.workoutName")](implementing-sirikit-images/plist05w.png#lightbox)
7. Добавить `ParameterVocabulary` ключа для `ParameterVocabularies` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist06w.png "Добавьте раздел ParameterVocabulary ParameterVocabularies ключ с типом массива")](implementing-sirikit-images/plist06w.png#lightbox)
8. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist07w.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist07w.png#lightbox)
9. Добавить `VocabularyItemIdentifier` ключа с **тип** из `String` и укажите уникальный ИД для термина:

    [![](implementing-sirikit-images/plist08w.png "Добавить ключ VocabularyItemIdentifier с типа String и укажите уникальный ИД для термина")](implementing-sirikit-images/plist08w.png#lightbox)
10. Добавить `VocabularyItemSynonyms` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist09w.png "Добавить ключ VocabularyItemSynonyms с типом массива")](implementing-sirikit-images/plist09w.png#lightbox)
11. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist10w.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist10w.png#lightbox)
12. Добавить `VocabularyItemPhrase` ключа с **тип** из `String` и термин определении приложения:

    [![](implementing-sirikit-images/plist11w.png "Добавить ключ VocabularyItemPhrase с типа String с термином определении приложения")](implementing-sirikit-images/plist11w.png#lightbox)
13. Добавить `VocabularyItemPronunciation` ключа с **тип** из `String` и фонетическое произношение термина:

    [![](implementing-sirikit-images/plist12w.png "Добавить раздел VocabularyItemPronunciation типа String и фонетическое произношение термина")](implementing-sirikit-images/plist12w.png#lightbox)
14. Добавить `VocabularyItemExamples` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist13w.png "Добавить ключ VocabularyItemExamples с типом массива")](implementing-sirikit-images/plist13w.png#lightbox)
15. Добавить несколько `String` ключей с примерами использования термина:

    [![](implementing-sirikit-images/plist14w.png "Добавить несколько ключей String с примерами использования термин")](implementing-sirikit-images/plist14w.png#lightbox)
16. Повторите описанные выше шаги для других пользовательских выражений необходимо определить приложения.
17. Свернуть `ParameterVocabularies` ключа.
18. Добавить `IntentPhrases` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist15w.png "Добавить ключ IntentPhrases с типом массива")](implementing-sirikit-images/plist15w.png#lightbox)
19. Добавьте новый раздел с **тип** из `Dictionary`:

    [![](implementing-sirikit-images/plist16w.png "Добавьте новый раздел с словаря типа")](implementing-sirikit-images/plist16w.png#lightbox)
20. Добавить `IntentName` ключа с **тип** из `String` и намерением для примера:

    [![](implementing-sirikit-images/plist17w.png "Добавьте раздел IntentName со строкового типа и цели для примера")](implementing-sirikit-images/plist17w.png#lightbox)
21. Добавить `IntentExamples` ключа с **тип** из `Array`:

    [![](implementing-sirikit-images/plist18w.png "Добавить ключ IntentExamples с типом массива")](implementing-sirikit-images/plist18w.png#lightbox)
22. Добавить несколько `String` ключей с примерами использования термина:

    [![](implementing-sirikit-images/plist19w.png "Добавить несколько ключей String с примерами использования термин")](implementing-sirikit-images/plist19w.png#lightbox)
23. Повторите описанные выше действия для любого намерения приложения нужно предоставлять примера использования.

-----

> [!IMPORTANT]
> **Примечание:** `AppIntentVocabulary.plist` будет зарегистрирована с Siri на тестовой устройства во время разработки и она может занять некоторое время для Siri для включения пользовательского словаря. В результате инженер-испытатель потребуется подождать несколько минут, прежде чем пытаться тестирования словаря конкретного приложения, если она была изменена.

Дополнительные сведения см. в разделе нашей [приложения справочную словаря](~/ios/platform/sirikit/understanding-sirikit.md) и Apple [Указание пользовательского словаря ссылка](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SpecifyingCustomVocabulary.html#//apple_ref/doc/uid/TP40016875-CH6-SW1).

## <a name="adding-an-intents-extension"></a>Добавление целей расширения

Теперь, когда приложение будет подготовлена к использованию SiriKit, разработчик следует добавлять в решение для обработки целей, необходимые для интеграции Siri целей расширения один (или более).

Для каждого необходимые расширения целей выполните следующее:

- Добавьте в решение приложения Xamarin.iOS проект способы расширения.
- Настройка расширения целей `Info.plist` файла.
- Измените основной класс расширения целей.

Дополнительные сведения см. в разделе нашей [Reference целей расширения](~/ios/platform/sirikit/understanding-sirikit.md) и Apple [Создание ссылку на расширение целей](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/CreatingtheIntentsExtension.html#//apple_ref/doc/uid/TP40016875-CH4-SW1).

### <a name="creating-the-extension"></a>Создание расширения

Чтобы добавить расширение способы решения, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Щелкните правой кнопкой мыши **имя решения** в **Pad решения** и выберите **добавить** > **добавить новый проект...** .
2. В диалоговом окне выберите **iOS** > **расширения** > **цель расширения** и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/intents05.png "Выберите расширение намерений")](implementing-sirikit-images/intents05.png#lightbox)
3. Далее введите **имя** намерение расширение и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/intents06.png "Введите имя для модуля намерений")](implementing-sirikit-images/intents06.png#lightbox)
4. Наконец, нажмите кнопку **создать** кнопку, чтобы добавить расширение намерением в решение приложения: 

    [![](implementing-sirikit-images/intents07.png "Добавьте в решение приложения расширение цели")](implementing-sirikit-images/intents07.png#lightbox)
5. В **обозревателе решений**, щелкните правой кнопкой мыши **ссылки** папки только что созданный намерение расширения. Проверьте имя Общие библиотеки проекта общего кода (создания приложения выше) и нажмите кнопку **ОК** кнопки: 

    [![](implementing-sirikit-images/intents08.png "Выберите имя проекта библиотеки общих общего кода")](implementing-sirikit-images/intents08.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Щелкните правой кнопкой мыши **имя решения** в **обозревателе решений** и выберите **добавить** > **добавить новый проект...** .
2. В диалоговом окне выберите **iOS** > **расширения** > **цель расширения** и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/intents05w.png "Выберите расширение намерений")](implementing-sirikit-images/intents05w.png#lightbox)
3. Далее введите **имя** намерение расширение и нажмите кнопку **ОК** кнопки.
5. В **обозревателе решений**, щелкните правой кнопкой мыши **ссылки** папки только что созданный намерение расширения. Проверьте имя Общие библиотеки проекта общего кода (создания приложения выше) и нажмите кнопку **ОК** кнопки: 

    [![](implementing-sirikit-images/intents08w.png "Выберите имя проекта библиотеки общих общего кода")](implementing-sirikit-images/intents08w.png#lightbox)
    
-----

Повторите эти шаги для число расширений намерения (на основе [разработку приложений для расширений](#Architecting-the-App-for-Extensions) выше), приложению требуется.

### <a name="configuring-the-infoplist"></a>Настройка Info.plist

Для каждого расширения целей, которые были добавлены в решение приложения, должна быть настроена в `Info.plist` файлов для работы с приложением.

Так же, как и любое расширение типичные приложения, приложение будет иметь существующие ключи `NSExtension` и `NSExtensionAttributes`. Для целей расширения существует два новых атрибута, которые должны быть настроены.

[![](implementing-sirikit-images/intents01.png "Два новых атрибута, которые должны быть настроены")](implementing-sirikit-images/intents01.png#lightbox)

- **IntentsSupported** — не требуется и состоит из массива имен классов намерение приложения хочет поддержки с целью расширения.
- **IntentsRestrictedWhileLocked** -необязательный ключ для приложения указать поведение экрана блокировки расширения. Он состоит из массива имен классов намерение приложения хочет пользователю необходимо выполнить вход для использования с намерением расширения.

Чтобы настроить расширение намерение `Info.plist` файла, дважды щелкните его в **обозревателе решений** чтобы открыть его для редактирования. Теперь, переключитесь в **источника** просмотра, а затем разверните `NSExtension` и `NSExtensionAttributes` ключей в редактор:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents02.png "Ключи NSExtension и NSExtensionAttributes в редакторе")](implementing-sirikit-images/intents02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents02w.png "Ключи NSExtension и NSExtensionAttributes в редакторе")](implementing-sirikit-images/intents02w.png#lightbox)

-----

Разверните `IntentsSupported` раздел и добавить это имя класса цель, это расширение будет поддерживать. Для примера приложения MonkeyChat, он поддерживает `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents09.png "Ключ INSendMessageIntent")](implementing-sirikit-images/intents09.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents09w.png "Ключ INSendMessageIntent")](implementing-sirikit-images/intents09w.png#lightbox)

-----

Если приложение требует при необходимости вход пользователя на устройство для использования данной цели, разверните `IntentRestrictedWhileLocked` ключа и добавьте имена классов способов, с ограниченным доступом. Для примера приложения MonkeyChat, пользователю необходимо войти Отправка мгновенного сообщения, поэтому мы добавили `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents10.png "Добавлен ключ INSendMessageIntent")](implementing-sirikit-images/intents10.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents10w.png "Добавлен ключ INSendMessageIntent")](implementing-sirikit-images/intents10w.png#lightbox)

-----


Полный список доступных доменов назначение, см. в разделе Apple [намерение домены ссылка](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Настройка основного класса

Затем разработчик потребуется настроить основной класс, который выступает в качестве главную точку входа для расширения намерением в Siri. Он должен быть подклассом `INExtension` которого соответствует `IINIntentHandler` делегата. Пример:

```csharp
using System;
using System.Collections.Generic;

using Foundation;
using Intents;

namespace MonkeyChatIntents
{
    [Register ("IntentHandler")]
    public class IntentHandler : INExtension, IINSendMessageIntentHandling, IINSearchForMessagesIntentHandling, IINSetMessageAttributeIntentHandling
    {
        #region Constructors
        protected IntentHandler (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override NSObject GetHandler (INIntent intent)
        {
            // This is the default implementation.  If you want different objects to handle different intents,
            // you can override this and return the handler you want for that particular intent.

            return this;
        }
        #endregion
        ...
    }
}
```

Имеется один метод одиночные, необходимо реализовать приложение на основной класс расширения намерение `GetHandler` метод. Этот метод передан целью по SiriKit и приложение должно возвращать **намерение обработчик** , соответствующий типу заданной цели.

Так как пример приложения MonkeyChat обрабатывает только одной целью, он возвращает сам в `GetHandler` метод. Если расширение обработано более одной целью, разработчик будет добавлен класс для каждого типа намерения и возвратить экземпляр на основе `Intent` передается в метод.

### <a name="handling-the-resolve-stage"></a>Обработка на этапе разрешения

На этапе разрешения где расширения намерение будет уточнение и проверка параметров передается в из Siri и устанавливаются диалога пользователя.

Для каждого параметра, который отправляется из Siri, имеется `Resolve` метод. Приложение, необходимо реализовать этот метод для каждого параметра, что приложение может потребоваться Siri на справку для получения правильного ответа от пользователя.

В случае MonkeyChat примера приложения цель расширения потребуется одного или нескольких получателей для отправки сообщения. Для каждого получателя в списке расширения необходимо выполнить поиск контактов, который можно будет получен следующий результат:

- Найден ровно один соответствующий запросу контакт.
- Обнаружены две или более подходящие контакты.
- Нет сопоставления контактов не найдены.

Кроме того MonkeyChat требует содержимое тела сообщения. Если пользователь не предоставил это, Siri должна запрашивать у пользователя для содержимого.

Расширение намерение будет должны правильно обрабатывать каждый из этих случаев.


```csharp
[Export ("resolveRecipientsForSearchForMessages:withCompletion:")]
public void ResolveRecipients (INSendMessageIntent intent, Action<INPersonResolutionResult []> completion)
{
    var recipients = intent.Recipients;
    // If no recipients were provided we'll need to prompt for a value.
    if (recipients.Length == 0) {
        completion (new INPersonResolutionResult [] { (INPersonResolutionResult)INPersonResolutionResult.NeedsValue });
        return;
    }

    var resolutionResults = new List<INPersonResolutionResult> ();

    foreach (var recipient in recipients) {
        var matchingContacts = new INPerson [] { recipient }; // Implement your contact matching logic here to create an array of matching contacts
        if (matchingContacts.Length > 1) {
            // We need Siri's help to ask user to pick one from the matches.
            resolutionResults.Add (INPersonResolutionResult.GetDisambiguation (matchingContacts));
        } else if (matchingContacts.Length == 1) {
            // We have exactly one matching contact
            resolutionResults.Add (INPersonResolutionResult.GetSuccess (recipient));
        } else {
            // We have no contacts matching the description provided
            resolutionResults.Add ((INPersonResolutionResult)INPersonResolutionResult.Unsupported);
        }
    }

    completion (resolutionResults.ToArray ());
}

[Export ("resolveContentForSendMessage:withCompletion:")]
public void ResolveContent (INSendMessageIntent intent, Action<INStringResolutionResult> completion)
{
    var text = intent.Content;
    if (!string.IsNullOrEmpty (text))
        completion (INStringResolutionResult.GetSuccess (text));
    else
        completion ((INStringResolutionResult)INStringResolutionResult.NeedsValue);
}
```

Дополнительные сведения см. в разделе нашей [разрешить этап Reference](~/ios/platform/sirikit/understanding-sirikit.md) и Apple [обработки Справочник целей и устранение](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ResolvingandHandlingIntents.html#//apple_ref/doc/uid/TP40016875-CH5-SW1).

### <a name="handling-the-confirm-stage"></a>Обработка Confirm рабочей области

На этапе подтверждения — где расширения намерение проверяет наличие в нем все данные для выполнения запроса пользователя. Приложение хочет отправить подтверждение вдоль будет дополнительных сведений о том, что приближается к произойдет Siri, он может быть подтверждена с пользователем, это предполагаемое действие.

```csharp
[Export ("confirmSendMessage:completion:")]
public void ConfirmSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Verify user is authenticated and the app is ready to send a message.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Ready, userActivity);
    completion (response);
}
```

Дополнительные сведения см. в разделе нашей [подтверждение этап Reference](~/ios/platform/sirikit/understanding-sirikit.md).

### <a name="processing-the-intent"></a>Назначение обработки

Это точка, где расширение намерение фактически выполняет задачи для выполнения запроса пользователя и передавать результаты Siri, пользователь получает уведомление о.


```csharp
public void HandleSendMessage (INSendMessageIntent intent, Action<INSendMessageIntentResponse> completion)
{
    // Implement the application logic to send a message here.
    ...

    var userActivity = new NSUserActivity (nameof (INSendMessageIntent));
    var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, userActivity);
    completion (response);
}

public void HandleSearchForMessages (INSearchForMessagesIntent intent, Action<INSearchForMessagesIntentResponse> completion)
{
    // Implement the application logic to find a message that matches the information in the intent.
    ...

    var userActivity = new NSUserActivity (nameof (INSearchForMessagesIntent));
    var response = new INSearchForMessagesIntentResponse (INSearchForMessagesIntentResponseCode.Success, userActivity);

    // Initialize with found message's attributes
    var sender = new INPerson (new INPersonHandle ("sarah@example.com", INPersonHandleType.EmailAddress), null, "Sarah", null, null, null);
    var recipient = new INPerson (new INPersonHandle ("+1-415-555-5555", INPersonHandleType.PhoneNumber), null, "John", null, null, null);
    var message = new INMessage ("identifier", "I am so excited about SiriKit!", NSDate.Now, sender, new INPerson [] { recipient });
    response.Messages = new INMessage [] { message };
    completion (response);
}

public void HandleSetMessageAttribute (INSetMessageAttributeIntent intent, Action<INSetMessageAttributeIntentResponse> completion)
{
    // Implement the application logic to set the message attribute here.
    ...

    var userActivity = new NSUserActivity (nameof (INSetMessageAttributeIntent));
    var response = new INSetMessageAttributeIntentResponse (INSetMessageAttributeIntentResponseCode.Success, userActivity);
    completion (response);
}
```

Дополнительные сведения см. в разделе нашей [обработки этап Reference](~/ios/platform/sirikit/understanding-sirikit.md).

## <a name="adding-an-intents-ui-extension"></a>Добавление целей расширения пользовательского интерфейса

Дополнительное расширение пользовательского интерфейса для целей предоставляет возможность выпуска пользовательского интерфейса приложения и фирменной символики в среду Siri и пользователей, которые вы подключены к приложению. Данное расширение приложение может обеспечить фирменной символики, а также visual и других сведений на запись.

[![](implementing-sirikit-images/intentsui01.png "Пример вывода целей расширения пользовательского интерфейса")](implementing-sirikit-images/intentsui01.png#lightbox)

Так же, как модуль намерения разработчика будет выполнять следующий шаг для расширения пользовательского интерфейса для целей:

- Добавьте в решение приложения Xamarin.iOS проект целей расширения пользовательского интерфейса.
- Настройка расширения пользовательского интерфейса целей `Info.plist` файла.
- Измените основной класс целей расширения пользовательского интерфейса.

Дополнительные сведения см. в разделе нашей [Reference расширения пользовательского интерфейса целей](~/ios/platform/sirikit/understanding-sirikit.md) и Apple [предоставления ссылок пользовательский интерфейс](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/ProvidingaCustomInterface.html#//apple_ref/doc/uid/TP40016875-CH7-SW1).

### <a name="creating-the-extension"></a>Создание расширения

Для добавления расширения пользовательского интерфейса целей в решение, выполните следующие действия.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Щелкните правой кнопкой мыши **имя решения** в **Pad решения** и выберите **добавить** > **добавить новый проект...** .
2. В диалоговом окне выберите **iOS** > **расширения** > **расширения пользовательского интерфейса намерение** и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/intents11.png "Выберите расширение намерения пользовательского интерфейса")](implementing-sirikit-images/intents11.png#lightbox)
3. Далее введите **имя** намерение расширение и нажмите кнопку **Далее** кнопки: 

    [![](implementing-sirikit-images/intents12.png "Введите имя для модуля намерений")](implementing-sirikit-images/intents12.png#lightbox)
4. Наконец, нажмите кнопку **создать** кнопку, чтобы добавить расширение намерением в решение приложения: 

    [![](implementing-sirikit-images/intents13.png "Добавьте в решение приложения расширение цели")](implementing-sirikit-images/intents13.png#lightbox)
5. В **обозревателе решений**, щелкните правой кнопкой мыши **ссылки** папки только что созданный намерение расширения. Проверьте имя Общие библиотеки проекта общего кода (создания приложения выше) и нажмите кнопку **ОК** кнопки: 

    [![](implementing-sirikit-images/intents14.png "Выберите имя проекта библиотеки общих общего кода")](implementing-sirikit-images/intents14.png#lightbox)
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Щелкните правой кнопкой мыши **имя решения** в **обозревателе решений** и выберите **добавить** > **добавить новый проект...**
2. В диалоговом окне выберите **iOS** > **расширения** > **расширения пользовательского интерфейса намерение** и нажмите кнопку **Далее** кнопка.
3. Далее введите **имя** намерение расширение и нажмите кнопку **ОК** кнопки.
5. В **обозревателе решений**, щелкните правой кнопкой мыши **ссылки** папки только что созданный намерение расширения. Проверьте имя Общие библиотеки проекта общего кода (создания приложения выше) и нажмите кнопку **ОК** кнопки.
    
-----

### <a name="configuring-the-infoplist"></a>Настройка Info.plist

Настройка расширения пользовательского интерфейса целей `Info.plist` файл для работы с приложением.

Так же, как и любое расширение типичные приложения, приложение будет иметь существующие ключи `NSExtension` и `NSExtensionAttributes`. Для целей расширения имеется один новый атрибут, который должен быть настроен:

[![](implementing-sirikit-images/intents03.png "Один новый атрибут, который должен быть настроен")](implementing-sirikit-images/intents03.png#lightbox)

**IntentsSupported** является обязательным и состоит из массива имен классов намерение приложения требуется поддержки с целью расширения.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Настройка расширения пользовательского интерфейса намерение `Info.plist` файла, дважды щелкните его в **обозревателе решений** чтобы открыть его для редактирования. Теперь, переключитесь в **источника** просмотра, а затем разверните `NSExtension` и `NSExtensionAttributes` ключей в редактор:

[![](implementing-sirikit-images/intents04.png "Ключи NSExtension и NSExtensionAttributes в редакторе")](implementing-sirikit-images/intents04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Настройка расширения пользовательского интерфейса намерение `Info.plist` файла, дважды щелкните его в **обозревателе решений** чтобы открыть его для редактирования. Разверните `NSExtension` и `NSExtensionAttributes` ключей в редактор:

[![](implementing-sirikit-images/intents04w.png "Указанное NSExtension и NSExtensionAttributes ключи в редакторе")](implementing-sirikit-images/intents04w.png#lightbox)

-----

Разверните `IntentsSupported` раздел и добавить это имя класса цель, это расширение будет поддерживать. Для примера приложения MonkeyChat, он поддерживает `INSendMessageIntent`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](implementing-sirikit-images/intents15.png "Ключ INSendMessageIntent")](implementing-sirikit-images/intents15.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](implementing-sirikit-images/intents15w.png "Ключ INSendMessageIntent")](implementing-sirikit-images/intents15w.png#lightbox)

-----

Полный список доступных доменов назначение, см. в разделе Apple [намерение домены ссылка](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2).

### <a name="configuring-the-main-class"></a>Настройка основного класса

Настройте основной класс, который выступает в качестве главную точку входа для расширения пользовательского интерфейса намерением в Siri. Он должен быть подклассом `UIViewController` которого соответствует `IINUIHostedViewController` интерфейса. Пример:

```csharp
using System;
using Foundation;
using CoreGraphics;
using Intents;
using IntentsUI;
using UIKit;

namespace MonkeyChatIntentsUI
{
    public partial class IntentViewController : UIViewController, IINUIHostedViewControlling
    {
        #region Constructors
        protected IntentViewController (IntPtr handle) : base (handle)
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

        public override void DidReceiveMemoryWarning ()
        {
            // Releases the view if it doesn't have a superview.
            base.DidReceiveMemoryWarning ();

            // Release any cached data, images, etc that aren't in use.
        }
        #endregion

        #region Public Methods
        [Export ("configureWithInteraction:context:completion:")]
        public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
        {
            // Do configuration here, including preparing views and calculating a desired size for presentation.

            if (completion != null)
                completion (DesiredSize ());
        }

        [Export ("desiredSize:")]
        public CGSize DesiredSize ()
        {
            return ExtensionContext.GetHostedViewMaximumAllowedSize ();
        }
        #endregion
    }
}
```

Передает Siri `INInteraction` экземпляр класса `Configure` метод `UIViewController` экземпляра внутри расширения пользовательского интерфейса назначение.

`INInteraction` Предоставляет три основных элемента информации для расширения:

1. Блокировка с намерением обрабатываемого объекта.
2. Объект ответа намерение из `Confirm` и `Handle` методов расширения назначение.
3. Состояние взаимодействие, которое определяет состояние взаимодействие между приложением и Siri.

`UIViewController` Экземпляр — это класс принцип для взаимодействия с Siri и, поскольку он наследует от `UIViewController`, у нее есть доступ ко всем возможностям UIKit.

При вызове метода Siri `Configure` метод `UIViewController` передает контекст представления, о том, что представление-контроллер будет размещаться либо в Siri Snippit или карта карты.

Siri будет также передать в обработчик завершения, приложения должны возвращать требуемый размер представления после завершения его настройки приложения.

### <a name="design-the-ui-in-ios-designer"></a>Разработка пользовательского интерфейса, в конструкторе iOS

Макет расширения пользовательского интерфейса намерения пользователя интерфейс в конструкторе iOS. Дважды щелкните расширения `MainInterface.storyboard` файла в **обозревателе решений** чтобы открыть его для редактирования. Перетащите все необходимые элементы пользовательского интерфейса для построения пользовательского интерфейса и сохраните изменения.

> [!IMPORTANT]
> **Примечание:** хотя и существует возможность добавить интерактивные элементы, такие как `UIButtons` или `UITextFields` для расширения пользовательского интерфейса намерение `UIViewController`, эти строго недопустимы в качестве намерения пользовательского интерфейса в неинтерактивной и пользователь не сможет взаимодействовать с ними.

### <a name="wire-up-the-user-interface"></a>Доступ к сети пользовательского интерфейса

С помощью расширения пользовательского интерфейса целей пользовательского интерфейса, созданные в конструкторе iOS, редактировать `UIViewController` подкласс и переопределить `Configure` метод следующим образом:

```csharp
[Export ("configureWithInteraction:context:completion:")]
public void Configure (INInteraction interaction, INUIHostedViewContext context, Action<CGSize> completion)
{
    // Do configuration here, including preparing views and calculating a desired size for presentation.
    ...

    // Return desired size
    if (completion != null)
        completion (DesiredSize ());
}

[Export ("desiredSize:")]
public CGSize DesiredSize ()
{
    return ExtensionContext.GetHostedViewMaximumAllowedSize ();
}
```

### <a name="overriding-the-default-siri-ui"></a>Переопределение Siri пользовательский Интерфейс по умолчанию

Расширения пользовательского интерфейса целей будут всегда отображаться вместе с другим содержимым Siri как значок приложения и имя в верхней части пользовательского интерфейса или, на основе пожеланий, кнопок (например, отправки или "Отмена") может быть отображен в нижней.

Существует несколько экземпляров, где приложение может заменить сведения, отображающий Siri пользователю по умолчанию, например системы обмена сообщениями или карты, где приложение можно заменить по умолчанию среда специально созданных для приложения.

Если необходимо переопределить элементы пользовательского интерфейса Siri, по умолчанию расширения пользовательского интерфейса целей `UIViewController` подкласс потребуется реализовать `IINUIHostedViewSiriProviding` интерфейс и согласиться на отображение элемента определенный интерфейс.

Добавьте следующий код в `UIViewController` подкласс сообщить Siri расширения пользовательского интерфейса намерение уже отображается содержимое сообщения:

```csharp
public bool DisplaysMessage {
    get {return true;}
}
```

### <a name="considerations"></a>Особенности

Apple предполагает, что разработчик принимать во внимание при разработке и реализации расширения пользовательского интерфейса намерение следующие аспекты:

- **Быть обоснованное из использования памяти** — так как расширения пользовательского интерфейса намерение являются временными и только показанные в течение некоторого времени, система налагает более жесткие ограничения памяти, чем используются с полной приложением.
- **Рассмотрим минимальный и максимальный размеры представления** -убедитесь, что намерением расширения пользовательского интерфейса выглядит неплохо на каждый тип устройства iOS, размер и ориентацию. Кроме того по размеру, приложение отправляет Siri не можно предоставлять.
- **Использование адаптивной шаблонов макета и гибкая** — еще раз, чтобы убедиться, что пользовательский Интерфейс смотрятся на каждом устройстве.

## <a name="summary"></a>Сводка

Эта статья охваченных SiriKit и показано, как его можно добавить в приложения Xamarin.iOS для предоставления служб, которые доступны пользователю, с помощью Siri и Maps приложения на устройстве iOS.




## <a name="related-links"></a>Связанные ссылки

- [Образец ElizaChat](https://developer.xamarin.com/samples/monotouch/ios10/ElizaChat/)
- [Руководство по программированию SiriKit](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
- [Справочник по Framework целей](https://developer.apple.com/reference/intents)
- [Способы ссылке платформы пользовательского интерфейса](https://developer.apple.com/reference/intentsui)
- [Блокировка с намерением домены ссылки](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/SiriDomains.html#//apple_ref/doc/uid/TP40016875-CH9-SW2)
