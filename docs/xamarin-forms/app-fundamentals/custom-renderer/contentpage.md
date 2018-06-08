---
title: Настройка ContentPage
description: ContentPage является визуальный элемент, который отображает одно представление и занимает большую часть экрана. В этой статье показано, как создать пользовательское средство отрисовки страницы ContentPage, позволяя разработчикам выполнять переопределить свои собственные настройки платформой собственного отрисовки по умолчанию.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 5fe7250b5b8fcea97d4fbe6846999be60e8e8673
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848178"
---
# <a name="customizing-a-contentpage"></a>Настройка ContentPage

_ContentPage является визуальный элемент, который отображает одно представление и занимает большую часть экрана. В этой статье показано, как создать пользовательское средство отрисовки страницы ContentPage, позволяя разработчикам выполнять переопределить свои собственные настройки платформой собственного отрисовки по умолчанию._

Каждый элемент управления Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) отрисовывается приложением Xamarin.Forms в iOS `PageRenderer` создается экземпляр класса, который в свою очередь создает собственный `UIViewController` элемента управления. На платформе Android `PageRenderer` класс создает экземпляры `ViewGroup` элемента управления. В универсальной платформы Windows (UWP), `PageRenderer` класс создает экземпляры `FrameworkElement` элемента управления. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) и соответствующие собственные элементы управления, которые реализуют:

![](contentpage-images/contentpage-classes.png "Отношение между классом ContentPage и реализации собственных элементов управления")

