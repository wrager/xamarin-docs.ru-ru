---
title: Функции nougat
description: Как приступить к работе с Xamarin.Android для разработки приложений для Android Nougat.
ms.prod: xamarin
ms.assetid: 5C74ABE2-C862-4ED0-8EA5-C7FEE5251D4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: fe544f8ac677987f8921ccb1c11b8930811b9553
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="nougat-features"></a>Функции nougat

_Как приступить к работе с Xamarin.Android для разработки приложений для Android Nougat._

Эта статья содержит общие функции, представленные в Android Nougat объясняется способ подготовки Xamarin.Android для разработки приложений Android Nougat и содержит ссылки на примеры приложений, которые показывают, как использовать возможности Android Nougat Xamarin.Android приложения.


## <a name="overview"></a>Обзор

[Android Nougat](https://developer.android.com/about/versions/nougat/android-7.0.html) — Google отслеживания для Android 6.0 Marshmallow. Поддерживает Xamarin.Android **Android 7.x привязок** в Xamarin Android 7.0 и более поздних версий. Android Nougat добавляет многие новые API для функции Nougat, описанные ниже; Эти API доступны для приложений Xamarin.Android, при использовании Xamarin.Android 7.0.

[![Образы героя планшеты и телефоны под управлением Android Nougat Android](nougat-images/android-n-hero-sml.png)](nougat-images/android-n-hero.png#lightbox)

Дополнительные сведения о Android 7.x API-интерфейсы в разделе [Android 7.1 для разработчиков](http://developer.android.com/preview/api-overview.html).
Список известных проблем Xamarin.Android 7.0, см. в разделе [заметки о выпуске](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/).

Android Nougat предоставляют множество новых функций, представляющие интерес для разработчиков Xamarin.Android. Эти функции включают перечисленные ниже.

-   **Поддержка нескольких окон** &ndash; это улучшение позволяет пользователям одновременно открыть два приложения на экране.

-   **Усовершенствования уведомления** &ndash; включает модернизированный уведомления системы в Android Nougat *прямого ответа* компонент, который пользователи могли быстро реагировать на текстовые сообщения непосредственно из уведомления ПОЛЬЗОВАТЕЛЬСКИЙ ИНТЕРФЕЙС. Кроме того, если приложение создает уведомления для полученных сообщений, новый *объединенными уведомления* функции можно объединить уведомления вместе как одну группу при получении нескольких сообщений.

-   **Экономия данных** &ndash; этой функции — это новая служба системы, которая позволяет уменьшить использование передачи данных в приложениях; он позволяет контролировать использование приложений передачи данных.

Кроме того, Android Nougat предоставляет множество улучшений интерес для разработки приложений, таких как новая функция конфигурации безопасности сети Doze в поездке, ключа аттестации, новые быстрого параметры API-интерфейсы, поддержка нескольких языкового стандарта, ICU4J API, WebView улучшения, доступ к функциям языка Java 8, доступ к каталогу с областью, API пользовательских указатель, виртуальной Реальности поддержка платформы, виртуальные файлы и фоновой обработки оптимизации.

В этой статье объясняется, как приступить к созданию приложений с Android Nougat опробовать новых возможностей и планировать работу миграции или компонента для платформы Android Nougat.


## <a name="requirements"></a>Требования

Для использования новых возможностей Android Nougat приложения на основе Xamarin требуется следующее.

-   **Visual Studio или Visual Studio для Mac** &ndash; Если вы используете Visual Studio версии 4.2.0.628 или более поздней Xamarin для Visual Studio не требуется. При использовании Visual Studio для Mac версии 6.1.0 или более поздней версии Visual Studio для Mac не требуется.

-   **Xamarin.Android** &ndash; Xamarin.Android 7.0 или более поздней версии необходимо установить и настроить с помощью Visual Studio или Visual Studio для Mac.

-   **Пакет SDK для Android** -Android 7.0 SDK (API 24) или более поздней версии должны устанавливаться через диспетчер Android SDK.

-   **Java Developer Kit** &ndash; требует разработки Xamarin Android 7.0 [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) или более поздней версии, если вы разрабатываете API уровня 24 или выше (JDK 8 также поддерживает уровни API более ранних, чем 24). 64-разрядной версии JDK 8 является обязательным, если вы используете пользовательские элементы управления или средство предварительного просмотра формы.

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.

Обратите внимание, что приложения должны быть перестроены Xamarin C6SR4 или более поздней версии для надежной работы с Android Nougat. Поскольку Android Nougat можно связать только с [NDK собственного библиотеках](https://developer.android.com/about/versions/nougat/android-7.0-changes.html), существующие приложения, такие как использование библиотек **Mono.Data.Sqlite.dll** аварийно завершает работу при работе в Android Nougat, если они не правильно Перестроить.



## <a name="getting-started"></a>Начало работы

Чтобы приступить к работе с Android Nougat Xamarin.Android, необходимо загрузить и установить новейшие средства и пакеты SDK, прежде чем создавать проект Android Nougat:

1.  Установите последние обновления Xamarin.Android из Xamarin.

2.  Установка **Android 7.0 (API 24)** пакеты и средства или более поздней версии.

3.  Создайте новый проект Xamarin.Android, предназначенное Android Nougat.

4.  Настройка для Android Nougat эмуляторе или устройстве.

Каждое из этих действий описан в следующих разделах:


### <a name="install-xamarin-updates"></a>Установка обновлений для Xamarin

Чтобы добавить поддержку Xamarin Android Nougat, смены каналов обновлений в Visual Studio или Visual Studio для Mac для Stable канала и применить последние обновления. Если необходимо также функции, которые в настоящее время доступны только в канале альфа или бета-версии, можно переключиться в канал альфа или бета-версии (каналов альфа- и бета-версии также поддерживают Android 7.x). Сведения о способах изменения канала обновлений (выпуски) см. в разделе [канал обновления](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).



### <a name="install-the-android-sdk"></a>Установка пакета SDK для Android

Создание проекта Xamarin Android 7.0, сначала необходимо использовать диспетчер Android SDK Manager для установки **пакет SDK для платформы Android N (API 24)** или более поздней версии. Также необходимо установить последнюю версию **пакет инструментов SDK Android**:

1.  Запустите диспетчер Android SDK (в Visual Studio для Mac с помощью **Сервис > Откройте диспетчер пакета SDK Android&hellip;**; в Visual Studio, используйте **средства > Android > Диспетчер Android SDK Manager**).

2.  Установка **Android 7.0 (API 24)** или более поздней версии:

    [![Выбор Android SDK Manager пакеты Android 7.0](nougat-images/preview-packages.png)](nougat-images/preview-packages.png#lightbox)

3.  Установите инструменты самой последней версии пакета SDK для Android:

    [![Выбор Android SDK Manager новейшие средства пакета SDK для Android](nougat-images/preview-tools.png)](nougat-images/preview-tools.png#lightbox)

    Необходимо установить пакет инструментов SDK Android версии 25.2.2 или более поздней версии, Android SDK платформы средств, 24.0.3 и более поздней версии и Android SDK сборки 24.0.2 или более поздней версии.

4.  Убедитесь, что **Java Development Kit расположение** настроен для JDK 1.8:

    [![Настройка пути JDK 8 в разделе Сервис — Параметры](nougat-images/use-jdk-1.8.png)](nougat-images/use-jdk-1.8.png#lightbox)

    Чтобы просмотреть этот параметр в Visual Studio, нажмите кнопку **Сервис > Параметры > Xamarin > Параметры Android**. В Visual Studio для Mac, выберите **предпочтения > проекты > расположения пакета SDK > Android**.



### <a name="start-a-xamarinandroid-project"></a>Запуск проекта Xamarin.Android

Создание нового проекта Xamarin.Android. Если вы не знакомы с разработки приложений Android с помощью Xamarin, см. раздел [Привет, Android](~/android/get-started/hello-android/index.md) Дополнительные сведения о создании проектов Xamarin.Android.

При создании проекта Android, необходимо настроить параметры целевой объект Android 7.0 или более поздней версии. Например, для целевого объекта проекта для Android 7.0, необходимо настроить целевой уровень Android API проекта, чтобы **Android 7.0 (API 24 - Nougat)**. Рекомендуется установить уровень target framework API 24 или более поздней версии. Дополнительные сведения о настройке Android API уровня уровней см. в разделе [основные сведения об уровнях Android API](~/android/app-fundamentals/android-api-levels.md).


> [!NOTE]
> В настоящее время необходимо задать **Android Минимальная версия** для **Android 7.0 (API 24 - Nougat)** для развертывания приложения на Android Nougat устройствах или эмуляторах.



### <a name="configure-an-emulator-or-device"></a>Настройка эмулятора или устройства

Если вы используете эмулятор, запустите диспетчер AVD Android и создать новое устройство, используя следующие параметры:

-   Устройство: Nexus 5 X Nexus 6 Nexus Nexus 6P, проигрыватель хранилища, 9 или пиксель C.
-   Целевая платформа: Android 7.0 - уровень API 24
-   Интерфейс ABI: x86 или x86\_64

Например это виртуальное устройство настраивается для эмуляции 6 хранилища:

[![Настройка с помощью Nexus 6 устройства, целевой Android 7.0 и Intel Atom x86 ЦП/ABI AVD](nougat-images/android-n-avd.png)](nougat-images/android-n-avd.png#lightbox)

При использовании физического устройства, такие как это связующее 5 X, 6 или 9 можно либо обновить устройство через автоматическое за обновлениями, беспроводные или загружать образ системы и flash устройства, напрямую. Дополнительные сведения о обновление устройства Android Nougat вручную см. в разделе [Беспроводная изображений для устройств хранилища](https://developers.google.com/android/nexus/ota).

Обратите внимание, что устройств 5 хранилища не поддерживается Android Nougat.



## <a name="new-features"></a>Новые функции

Android Nougat дает ряд новых функций и возможностей, таких как поддержка нескольких окон, усовершенствования уведомления и заставки данных. В следующих разделах, выделите эти возможности и предоставляют ссылки, которые помогут вам приступить к работе с их в приложении.



### <a name="multi-window-mode"></a>Режим несколькими окна

Режим несколькими окна позволяет пользователям одновременно открыть два приложения с поддержкой полной многозадачной. Эти приложения могут выполняться side-by-side (альбомная) или один выше другие (книжная) в режиме разделенного окна.
Пользователи могут перетаскивать разделитель между приложениями, чтобы изменить их размер, и их можно вырезать и вставлять содержимое между приложениями. Когда два приложения должны быть представлены в режиме несколькими окна, выбранное действие продолжает выполняться при невыбранном действия приостановлена, но по-прежнему видны. Режим несколькими окна не изменяет жизненный цикл Android действия.

[![Пример приложения, работающие в режиме несколькими окна в книжной и альбомной ориентации](nougat-images/multi-window-mode.png)](nougat-images/multi-window-mode.png#lightbox)

Можно настроить как действия приложения Xamarin.Android поддерживают режим несколькими окна. Например можно настроить атрибуты, которые задать минимальный размер и по умолчанию высоту и ширину приложения в режиме несколькими окна. Вы можете использовать новый `Activity.IsInMultiWindowMode` свойства, чтобы определить, находится ли действие в режиме несколькими окна. Пример:

```csharp
if (!IsInMultiWindowMode) {
    multiDisabledMessage.Visibility = ViewStates.Visible;
} else {
    multiDisabledMessage.Visibility = ViewStates.Gone;
}
```

[MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) образец приложения включает в себя код C#, демонстрирующий способ воспользоваться преимуществами нескольких окна интерфейса пользователя приложения.

Дополнительные сведения о режиме несколькими окна см. в разделе [поддержка нескольких окон](https://developer.android.com/guide/topics/ui/multi-window.html).



### <a name="enhanced-notifications"></a>Улучшенные уведомления

Android Nougat вводит системы модернизированный уведомлений. Он поддерживает новый компонент прямого ответа, который позволяет пользователям быстро ответ на уведомления для входящих сообщений текст непосредственно в уведомлении пользовательского интерфейса. Начиная с Android 7.0, уведомлений, сообщения можно объединять, как одну группу при получении нескольких сообщений. Кроме того разработчики могут настраивать уведомления представлений, использовать оформление системы в уведомлениях и воспользоваться преимуществами новых шаблонов уведомлений, при создании уведомлений.


#### <a name="direct-reply"></a>Прямого ответа

Когда пользователь получает уведомление для входящего сообщения, Android Nougat позволяет ответить на сообщение в рамках уведомления (а не открываются приложения обмена сообщениями для отправки ответа).
Встроенная ответить благодаря этой функции пользователи могут быстро отреагировать на сообщение SMS или текст непосредственно в интерфейс уведомлений:

[![Снимок экрана уведомления с полем встроенного прямого ответа](nougat-images/notifications-inline-reply-sml.png)](nougat-images/notifications-inline-reply.png#lightbox)

Для поддержки этой возможности в приложении, необходимо добавить *встроенные действия ответа* в приложение через [RemoteInput](https://developer.xamarin.com/api/type/Android.App.RemoteInput/) таким образом, пользователи могут отправлять ответы через текст непосредственно из уведомления о пользовательском Интерфейсе.
Например, в следующем коде сборок `RemoteInput` для получения ввода текста, сборок, ожидающие цель для действия ответа и создает удаленный операцию ввода включен:

```csharp
// Build a RemoteInput for receiving text input:
var remoteInput = new Android.Support.V4.App.RemoteInput.Builder (EXTRA_REMOTE_REPLY)
    .SetLabel (GetString (Resource.String.reply))
    .Build ();

// Build a Pending Intent for the reply action to trigger:
PendingIntent replyIntent = PendingIntent.GetBroadcast (ApplicationContext,
                                conversation.ConversationId,
                                GetMessageReplyIntent (conversation.ConversationId),
                                PendingIntentFlags.UpdateCurrent);

// Build an Android 7.0 compatible Remote Input enabled action:
NotificationCompat.Action actionReplyByRemoteInput = new NotificationCompat.Action.Builder (
    Resource.Drawable.notification_icon,
    GetString (Resource.String.reply),
    replyIntent).AddRemoteInput (remoteInput).Build ();
```

Это действие будет добавлен на уведомление:

```csharp
// Create the notification:
NotificationCompat.Builder builder = new NotificationCompat.Builder (ApplicationContext)
   .SetSmallIcon (Resource.Drawable.notification_icon)
   ...
   .AddAction (actionReplyByRemoteInput);
```

[Службы сообщений](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) образец приложения включает в себя код C#, который демонстрирует расширение уведомления с `RemoteInput` объекта. Дополнительные сведения о добавлении ответ встроенных действий для приложения Android 7.0 или более поздней версии, см. Android [ответе уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#direct) раздела.


#### <a name="bundled-notifications"></a>Распространение уведомлений

Android Nougat можно группировать сообщения уведомления (например, раздел по сообщению) и отображать группы, а не каждого отдельного сообщения.
Это *объединенными уведомления* позволяют пользователям возможность закрыть или архивировать группу уведомлений в одно действие. Можно сдвинуть вниз для развертывания пакета уведомления, чтобы просмотреть подробные сведения о каждом уведомлении:

[![Снимок экрана примера распространение уведомлений](nougat-images/bundled-notifications-sml.png)](nougat-images/bundled-notifications.png#lightbox)

Для поддержки уведомлений о распространение, приложение может использовать [Builder.SetGroup](https://developer.xamarin.com/api/member/Android.App.Notification+Builder.SetGroup/p/System.String/) метод, чтобы объединить похожие уведомления. Дополнительные сведения о группах объединенной уведомления в Android N см. в разделе Android [объединение уведомления](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#bundle) раздела.


#### <a name="custom-views"></a>Пользовательские представления

Android Nougat позволяет создавать представления настраиваемое уведомление с заголовков уведомлений системы, действия и расширяемый макеты. Дополнительные сведения о представлениях настраиваемое уведомление в Android Nougat см. в разделе Android [усовершенствования уведомления](https://developer.android.com/about/versions/nougat/android-7.0.html#notification_enhancements) раздела.



### <a name="data-saver"></a>Экономия данных

Начиная с Android Nougat, пользователи могут включить новый *данных экономия* задание фоновых, блоки данных об использовании. Этот параметр также сообщает приложения к использованию меньшего объема данных на переднем плане, везде, где это возможно. [ConnectivityManager](https://developer.xamarin.com/api/type/Android.Net.ConnectivityManager/) была расширена в Android Nougat, чтобы приложение можно проверить ли пользователь включил заставки данных, чтобы приложения могли принимать с целью ограничить его использование данных при включении данных заставки.

Дополнительные сведения о новой функции данных заставки в Android Nougat см. в разделе Android [оптимизации использования сети данных](https://developer.android.com/training/basics/network-ops/data-saver.html) раздела.



### <a name="app-shortcuts"></a>Сочетания клавиш в приложение

Android 7.1 появился *клавиш приложения* функция, которая позволяет пользователям быстро начала общие или рекомендуемые задачи с вашим приложением.
Чтобы активировать меню ярлыков, пользователь долго нажатия значка приложения для второго и последующих &ndash; меню отображается с помощью быстрого вибрация.
Освобождение нажать клавиши в результате меню будет оставаться:

[![Пример окна приложения контекстное меню для приложения обмена сообщениями](nougat-images/app-shortcuts-sml.png)](nougat-images/app-shortcuts.png#lightbox)

Этот компонент является доступной только уровень API 25 или выше.
Дополнительные сведения о новой функции клавиш приложения в Android 7.1 см. в разделе Android [клавиш приложения](https://developer.android.com/guide/topics/ui/shortcuts.html) раздела.


### <a name="sample-code"></a>Пример кода

Несколько примеров Xamarin.Android показывают, как использовать преимущества функций Android Nougat:

-   [MultiWindowPlayground](https://developer.xamarin.com/samples/monodroid/android-n/MultiWindowPlayground/) демонстрируется использование нескольких окон API, доступных в Android Nougat. Пример приложения можно переключиться в режим несколькими windows, чтобы увидеть, как влияет жизненного цикла приложения и поведения.

-   [Службой обмена сообщениями](https://developer.xamarin.com/samples/monodroid/android-n/MessagingService/) использует простую службу, которая отправляет уведомления `NotificationCompatManager`. Он также расширяет уведомлений с `RemoteInput` объекта, чтобы разрешить устройствам Android Nougat ответ через текст непосредственно от уведомления без необходимости открывать приложение.

-   [Активные уведомления](https://developer.xamarin.com/samples/monodroid/android-n/ActiveNotifications/) демонстрируется использование `NotificationManager` API, чтобы выяснить, сколько уведомлений приложения в данный момент отображается.

-   [Областью действия доступ к каталогу](https://developer.xamarin.com/samples/monodroid/android-n/ScopedDirectoryAccess/) показано, как использовать доступ к каталогу с областью API для упрощения доступа к определенные каталоги. Это служит в качестве альтернативы для того, чтобы определить `READ_EXTERNAL_STORAGE` или `WRITE_EXTERNAL_STORAGE` разрешения в манифесте.

-   [Прямая Загрузка](https://developer.xamarin.com/samples/monodroid/android-n/DirectBoot/) показано, как сохранить данные в зашифрованные устройства хранения, который всегда будет доступно, пока устройство загружается как до и после ввода credentials(PIN/Pattern/Password) любого пользователя.


## <a name="summary"></a>Сводка

В этой статье представлена Android Nougat и объяснено, как установить и настроить новейшие средства и пакеты для разработки Xamarin.Android на Android Nougat. Оно также предоставляет обзор ключевых функций, доступных в Android Nougat со ссылками на исходный код примера, которые помогут начать создание приложения для Android Nougat.


## <a name="related-links"></a>Связанные ссылки

- [Android 7.1 для разработчиков](https://developer.android.com/about/versions/nougat/android-7.1.html)
- [Заметки о выпуске Xamarin Android 7.0](https://developer.xamarin.com/releases/android/xamarin.android_7/xamarin.android_7.0/)
