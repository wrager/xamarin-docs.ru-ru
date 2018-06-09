---
title: Доступ к видеотеке устройства
description: В этой статье объясняется, как получить доступ к видеотеке устройства в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 364C1D43-EAAE-45B9-BE24-0DA5AE74C4D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: 7e9f7ad93ae8828155847b923cb2779b3146f63e
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240704"
---
# <a name="accessing-the-devices-video-library"></a>Доступ к видеотеке устройства

Большинство современных мобильных устройств и настольных компьютеров имеют возможность записи видео, с помощью камеры устройства. Видео, которые создает пользователь затем сохраняются в виде файлов на устройстве. Эти файлы можно получить из библиотеки изображений и воспроизводить на `VideoPlayer` класса так же, как любые другие видео.

## <a name="the-photo-picker-dependency-service"></a>Службы зависимостей Выбор фото

Каждый из трех платформ включает средство, позволяющее пользователю выбрать фото или видео из библиотеки изображений устройства. Первым этапом воспроизведение видео из библиотеки изображений устройства создание зависимостей службы, которая вызывает выбор изображений для каждой платформы. Службы зависимостей, описанной ниже очень похож на один определенный в [ **комплектации фотографию из библиотеки рисунков** ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md) статьи, за исключением того, что видео выбора возвращает имя файла, а не `Stream`объекта.

Проект .NET Standard библиотеки определяет интерфейс с именем `IVideoPicker` для зависимостей службы:

```csharp
namespace FormsVideoLibrary
{
    public interface IVideoPicker
    {
        Task<string> GetVideoFileAsync();
    }
}
```

Каждый из трех платформ содержит класс с именем `VideoPicker` , реализующий этот интерфейс.

### <a name="the-ios-video-picker"></a>Выбор видео iOS

IOS `VideoPicker` использует iOS [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) для доступа к библиотеке изображений, указание, что он должен быть ограничен видео (так называемый «фильмы») в iOS `MediaType` свойство. Обратите внимание, что `VideoPicker` явным образом реализует `IVideoPicker` интерфейса. Следует также обратить внимание `Dependency` атрибут, определяющий этот класс в качестве службы зависимостей. Это два требования, позволяющие Xamarin.Forms для поиска службы зависимостей в проекте платформы.

```csharp
using System;
using System.Threading.Tasks;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.iOS.VideoPicker))]

namespace FormsVideoLibrary.iOS
{
    public class VideoPicker : IVideoPicker
    {
        TaskCompletionSource<string> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<string> GetVideoFileAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.SavedPhotosAlbum,
                MediaTypes = new string[] { "public.movie" }
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<string>();
            return taskCompletionSource.Task;
        }

        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            if (args.MediaType == "public.movie")
            {
                taskCompletionSource.SetResult(args.MediaUrl.AbsoluteString);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}
```

### <a name="the-android-video-picker"></a>Android Выбор видео

Android реализация `IVideoPicker` требует метод обратного вызова, который является частью действий приложения. По этой причине `MainActivity` класс определяет два свойства, поля и метод обратного вызова:

```csharp
namespace VideoPlayerDemos.Droid
{
    ···
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            Current = this;
            ···
        }

        // Field, properties, and method for Video Picker
        public static MainActivity Current { private set; get; }

        public static readonly int PickImageId = 1000;

        public TaskCompletionSource<string> PickImageTaskCompletionSource { set; get; }

        protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
        {
            base.OnActivityResult(requestCode, resultCode, data);

            if (requestCode == PickImageId)
            {
                if ((resultCode == Result.Ok) && (data != null))
                {
                    // Set the filename as the completion of the Task
                    PickImageTaskCompletionSource.SetResult(data.DataString);
                }
                else
                {
                    PickImageTaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

`OnCreate` Метод в `MainActivity` сохраняет свой собственный экземпляр в статический `Current` свойство. Это позволяет реализовать `IVideoPicker` для получения `MainActivity` экземпляра для запуска **Выбор видео** выбора:

```csharp
using System;
using System.Threading.Tasks;
using Android.Content;
using Xamarin.Forms;

// Need application's MainActivity
using VideoPlayerDemos.Droid;

[assembly: Dependency(typeof(FormsVideoLibrary.Droid.VideoPicker))]

namespace FormsVideoLibrary.Droid
{
    public class VideoPicker : IVideoPicker
    {
        public Task<string> GetVideoFileAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("video/*");
            intent.SetAction(Intent.ActionGetContent);

            // Get the MainActivity instance
            MainActivity activity = MainActivity.Current;

            // Start the picture-picker activity (resumes in MainActivity.cs)
            activity.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Video"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            activity.PickImageTaskCompletionSource = new TaskCompletionSource<string>();

