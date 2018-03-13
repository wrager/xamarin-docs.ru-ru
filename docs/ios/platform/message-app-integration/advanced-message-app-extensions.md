---
title: "Расширенные сообщения приложения расширения"
description: "В этой статье показано Дополнительно приемы работы с расширений сообщений приложения в решении Xamarin.iOS, которое интегрируется с приложением сообщения и представляет новые функциональные возможности для пользователя."
ms.topic: article
ms.prod: xamarin
ms.assetid: 394A1FDA-AF70-4493-9B2C-4CFE4BE791B6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fcfd1fd2ec9271bb5e8d9e09b43b7dc4cf3b3f12
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="advanced-message-app-extensions"></a>Расширенные сообщения приложения расширения

_В этой статье показано Дополнительно приемы работы с расширений сообщений приложения в решении Xamarin.iOS, которое интегрируется с приложением сообщения и представляет новые функциональные возможности для пользователя._


Новое в iOS 10, расширение сообщения приложение интегрируется с **сообщений** приложение и содержит новые функции для пользователя. Расширения можно отправить как текст, наклейки, файлов мультимедиа и интерактивные сообщения.

## <a name="about-message-app-extensions"></a>О расширениях сообщение приложения

Как уже говорилось выше, расширение сообщения приложение интегрируется с **сообщений** приложение и содержит новые функции для пользователя. Расширения можно отправить как текст, наклейки, файлов мультимедиа и интерактивные сообщения. Доступны два типа расширения приложения сообщение:

- **Пакеты наклейке** -содержит коллекцию наклейки, которые пользователь может добавить в сообщение. Наклейка пакетов можно создать без написания кода.
- **iMessage приложения** -может создать собственный пользовательский интерфейс в приложении сообщений для выбора наклейки, ввода текста, включая файлов мультимедиа (с помощью преобразования типов необязательно) и создание, изменение и отправке сообщений взаимодействия.

Сообщения приложений расширения предоставляют три основных типа содержимого:

- **Интерактивные сообщения** -— это тип содержимого пользовательское сообщение, которое создает приложение, когда пользователь нажимает на сообщения, приложение будет запускаться на переднем плане.
- **Наклейки** -образов, созданный приложением, которое может быть включено в сообщениях, отправляемых между пользователями. См. наш [мороженого построитель](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) образец приложения для пример реализации наклейке пакет приложения.
- **Другое содержимое поддерживается** -приложение может предоставить содержимое, например фотографий, видео, текст или ссылки типа, который всегда поддерживается сообщений приложения.

Новые для iOS 10 сообщения приложения теперь включает собственный выделенный, встроенных App Store. Все приложения, имеющие расширения приложений сообщений будет отображаются и в этом хранилище. Ящик приложения новые сообщения будут отображаться все приложения, которые были загружены в магазине приложений сообщений для обеспечения быстрого доступа для пользователей.

Также новые в iOS 10 Apple добавил однозначного соответствия приложения встроенного которой пользователь может легко обнаружить приложение. Например если одного пользователя из приложения, которое не имеет 2-й пользователь отправляет содержимое на другой (например, наклейке для примера), имя приложения, отправляющего отображается в области содержимого в журнале сообщений. Если пользователь нажимает кнопку приложения name, мы открыть сообщение App Store и приложения, выбранного в хранилище.

Сообщения приложений расширения похожи на существующие приложения iOS, разработчику не знакомы с созданием и получат доступ для всех стандартных платформ и функций приложения стандартных операций ввода-вывода. Пример:

- Они имеют доступ к Покупка из приложения.
- Они имеют доступ к Apple Pay.
- Они имеют доступ к оборудованию устройства, такие как камеры.

Расширения приложений сообщений поддерживается только для операций ввода-вывода 10, тем не менее, можно просматривать на устройствах watchOS и macOS содержимое, которое будет отправлять эти расширения. Новый _недавних объектов страницы_ добавлен watchOS 3, отображает последние наклейки, которые были отправлены по телефону, включая модули обработки сообщений приложения, и пользователи могут отправить эти наклейки с часами.

