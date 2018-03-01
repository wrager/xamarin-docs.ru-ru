---
title: "Нестандартное расположение видео"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6D792264-30FF-46F7-8C1B-2FEF9D277DF4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d4446d30491ee796ca93eadf2e107fc9d74748df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="custom-video-positioning"></a>Нестандартное расположение видео

Элементы управления транспорт, реализуемый каждой платформы включается панель позиции. Эта панель напоминает slider или полосы прокрутки и показывает текущее положение видео в пределах его общая длительность. Кроме того пользователю можно управлять панели позиции, чтобы перейти на новую позицию в видео вперед или назад.

В этой статье показано, как можно реализовать собственную настраиваемую позицию строки.

## <a name="the-duration-property"></a>Значение свойства Duration

Один из элементов сведения, `VideoPlayer` должна поддерживать пользовательское положение строки — это продолжительность видео. `VideoPlayer` Определяет только для чтения `Duration` свойство типа `TimeSpan`:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Duration read-only property
        private static readonly BindablePropertyKey DurationPropertyKey =
            BindableProperty.CreateReadOnly(nameof(Duration), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public static readonly BindableProperty DurationProperty = DurationPropertyKey.BindableProperty;

        public TimeSpan Duration
        {
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        TimeSpan IVideoPlayerController.Duration
        {
            set { SetValue(DurationPropertyKey, value); }
            get { return Duration; }
        }
        ···
    }
}
```

Как `Status` свойство описано в [предыдущей статьи](custom-transport.md), то `Duration` свойство доступно только для чтения. Оно определено с закрытым `BindablePropertyKey` и может быть задано только с помощью ссылки на `IVideoPlayerController` интерфейс, который содержит это `Duration` свойство:

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

Также Обратите внимание, обработчик изменения свойства, который вызывает метод с именем `SetTimeToEnd` , которое описано далее в этой статье.

Продолжительность видео — *не* доступен сразу после `Source` свойство `VideoPlayer` имеет значение. Видеофайл должны быть частично загружены до базового видеопроигрыватель можно определить его длительности.

Вот, как каждый из модулей подготовки отчетов платформы получает длительность видео.

### <a name="video-duration-in-ios"></a>Длительность видео в iOS

В iOS, длительность видео извлекается из `Duration` свойство `AVPlayerItem`, но не сразу после `AVPlayerItem` создается. Имеется возможность задать наблюдателя операций ввода-вывода для `Duration` свойства, но `VideoPlayerRenderer` получает длительность в `UpdateStatus` метод, который вызывается 10 раз в секунду:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ((IVideoPlayerController)Element).Duration = ConvertTime(playerItem.Duration);
                ···
            }
        }

        TimeSpan ConvertTime(CMTime cmTime)
        {
            return TimeSpan.FromSeconds(Double.IsNaN(cmTime.Seconds) ? 0 : cmTime.Seconds);

        }
        ···
    }
}
```

`ConvertTime` Метод преобразует `CMTime` объект `TimeSpan` значение.

### <a name="video-duration-in-android"></a>Длительность видео в Android

`Duration` Свойство Android `VideoView` сообщает допустимое время в миллисекундах при `Prepared` событие `VideoView` возникает. Android `VideoPlayerRenderer` класса для получения использует этот обработчик `Duration` свойство:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnVideoViewPrepared(object sender, EventArgs args)
        {
            ···
            ((IVideoPlayerController)Element).Duration = TimeSpan.FromMilliseconds(videoView.Duration);
        }
        ···
    }
}
```

### <a name="video-duration-in-uwp"></a>Длительность видео в UWP

`NaturalDuration` Свойство `MediaElement` — `TimeSpan` значение и вступления в силу, когда `MediaElement` активируется `MediaOpened` событий:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        void OnMediaElementMediaOpened(object sender, RoutedEventArgs args)
        {
            ((IVideoPlayerController)Element).Duration = Control.NaturalDuration.TimeSpan;
        }
        ···
    }
}
```

## <a name="the-position-property"></a>Свойство

