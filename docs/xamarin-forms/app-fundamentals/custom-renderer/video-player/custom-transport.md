---
title: Элементы управления пользовательского транспорта видео
description: В этой статье объясняется, как реализовать пользовательский транспорт элементов управления в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: CE9E955D-A9AC-4019-A5D7-6390D80DECA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: a20c68d5f86dad852a4425206846292c1c6c5838
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241664"
---
# <a name="custom-video-transport-controls"></a>Элементы управления пользовательского транспорта видео

В элементах управления транспорта видеопроигрыватель включают кнопок, которые выполняют функции **воспроизведение**, **Пауза**, и **остановить**. Эти кнопки обычно определяются с помощью знакомых значков, а не текст и **воспроизведение** и **Пауза** функции обычно объединяются в одну кнопку.

По умолчанию `VideoPlayer` отображает транспорта элементов управления, поддерживаемых каждой платформы. При задании `AreTransportControlsEnabled` свойства `false`, эти элементы управления, подавляются. Затем можно управлять `VideoPlayer` программным образом или создать свои собственные элементы управления транспорта.

## <a name="the-play-pause-and-stop-methods"></a>Методы воспроизведение, Пауза и остановка

`VideoPlayer` Класс определяет три метода с именем `Play`, `Pause`, и `Stop` , реализуемые инициирования событий:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        public event EventHandler PlayRequested;

        public void Play()
        {
            PlayRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler PauseRequested;

        public void Pause()
        {
            PauseRequested?.Invoke(this, EventArgs.Empty);
        }

        public event EventHandler StopRequested;

        public void Stop()
        {
            StopRequested?.Invoke(this, EventArgs.Empty);
        }
    }
}
```

Обработчики событий для этих событий, задаваемыми `VideoPlayerRenderer` класса в каждой из платформ, как показано ниже:

### <a name="ios-transport-implementations"></a>реализации транспорта операций ввода-вывода

Версия iOS `VideoPlayerRenderer` использует `OnElementChanged` метод, чтобы задать обработчики для этих трех событий при `NewElement` свойство не `null` и отсоединяет обработчик событий при `OldElement` не `null`:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        AVPlayer player;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            player.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            player.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            player.Pause();
            player.Seek(new CMTime(0, 1));
        }
    }
}
```

Обработчики событий реализуются путем вызова методов `AVPlayer` объекта. Имеется не `Stop` метод `AVPlayer`, поэтому он имитируется Приостановка видео и перемещает позицию в начало.

### <a name="android-transport-implementations"></a>Реализации транспорта Android

Реализация Android аналогичен реализации операций ввода-вывода. Обработчики для трех функций устанавливаются при `NewElement` не `null` и при отсоединении `OldElement` не `null`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        void OnPlayRequested(object sender, EventArgs args)
        {
            videoView.Start();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            videoView.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            videoView.StopPlayback();
        }
    }
}
```

Три функции вызова методов, определенных `VideoView`.

### <a name="uwp-transport-implementations"></a>Реализации транспорта UWP

UWP реализации функций три транспортного очень похожа на iOS и Android реализации:

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
                args.NewElement.PlayRequested += OnPlayRequested;
                args.NewElement.PauseRequested += OnPauseRequested;
                args.NewElement.StopRequested += OnStopRequested;
            }

            if (args.OldElement != null)
            {
                ···
                args.OldElement.PlayRequested -= OnPlayRequested;
                args.OldElement.PauseRequested -= OnPauseRequested;
                args.OldElement.StopRequested -= OnStopRequested;
            }
        }
        ···
        // Event handlers to implement methods
        void OnPlayRequested(object sender, EventArgs args)
        {
            Control.Play();
        }

        void OnPauseRequested(object sender, EventArgs args)
        {
            Control.Pause();
        }

        void OnStopRequested(object sender, EventArgs args)
        {
            Control.Stop();
        }
    }
}
```

## <a name="the-video-player-status"></a>Состояние видеопроигрыватель

Реализация **воспроизведение**, **Пауза**, и **остановить** функции недостаточно для поддержки элементов управления транспорта. Часто **воспроизведение** и **Пауза** команды реализуются с помощью одной кнопки, которая изменяет свой внешний вид для указания, в настоящее время наличия видео воспроизводится или приостановлена. Кроме того эта кнопка не следует включена даже если видео еще не загружен.

