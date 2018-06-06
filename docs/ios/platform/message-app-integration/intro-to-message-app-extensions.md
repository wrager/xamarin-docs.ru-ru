---
title: Расширение приложения основы сообщений в Xamarin.iOS
description: В этой статье показано как включать расширение сообщение приложения в решении Xamarin.iOS, которое интегрируется с приложением сообщения и представляет новые функциональные возможности для пользователя.
ms.prod: xamarin
ms.assetid: 0CFB494C-376C-449D-B714-9E82644F9DA3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: bc06d73543b9e0bd1e8715fc722b0a95af7d9f07
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787914"
---
# <a name="message-app-extension-basics-in-xamarinios"></a>Расширение приложения основы сообщений в Xamarin.iOS

_В этой статье показано как включать расширение сообщение приложения в решении Xamarin.iOS, которое интегрируется с приложением сообщения и представляет новые функциональные возможности для пользователя._

Новое в iOS 10, расширение сообщения приложение интегрируется с **сообщений** приложение и содержит новые функции для пользователя. Расширения можно отправить как текст, наклейки, файлов мультимедиа и интерактивные сообщения.

## <a name="about-message-app-extensions"></a>О расширениях сообщение приложения

Как уже говорилось выше, расширение сообщения приложение интегрируется с **сообщений** приложение и содержит новые функции для пользователя. Расширения можно отправить как текст, наклейки, файлов мультимедиа и интерактивные сообщения. Доступны два типа расширения приложения сообщение:

- **Пакеты наклейке** -содержит коллекцию наклейки, которые пользователь может добавить в сообщение. Наклейка пакетов можно создать без написания кода.
- **iMessage приложения** -может создать собственный пользовательский интерфейс в приложении сообщений для выбора наклейки, ввода текста, включая файлов мультимедиа (с помощью преобразования типов необязательно) и создание, изменение и отправке сообщений взаимодействия.

Сообщения приложений расширения предоставляют три основных типа содержимого:

- **Интерактивные сообщения** -— это тип содержимого пользовательское сообщение, которое создает приложение, когда пользователь нажимает на сообщения, приложение будет запускаться на переднем плане.
- **Наклейки** -образов, созданный приложением, которое может быть включено в сообщениях, отправляемых между пользователями.
- **Другое содержимое поддерживается** - приложение может предоставить содержимое, например фотографий, видео, текст или ссылки на другие документы типы, которые всегда поддерживаются приложением сообщения.

Новые для iOS 10 сообщения приложения теперь включает собственный выделенный, встроенных App Store. Все приложения, имеющие расширения приложений сообщений будет отображаются и в этом хранилище. Ящик приложения новые сообщения будут отображаться все приложения, которые были загружены в магазине приложений сообщений для обеспечения быстрого доступа для пользователей.

Также новые в iOS 10 Apple добавил однозначного соответствия приложения встроенного которой пользователь может легко обнаружить приложение. Например если одного пользователя из приложения, которое не имеет 2-й пользователь отправляет содержимое на другой (например, наклейке для примера), имя приложения, отправляющего отображается в области содержимого в журнале сообщений. Если пользователь нажимает кнопку приложения name, мы открыть сообщение App Store и приложения, выбранного в хранилище.

Сообщения приложений расширения похожи на существующие приложения iOS, разработчику не знакомы с созданием и получат доступ для всех стандартных платформ и функций приложения стандартных операций ввода-вывода. Пример:

- Они имеют доступ к Покупка из приложения.
- Они имеют доступ к Apple Pay.
- Они имеют доступ к оборудованию устройства, такие как камеры.

Расширения приложений сообщений поддерживается только для операций ввода-вывода 10, тем не менее, можно просматривать на устройствах watchOS и macOS содержимое, которое будет отправлять эти расширения. Новый _недавних объектов страницы_ добавлен watchOS 3, отображает последние наклейки, которые были отправлены по телефону, включая модули обработки сообщений приложения, и пользователи могут отправить эти наклейки с часами.