`VideoPlayer` Кроме того, должен `Position` свойство, которое увеличивается от нуля до `Duration` как воспроизведения видео. `VideoPlayer` реализует это свойство как `Position` свойство в UWP `MediaElement`, который является обычного привязываемые свойства с открытыми `set` и `get` методы доступа:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        // Position property
        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan(),
                propertyChanged: (bindable, oldValue, newValue) => ((VideoPlayer)bindable).SetTimeToEnd());

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }
        ···
    }
}
```

`get` Метода доступа возвращает текущую позицию видео воспроизводится, но `set` доступа предназначен реагировать на пользователя манипуляции позиции строки, перемещая позицию видео вперед и назад.

В iOS и Android, имеет свойство, которое получает текущую позицию только `get` метод доступа и `Seek` метод доступен для выполнения этой задачи второй. Если вы думаете о его отдельные `Seek` возможно более разумным подходом, чем один метод `Position` свойство. Один `Position` свойство имеет специфические проблемы: во время воспроизведения видео, `Position` свойство необходимо постоянно обновляются для отражения нового положения. Но вы не хотите большинство изменений `Position` свойство видеопроигрывателя для перемещения в новое положение в видео. Если это происходит, видеопроигрыватель может отвечать на основе последней значение `Position` бы продвижения свойств и видео.

Несмотря на проблемы, связанные с реализацией `Position` свойство с `set` и `get` методы доступа, такой подход был выбран, так как он был совместим с UWP `MediaElement`, и она имеет значительное преимущество с привязкой к данным: `Position` Свойство `VideoPlayer` могут быть привязаны к ползунок, используемый для отображения положение и поиска на новое место. Однако, ряд мер предосторожности необходимы, если такая реализация `Position` свойства, чтобы избежать циклов обратной связи.

### <a name="setting-and-getting-ios-position"></a>Настройка и получение позиции iOS

В iOS `CurrentTime` свойство `AVPlayerItem` объект показывает текущее положение воспроизведения видео. IOS `VideoPlayerRenderer` задает `Position` свойство в `UpdateStatus` обработчик в то же время, которое задает `Duration` свойства:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            if (playerItem != null)
            {
                ···
                ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, ConvertTime(playerItem.CurrentTime));
            }
        }
        ···
    }
}
```

Модуль подготовки отчетов обнаруживает, когда `Position` значение свойства из `VideoPlayer` изменилось в `OnElementPropertyChanged` переопределить и использует новое значение для вызова `Seek` метод `AVPlayer` объекта:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                TimeSpan controlPosition = ConvertTime(player.CurrentTime);

                if (Math.Abs((controlPosition - Element.Position).TotalSeconds) > 1)
                {
                    player.Seek(CMTime.FromSeconds(Element.Position.TotalSeconds, 1));
                }
            }
        }
        ···
    }
}
```

Имейте в виду, что каждый раз `Position` свойство в `VideoPlayer` устанавливается на основе `OnUpdateStatus` обработчик, `Position` активируется свойство `PropertyChanged` события, который обнаруживается в `OnElementPropertyChanged` переопределения. Для большинства этих изменений `OnElementPropertyChanged` метод ничего не делает. В противном случае после каждого изменения в позиции, видео, он будет перемещен в той же позиции, в которую он достиг!

Во избежание этого цикла отзыв `OnElementPropertyChanged` вызывает только метод `Seek` при разницу между `Position` свойство и в текущей позиции `AVPlayer` больше, чем одной секунде.

### <a name="setting-and-getting-android-position"></a>Настройка и получение Android позиции

Как и в модуле подготовки iOS, Android `VideoPlayerRenderer` задает новое значение для `Position` свойство в `OnUpdateStatus` обработчика. `CurrentPosition` Свойство `VideoView` содержит новое положение в единицах миллисекунд:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ···
            TimeSpan timeSpan = TimeSpan.FromMilliseconds(videoView.CurrentPosition);
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, timeSpan);
        }
        ···
    }
}
```