Эти требования подразумевают видеопроигрыватель необходимо сделать доступными текущее состояние, указывающее, если он воспроизводится или приостановлена или если он еще не готов для воспроизведения видео. (Три платформы также поддерживают свойства, которые показывают, если видео может быть приостановлена, можно также переместить на новую позицию, но эти свойства применяются для потоковой передачи видео, а не видеофайлов, поэтому они не поддерживаются в `VideoPlayer` описанные здесь.)

**VideoPlayerDemos** проект включает `VideoStatus` перечисления с три члена:

```csharp
namespace FormsVideoLibrary
{
    public enum VideoStatus
    {
        NotReady,
        Playing,
        Paused
    }
}
```

`VideoPlayer` Класс определяет реального-свойство только для связывания с именем `Status` типа `VideoStatus`. Это свойство определяется только для чтения, так как он должен быть задан только модуль подготовки отчетов платформы:

```csharp
using System;
using Xamarin.Forms;

namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Status read-only property
        private static readonly BindablePropertyKey StatusPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Status), typeof(VideoStatus), typeof(VideoPlayer), VideoStatus.NotReady);

        public static readonly BindableProperty StatusProperty = StatusPropertyKey.BindableProperty;

        public VideoStatus Status
        {
            get { return (VideoStatus)GetValue(StatusProperty); }
        }

        VideoStatus IVideoPlayerController.Status
        {
            set { SetValue(StatusPropertyKey, value); }
            get { return Status; }
        }
        ···
    }
}
```

Как правило, связываемое свойство только для чтения будет иметь закрытый `set` метода доступа в `Status` свойство, чтобы его можно задать в классе. Для `View` производный класс, поддерживаемые модули подготовки отчетов, однако свойство должно задаваться из вне класса, но только модулем подготовки платформы.

По этой причине определено другое свойство с именем `IVideoPlayerController.Status`. Это является явной реализацией интерфейса и стало возможным, `IVideoPlayerController` интерфейс, который `VideoPlayer` класс реализует:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPlayerController
    {
        VideoStatus Status { set; get; }

        TimeSpan Duration { set; get; }
    }
}
```

Это аналогично тому, как [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) управления использует [ `IWebViewController` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IWebViewController/) интерфейс для реализации `CanGoBack` и `CanGoForward` свойства. (См. исходный код из [ `WebView` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/WebView.cs) и его модули подготовки отчетов сведения.)

Это делает возможным для класса, внешних по отношению к `VideoPlayer` для задания `Status` свойства с помощью ссылки на `IVideoPlayerController` интерфейса. (Вы увидите код вскоре.) Может быть установлено из других классов, а также, но маловероятно, задаваемый случайно. Самое главное `Status` невозможно задать свойство через привязку данных.

Для модулей подготовки отчетов в обеспечении это `Status` обновления, свойство `VideoPlayer` класс определяет `UpdateStatus` событие, которое запускается каждые десять раз меньше секунды:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        public event EventHandler UpdateStatus;

        public VideoPlayer()
        {
            Device.StartTimer(TimeSpan.FromMilliseconds(100), () =>
            {
                UpdateStatus?.Invoke(this, EventArgs.Empty);
                return true;
            });
        }
        ···
    }
}
```

### <a name="the-ios-status-setting"></a>Параметр состояния операций ввода-вывода

