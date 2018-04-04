---
title: Создание видеопроигрывателей платформы
ms.prod: xamarin
ms.assetid: EEE2FB9B-EB73-4A3F-A859-7A1D4808E149
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 316a6b9e9ce65dfb3c19d611f1b4976709546b5f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="creating-the-platform-video-players"></a>Создание видеопроигрывателей платформы

[ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) решение содержит весь код для реализации видеопроигрывателя для Xamarin.Forms. Он также включает несколько страниц, демонстрирует использование видеопроигрыватель внутри приложения. Все `VideoPlayer` кода и его модули подготовки отчетов для платформы, находятся в папках проекта с именем `FormsVideoLibrary`, а также использовать пространство имен `FormsVideoLibrary`. Это следует сделать легко копировать файлы в приложении и ссылаться на классы.

## <a name="the-video-player"></a>Видеопроигрыватель

[ `VideoPlayer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos/VideoPlayer.cs) Класс является частью **VideoPlayerDemos** Переносимая библиотека классов (PCL), являющихся общими для платформы. Он является производным от `View`:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
    }
}
```

Члены этого класса (и `IVideoPlayerController` интерфейса) описаны в последующих статьях.

Каждый из трех платформ содержит класс с именем `VideoPlayerRenderer` , содержащий специфический для платформы код для реализации видеопроигрыватель. Основная задача этого модуля подготовки отчетов является создание видеопроигрывателя для данной платформы.

### <a name="the-ios-player-view-controller"></a>Представление контроллер проигрывателя iOS

При реализации видеопроигрыватель в iOS, участвуют несколько классов. Во-первых, приложение создает [ `AVPlayerViewController` ](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) , а затем задает [ `Player` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.Player/) свойство для объекта типа [ `AVPlayer` ](https://developer.xamarin.com/api/type/AVFoundation.AVPlayer/). Когда игрок назначается источник видео необходимы дополнительные классы.

Все модули подготовки отчетов, iOS, например [ `VideoPlayerRenderer` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/CustomRenderers/VideoPlayerDemos/VideoPlayerDemos/VideoPlayerDemos.iOS/VideoPlayerRenderer.cs) содержит `ExportRenderer` атрибут, который идентифицирует `VideoPlayer` представления с помощью модуля подготовки отчетов:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using AVFoundation;
using AVKit;
using CoreMedia;
using Foundation;
using UIKit;

using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.iOS.VideoPlayerRenderer))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
    }
}
```

Обычно модуль подготовки отчетов, который задает управления платформой является производным от [ `ViewRenderer<View, NativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs) класса, где `View` подход — Xamarin.Forms `View` производный класс (в этом случае `VideoPlayer`) и `NativeView` является iOS `UIView` класс, производный от класса модуля подготовки отчетов. Для этого модуля подготовки отчетов, универсального аргумента просто равно `UIView`, по причинам, вскоре вы увидите.

Если модуль подготовки основано на `UIViewController` производный класс (что является), то класс должен переопределять `ViewController` свойства и возвращаемые представление контроллер, в данном случае `AVPlayerViewController`. То есть назначение `_playerViewController` поля:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        AVPlayerItem playerItem;
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Create AVPlayerViewController
                    _playerViewController = new AVPlayerViewController();

                    // Set Player property to AVPlayer
                    player = new AVPlayer();
                    _playerViewController.Player = player;

                    // Use the View from the controller as the native control
                    SetNativeControl(_playerViewController.View);
                }
                ···
            }
        }
        ···
    }
}
```

Основную ответственность за `OnElementChanged` переопределение заключается в проверке, если `Control` свойство `null` и, если да, создать элемент управления платформы и передать его в `SetNativeControl` метод. В этом случае этот объект доступен только из `View` свойство `AVPlayerViewController`. Что `UIView` производный класс оказался закрытый класс с именем `AVPlayerView`, но так как он является закрытым, он не может быть указан явным образом как второй аргумент универсального `ViewRenderer`.

Обычно `Control` свойство класса модуля подготовки отчетов соответственно ссылается на `UIView` используется для реализации модуля подготовки отчетов, но в этом случае `Control` свойство не используется в другом месте.

### <a name="the-android-video-view"></a>Android Просмотр видео

Модуль подготовки отчетов, Android для `VideoPlayer` основан на Android [ `VideoView` ](https://developer.xamarin.com/api/type/Android.Widget.VideoView/) класса. Однако если `VideoView` используется для воспроизведения видео в приложении Xamarin.Forms видео заливки отведенное области для `VideoPlayer` без сохранения правильных пропорций. Для этой ошибки (как вы увидите в ближайшее время), `VideoView` становится дочерним Android `RelativeLayout`. Объект `using` директива определяет `ARelativeLayout` позволяет отличать его от Xamarin.Forms `RelativeLayout`, и это второй универсального аргумента `ViewRenderer`:

```csharp
using System;
using System.ComponentModel;
using System.IO;