## <a name="about-the-messages-framework"></a>О платформе сообщений

Новые для iOS 10 сообщений framework обеспечивает взаимодействие между расширения приложений для сообщения и сообщения приложения на устройства iOS. Когда пользователь запускает приложение в приложении сообщения, эта платформа позволяет приложению должны быть обнаружены и предоставляет данные и контекст, необходимый для размещения его пользовательского интерфейса.

После запуска приложения пользователь взаимодействует с его, чтобы создать новое содержимое для совместного использования в сообщении. Затем приложение использует платформу сообщений для передачи только что созданный содержимого в приложение для обработки сообщений.

Framework сообщений и расширения приложений сообщений построены на основе существующих iOS технологии расширения приложения. Дополнительные сведения о расширениях приложения см. в разделе Apple [руководство по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214).

В отличие от других точек расширения, предоставленные Apple во всей системе содержит узел приложения для их расширения приложений сообщений с момента самого сообщения приложения используется в качестве контейнера разработчику не требуется. Тем не менее разработчик может, включая расширение сообщение приложения внутри приложения iOS новый или существующий и доставки его вместе с пакета.

Если расширения сообщений приложения, включенный в пакет приложения iOS, на начальном экране устройства и в ящике сообщения приложения из в приложении сообщений будет отображаться значок приложения. Если он не входит в комплект приложения, сообщение расширения приложений отображался только в ящике сообщения приложения.

Даже если расширения сообщений приложения не включен в пакет приложения узла, разработчику необходимо предоставить значок приложения в пакете расширения приложений сообщения, как этот значок, который будет отображаться в других частях системы, например ящик сообщения приложения или параметры , для расширения.

## <a name="about-stickers"></a>О наклейки

Apple, разработанные наклейки — это новый способ iMessage пользователи могут обмениваться данными, позволяя наклейки отправляться встроенного как любое другое содержимое сообщения, или они могут присоединяться к предыдущей пузырьки сообщения внутри диалога.

Что такое наклейки

- Они являются образы, которые предоставляет расширения приложений для сообщения.
- Они могут быть либо анимированного или статического изображения.
- Они предоставляют новый способ совместного использования содержимого образа из внутри приложения.

Существует два способа создания наклейки:

1. Расширения наклейке пакет сообщений приложения могут создаваться из внутри Xcode без включения любого кода. Все, что требуется — активов для наклейки и значки приложений.
2. Создав стандартным расширением сообщения приложений, предоставляющий наклейки из код с помощью framework сообщения.

### <a name="creating-sticker-packs"></a>Создание пакетов наклейке

Пакеты наклейке создаются из специальных шаблона в Xcode и просто укажите статический набор графических ресурсов, которые можно использовать в качестве наклейки. Как уже говорилось выше, они не требуют любой код, разработчик просто перетаскивает файлы изображений в папку пакета наклейке внутри каталога наклейки активов.

Изображения должны быть включены в пакет наклейке она должна удовлетворять следующим требованиям:

- Изображения должны быть в формате PNG, APNG, GIF или JPEG. Apple предполагает использование только форматы PNG и APNG при предоставлении наклейке активы.
- Анимированный наклейки поддерживает только форматы APNG и GIF.
- Наклейке изображения должен предоставить прозрачный фон, так как их можно разместить над пузырьки сообщения в диалоге пользователем.
- Файлы отдельных изображений должно быть менее 500 КБ.
- Изображения не может быть меньше, чем 100 x 100 точек или большего размера, указывающий 206 x 206.

> [!IMPORTANT]
> Образы наклейке всегда должны предоставляться в `@3x` разрешение в диапазоне от 300 x 300 для 618 x 618 пикселей. Система автоматически создаст `@2x` и `@1x` версий во время выполнения при необходимости.

Apple предлагает тестирование активы изображений наклейке от различных различных цветной фон (например, белый, черный, красный, желтый и несколькими цветная) и более чем фотографии, чтобы убедиться, что они выглядят наиболее во всех возможных случаях.