IOS `VideoPlayerRenderer` задает обработчик для `UpdateStatus` событий (и отсоединяет этот обработчик при базовый `VideoPlayer` элемент отсутствует) и использует обработчик, чтобы задать `Status` свойства:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (player.Status)
            {
                case AVPlayerStatus.ReadyToPlay:
                    switch (player.TimeControlStatus)
                    {
                        case AVPlayerTimeControlStatus.Playing:
                            videoStatus = VideoStatus.Playing;
                            break;

                        case AVPlayerTimeControlStatus.Paused:
                            videoStatus = VideoStatus.Paused;
                            break;
                    }
                    break;
                }
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
            ···
        }
        ···
    }
}
```

Два свойства `AVPlayer` должен осуществляться: [ `Status` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.Status/) свойство типа `AVPlayerStatus` и [ `TimeControlStatus` ](https://developer.xamarin.com/api/property/AVFoundation.AVPlayer.TimeControlStatus/) свойство типа `AVPlayerTimeControlStatus`. Обратите внимание, что `Element` свойство (который является `VideoPlayer`) должен быть приведен к `IVideoPlayerController` для задания `Status` свойство.

### <a name="the-android-status-setting"></a>Параметр состояния Android

[ `IsPlaying` ](https://developer.xamarin.com/api/property/Android.Widget.VideoView.IsPlaying/) Свойство Android `VideoView` является логическое значение, указывающее, только если видео воспроизводится или приостановлена. Чтобы определить, если `VideoView` не могут ни play ни Приостановка видео еще `Prepared` событие `VideoView` должны быть обработаны. Эти два обработчика задаются в `OnElementChanged` метода и во время отсоединения `Dispose` переопределения:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;

        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
            if (args.NewElement != null)
            {
                if (Control == null)
                {
                    ···
                    videoView.Prepared += OnVideoViewPrepared;
                    ···
                }
                ···
                args.NewElement.UpdateStatus += OnUpdateStatus;
                ···
            }

            if (args.OldElement != null)
            {
                args.OldElement.UpdateStatus -= OnUpdateStatus;
                ···
            }

        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null && videoView != null)
            {
                videoView.Prepared -= OnVideoViewPrepared;
            }
            if (Element != null)
            {
                Element.UpdateStatus -= OnUpdateStatus;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`UpdateStatus` Обработчик использует `isPrepared` поля (в `Prepared` обработчика) и `IsPlaying` задаваемого свойства `Status` свойство:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        VideoView videoView;
        ···
        bool isPrepared;
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            isPrepared = true;
            ···
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            VideoStatus status = VideoStatus.NotReady;

            if (isPrepared)
            {
                status = videoView.IsPlaying ? VideoStatus.Playing : VideoStatus.Paused;
            }
            ···
        }
        ···
    }
}
```

### <a name="the-uwp-status-setting"></a>Параметр состояния UWP

