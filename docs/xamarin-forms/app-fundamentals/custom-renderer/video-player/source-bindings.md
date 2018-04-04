---
title: Видео источников привязки для проигрывателя
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: bebf6fd905dd374822eb6974b28f1ac92a36c1bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="binding-video-sources-to-the-player"></a>Видео источников привязки для проигрывателя

Когда `Source` свойство `VideoPlayer` новый видеофайл задано представление, существующего видео воспроизведение прекращается и начинается новый видео. Это продемонстрировано на **выберите видео в Интернете** страница [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) образца. Страница содержит `ListView` заголовками три видео, на который ссылается **App.xaml** файла:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        
        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

При выборе видео `ItemSelected` выполняется обработчик событий в файле кода. Обработчик удаляет все пробелы и апострофы из заголовка и использует его в качестве ключа для получения одного из ресурсов, определенных в **App.xaml** файла. Что `UriVideoSource` объекта присваивается значение `Source` свойство `VideoPlayer`.

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

При первой загрузке страницы не выбран элемент в `ListView`, поэтому необходимо выбрать один для видео, чтобы начать воспроизведение:

[![Выберите видео в Интернете](source-bindings-images/selectwebvideo-small.png "выберите видео в Интернете")](source-bindings-images/selectwebvideo-large.png#lightbox "выберите видео в Интернете")

`Source` Свойство `VideoPlayer` поддерживаемый привязываемые свойства, которое означает, что он может быть целевого объекта привязки данных. Это продемонстрировано на **привязки к VideoPlayer** страницы. Разметка **BindToVideoPlayer.xaml** файла поддерживается следующий класс, инкапсулирующий название видео, а также соответствующий `VideoSource` объекта:

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

`ListView` В **BindToVideoPlayer.xaml** файл содержит массив этих `VideoInfo` объектов, каждое из которых инициализируется название видео и `UriVideoSource` объекта из словаря ресурсов в  **App.XAML**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`Source` Свойство `VideoPlayer` привязан к `ListView`. `Path` Привязки указывается как `SelectedItem.VideoSource`, который является составного контура, состоящий из двух свойств: `SelectedItem` — это свойство `ListView`. Выбранный элемент имеет тип `VideoInfo`, который имеет `VideoSource` свойства.

Как и в первом **выберите видео в Интернете** страницы, ни один элемент первоначально выбранный из `ListView`, поэтому необходимо выбрать один из видео до начала воспроизведения.


## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