## <a name="about-interactive-messages"></a>О сообщениях, интерактивный

Интерактивные сообщения представить пузырек пользовательского сообщения и предоставляемые расширением сообщений приложения. Они позволяют пользователю создавать интерактивные сообщения содержимого, вставьте его в поле ввода сообщение и отправьте его.

[![](advanced-message-app-extensions-images/interactive01.png "Создание содержимого интерактивной сообщения")](advanced-message-app-extensions-images/interactive01.png#lightbox)

Получателя может отвечать на сообщения об интерактивной, коснувшись его пузырьков сообщения в журнале сообщений для загрузки расширения сообщений приложения, которому она создана. Расширение будет запущенной весь экран и разрешить пользователю для составления ответа и отправить его исходного пользователя.

[![](advanced-message-app-extensions-images/interactive02.png "Расширение запущен в полноэкранном режиме")](advanced-message-app-extensions-images/interactive02.png#lightbox)


Ниже подробно рассматриваются следующие темы:

- Обзор интерфейса API сообщений
- Жизненный цикл расширения
- Сообщения
- Отправка сообщения

## <a name="messages-api-overview"></a>Обзор интерфейса API сообщений

При вызове пользователем расширение приложения сообщение будет отображаться в нижней части сообщения из журнала в режиме компактное представление:

[![](advanced-message-app-extensions-images/interactive03.png "Обзор интерфейса API сообщений")](advanced-message-app-extensions-images/interactive03.png#lightbox)

1. `MSMessageAppViewController` Объект в расширение сообщений приложения является основной класс, который вызывается при расширении представления отображается для пользователя.
2. Диалог выводится пользователю как `MSConversation` экземпляр объекта.
3. `MSMessage` Класс представляет данной пузырьковой сообщения в диалоге.
4. `MSSession` Определяет способ отправки сообщения.
5. `MSMessageTemplateLayout` Управляет отображением сообщения

## <a name="the-extension-lifecycle"></a>Жизненный цикл расширения

Рассмотрим процесс расширения приложения сообщения, активацию:

[![](advanced-message-app-extensions-images/interactive04.png "Процесс расширения приложения сообщения, активацию")](advanced-message-app-extensions-images/interactive04.png#lightbox)

1. При запуске расширения (например, из ящика приложения) сообщение приложения будет запущен процесс.
2. `DidBecomeActive` Именем метода и передается `MSConversation` , представляющий диалога, в котором выполняется расширение приложения сообщения.
3. Так как расширение основан `UIViewController` оба `ViewWillAppear` и `ViewDidAppear` вызываются.

Далее рассмотрим процесс расширения приложения сообщение становится деактивации:

[![](advanced-message-app-extensions-images/interactive05.png "Процесс расширения приложения сообщение становится деактивации")](advanced-message-app-extensions-images/interactive05.png#lightbox)

1. При отключении расширения сообщений приложения, `ViewWillDisappear` сначала будет вызван метод.
2. Затем `ViewDidDisappear` вызываемого метода.
3. `WillResignActive` Именем метода и передается `MSConversation` , представляющий диалога, в котором выполняется расширение приложения сообщения. На этом этапе соединение между приложением сообщения и расширения будут выпущены.
4. В более поздний момент процесс завершается приложением сообщения.

Так как расширение недолго процесса, он завершается агрессивно системой в целях экономии обработки и от аккумулятора. Разработчику необходимо учитывать это при разработке и реализации расширения сообщений приложения.

## <a name="composing-a-message"></a>Сообщения

После расширения приложения сообщения выполняется в процессе и отображается пользовательский интерфейс, можно использовать следующий код для сообщения:

```csharp
MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
{
    var components = new NSUrlComponents {
        QueryItems = iceCream.QueryItems
    };

    var layout = new MSMessageTemplateLayout {
        Image = iceCream.RenderSticker (true),
        Caption = caption
    };

    var message = new MSMessage (session ?? new MSSession()) {
        Url = components.Url,
        Layout = layout
    };

    return message;
}
```

Этот код создает новый `MSMessage` и задает некоторые свойства (такие как `Url`). Хотя сообщение может быть создан только для операций ввода-вывода, отправкой операций ввода-вывода и macOS для отображения.

При нажатии пузырька сообщения диалога на macOS Mac попытается открыть адрес, указанный в URL-адрес веб-браузера. В результате веб-сайта разработчика сможете показывать некоторые представление сообщения в веб-браузер на macOS на основе машины.

`AccessibilityLabel` Свойство используется с экрана, чтобы прочитать описание диалога для пользователя. `Layout` Свойство указывает, как сообщение будет отображаться, в настоящее время только `MSMessageTemplateLayout` поддерживается и имеет следующий вид:

[![](advanced-message-app-extensions-images/interactive06.png "Шаблон MSMessageTemplateLayout")](advanced-message-app-extensions-images/interactive06.png#lightbox)

`Image` Свойство `MSMessageTemplateLayout` предоставляет содержимое для основной части MessageBubble на экране. `MediaFileUrl` Свойств также предоставляет содержимое тела сообщения пузырька, но позволяет для содержимого, которое не поддерживается `UIImage` (например, файл видео, будет произведен цикл в фоновом режиме). Если оба `Image` и `MediaFileUrl` свойства предоставляются `Image` свойство будет иметь приоритет. `MediaFileUrl` Поддерживает PNG, JPEG, GIF и видео (в любом формате, который может воспроизводиться платформой проигрывателя) форматы мультимедиа.

Рекомендуемый размер — 300 x 300 пикселей в разрешении 3 x. Немного увеличить и уменьшить активы, также являются допустимыми и Apple предлагает несколько различных размеров для получения наилучших результатов тестирования. Сообщение приложения будет образец вниз и масштабировать при необходимости этот носитель.

Активы, отправленных в приемник, любой подключено будет автоматически выполняется приложением сообщения для оптимизации их из передачи по сети. По этой причине Apple не рекомендует включая текст на носителе, прикрепленных к сообщению, поскольку мультимедиа будет уменьшен и сжимаются для передачи, тем самым потенциально визуализации текста преобразованной.

`ImageTitle` И `ImageSubtitle` свойства введите описание для носителя, представленных в пузырьковой сообщения. Эти свойства будут отправляться как текст получающего где они недоставало отображается в левом нижнем углу изображения.

`Caption`, `SubCaption`, `TrailingCaption` И `TrailingSubcaption` дополнительные свойства описания изображения и отображается в разделе под изображением. Все эти свойства для параметра `null` создаст пузырек сообщения без области заголовка:

[![](advanced-message-app-extensions-images/interactive07.png "Пузырек сообщения без области заголовка")](advanced-message-app-extensions-images/interactive07.png#lightbox)

Последний следует заметить, что приложения "сообщения" прорисовывает значок расширения сообщений приложения в верхний левый угол пузырька сообщения.

## <a name="sending-a-message"></a>Отправка сообщения

Один раз `MSMessage` был состоит, можно использовать для отправки в него следующий код:

```csharp
public void SendMessage (MSMessage message)
{
    // Insert the message into the conversation
    ActiveConversation.InsertMessage (message, (error) => {
        // Did the message send successfully?
        if (error == null) {
            // Handle successful send
        } else {
            // Report Error
            Console.WriteLine ("Error: {0}", error);
        }
    };

}
```

`ActiveConversation` Свойство `MSMessagesAppViewController` будет содержать текущего диалога, расширение приложения сообщение было запущено в.

Вызовите `InsertMessage` из `MSConversation` есть сообщение диалога и обработки всех ошибок, которые могут возникнуть. Если сообщение успешно включен, сообщение пузырек будет отображаться в поле ввода.

Кроме того расширение можно отправить различные типы данных для диалога, такие как:

- **Text** - `ActiveConversation.InsertText ("Message", (error) => {...});`
- **Вложения** - `ActiveConversation.InsertAttachment (new NSUrl ("path"), "filename", (error) => {...});`
- **Наклейки**  -  `ActiveConversation.InsertSticker (sticker, (obj) => {...});` где `sticker` — `MSSticker`.

После новое содержимое поля ввода, пользователь может отправлять сообщения, коснувшись синий **отправки** кнопку (так же, как любое обычное сообщение). Нет возможности для расширения сообщения приложения для автоматической отправки содержимого, этот процесс полностью находится под управлением пользователя.

## <a name="handling-the-compact-and-expanded-modes"></a>Режимы Compact и развернутым обработки

Расширение приложения сообщения могут отображаться в одном из двух различных режимов просмотра:

[![](advanced-message-app-extensions-images/interactive08.png "Расширение приложения сообщений, отображаемых в двух различных режимов просмотра: ко & разреженный")](advanced-message-app-extensions-images/interactive08.png#lightbox)

- **Compact** -это является режимом по умолчанию, где расширение сообщение приложения может занимать до 25% нижней представления сообщений. В компактный режим приложения нет доступа с помощью клавиатуры, горизонтальной прокрутки или проведите распознавателей жестов. Приложение может получить доступ к полю ввода и вызовы `InsertMessage` мгновенно будет отображаться для пользователя существует.
- **Развернуть** -расширение приложения сообщение заполняет все представление сообщения. Он не имеет доступа к полю ввода, но имеет доступа к клавиатуры, горизонтальной прокрутки и проведите распознавателей жестов.

Расширение приложения сообщение может переключаться между этими моделями программным способом или вручную пользователем в любое время и должен быть мгновенной реагирует на любое изменение режима в представлении.

Рассмотрим следующий пример обработки переключение между двух различных режимов просмотра. Для каждого состояния потребуются два разных представления контроллера. `StickerBrowserViewController` Дескрипторы **Compact** представление и `AddStickerViewController` обрабатывающий **разреженный** представления:

```csharp
using System;

using Messages;
using Foundation;
using UIKit;

namespace MessagesExtension {
    public partial class MessagesViewController : MSMessagesAppViewController, IIceCreamsViewControllerDelegate, IBuildIceCreamViewControllerDelegate {
        public MessagesViewController (IntPtr handle) : base (handle)
        {
        }

        public override void WillBecomeActive (MSConversation conversation)
        {
            base.WillBecomeActive (conversation);

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, PresentationStyle);
        }

        public override void WillTransition (MSMessagesAppPresentationStyle presentationStyle)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected an active converstation");

            // Present the view controller appropriate for the conversation and presentation style.
            PresentViewController (conversation, presentationStyle);
        }

        void PresentViewController (MSConversation conversation, MSMessagesAppPresentationStyle presentationStyle)
        {
            // Determine the controller to present.
            UIViewController controller;

            if (presentationStyle == MSMessagesAppPresentationStyle.Compact) {
                // Show a list of previously created ice creams.
                controller = InstantiateIceCreamsController ();
            } else {
                var iceCream = new IceCream (conversation.SelectedMessage);
                controller = iceCream.IsComplete ? InstantiateCompletedIceCreamController (iceCream) : InstantiateBuildIceCreamController (iceCream);
            }

            foreach (var child in ChildViewControllers) {
                child.WillMoveToParentViewController (null);
                child.View.RemoveFromSuperview ();
                child.RemoveFromParentViewController ();
            }

            AddChildViewController (controller);
            controller.View.Frame = View.Bounds;
            controller.View.TranslatesAutoresizingMaskIntoConstraints = false;
            View.AddSubview (controller.View);

            controller.View.LeftAnchor.ConstraintEqualTo (View.LeftAnchor).Active = true;
            controller.View.RightAnchor.ConstraintEqualTo (View.RightAnchor).Active = true;
            controller.View.TopAnchor.ConstraintEqualTo (View.TopAnchor).Active = true;
            controller.View.BottomAnchor.ConstraintEqualTo (View.BottomAnchor).Active = true;

            controller.DidMoveToParentViewController (this);
        }

        UIViewController InstantiateIceCreamsController ()
        {
            // Instantiate a `IceCreamsViewController` and present it.
            var controller = Storyboard.InstantiateViewController (IceCreamsViewController.StoryboardIdentifier) as IceCreamsViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an IceCreamsViewController from the storyboard");

            controller.Builder = this;
            return controller;
        }

        UIViewController InstantiateBuildIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (BuildIceCreamViewController.StoryboardIdentifier) as BuildIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an BuildIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            controller.Builder = this;

            return controller;
        }

        public UIViewController InstantiateCompletedIceCreamController (IceCream iceCream)
        {
            // Instantiate a `BuildIceCreamViewController` and present it.
            var controller = Storyboard.InstantiateViewController (CompletedIceCreamViewController.StoryboardIdentifier) as CompletedIceCreamViewController;
            if (controller == null)
                throw new Exception ("Unable to instantiate an CompletedIceCreamViewController from the storyboard");

            controller.IceCream = iceCream;
            return controller;
        }

        public void DidSelectAdd (IceCreamsViewController controller)
        {
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void Build (BuildIceCreamViewController controller, IceCreamPart iceCreamPart)
        {
            var conversation = ActiveConversation;
            if (conversation == null)
                throw new Exception ("Expected a conversation");

            var iceCream = controller.IceCream;
            if (iceCream == null)
                throw new Exception ("Expected the controller to be displaying an ice cream");

            string messageCaption = string.Empty;
            var b = iceCreamPart as Base;
            var s = iceCreamPart as Scoops;
            var t = iceCreamPart as Topping;

            if (b != null) {
                iceCream.Base = b;
                messageCaption = "Let's build an ice cream";
            } else if (s != null) {
                iceCream.Scoops = s;
                messageCaption = "I added some scoops";
            } else if (t != null) {
                iceCream.Topping = t;
                messageCaption = "Our finished ice cream";
            } else {
                throw new Exception ("Unexpected type of ice cream part selected.");
            }

            // Create a new message with the same session as any currently selected message.
            var message = ComposeMessage (iceCream, messageCaption, conversation.SelectedMessage?.Session);

            // Add the message to the conversation.
            conversation.InsertMessage (message, null);

            // If the ice cream is complete, save it in the history.
            if (iceCream.IsComplete) {
                var history = IceCreamHistory.Load ();
                history.Append (iceCream);
                history.Save ();
            }

            Dismiss ();
        }

        MSMessage ComposeMessage (IceCream iceCream, string caption, MSSession session = null)
        {
            var components = new NSUrlComponents {
                QueryItems = iceCream.QueryItems
            };

            var layout = new MSMessageTemplateLayout {
                Image = iceCream.RenderSticker (true),
                Caption = caption
            };

            var message = new MSMessage (session ?? new MSSession()) {
                Url = components.Url,
                Layout = layout
            };

            return message;
        }
    }
}
```

`DidTransition` Метод переопределяется для обработки переключения между двумя режимами:

```csharp
public override void DidTransition (MSMessagesAppPresentationStyle presentationStyle)
{
    base.DidTransition (presentationStyle);

    // Take action based on style
    switch (presentationStyle) {
    case MSMessagesAppPresentationStyle.Compact:
        PresentStickerBrowser ();
        break;
    case MSMessagesAppPresentationStyle.Expanded:
        PresentAddSticker ();
        break;
    }
}
```

При необходимости приложение было бы использовать `WillTransition` метод для обработки режим Просмотр, изменение, прежде чем будет предоставлен пользователю (как показано в приведенном выше примере построитель Icecream). Дополнительные сведения см. в разделе нашей [дополнительная настройка наклейке](~/ios/platform/message-app-integration/intro-to-message-app-extensions.md) документации.

## <a name="replying-to-a-message"></a>При ответе на сообщение

Существует два варианта, расширение сообщения приложения должен обработать при ответе на сообщение.

[![](advanced-message-app-extensions-images/interactive09.png "Расширение приложения сообщения в режимах неактивно и активные")](advanced-message-app-extensions-images/interactive09.png#lightbox)

- **Расширение — неактивный** -имеется один пузырьков сообщение расширения приложения сообщение в сообщение запись, предоставляющую пользователь для активации расширения и продолжить интерактивный диалог.
- **Расширение активен** -пользователя можно коснуться пузырек сообщение расширения приложения сообщение запись сообщений в режим представления раскрываются и продолжить интерактивный процесс с места остановки.

### <a name="the-extension-is-inactive"></a>Расширение неактивен.

Когда пузырек сообщения, которые используются пользователем в текст сообщения и расширения приложения сообщений неактивна, произойдет следующим образом:

[![](advanced-message-app-extensions-images/interactive10.png "Обработка неактивные пузырьков сообщения")](advanced-message-app-extensions-images/interactive10.png#lightbox)

1. Пользователь касается пузырьков расширения сообщения.
2. При запуске расширения приложения сообщение будет запущен процесс.
3. `DidBecomeActive` Именем метода и передается `MSConversation` , представляющий диалога, в котором выполняется расширение приложения сообщения.
4. Так как расширение основан `UIViewController` оба `ViewWillAppear` и `ViewDidAppear` вызываются.

По завершении процесса расширение сообщение приложения откроется в режиме просмотра раскрываются.

### <a name="the-extension-is-active"></a>Расширение активен.

При пузырек сообщения, которые используются пользователем в текст сообщения и расширения приложения сообщения активен, произойдет следующим образом:

[![](advanced-message-app-extensions-images/interactive11.png "Обработка active пузырьков сообщения")](advanced-message-app-extensions-images/interactive11.png#lightbox)

1. Пользователь касается пузырьков расширения сообщения.
2. Поскольку расширения приложения сообщение уже активно, `WillTransition` метод `MSMessagesAppViewController` вызывается для обработки, переключение с сжатия в режиме просмотра раскрываются.
3. `DidSelectMessage` Метод `MSMessagesAppViewController` вызывается и передается `MSMessage` и `MSConversation` , к которому принадлежит сообщение пузырьков.
4. `DidTransition` Метод `MSMessagesAppViewController` вызывается для обработки, переключение с сжатия в режиме просмотра раскрываются.

Еще раз при завершении процесса, расширение сообщение приложения откроется в режиме просмотра раскрываются.

### <a name="accessing-the-selected-message"></a>Доступ к выбранному сообщению

В любом случае, когда пользователь касается сообщение пузырьков, принадлежащих расширения приложения сообщение, он потребуется для получения доступа к `MSMessage` , было выполнено `SelectedMessage` свойство `MSConversation`.

Пример:

```csharp
using System;
using Foundation;
using Messages;
using UIKit;


namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        ...

        #region Override Methods
        public override void DidSelectMessage (MSMessage message, MSConversation conversation)
        {
            base.DidSelectMessage (message, conversation);

            // Get selected message
            var selected = conversation.SelectedMessage;

            // Present the user interface to continue editing
            // the conversation
            ...
        }
        #endregion
    }
}
```

Выбранное сообщение должен отображаться в пользовательском Интерфейсе расширение сообщение приложения и пользователя должны составлять ответа.

## <a name="removing-partially-completed-messages"></a>Удаление частично завершена сообщений

В процессе отправки различным этапам интерактивный диалог между двумя пользователя в диалоге, частично завершенной пузырьки сообщения можно запустить, чтобы не перегружать текст сообщения:

[![](advanced-message-app-extensions-images/interactive12.png "Можно частично завершенной пузырьки сообщение загромождения текст сообщения")](advanced-message-app-extensions-images/interactive12.png#lightbox)

Вместо этого расширения сообщения приложения следует сворачивание предыдущего сообщения пузырьков в сжатой комментария в текст сообщения:

[![](advanced-message-app-extensions-images/interactive13.png "Свертывание пузырьков предыдущего сообщения текст сообщения")](advanced-message-app-extensions-images/interactive13.png#lightbox)

Эта операция обрабатывается с помощью `MSSession` сворачивание всех существующих действий. Поэтому `DidSelectMessage` метод `MSMessagesAppViewController` класса может быть изменено для выглядеть следующим образом:

```csharp
public override void DidSelectMessage (MSMessage message, MSConversation conversation)
{
    base.DidSelectMessage (message, conversation);

    MSSession session;
    var summary = "";

    // Get selected message
    var selected = conversation.SelectedMessage;

    // Does the selected message already have a session?
    if (selected.Session == null) {
        // No, create a new session
        session = new MSSession ();
        summary = "Let's build an ice cream";
    } else {
        // Yes, use the existing session
        session = selected.Session;
        summary = "I added some scoops";
    }

    // Create an instance of the message being composed
    var existingMessage = new MSMessage (session);
    message.SummaryText = summary;
    ...

    // Present the user interface to continue editing
    // the conversation
    ...
}
```

Если выбранное сообщение уже выход из `MSSession`, в противном случае используется новый `MSSession` создается. `SummaryText` Свойство `MSMessage` используется для добавления заголовка в свернутом предыдущие шаги. Если `SummaryText` свойству `null`, на предыдущих этапах диалог будет полностью удален из Transcript диалога.

## <a name="advanced-message-api-features"></a>Дополнительные функции API сообщения

С основные возможности нового API сообщение выше подробно рассматривается далее просмотрите некоторые более сложные функции, которые Apple встроенные в framework.

Во-первых, существует несколько других методов переопределения в `MSMessagesAppViewController` классов, которые предоставляют более широкий доступ к диалога:

- `DidStartSendingMessage` — Вызывается, когда пользователь нажимает кнопку отправки. Это означает, что сообщение действительно была доставки сообщения получателю, точно так же, что запущен процесс отправки.
- `DidCancelSendingMessage` -Это происходит, когда пользователь касается *X* кнопки в правом верхнем углу пузырек сообщение Transcript диалога.
- `DidReceiveMessage` -Этот метод вызывается, при включенном расширения приложения сообщение было получено новое сообщение от одного из участников диалога.

### <a name="group-conversations"></a>Группы диалогов

Расширение сообщение приложения может использоваться во время участвующих пользователей в группы диалогов (с 3 или более лиц) и это будет необходимо принимать во внимание при разработке и реализации расширения сообщений приложения.

Рассмотрим следующие взаимодействие группы диалога с тремя пользователями:

[![](advanced-message-app-extensions-images/interactive14.png "Взаимодействие в группу диалога с трех пользователей")](advanced-message-app-extensions-images/interactive14.png#lightbox)

1. Пользователь 1 отправляет группу интерактивные сообщения попросить пользователя 2 и 3 пользователя для выбора начинки гамбургер.
2. Пользователь 2 выбирает tomatoes.
3. Пользователь 3 выбирает Соленья.
4. Пользователь 2 и 3 пользовательского выбранные поступают на 1 пользователя практически одновременно. В результате выбора пользователем 2 сворачиваться в строки сводки и недоступна. Этот случай может также была отражена, при выборе пользователем 2 выполняется свернуты показывается и пользователь 3.

В обоих случаях такое поведение является нежелательным как 1 пользователь должен иметь доступ к пользователя 2 и 3 пользовательского выбранные. Чтобы решить эту проблему, предлагает Apple, расширение приложения сообщения состояния сообщения хранятся в облаке и использовать свойство URL-адреса `MSMessage` (который отправляется между пользователями) для доступа к это состояние.

Когда пользователь отправляет сообщение, маркер сеанса создается и помещается в облако с текущим состоянием сообщения. При щелчке пользователем пузырек сообщения в диалоге записи маркера сеанса используется для получения текущего состояния сеанса из облака.

### <a name="sender-identifiers"></a>Идентификаторов отправителя

Для обсуждения, доступ к идентификатор отправителя сообщения, рассмотрим в качестве примера группы диалога, приведенном выше:

[![](advanced-message-app-extensions-images/interactive15.png "Группа диалога отправки идентификаторов")](advanced-message-app-extensions-images/interactive15.png#lightbox)

1. Снова пользователя 1 отправляет группу интерактивные сообщения попросить пользователя 2 и 3 пользователя для выбора начинки гамбургер.
2. Пользователь 3 выбирает Соленья.
3. Выбор пользователя 3 поступлении пользователя 1 и 2 пользователя еще не ответил.
4. Поскольку очень Apple отвечает конфиденциальность пользователей, расширение приложения сообщения только знает о уникальный идентификатор (как `NSUUID`), назначенный каждому участнику диалога. На локальном устройстве известен только идентификатор текущего пользователя.
5. `MSMessage` Имеет `SenderIdentifier` свойство, которое совпадает с одним пользователем в список участников известен в модуль.
6. Каждое устройство пользователя имеет собственную копию список участников, где снова, известен только идентификатор локального пользователя.
7. При отправке сообщения, его `SenderIdentifier` свойства также известно, что локального пользователя.

Идентификаторов отправителя можно использовать одним из следующих способов:

- Просмотрев список участников расширения можно получить количество пользователей в диалоге.
- Когда модуль получает сообщение от пользователя, он может отслеживать связи идентификатора отправителя. При получении другого сообщения с тем же идентификатором отправителя, расширение понимает, что из того же пользователя.
- Они могут использоваться для идентификации определенного пользователя в диалоге.

Идентификатор отправителя может использоваться в любом из полей из `MSMessageTemplateLayout` посредством задания префикса знак доллара (`$`). Пример:

```csharp
// Pass along the sender identifier
var layout = new MSMessageTemplateLayout()
layout.Caption = string.format("${0} wants pickles.",Conversation.LocalParticipantIdentifier.UuidString);
```

При отображении сообщений приложения пузырек сообщения с этим типом форматирования заменит `$uuid...` с имя контакта из участников диалога, отправившего сообщение.

Идентификатор отправителя является уникальным для каждого устройства, поэтому просмотрев на схеме выше Обратите внимание, устройства 1 пользователя и устройства 3 пользователь имеет другой уникальный идентификатор отправителя для каждого из участников диалога.

Идентификаторов отправителя распространяются на установку расширения сообщений приложения. Таким образом, если пользователь удаляет и повторно устанавливает расширение сообщение приложения для установки нового будут создаваться новые идентификаторы отправителя.

Чтобы получить доступ к идентификаторов отправителя, расширения можно использовать следующий код:

```csharp
public override void DidStartSendingMessage (MSMessage message, MSConversation conversation)
{
    base.DidStartSendingMessage (message, conversation);

    // Get the sender's participant ID
    var senderID = message.SenderParticipantIdentifier;

    // Get the local participant ID
    var localID = conversation.LocalParticipantIdentifier;

    // Get the remote participant IDs
    var remoteIDs = conversation.RemoteParticipantIdentifiers;
}
```

## <a name="supported-platforms"></a>Поддерживаемые платформы

Интерактивные сообщения, созданные расширение приложения сообщения доставляются на следующих платформах Apple:

- watchOS 3
- macOS Sierra
- iOS 10

Из трех платформ iOS только 10 позволит пользователям создавать интерактивные сообщение. На macOS Сьерра, если пользователь щелкает интерактивные пузырьков сообщение URL-адрес, присоединенных к `MSMessage` будут открыты в Safari и должны отображаться представление сообщения.

WatchOS приложение сообщений может сдачи интерактивные сообщения на подключенном устройстве iOS где пользователя можно составить ответа.

Новый API сообщений имеет поддержка возврата интерактивный отображается при получении сообщения в более старых платформ Apple:

- watchOS 2 +
- OS X 10.11 +
- iOS 9 +

Они будут доставлены в формате резервной как два отдельных сообщения:

- Один будет изображения в соответствии со макет шаблона.
- Другой будет URL-адрес, как указано в `MSMessage`.

## <a name="summary"></a>Сводка

В этой статье представил дополнительные методы работа расширения сообщений приложения в Xamarin.iOS решение, которое интегрируется с **сообщений** приложения и присутствуют новые функциональные возможности для пользователя.


## <a name="related-links"></a>Связанные ссылки

- [Построитель мороженого (пример)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Справочник по сообщений](https://developer.apple.com/reference/messages)
- [Руководство по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