using Android.Content;
using Android.Media;
using Android.Widget;
using ARelativeLayout = Android.Widget.RelativeLayout;

using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.Droid.VideoPlayerRenderer))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        public VideoPlayerRenderer(Context context) : base(context)
        {
        }
        ···
    }
}
```

Начиная с версии 2.5 Xamarin.Forms, Android модулей подготовки отчетов должен содержать конструктор с `Context` аргумент.

`OnElementChanged` Переопределение создает оба `VideoView` и `RelativeLayout` и задает параметры макета для `VideoView` по центру в `RelativeLayout`.


```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    // Save the VideoView for future reference
                    videoView = new VideoView(Context);

                    // Put the VideoView in a RelativeLayout
                    ARelativeLayout relativeLayout = new ARelativeLayout(Context);
                    relativeLayout.AddView(videoView);

                    // Center the VideoView in the RelativeLayout
                    ARelativeLayout.LayoutParams layoutParams =
                        new ARelativeLayout.LayoutParams(LayoutParams.MatchParent, LayoutParams.MatchParent);
                    layoutParams.AddRule(LayoutRules.CenterInParent);
                    videoView.LayoutParameters = layoutParams;

                    // Handle a VideoView event
                    videoView.Prepared += OnVideoViewPrepared;

                    // Use the RelativeLayout as the native control
                    SetNativeControl(relativeLayout);
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            base.Dispose(disposing);
        }

        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
    }
}
```

Обработчик `Prepared` прикрепляется в этом методе и отключении от события `Dispose` метод. Это событие возникает, когда `VideoView` имеет достаточно сведений, чтобы начать воспроизведение видеофайла.

### <a name="the-uwp-media-element"></a>Элемент мультимедиа UWP

В универсальной платформы Windows (UWP), является наиболее распространенные видеопроигрыватель [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/). Эта документация для `MediaElement` указывает, что [ `MediaPlayerElement` ](/uwp/api/windows.ui.xaml.controls.mediaplayerelement/) следует использовать, когда необходим только для поддержки версий Windows 10, начиная с 1607 сборки.

`OnElementChanged` Переопределения должен создать `MediaElement`, задать несколько обработчиков событий и передачи `MediaElement` объект `SetNativeControl`:

```csharp
using System;
using System.ComponentModel;

using Windows.Storage;
using Windows.Storage.Streams;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

using Xamarin.Forms;
using Xamarin.Forms.Platform.UWP;