Кроме того, как модуль подготовки iOS и Android модуля подготовки отчетов вызывает `SeekTo` метод `VideoView` при `Position` изменилось значение свойства, но только в том случае, когда изменение становится более чем на одну секунду отличается от `CurrentPosition` значение `VideoView`:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs(videoView.CurrentPosition - Element.Position.TotalMilliseconds) > 1000)
                {
                    videoView.SeekTo((int)Element.Position.TotalMilliseconds);
                }
            }
        }
        ···
    }
}
```

### <a name="setting-and-getting-uwp-position"></a>Настройка и получение позиции UWP

UWP `VideoPlayerRenderer` дескрипторы `Position` таким же образом, как iOS и Android модули подготовки отчетов, но поскольку `Position` свойство UWP `MediaElement` также `TimeSpan` значение, то преобразование не требуется:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        protected override void OnElementPropertyChanged(object sender, PropertyChangedEventArgs args)
        {
            ···
            else if (args.PropertyName == VideoPlayer.PositionProperty.PropertyName)
            {
                if (Math.Abs((Control.Position - Element.Position).TotalSeconds) > 1)
                {
                    Control.Position = Element.Position;
                }
            }
        }
        ···
        void OnUpdateStatus(object sender, EventArgs args)
        {
            ((IElementController)Element).SetValueFromRenderer(VideoPlayer.PositionProperty, Control.Position);
        }
        ···
    }
}
```

## <a name="calculating-a-timetoend-property"></a>Вычисление свойства TimeToEnd

Иногда видеопроигрывателей показывают время, оставшееся в видео. Это значение начинается с длительность видео при видео начинается и уменьшается до нуля, когда заканчивается видео.

`VideoPlayer` включает в себя только для чтения `TimeToEnd` свойство, которое обрабатывается целиком в пределах класса на основе изменений `Duration` и `Position` свойства:

```csharp
namespace FormsVideoLibrary
{
    public class VideoPlayer : View, IVideoPlayerController
    {
        ···
        private static readonly BindablePropertyKey TimeToEndPropertyKey =
            BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(VideoPlayer), new TimeSpan());

        public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

        public TimeSpan TimeToEnd
        {
            private set { SetValue(TimeToEndPropertyKey, value); }
            get { return (TimeSpan)GetValue(TimeToEndProperty); }
        }

        void SetTimeToEnd()
        {
            TimeToEnd = Duration - Position;
        }
        ···
    }
}
```

`SetTimeToEnd` Метод вызывается из обработчиков измененных свойств обоих `Duration` и `Position`.

## <a name="a-custom-slider-for-video"></a>Пользовательские ползунок для видео

Можно написать пользовательский элемент управления для позиции строки, или использовать Xamarin.Forms `Slider` или класс, производный от `Slider`, например следующая `PositionSlider` класса. Этот класс определяет два новых свойства с именем `Duration` и `Position` типа `TimeSpan` , которые предназначены для быть привязкой к данным для двух свойств с тем же именем в `VideoPlayer`. Обратите внимание, что для привязки режим по умолчанию `Position` свойство является двусторонней:

```csharp
namespace FormsVideoLibrary
{
    public class PositionSlider : Slider
    {
        public static readonly BindableProperty DurationProperty =
            BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                    });

        public TimeSpan Duration
        {
            set { SetValue(DurationProperty, value); }
            get { return (TimeSpan)GetValue(DurationProperty); }
        }

        public static readonly BindableProperty PositionProperty =
            BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                    defaultBindingMode: BindingMode.TwoWay,
                                    propertyChanged: (bindable, oldValue, newValue) =>
                                    {
                                        double seconds = ((TimeSpan)newValue).TotalSeconds;
                                        ((PositionSlider)bindable).Value = seconds;
                                    });

        public TimeSpan Position
        {
            set { SetValue(PositionProperty, value); }
            get { return (TimeSpan)GetValue(PositionProperty); }
        }

        public PositionSlider()
        {
            PropertyChanged += (sender, args) =>
            {
                if (args.PropertyName == "Value")
                {
                    TimeSpan newPosition = TimeSpan.FromSeconds(Value);

                    if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                    {
                        Position = newPosition;
                    }
                }
            };
        }
    }
}
```