            // Return Task object
            return activity.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Дополнения к `MainActivity` являются только код в [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) решение, где необходимо изменить для поддержки обычный код приложения `FormsVideoLibrary` классы.

### <a name="the-uwp-video-picker"></a>Выбор видео UWP

Реализация UWP `IVideoPicker` интерфейс использует UWP [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/). Он начинает поиск файла с библиотекой рисунков и ограничивает типы файлов MP4 и WMV (Windows Media Video):

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using Windows.Storage.Pickers;
using Xamarin.Forms;

[assembly: Dependency(typeof(FormsVideoLibrary.UWP.VideoPicker))]

namespace FormsVideoLibrary.UWP
{
    public class VideoPicker : IVideoPicker
    {
        public async Task<string> GetVideoFileAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary
            };

            openPicker.FileTypeFilter.Add(".wmv");
            openPicker.FileTypeFilter.Add(".mp4");

            // Get a file and return the path
            StorageFile storageFile = await openPicker.PickSingleFileAsync();
            return storageFile?.Path;
        }
    }
}
```

## <a name="invoking-the-dependency-service"></a>Вызов службы зависимостей

**Воспроизведение видео библиотеки** страница [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/) показано, как использовать службу видео выбора зависимостей. XAML-файл содержит `VideoPlayer` экземпляра и `Button` с меткой **Показать видеотека**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayLibraryVideoPage"
             Title="Play Library Video">
    <StackLayout>
        <video:VideoPlayer x:Name="videoPlayer"
                           VerticalOptions="FillAndExpand" />

        <Button Text="Show Video Library"
                Margin="10"
                HorizontalOptions="Center"
                Clicked="OnShowVideoLibraryClicked" />
    </StackLayout>
</ContentPage>
```

Файл кода содержит `Clicked` обработчик `Button`. Вызов службы зависимостей требуется вызов `DependencyService.Get` для получения реализации `IVideoPicker` интерфейс в проекте платформы. `GetVideoFileAsync` На этом экземпляре затем вызывается метод:

```csharp
namespace VideoPlayerDemos
{
    public partial class PlayLibraryVideoPage : ContentPage
    {
        public PlayLibraryVideoPage()
        {
            InitializeComponent();
        }

       async void OnShowVideoLibraryClicked(object sender, EventArgs args)
        {
            Button btn = (Button)sender;
            btn.IsEnabled = false;

            string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();

            if (!String.IsNullOrWhiteSpace(filename))
            {
                videoPlayer.Source = new FileVideoSource
                {
                    File = filename
                };
            }

            btn.IsEnabled = true;
        }
    }
}
```

`Clicked` Обработчик затем использует это имя файла для создания `FileVideoSource` объекта и настройте ее `Source` свойство `VideoPlayer`.

Каждый из `VideoPlayerRenderer` классов содержит код из его `SetSource` метод для объектов типа `FileVideoSource`. Это показано ниже:

### <a name="handling-ios-files"></a>Обработка операций ввода-вывода файлов

Версия iOS `VideoPlayerRenderer` процессов `FileVideoSource` объектов с помощью статического `Asset.FromUrl` метод с именем файла. Это создание `AVAsset` объект, представляющий файл в библиотеке изображений устройства:

```csharp
namespace FormsVideoLibrary.iOS
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, UIView>
    {
        ···
        void SetSource()
        {
            AVAsset asset = null;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string uri = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(uri))
                {
                    asset = AVAsset.FromUrl(new NSUrl(uri));
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-android-files"></a>Обработка файлов Android

При обработке объектов типа `FileVideoSource`, реализация Android `VideoPlayerRenderer` использует `SetVideoPath` метод `VideoView` для указания файла в библиотеке изображений устройства:

```csharp
namespace FormsVideoLibrary.Droid
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, ARelativeLayout>
    {
        ···
        void SetSource()
        {
            isPrepared = false;
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    videoView.SetVideoPath(filename);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="handling-uwp-files"></a>Обработка файлов UWP

При обработке объектов типа `FileVideoSource`, реализация UWP `SetSource` метод должен создать `StorageFile` , откройте этот файл для чтения и передать объект stream `SetSource` метод `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        protected override void OnElementChanged(ElementChangedEventArgs<VideoPlayer> args)
        {
            ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is FileVideoSource)
            {
                // Code requires Pictures Library in Package.appxmanifest Capabilities to be enabled
                string filename = (Element.Source as FileVideoSource).File;

                if (!String.IsNullOrWhiteSpace(filename))
                {
                    StorageFile storageFile = await StorageFile.GetFileFromPathAsync(filename);
                    IRandomAccessStreamWithContentType stream = await storageFile.OpenReadAsync();
                    Control.SetSource(stream, storageFile.ContentType);
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

Для всех трех платформ видео начинает воспроизводиться практически сразу после видео источник установлен, так как файл на устройстве и не нужно загружать.



## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
- [Подбор фотографию из библиотеки рисунков](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
