---
title: Загрузка видео ресурсов приложения
description: В этой статье объясняется, как загрузить видео, хранимые как ресурсы приложения в приложении видеопроигрывателя с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: F75BD540-9354-4C17-A119-57F3DEC66D54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: f28b0dc8e25cb2e498f4101175005f05a5c5a6ef
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241036"
---
# <a name="loading-application-resource-videos"></a>Загрузка видео ресурсов приложения

Пользовательские модули подготовки отчетов для `VideoPlayer` представления можно использовать для воспроизведения видео файлов, внедренных в проектах отдельных платформы как ресурсы приложения. Однако текущая версия `VideoPlayer` сможет подключиться к ресурсам, внедренных в библиотеке .NET Standard.

Чтобы загрузить эти ресурсы, создайте экземпляр `ResourceVideoSource` , установив `Path` свойство имени файла (или в папку и имя файла) ресурса. Кроме того, вы можете вызвать статический `VideoSource.FromResource` метод, чтобы создать ссылку на ресурс. Затем задайте `ResourceVideoSource` объект `Source` свойство `VideoPlayer`.

## <a name="storing-the-video-files"></a>Хранение файлов видео

Сохранение файла видео в проекте платформы отличается для трех платформ.

### <a name="ios-video-resources"></a>видео-ресурсов операций ввода-вывода

В проект iOS можно хранить видео в **ресурсов** папку или подпапку **ресурсов** папки. Файл изображения должен содержать `Build Action` из `BundleResource`. Задать `Path` свойство `ResourceVideoSource` к имени файла, например, **MyFile.mp4** для файла в **ресурсов** папки, или **MyFolder/MyFile.mp4**, где **MyFolder** вложена **ресурсов**.

В **VideoPlayerDemos** решения, **VideoPlayerDemos.iOS** проект содержит вложенную папку **ресурсов** с именем **видео** содержащий файл с именем **iOSApiVideo.mp4**. Это связано с коротким видео, которое показывает, как использовать веб-сайт Xamarin для ссылки на документацию для iOS `AVPlayerViewController` класса.

### <a name="android-video-resources"></a>Android видео-ресурсов

В проекте Android видео должны храниться во вложенной **ресурсов** с именем **необработанные**. **Необработанные** папка не может содержать вложенные папки. Предоставьте видеофайл `Build Action` из `AndroidResource`. Задать `Path` свойство `ResourceVideoSource` к имени файла, например, **MyFile.mp4**.

**VideoPlayerDemos.Android** проект содержит вложенную папку **ресурсов** с именем **необработанные**, которая содержит файл с именем **AndroidApiVideo.mp4**.

### <a name="uwp-video-resources"></a>Видеоресурсов UWP

В проекте универсальной платформы Windows можно хранить видео в любой папке проекта. Дайте файлу `Build Action` из `Content`. Задать `Path` свойство `ResourceVideoSource` к папке и имя файла, например, **MyFolder/MyVideo.mp4**.

**VideoPlayerDemos.UWP** проект содержит папку с именем **видео** с файлом **UWPApiVideo.mp4**.

## <a name="loading-the-video-files"></a>Загрузка файлов видео

Каждый из классов отрисовки платформы содержит код в его `SetSource` метод для загрузки видеофайлов, хранимые как ресурсы.

### <a name="ios-resource-loading"></a>Загрузка ресурсов операций ввода-вывода

Версия iOS `VideoPlayerRenderer` использует `GetUrlForResource` метод `NSBundle` для загрузки ресурсов. Полный путь необходимо разделить на имя файла, расширение и каталог. Код использует `Path` класс в .NET `System.IO` пространство имен для разделения путь к файлу на эти компоненты:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhitespace(path))
                {
                    string directory = Path.GetDirectoryName(path);
                    string filename = Path.GetFileNameWithoutExtension(path);
                    string extension = Path.GetExtension(path).Substring(1);
                    NSUrl url = NSBundle.MainBundle.GetUrlForResource(filename, extension, directory);
                    asset = AVAsset.FromUrl(url);
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="android-resource-loading"></a>Загрузка ресурсов Android

Android `VideoPlayerRenderer` использует для создания имени файла и имени пакета `Uri` объекта. Имя пакета — имя приложения, в этом случае **VideoPlayerDemos.Android**, которые можно получить из статического `Context.PackageName` свойство. Итоговые `Uri` объект затем передается `SetVideoURI` метод `VideoView`:

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
            else if (Element.Source is ResourceVideoSource)
            {
                string package = Context.PackageName;
                string path = (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    string filename = Path.GetFileNameWithoutExtension(path).ToLowerInvariant();
                    string uri = "android.resource://" + package + "/raw/" + filename;
                    videoView.SetVideoURI(Android.Net.Uri.Parse(uri));
                    hasSetSource = true;
                }
            }
            ···
        }
        ···
    }
}
```

### <a name="uwp-resource-loading"></a>Загрузка ресурсов UWP

UWP `VideoPlayerRenderer` создает `Uri` объекта для пути и назначает ей `Source` свойства `MediaElement`:

```csharp
namespace FormsVideoLibrary.UWP
{
    public class VideoPlayerRenderer : ViewRenderer<VideoPlayer, MediaElement>
    {
        ···
        async void SetSource()
        {
            bool hasSetSource = false;
            ···
            else if (Element.Source is ResourceVideoSource)
            {
                string path = "ms-appx:///" + (Element.Source as ResourceVideoSource).Path;

                if (!String.IsNullOrWhiteSpace(path))
                {
                    Control.Source = new Uri(path);
                    hasSetSource = true;
                }
            }
        }
        ···
    }
}
```

## <a name="playing-the-resource-file"></a>Воспроизведение файла ресурсов

**Воспроизведение видео ресурсов** страницы в **VideoPlayerDemos** использует решение `OnPlatform` класс, чтобы указать файл видео для каждой платформы:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.PlayVideoResourcePage"
             Title="Play Video Resource">
    <video:VideoPlayer>
        <video:VideoPlayer.Source>
            <video:ResourceVideoSource>
                <video:ResourceVideoSource.Path>
                    <OnPlatform x:TypeArguments="x:String">
                        <On Platform="iOS" Value="Videos/iOSApiVideo.mp4" />
                        <On Platform="Android" Value="AndroidApiVideo.mp4" />
                        <On Platform="UWP" Value="Videos/UWPApiVideo.mp4" />
                    </OnPlatform>
                </video:ResourceVideoSource.Path>
            </video:ResourceVideoSource>
        </video:VideoPlayer.Source>
    </video:VideoPlayer>
</ContentPage>
```

Если ресурс операций ввода-вывода хранится в **ресурсов** папки, и если ресурс UWP хранится в корневой папке проекта, можно использовать такое же имя файла для трех платформ. Если это так, то это имя можно задать непосредственно в `Source` свойство `VideoPlayer`.

Вот этой страницы, работающих на трех платформ.

[![Воспроизведение видео ресурсов](loading-resources-images/playvideoresource-small.png "воспроизведение видео ресурсов")](loading-resources-images/playvideoresource-large.png#lightbox "воспроизведение видео ресурсов")

Теперь вы знаете как [загрузить видео из Web URI](web-videos.md) и воспроизведение внедренные ресурсы. Кроме того, вы можете [загрузить видео из устройства видеотека](accessing-library.md).


## <a name="related-links"></a>Связанные ссылки

- [Видеодемонстрации проигрывателя (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