[assembly: ExportRenderer(typeof(FormsVideoLibrary.VideoPlayer),
                          typeof(FormsVideoLibrary.UWP.VideoPlayerRenderer))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            base.OnElementChanged(args);

            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    MediaElement mediaElement = new MediaElement();
                    SetNativeControl(mediaElement);

                    mediaElement.MediaOpened += OnMediaElementMediaOpened;
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                }
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                Control.MediaOpened -= OnMediaElementMediaOpened;
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }        
        ···
    }
}
```

Два обработчика событий, отсоединены в `Dispose` событий для модуля подготовки отчетов.

## <a name="showing-the-transport-controls"></a>Отображение элементов управления движением

Все видеопроигрывателей включены в трех платформ поддерживают набор по умолчанию элементы управления транспорта с кнопками для воспроизведения и приостановки и полоса укажите текущую позицию в видео, а также для перемещения на новое место.

`VideoPlayer` Класс определяет свойство с именем `AreTransportControlsEnabled` и задает значение по умолчанию `true`:


```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // AreTransportControlsEnabled property
        public static readonly BindableProperty AreTransportControlsEnabledProperty =
            BindableProperty.Create(nameof(AreTransportControlsEnabled), typeof(bool), typeof(VideoPlayer), true);

        public bool AreTransportControlsEnabled
        {
            set { SetValue(AreTransportControlsEnabledProperty, value); }
            get { return (bool)GetValue(AreTransportControlsEnabledProperty); }
        }
        ···
    }
}
```

Несмотря на то, что это свойство содержит как `set` и `get` методов доступа, модуль подготовки отчетов должен обрабатывать случаи, только в том случае, если свойство имеет значение. `get` Доступа просто возвращает текущее значение свойства.

Такие свойства, как `AreTransportControlsEnabled` обрабатываются в модулях подготовки отчетов платформы двумя способами:

- В первый раз — при создании Xamarin.Forms `VideoPlayer` элемента. Это указано в `OnElementChanged` переопределить модуля подготовки отчетов при `NewElement` свойство не `null`. В настоящее время можно установить модуль подготовки отчетов является собственной платформы видеопроигрыватель от начального значения этого свойства, как определено в `VideoPlayer`.

- Если свойство в `VideoPlayer` изменения позже, то `OnElementPropertyChanged` вызова в модуль подготовки отчетов. Это позволяет модуль подготовки отчетов для обновления платформы видеопроигрыватель, на основе нового значения свойства.

Вот как `AreTransportControlsEnabled` свойство обрабатывается в трех платформ:

### <a name="ios-playback-controls"></a>элементы управления воспроизведением iOS

Свойство iOS `AVPlayerViewController` , задающий отображение транспорта контроль не [ `ShowsPlaybackControls` ](https://developer.xamarin.com/api/property/AVKit.AVPlayerViewController.ShowsPlaybackControls/). Вот как это свойство задается в iOS `VideoViewRenderer`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        AVPlayerViewController _playerViewController;       // solely for ViewController property

        public override UIViewController ViewController => _playerViewController;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            ((AVPlayerViewController)ViewController).ShowsPlaybackControls = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

`Element` Свойство модуля подготовки отчетов ссылается `VideoPlayer` класса.

### <a name="the-android-media-controller"></a>Контроллер Android мультимедиа

В Android, отображение элементов управления движением требует создания [ `MediaController` ](https://developer.xamarin.com/api/type/Android.Widget.MediaController/) объектов и связывание его с `VideoView` объекта. Механизм, позволяющий демонстрируются в `SetAreTransportControlsEnabled` метод:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        MediaController mediaController;    // Used to display transport controls
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            if (Element.AreTransportControlsEnabled)
            {
                mediaController = new MediaController(Context);
                mediaController.SetMediaPlayer(videoView);
                videoView.SetMediaController(mediaController);
            }
            else
            {
                videoView.SetMediaController(null);

                if (mediaController != null)
                {
                    mediaController.SetMediaPlayer(null);
                    mediaController = null;
                }
            }
        }
        ···
    }
}
```

### <a name="the-uwp-transport-controls-property"></a>Свойства элементов управления движением UWP

UWP `MediaElement` определяет свойство с именем [ `AreTransportControlsEnabled` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_AreTransportControlsEnabled), после чего свойство устанавливается на основе `VideoPlayer` свойство с тем же именем:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                SetAreTransportControlsEnabled();
                ···
            }
        }

        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(sender, args);

            if (args.PropertyName == VideoPlayer.AreTransportControlsEnabledProperty.PropertyName)
            {
                SetAreTransportControlsEnabled();
            }
            ···
        }

        void SetAreTransportControlsEnabled()
        {
            Control.AreTransportControlsEnabled = Element.AreTransportControlsEnabled;
        }
        ···
    }
}
```

Еще одно свойство необходим начать воспроизведение видео: это важно `Source` свойство, которое ссылается на файл изображения. Реализация `Source` свойство описано в следующей статье [воспроизведение видео в Интернете](web-videos.md).


## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