Наклейка пакетов можно предоставить наклейки в одном из трех доступных размеров:

- **Небольшие** - 100 x 100 точек.
- **Средний** - 136 x 136 точек. Это размер по умолчанию.
- **Большие** — точек 206 x 206.

Задание размера для всего пакета наклейке и предоставляет графические активы, которые соответствуют запрошенного размера, для получения наилучших результатов в браузере наклейке внутри приложения "сообщения" только с помощью инспектора атрибуты в Xcode.

Дополнительные сведения см. в разделе нашей [мороженого построитель](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/) приложения и Apple [сообщений ссылка](https://developer.apple.com/reference/messages).

## <a name="creating-a-custom-sticker-experience"></a>Создает наклейке пользовательский интерфейс

Если приложению требуется несколько элементов управления или гибкость, чем обеспечивается пакетом наклейке, его можно включать расширение сообщение приложения и предоставить наклейки через framework сообщения взаимодействие наклейке Custom.

Каковы преимущества создает пользовательский интерфейс наклейке?

1. Позволяет приложению настраивать способ отображения наклейки для пользователей приложения. Например для представления наклейки в формате, отличные от стандартных сетки макета или в другой цвет фона.
2. Позволяет наклейки создаваться динамически из кода, вместо включения как статическое изображение активы.
3. Позволяет активы изображений наклейке динамически загружаемых из веб-сервер разработчика без необходимости выпускает новую версию в магазине приложений.
4. Для создания наклейки на лету позволяет доступ камеры устройства.
5. Позволяет покупки из приложений, пользователь может приобрести дополнительные наклейки из внутри приложения.

Чтобы создает пользовательский интерфейс наклейке, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac.
2. Откройте решение для добавления расширения приложения для сообщения. 
3. Выберите **iOS** > **расширения** > **iMessage расширения** и нажмите кнопку **Далее** кнопки: 

    [![](intro-to-message-app-extensions-images/message01.png "Выберите iMessage расширения")](intro-to-message-app-extensions-images/message01.png#lightbox)
4. Введите **имя расширения** и нажмите кнопку **Далее** кнопки: 

    [![](intro-to-message-app-extensions-images/message02.png "Введите имя расширения")](intro-to-message-app-extensions-images/message02.png#lightbox)
5. Нажмите кнопку **создать** кнопки для построения расширения: 

    [![](intro-to-message-app-extensions-images/message03.png "Нажмите кнопку \"Создать\"")](intro-to-message-app-extensions-images/message03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio.
2. Откройте решение, чтобы добавить расширение сообщений приложения.
3. Выберите ** iOS расширения > iMessage расширения (iOS) ** и нажмите кнопку **Далее** кнопки:

    [![Выберите iMessage расширения (iOS)](intro-to-message-app-extensions-images/message01.w157-sml.png)](intro-to-message-app-extensions-images/message01.w157.png#lightbox)

4. Введите **имя** и нажмите кнопку **ОК** кнопки

-----

По умолчанию `MessagesViewController.cs` файл будет добавлен в решение. Это главная точка входа в модуле, и он наследует от `MSMessageAppViewController` класса.

Сообщения framework предоставляет классы для представления доступных наклейки для пользователя:

- `MSStickerBrowserViewController` — Управляет представление, которое наклейки, отображаются в. Она также соответствует `IMSStickerBrowserViewDataSource` интерфейс для возврата числа наклейке и наклейке для браузера заданного индекса.
- `MSStickerBrowserView` -Это представление, доступные наклейки будет отображаться в.
- `MSStickerSize` -Решает размеры отдельных ячеек сетки наклейки выводятся в представлении обозревателя.

### <a name="creating-a-custom-sticker-browser"></a>Создание пользовательских наклейке браузера

Разработчик также можно настроить наклейке взаимодействие для пользователя, предоставляя браузера наклейке настраиваемый (`MSMessageAppBrowserViewController`) в расширении сообщений приложения. Браузер наклейке пользовательские изменения как наклейки должны быть представлены пользователю при их выбирается наклейке для включения в потоке сообщений.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **Pad решения**, щелкните правой кнопкой мыши имя проекта расширения и выберите **добавить** > **новый файл...**   >  **iOS | Apple Watch** > **интерфейс контроллера**.
2. Введите `StickerBrowserViewController` для **имя** и нажмите кнопку **New** кнопки: 

    [![](intro-to-message-app-extensions-images/browser01.png "Введите StickerBrowserViewController имени")](intro-to-message-app-extensions-images/browser01.png#lightbox)
3. Откройте `StickerBrowserViewController.cs` файл для редактирования.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений**, щелкните правой кнопкой мыши имя проекта расширения и выберите **добавить** > **новый файл...**   >  **iOS | Apple Watch** > **интерфейс контроллера**.
2. Введите `StickerBrowserViewController` для **имя** и нажмите кнопку **New** кнопки: 

    [![](intro-to-message-app-extensions-images/browser01.w157-sml.png "Введите StickerBrowserViewController имени")](intro-to-message-app-extensions-images/browser01.w157.png#lightbox)
3. Откройте `StickerBrowserViewController.cs` файл для редактирования.

-----

Сделать `StickerBrowserViewController.cs` выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MonkeyStickers
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MSStickerSize stickerSize) : base (stickerSize)
        {
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            ...
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();
        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            return Stickers[(int)index];
        }
        #endregion
    }
}
```

Рассмотрим приведенный выше подробно. Создает хранилище для наклейки, которые данное расширение предоставляет:

```csharp
public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
```

И переопределяет два метода `MSStickerBrowserViewController` класса для предоставления данных для браузера из этого хранилища данных:

```csharp
public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
{
    return Stickers.Count;
}

public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
{
    return Stickers[(int)index];
}
```

`CreateSticker` Метод возвращает путь ресурса изображения из пакета расширения и использует его для создания нового экземпляра `MSSticker` из этого актива, который добавляется в коллекцию:

```csharp
private void CreateSticker (string assetName, string localizedDescription)
{

    // Get path to asset
    var path = NSBundle.MainBundle.PathForResource (assetName, "png");
    if (path == null) {
        Console.WriteLine ("Couldn't create sticker {0}.", assetName);
        return;
    }

    // Build new sticker
    var stickerURL = new NSUrl (path);
    NSError error = null;
    var sticker = new MSSticker (stickerURL, localizedDescription, out error);
    if (error == null) {
        // Add to collection
        Stickers.Add (sticker);
    } else {
        // Report error
        Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
    }
}
```

`LoadSticker` Метод вызывается из `ViewDidLoad` создать наклейке из именованного ресурса изображения (входит в комплект приложения) и добавить его в коллекцию наклейки.

Для реализации обозревателя наклейке Custom, измените `MessagesViewController.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using UIKit;
using Messages;

namespace MonkeyStickers
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public StickerBrowserViewController BrowserViewController { get; set;}
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


            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);
        }
        #endregion
    }
}
```

Изучать этот код подробно, он создает хранилище для пользовательского браузера:

```csharp
public StickerBrowserViewController BrowserViewController { get; set;}
```

И в `ViewDidLoad` метод, он создает и настраивает новый браузер:

```csharp
// Create new browser and configure it
BrowserViewController = new StickerBrowserViewController (MSStickerSize.Regular);
BrowserViewController.View.Frame = View.Frame;
BrowserViewController.ChangeBackgroundColor (UIColor.Gray);
```

Затем он добавляет браузер для просмотра его:

```csharp
// Add to view
AddChildViewController (BrowserViewController);
BrowserViewController.DidMoveToParentViewController (this);
View.AddSubview (BrowserViewController.View);
```

### <a name="further-sticker-customization"></a>Дополнительная настройка наклейке

Дополнительная настройка наклейке можно путем включения в расширение приложения сообщения только два класса:

- `MSStickerView`
- `MSSticker`

С помощью выше методов, расширение может поддерживать наклейке выделение, которое не зависит от стандартного метода наклейке браузера. Кроме того наклейке отображение может переключаться между двух различных режимов просмотра:

- **Compact** -это является режимом по умолчанию, где представление наклейке может занимать до 25% нижней представления сообщений.
- **Развернуть** -представление наклейке заполняет все представление сообщения.

В этом представлении наклейке может переключаться между этими моделями программным способом или вручную пользователем.

Рассмотрим следующий пример обработки переключение между двух различных режимов просмотра. Для каждого состояния потребуются два разных представления контроллера. `StickerBrowserViewController` Дескрипторы **Compact** представление и выглядит следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Messages;
using Foundation;

namespace MessageExtension
{
    public partial class StickerBrowserViewController : MSStickerBrowserViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set; }
        public List<MSSticker> Stickers { get; set; } = new List<MSSticker> ();
        #endregion

        #region Constructors
        public StickerBrowserViewController (MessagesViewController messagesAppViewController, MSStickerSize stickerSize) : base (stickerSize)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Private Methods
        private void CreateSticker (string assetName, string localizedDescription)
        {

            // Get path to asset
            var path = NSBundle.MainBundle.PathForResource (assetName, "png");
            if (path == null) {
                Console.WriteLine ("Couldn't create sticker {0}.", assetName);
                return;
            }

            // Build new sticker
            var stickerURL = new NSUrl (path);
            NSError error = null;
            var sticker = new MSSticker (stickerURL, localizedDescription, out error);
            if (error == null) {
                // Add to collection
                Stickers.Add (sticker);
            } else {
                // Report error
                Console.WriteLine ("Error, couldn't create sticker {0}: {1}", assetName, error);
            }
        }

        private void LoadStickers ()
        {

            // Load sticker assets from disk
            CreateSticker ("add", "Add New Sticker");
            CreateSticker ("canada", "Canada Sticker");
            CreateSticker ("clouds", "Clouds Sticker");
            CreateSticker ("tree", "Tree Sticker");
        }
        #endregion

        #region Public Methods
        public void ChangeBackgroundColor (UIColor color)
        {
            StickerBrowserView.BackgroundColor = color;

        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            LoadStickers ();

        }

        public override nint GetNumberOfStickers (MSStickerBrowserView stickerBrowserView)
        {
            return Stickers.Count;
        }

        public override MSSticker GetSticker (MSStickerBrowserView stickerBrowserView, nint index)
        {
            // Wanting to add a new sticker?
            if (index == 0) {
                // Yes, ask controller to present add sticker interface
                MessagesAppViewController.AddNewSticker ();
                return null;
            } else {
                // No, return existing sticker
                return Stickers [(int)index];
            }
        }
        #endregion
    }
}
```