Процесс подготовки отчета можно считать преимущества реализации настроек платформой путем создания пользовательского средства визуализации для [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms страницы.
1. [Использовать](#Consuming_the_Xamarin.Forms_Page) страницы из Xamarin.Forms.
1. [Создание](#Creating_the_Page_Renderer_on_each_Platform) пользовательское средство отрисовки для страницы на каждой платформе.

Каждый элемент теперь обсуждаются в свою очередь, для реализации `CameraPage` , предоставляющий динамической камеру веб-канала и собирать фотографии.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Создание страницы Xamarin.Forms

Неизмененные [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) можно добавить в общий проект Xamarin.Forms, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

Аналогичным образом, файл кода для [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) также должны оставаться без изменений, как показано в следующем примере кода:

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

В следующем примере кода показано, как страница может быть создано в C#:

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

Экземпляр `CameraPage` будет использоваться для отображения динамической канала на каждой платформе камеры. Настройки элемента управления будет исполняться в пользовательское средство отрисовки, поэтому требуется без дополнительных изменений в `CameraPage` класса.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Использование Xamarin.Forms страницы

Пустые `CameraPage` должны отображаться в приложении Xamarin.Forms. Это происходит, когда кнопка на `MainPage` касании экземпляр, который в свою очередь выполняет `OnTakePhotoButtonClicked` метод, как показано в следующем примере кода:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

Этот код просто переходит к `CameraPage`, на какие пользовательские модули подготовки отчетов будет настраивать внешний вид страницы для каждой платформы.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>Создание страницы модуля подготовки отчетов для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `PageRenderer` класса.
1. Переопределить `OnElementChanged` метод, который использует собственный логику страницы и записи для настройки страницы. `OnElementChanged` Метод вызывается, когда создается соответствующий элемент управления Xamarin.Forms.
1. Добавить `ExportRenderer` для модуля подготовки отчетов класс страницы, чтобы указать, что он будет использоваться для визуализации страницы Xamarin.Forms атрибута. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Это необязательно для предоставления разрывами страниц в каждом проекте платформы. Если модуль подготовки страницы не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для страницы.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](contentpage-images/solution-structure.png "Обязанности проекта CameraPage пользовательского модуля подготовки отчетов")

`CameraPage` Экземпляром визуализируется с платформой `CameraPageRenderer` классы, которые являются производными от `PageRenderer` класса для данной платформы. Это приведет к каждой `CameraPage` экземпляр готовится к просмотру с камеры в реальном времени веб-канал, как показано на следующем снимке экрана:

![](contentpage-images/screenshots.png "CameraPage на каждой платформе")

`PageRenderer` Предоставляемых классами `OnElementChanged` метод, который вызывается при Xamarin.Forms страницы для отображения соответствующего неуправляемого элемента управления. Этот метод принимает `ElementChangedEventArgs` параметра, который содержит `OldElement` и `NewElement` свойства. Эти свойства представляют собой элемент Xamarin.Forms, модуль подготовки отчетов *было* присоединен и элемент Xamarin.Forms, модуль подготовки отчетов *—* присоединенного, соответственно. В примере приложения `OldElement` свойство будет `null` и `NewElement` свойство будет содержать ссылку на `CameraPage` экземпляра.

Переопределенная версия `OnElementChanged` метод `CameraPageRenderer` класса — это место для выполнения настройки страницу машинный код. Можно получить ссылку на экземпляр page Xamarin.Forms, которое отображается при помощи `Element` свойство.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя отображаемой странице Xamarin.Forms типа и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация `CameraPageRenderer` пользовательского средства визуализации для каждой платформы.

### <a name="creating-the-page-renderer-on-ios"></a>Создание страницы модуля подготовки отчетов для операций ввода-вывода

В следующем примере кода показано разрывами страниц для платформы iOS.

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Вызов базового класса `OnElementChanged` метод создает iOS `UIViewController` элемента управления. Поток динамической камеры только подготавливается к просмотру условии, что модуль подготовки отчетов, уже не присоединяется к существующим элементом Xamarin.Forms и при условии, что существует экземпляр страницы, который отображается пользовательского модуля подготовки отчетов.

Страница затем настраивается путем ряд методов, использующих `AVCapture` API-интерфейсы для предоставления динамический поток из камеры и собирать фотографии.

### <a name="creating-the-page-renderer-on-android"></a>Создание страницы модуля подготовки отчетов в Android

В следующем примере кода показано разрывами страниц для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

Вызов базового класса `OnElementChanged` метод создает Android `ViewGroup` элемента управления, который представляет собой группу представления. Поток динамической камеры только подготавливается к просмотру условии, что модуль подготовки отчетов, уже не присоединяется к существующим элементом Xamarin.Forms и при условии, что существует экземпляр страницы, который отображается пользовательского модуля подготовки отчетов.

Затем пользовательские страницы путем вызова ряд методов, использующих `Camera` API, чтобы предоставить динамический поток из камеры и собирать фотографии, перед `AddView` вызова метода для добавления динамической камеры потока пользовательского интерфейса для `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>Создание страницы модуля подготовки отчетов на UWP

В следующем примере кода показано разрывами страниц для UWP.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

Вызов базового класса `OnElementChanged` метод создает `FrameworkElement` управления, отображении страницы. Поток динамической камеры только подготавливается к просмотру условии, что модуль подготовки отчетов, уже не присоединяется к существующим элементом Xamarin.Forms и при условии, что существует экземпляр страницы, который отображается пользовательского модуля подготовки отчетов. Затем пользовательские страницы путем вызова ряд методов, использующих `MediaCapture` API, чтобы предоставить динамический поток из камеры и собирать фотографии, перед добавлением настроенной страницы `Children` коллекции для отображения.

При реализации пользовательского средства отрисовки, который является производным от `PageRenderer` на UWP, `ArrangeOverride` метод также должен быть реализован для размещения элементов управления страницы, так как базовый модуль подготовки отчетов не знает, что с ними делать. В противном случае — пустая страница результатов. Таким образом, в этом примере `ArrangeOverride` вызовы метода `Arrange` метод `Page` экземпляра.

> [!NOTE]
> Очень важно для остановки и удаления объектов, которые обеспечивают доступ к камере в приложении UWP. Невыполнение этого требования может повлиять на другие приложения, которые пытаются получить доступ к камере устройства. Дополнительные сведения см. в разделе [камеры предварительного просмотра](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access).

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создать пользовательское средство отрисовки для [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) страницу, позволяя разработчикам переопределять собственного отрисовки по умолчанию с свои собственные настройки конкретную платформу. Объект `ContentPage` является визуальный элемент, который отображает одно представление и занимает большую часть экрана.


## <a name="related-links"></a>Связанные ссылки

- [CustomRendererContentPage (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
