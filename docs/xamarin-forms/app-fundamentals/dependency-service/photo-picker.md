---
title: Подбор фотографию из библиотеки рисунков
description: В этой статье описывается класс помощью Xamarin.Forms DependencyService используется для выбора фотографию из библиотеки рисунков телефона.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: af5f499687e1ef0b7c245ca524e33cd9d31683cb
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242473"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Подбор фотографию из библиотеки рисунков

В этой статье описывается создание приложения, которое позволяет пользователю выбрать фотографию из библиотеки рисунков телефона. Поскольку Xamarin.Forms не содержит эту функцию, бывает необходимо использовать [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) для доступа к встроенным API для каждой платформы.  В этой статье будут рассмотрены следующие действия для использования `DependencyService` для выполнения этой задачи:

- **[Создание интерфейса](#Creating_the_Interface)**  &ndash; понять, как интерфейс создается в общем коде.
- **[Реализация iOS](#iOS_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинный код для iOS.
- **[Реализация Android](#Android_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для Android.
- **[Универсальные реализации платформы Windows](#UWP_Implementation)**  &ndash; Узнайте, как реализовать интерфейс в машинном коде для универсальной платформы Windows (UWP).
- **[Реализация в общем коде](#Implementing_in_Shared_Code)**  &ndash; использование `DependencyService` вызывать собственную реализацию из общего кода.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Создание интерфейса

Сначала необходимо Создайте интерфейс в общий код, который выражает нужной функции. В случае с приложением фото комплектации требуется только один метод. Это определяется в [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) интерфейс в библиотеке .NET Standard этого примера кода:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Как асинхронная определен метод, так как метод должен возвращать быстро, но не может вернуть `Stream` объект для выбранной фотографии, пока пользователь не просматривать библиотеки рисунков и выбран один.

Этот интерфейс реализуется на всех платформах, с помощью кода под конкретную платформу.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Реализация iOS

Реализация iOS `IPicturePicker` интерфейс использует [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) как описано в [ **выберите фотографию из коллекции** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) инструкций и [пример кода](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

Реализация операций ввода-вывода содержится в [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) класса в проекте iOS в образце кода. Чтобы сделать этот класс видимым для `DependencyService` manager, необходимо определить класс с [`assembly`] атрибут типа `Dependency`, и должны быть открытыми и явно реализовать класс `IPicturePicker` интерфейс:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` Метод создает `UIImagePickerController` и инициализирует его, чтобы выбрать образы из библиотеки фото. Требуются два обработчика событий: один для, когда пользователь выбирает фотографии, а другой — для когда пользователь отменяет отображение фото библиотеки. `PresentModalViewController` Библиотеки фотография пользователя.

На этом этапе `GetImageStreamAsync` метод должен возвращать `Task<Stream>` объект в код, который его вызывает. Эта задача выполняется только в том случае, когда пользователь закончил взаимодействия с библиотекой фотографий и вызывается один из обработчиков событий. Для ситуаций, подобных [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) важно класса. Этот класс предоставляет `Task` объект правильную универсального типа для возврата из `GetImageStreamAsync` метод и класс можно позже сигнализировать, что после завершения задачи.

`FinishedPickingMedia` Обработчик событий вызывается, когда пользователь выбрал рисунка. Тем не менее, предоставляет обработчик `UIImage` объекта и `Task` должен возвращать .NET `Stream` объекта. Это выполняется в два этапа: `UIImage` объекта сначала преобразовывается в JPEG-файл в памяти, хранятся в `NSData` объекта, а затем `NSData` преобразуется объект .NET `Stream` объекта. Вызов `SetResult` метод `TaskComkpletionSource` завершения задачи объекта, предоставляя `Stream` объекта:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
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

Приложения iOS требуется разрешение пользователя для доступа к библиотеке фото телефона. Добавьте следующий код в `dict` раздел файла Info.plist:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Реализация Android

Android реализация использует метод, описанный в [ **выбрать изображение** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) инструкций и [пример кода](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Тем не менее, — метод, который вызывается, когда пользователь выбрал образ из библиотеки рисунков `OnActivityResult` переопределить в класс, производный от `Activity`. По этой причине нормали [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) класса в проекте Android были дополнены поля, свойства и переопределение `OnActivityResult` метод:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

`OnActivityResult`Переопределение соответствует файлу выбранного изображения с Android `Uri` объект, но это может быть преобразован в .NET `Stream` путем вызова метода `OpenInputStream` метод `ContentResolver` объекта, который был получен из действия `ContentResolver` свойство.

Как реализация iOS, Android реализация использует `TaskCompletionSource` для оповещения о том, что задача завершена. Это `TaskCompletionSource` объект определяется как открытые свойства `MainActivity` класса. Это позволяет ссылаться на свойство [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) класса в проекте Android. Это класс с `GetImageStreamAsync` метод:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Этот метод получает `MainActivity` класса для нескольких целей: для `Instance` свойство, для `PickImageId` поля для `TaskCompletionSource` свойство и для вызова `StartActivityForResult`. Этот метод определяется `FormsApplicationActivity` класс, являющийся базовым классом `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>Реализация UWP

В отличие от iOS и Android реализации, реализация Выбор фото для универсальной платформы Windows не требует `TaskCompletionSource` класса. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Класс использует [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) класса, чтобы получить доступ к библиотеке фото. Поскольку `PickSingleFileAsync` метод `FileOpenPicker` сам по себе является асинхронным, `GetImageStreamAsync` можно просто использовать метод `await` этот метод (и другими асинхронными методами) и возвращают `Stream` объекта:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Реализация в общий код

Теперь, когда интерфейс реализован для каждой платформы, приложение в библиотеке .NET Standard сможете использовать его.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Класс создает `Button` выбирает фотографию:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` Обработчик использует `DependencyService` класса для вызова `GetImageStreamAsync`. Это приводит к вызову в проекте платформы. Если метод возвращает `Stream` объекта, а затем создается обработчик `Image` элемент для этого рисунка с `TabGestureRecognizer`и заменяет `StackLayout` на странице с помощью, `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Коснувшись `Image` элемент возвращает страницу значение normal.


## <a name="related-links"></a>Связанные ссылки

- [Выберите фотографию из коллекции (iOS)](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Выберите изображение (Android)](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [Помощью DependencyService (пример)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