`AddStickerViewController` Обрабатывающий **разреженный** наклейке представление и посмотрите, как показано ниже:

```csharp
using System;
using Foundation;
using UIKit;
using Messages;

namespace MessageExtension
{
    public class AddStickerViewController : UIViewController
    {
        #region Computed Properties
        public MessagesViewController MessagesAppViewController { get; set;}
        public MSSticker NewSticker { get; set;}
        #endregion

        #region Constructors
        public AddStickerViewController (MessagesViewController messagesAppViewController)
        {
            // Initialize
            this.MessagesAppViewController = messagesAppViewController;
        }
        #endregion

        #region Override Method
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Build interface to create new sticker
            var cancelButton = new UIButton (UIButtonType.RoundedRect);
            cancelButton.TouchDown += (sender, e) => {
                // Cancel add new sticker
                MessagesAppViewController.CancelAddNewSticker ();
            };
            View.AddSubview (cancelButton);

            var doneButton = new UIButton (UIButtonType.RoundedRect);
            doneButton.TouchDown += (sender, e) => {
                // Add new sticker to collection
                MessagesAppViewController.AddStickerToCollection (NewSticker);
            };
            View.AddSubview (doneButton);
            
            ...
        }
        #endregion
    }
}
```

`MessageViewController` Реализует эти контроллеры представление, чтобы диск запрошенное состояние:

```csharp
using System;
using UIKit;
using Messages;

namespace MessageExtension
{
    public partial class MessagesViewController : MSMessagesAppViewController
    {
        #region Computed Properties
        public bool IsAddingSticker { get; set;}
        public StickerBrowserViewController BrowserViewController { get; set; }
        public AddStickerViewController AddStickerController { get; set;}
        #endregion

        #region Constructors
        protected MessagesViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Public Methods
        public void PresentStickerBrowser ()
        {
            // Is the Add sticker view being displayed?
            if (IsAddingSticker) {
                // Yes, remove it from view
                AddStickerController.RemoveFromParentViewController ();
                AddStickerController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (BrowserViewController);
            BrowserViewController.DidMoveToParentViewController (this);
            View.AddSubview (BrowserViewController.View);

            // Save mode
            IsAddingSticker = false;
        }

        public void PresentAddSticker ()
        {
            // Is the sticker browser being displayed?
            if (!IsAddingSticker) {
                // Yes, remove it from view
                BrowserViewController.RemoveFromParentViewController ();
                BrowserViewController.View.RemoveFromSuperview ();
            }

            // Add to view
            AddChildViewController (AddStickerController);
            AddStickerController.DidMoveToParentViewController (this);
            View.AddSubview (AddStickerController.View);

            // Save mode
            IsAddingSticker = true;
        }

        public void AddNewSticker ()
        {
            // Switch to expanded view mode
            Request (MSMessagesAppPresentationStyle.Expanded);
        }

        public void CancelAddNewSticker ()
        {
            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }

        public void AddStickerToCollection (MSSticker sticker)
        {
            // Add sticker to collection
            BrowserViewController.Stickers.Add (sticker);

            // Switch to compact view mode
            Request (MSMessagesAppPresentationStyle.Compact);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Create new browser and configure it
            BrowserViewController = new StickerBrowserViewController (this, MSStickerSize.Regular);
            BrowserViewController.View.Frame = View.Frame;
            BrowserViewController.ChangeBackgroundColor (UIColor.Gray);

            // Create new Add controller and configure it as well
            AddStickerController = new AddStickerViewController (this);
            AddStickerController.View.Frame = View.Frame;

            // Initially present the sticker browser
            PresentStickerBrowser ();
        }

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
        #endregion
    }
}
```

При запросе для добавления новых наклейке их доступными коллекцию новый `AddStickerViewController` становится видимым контроллером и представлением наклейке вводит **разреженный** представления:

```csharp
// Switch to expanded view mode
Request (MSMessagesAppPresentationStyle.Expanded);
```

Когда пользователь выбирает наклейке для добавления, он добавляется в свою коллекцию доступных и **Compact** запрашивается представление:

```csharp
public void AddStickerToCollection (MSSticker sticker)
{
    // Add sticker to collection
    BrowserViewController.Stickers.Add (sticker);

    // Switch to compact view mode
    Request (MSMessagesAppPresentationStyle.Compact);
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

## <a name="summary"></a>Сводка

, Описанные в этой статье расширение сообщений приложения в решении Xamarin.iOS, которое интегрируется с **сообщений** приложения и присутствуют новые функциональные возможности для пользователя. Он рассматривается использование расширения текста, наклейки, файлов мультимедиа и интерактивные сообщения для отправки.



## <a name="related-links"></a>Связанные ссылки

- [Построитель мороженого (пример)](https://developer.xamarin.com/samples/monotouch/ios10/IceCreamBuilder/)
- [Справочник по сообщений](https://developer.apple.com/reference/messages)
- [Руководство по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214)