Обработчик изменения свойства `Duration` наборов свойств `Maximum` базового объекта `Slider` для `TotalSeconds` свойство `TimeSpan` значение. Аналогичным образом, изменения свойств обработчик `Position` задает `Value` свойство `Slider`. Таким образом, базовый `Slider` отслеживает положение `PositionSlider`.

`PositionSlider` Обновляется из основного `Slider` в единственном экземпляре: когда пользователь управляет `Slider` для указания того, дополнительные видео, или они будут отменены на новое место. Это определяется в `PropertyChanged` обработчик в конструкторе `PositionSlider`. Обработчик проверяет наличие изменений в `Value` свойство, и если оно отличается от `Position` свойство, то `Position` свойству из `Value` свойство.

В теории внутреннего `if` инструкции может быть записан следующим образом:

```csharp
if (newPosition.Seconds != Position.Seconds)
{
    Position = newPosition;
}
```

Однако реализация Android `Slider` имеет только 1000 отдельных шагов, вне зависимости от `Minimum` и `Maximum` параметры. Длина видео больше, чем 1 000 секунд, а затем два различных `Position` значения будет соответствовать один и тот же `Value` параметра `Slider`и это `if` инструкции приведет к запуску ложный положительный результат для манипуляции пользователя из `Slider`. Вместо этого убедитесь, что новое положение и положение существующих больше чем одну сотую общую длительность безопаснее.

## <a name="using-the-positionslider"></a>С помощью PositionSlider

Документация по UWP [ `MediaElement` ](/uwp/api/Windows.UI.Xaml.Controls.MediaElement/) предупреждает о привязке к `Position` свойство так, как часто обновляет свойство. В документации рекомендуется использовать таймер для запроса `Position` свойство.

Это хороший рекомендации, но три `VideoPlayerRenderer` классы косвенно уже используется таймер для обновления `Position` свойство. `Position` Свойство изменяется в обработчике `UpdateStatus` событие, которое возникает, только 10 раз в секунду.

Таким образом `Position` свойство `VideoPlayer` могут быть привязаны к `Position` свойство `PositionSlider` без проблем с производительностью, как показано в **позиции штрих** страницы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.CustomPositionBarPage"
             Title="Custom Position Bar">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           AreTransportControlsEnabled="False"
                           Source="{StaticResource ElephantsDream}" />

        ···

        <StackLayout Grid.Row="1"
                     Orientation="Horizontal"
                     Margin="10, 0"
                     BindingContext="{x:Reference videoPlayer}">

            <Label Text="{Binding Path=Position,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center"/>

            ···

            <Label Text="{Binding Path=TimeToEnd,
                                  StringFormat='{0:hh\\:mm\\:ss}'}"
                   VerticalOptions="Center" />
        </StackLayout>

        <video:PositionSlider Grid.Row="2"
                              Margin="10, 0, 10, 10"
                              BindingContext="{x:Reference videoPlayer}"
                              Duration="{Binding Duration}"           
                              Position="{Binding Position}">
            <video:PositionSlider.Triggers>
                <DataTrigger TargetType="video:PositionSlider"
                             Binding="{Binding Status}"
                             Value="{x:Static video:VideoStatus.NotReady}">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </video:PositionSlider.Triggers>
        </video:PositionSlider>
    </Grid>
</ContentPage>
```

Скрывает первый кнопку с многоточием (···) `ActivityIndicator`; это же, как и в предыдущем **пользовательский транспорт** страницы. Обратите внимание на два `Label` отображение элементов `Position` и `TimeToEnd` свойства. Многоточие между этими двумя `Label` элементы скрывает два `Button` элементы, отображаемые в **пользовательский транспорт** для воспроизведение, Пауза и остановка. Логику кода также является таким же, как **пользовательский транспорт** страницы.

[![Нестандартное расположение](custom-positioning-images/custompositioning-small.png "нестандартное расположение")](custom-positioning-images/custompositioning-large.png "нестандартное расположение")

Это заключительный шаг обсуждение `VideoPlayer`.

## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