UWP `VideoPlayerRenderer` использует `UpdateStatus` событий, но это не нужно для параметра `Status` свойство. `MediaElement` Определяет [ `CurrentStateChanged` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentStateChanged) событие, модуль подготовки отчетов позволяет получать уведомления при [ `CurrentState` ](/uwp/api/windows.ui.xaml.controls.mediaelement#Windows_UI_Xaml_Controls_MediaElement_CurrentState) измененное свойство. Свойство отсоединяется в `Dispose` переопределения:

```csharp
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
                    ···
                    mediaElement.CurrentStateChanged += OnMediaElementCurrentStateChanged;
                };
                ···
            }
            ···
        }

        protected override void Dispose(bool disposing)
        {
            if (Control != null)
            {
                ···
                Control.CurrentStateChanged -= OnMediaElementCurrentStateChanged;
            }

            base.Dispose(disposing);
        }
        ···
    }
}
```

`CurrentState` Свойство относится к типу [ `MediaElementState` ](/uwp/api/windows.ui.xaml.media.mediaelementstate)и сопоставляет легко `VideoStatus`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementCurrentStateChanged(object sender, RoutedEventArgs args)
        {
            VideoStatus videoStatus = VideoStatus.NotReady;

            switch (Control.CurrentState)
            {
                case MediaElementState.Playing:
                    videoStatus = VideoStatus.Playing;
                    break;

                case MediaElementState.Paused:
                case MediaElementState.Stopped:
                    videoStatus = VideoStatus.Paused;
                    break;
            }

            ((IVideoPlayerController)Element).Status = videoStatus;
        }
        ···
    }
}
```

## <a name="play-pause-and-stop-buttons"></a>Воспроизведение, Пауза и остановка кнопок

Используя символы Юникода для символьных **воспроизведение**, **Пауза**, и **остановить** изображений, затруднительно. [Прочие технические](https://unicode-table.com/en/blocks/miscellaneous-technical/) раздел стандарта Юникод определяет три специальные символы, первый взгляд подходит для этой цели. Эти особые значения приведены ниже.

- 0x23F5 (черный средний вправо треугольник) или &#x23F5; для **воспроизведения**
- 0x23F8 (двойная вертикальная черта) или &#x23F8; для **Пауза**
- 0x23F9 (черный квадрат) или &#x23F9; для **остановить**

Независимо от того как эти символы отображаются в браузере (и их обработки разных браузерах по-разному), они не отображаются постоянно на платформах, поддерживаемых в Xamarin.Forms. Для устройств iOS и UWP **Пауза** и **остановить** символы имеют вид с синим фоном 3D и белым цветом переднего плана. В противном случае на Android, где символ — просто синего. Однако кодовой точки в 0x23F5 для **воспроизведение** нет, одинаково на UWP и он поддерживается даже в iOS и Android.

По этой причине кодовой точки в 0x23F5 не может использоваться для **воспроизведение**. Является хорошим замены:

- 0x25B6 (черный треугольник вправо) или &#x25B6; для **воспроизведения**

Эта возможность поддерживается всех трех платформах, за исключением того, что это обычный черный треугольник, которые не используются в трехмерной вида **Пауза** и **остановить**. Один из вариантов является кодовой точки в 0x25B6 с кодом варианта:

- 0x25B6 следуют 0xFE0F (variant 16) или &#x25B6; &#xFE0F; для **воспроизведения**

Это используется в разметке, показано ниже. На iOS, он предоставляет **воспроизведение** символов 3D одинаково как **Пауза** и **остановить** кнопки, но вариант не работает в Android и UWP.

**Пользовательский транспорт** страницы наборы **AreTransportControlsEnabled** свойства **false** и включает `ActivityIndicator` отображаемое во время загрузки видео и два кнопки. `DataTrigger` объекты используются для включения и отключения `ActivityIndicator` и кнопки и для переключения между первой кнопки **воспроизведение** и **Пауза**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomTransportPage"
             Title="Custom Transport">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AutoPlay="False"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource BigBuckBunny}" />

        <ActivityIndicator Grid.Row="0"
                           Color="Gray"
                           IsVisible="False">
            <ActivityIndicator.Triggers>
                <DataTrigger TargetType="ActivityIndicator"
                             Binding="{Binding Source={x:Reference videoPlayer},
                                               Path=Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsVisible" Value="True" />
                    <Setter Property="IsRunning" Value="True" />
                </DataTrigger>
            </ActivityIndicator.Triggers>
        </ActivityIndicator>

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="0, 10"
                     BindingContext="{x:Reference videoPlayer}">

            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.Playing}">
                        <Setter Property="Text" Value="&#x23F8; Pause" />
                    </DataTrigger>

                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>

            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding Status}"
                                 Value="{x:Static video:VideoStatus.NotReady}">
                        <Setter Property="IsEnabled" Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

Подробно в статье описаны триггеры данных [триггеры данных](~/xamarin-forms/app-fundamentals/triggers.md#data).

Файл кода содержит обработчики событий для кнопки `Clicked` событий:

```csharp
namespace VideoPlayerDemos
{
    public partial class CustomTransportPage : ContentPage
    {
        public CustomTransportPage()
        {
            InitializeComponent();
        }

        void OnPlayPauseButtonClicked(object sender, EventArgs args)
        {
            if (videoPlayer.Status == VideoStatus.Playing)
            {
                videoPlayer.Pause();
            }
            else if (videoPlayer.Status == VideoStatus.Paused)
            {
                videoPlayer.Play();
            }
        }

        void OnStopButtonClicked(object sender, EventArgs args)
        {
            videoPlayer.Stop();
        }
    }
}
```

Поскольку `AutoPlay` задано значение `false` в **CustomTransport.xaml** файл, вам потребуется нажать **воспроизведение** когда он становится доступным для начала видео. Кнопки определены, чтобы их эквиваленты текст сопровождаются символов Юникода, описанных выше. Кнопки имеют согласованного внешнего вида для каждой платформы, при воспроизведении видео:

[![Пользовательский транспорт воспроизведение](custom-transport-images/customtransportplaying-small.png "воспроизведение пользовательский транспорт")](custom-transport-images/customtransportplaying-large.png#lightbox "воспроизведение пользовательского транспорта")

Но на Android и UWP **воспроизведение** кнопка выглядит очень по-разному при приостановке видео:

[![Пользовательский транспорт приостановлена](custom-transport-images/customtransportpaused-small.png "приостановлена пользовательский транспорт")](custom-transport-images/customtransportpaused-large.png#lightbox "пользовательский транспорт приостановлена")

В реальном приложении вы вероятно, захотите использовать свои собственные образы растрового изображения для кнопок для достижения визуальное единообразие.


## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
