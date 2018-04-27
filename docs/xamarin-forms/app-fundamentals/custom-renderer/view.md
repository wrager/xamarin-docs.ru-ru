---
title: Реализация представления
description: Элементы управления Xamarin.Forms настраиваемого пользовательского интерфейса должен быть производным от класса представления, который используется для размещения макетов и элементов управления на экране. В этой статье показано, как создать пользовательское средство отрисовки для пользовательского элемента управления Xamarin.Forms, который используется для отображения потока предварительного просмотра видео с камеры в устройстве.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: a8ab35b3ec13c76e1e00da6e3265e3e337e37b7e
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2018
---
# <a name="implementing-a-view"></a>Реализация представления

_Элементы управления Xamarin.Forms настраиваемого пользовательского интерфейса должен быть производным от класса представления, который используется для размещения макетов и элементов управления на экране. В этой статье показано, как создать пользовательское средство отрисовки для пользовательского элемента управления Xamarin.Forms, который используется для отображения потока предварительного просмотра видео с камеры в устройстве._

Каждый Xamarin.Forms представление имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) визуализируется в Xamarin.Forms приложение в iOS, `ViewRenderer` создается экземпляр класса, который в свою очередь создает собственный `UIView` элемента управления. На платформе Android `ViewRenderer` класс создает экземпляр собственного `View` элемента управления. В универсальной платформы Windows (UWP), `ViewRenderer` класс создает экземпляр собственного `FrameworkElement` элемента управления. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) и соответствующие собственные элементы управления, которые реализуют:

![](view-images/view-classes.png "Связь между класс представления и его реализации собственных классов")

Процесс подготовки отчета можно использовать для реализации настроек платформой путем создания пользовательского средства визуализации для [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Control) Xamarin.Forms пользовательского элемента управления.
1. [Использовать](#Consuming_the_Custom_Control) пользовательского элемента управления с помощью Xamarin.Forms.
1. [Создание](#Creating_the_Custom_Renderer_on_each_Platform) пользовательское средство отрисовки для элемента управления на каждой платформе.

Каждый элемент теперь обсуждаются в свою очередь, чтобы реализовать `CameraPreview` модуля подготовки отчетов, отображается предварительный просмотр потока видео с камеры в устройстве. Коснувшись поток видео будет останавливать и запускать его.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Создание пользовательского элемента управления

Пользовательский элемент управления можно создать путем создания подкласса [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класса, как показано в следующем примере кода:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` Пользовательский элемент управления создается в проекта переносимой библиотеки классов (PCL) и определяет API-интерфейса для элемента управления. Предоставляет пользовательский элемент управления `Camera` свойство, используемое для управления ли видеопоток должен отображаться из передний или задний камеру на устройстве. Если не указано значение `Camera` свойства при создании элемента управления, по умолчанию используется для указания задней камеры.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Использование пользовательского элемента управления

`CameraPreview` Пользовательский элемент управления может ссылаться на языке XAML в проект PCL объявление пространства имен для его расположения и с помощью префикса пространства имен для элемента пользовательского элемента управления. В следующем примере кода показан способ `CameraPreview` пользовательский элемент управления может использоваться XAML-страницы:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` Префикс пространства имен можно присвоить любое имя. Тем не менее `clr-namespace` и `assembly` значения должны соответствовать сведений пользовательского элемента управления. После объявления пространства имен, префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показан способ `CameraPreview` пользовательский элемент управления может использоваться на странице C#:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Экземпляр `CameraPreview` пользовательский элемент управления будет использоваться для отображения поток предварительного просмотра видео с камеры в устройстве. Помимо указания значения при необходимости `Camera` свойства настройки элемента управления будет осуществляться в пользовательское средство отрисовки.

Возможность добавления пользовательского средства отрисовки в каждый проект приложения для создания элементов управления предварительной версии камеры конкретную платформу.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского средства визуализации для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `ViewRenderer<T1,T2>` класс, который отображает пользовательский элемент управления. Первый аргумент типа должен быть пользовательского элемента управления, в данном случае является модуль подготовки отчетов `CameraPreview`. Второй аргумент типа должен быть собственный элемент управления, который будет реализован пользовательский элемент управления.
1. Переопределить `OnElementChanged` метода, который отображает пользовательскую логику управления и записи для ее настройки. Этот метод вызывается, когда создается соответствующий элемент управления Xamarin.Forms.
1. Добавить `ExportRenderer` атрибут класс пользовательского средства отрисовки, чтобы указать, что он будет использоваться для подготовки к просмотру пользовательский элемент управления с помощью Xamarin.Forms. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Для большинства элементов Xamarin.Forms является необязательным для предоставления пользовательского средства отрисовки в каждом проекте платформы. Если пользовательское средство отрисовки не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для базового класса элемента управления. Однако пользовательские модули подготовки отчетов, необходимы в каждом проекте платформы при подготовке к просмотру [представление](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) элемента.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](view-images/solution-structure.png "Обязанности проекта CameraPreview пользовательского модуля подготовки отчетов")

`CameraPreview` Пользовательский элемент управления отрисовывается классами платформой модуля подготовки отчетов, которые являются производными от `ViewRenderer` класса для каждой платформы. Это приведет к каждой `CameraPreview` пользовательский элемент управления готовится к просмотру с помощью элементов управления от платформы, как показано на следующем снимке экрана:

![](view-images/screenshots.png "CameraPreview на каждой платформе")

`ViewRenderer` Предоставляемых классами `OnElementChanged` метод, который вызывается при создании пользовательского элемента управления Xamarin.Forms для отображения соответствующего неуправляемого элемента управления. Этот метод принимает `ElementChangedEventArgs` параметра, который содержит `OldElement` и `NewElement` свойства. Эти свойства представляют собой элемент Xamarin.Forms, модуль подготовки отчетов *было* присоединен и элемент Xamarin.Forms, модуль подготовки отчетов *—* присоединенного, соответственно. В примере приложения `OldElement` свойство будет `null` и `NewElement` свойство будет содержать ссылку на `CameraPreview` экземпляра.

Переопределенная версия `OnElementChanged` метод в каждом классе платформой модуля подготовки отчетов — это место для выполнения при создании экземпляра собственного элемента управления и настройки. `SetNativeControl` Метод должен использоваться для создания экземпляра собственного элемента управления, и этот метод также присвоит ссылкой на элемент управления для `Control` свойства. Кроме того, можно получить ссылку на элемент управления Xamarin.Forms, который готовится к просмотру с помощью `Element` свойство.

В некоторых случаях `OnElementChanged` метод может вызываться несколько раз. Таким образом чтобы предотвратить утечку памяти, необходимо соблюдать осторожность при создании нового собственного элемента управления. В следующем примере кода показан подход, используемый при создании собственного элемента управления в пользовательском отрисовщике:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Собственный элемент управления должен создаваться только один раз, когда свойству `Control` задано значение `null`. Элемент управления следует настраивать и подписывать на обработчики событий только при присоединении пользовательского отрисовщика к новому элементу Xamarin.Forms. Аналогичным образом все обработчики событий, которые были подписаны на должно быть отменена при изменении элемента модуля подготовки отчетов, к которому присоединена. Этот подход поможет для создания высокопроизводительных пользовательское средство отрисовки, который не страдает от утечек памяти.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя типа пользовательского элемента управления Xamarin.Forms готовится к просмотру и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматриваются реализации каждого пользовательского средства отрисовки платформой класса.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

В следующем примере кода показано пользовательское средство отрисовки для платформы iOS.

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

При условии, что `Control` свойство `null`, `SetNativeControl` метод вызывается для создания нового экземпляра `UICameraPreview` управления и назначить ссылку на него в `Control` свойство. `UICameraPreview` Платформой пользовательский элемент управления, который использует `AVCapture` API-интерфейсы для предоставления поток предварительного просмотра из камеры. Он предоставляет `Tapped` событие, которое обрабатывается `OnCameraPreviewTapped` метод, чтобы остановить и запустить предварительный просмотр видео при его выборе элемента. `Tapped` Событий подписана при пользовательское средство отрисовки для нового элемента Xamarin.Forms, а подписка отменена только при присоединении элемента модуля подготовки отчетов к изменениям.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

В следующем примере кода показано пользовательское средство отрисовки для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

При условии, что `Control` свойство `null`, `SetNativeControl` метод вызывается для создания нового экземпляра `CameraPreview` управления и назначить ему ссылку на него для `Control` свойства. `CameraPreview` Платформой пользовательский элемент управления, который использует `Camera` API, чтобы предоставить Предварительный просмотр потока из камеры. `CameraPreview` Элемент управления, затем настроен, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms. Эта конфигурация включает в себя создание новых собственный `Camera` объекта для доступа к конкретной аппаратной камеры и регистрации обработчика событий для обработки `Click` событий. В свою очередь этот обработчик будет останавливать и запускать Предварительный просмотр видео, при его выборе элемента. `Click` Событий подписку на, если элемент Xamarin.Forms модуль подготовки отчетов подключен к изменениям.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского средства визуализации на UWP

В следующем примере кода показано пользовательское средство отрисовки для UWP.

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        MediaCapture mediaCapture;
        CaptureElement captureElement;
        CameraOptions cameraOptions;
        Application app;
        bool isPreviewing = false;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                ...
                captureElement = new CaptureElement ();
                captureElement.Stretch = Stretch.UniformToFill;

                InitializeAsync ();
                SetNativeControl (captureElement);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped (object sender, TappedRoutedEventArgs e)
        {
            if (isPreviewing) {
                await StopPreviewAsync ();
            } else {
                await StartPreviewAsync ();
            }
        }
        ...
    }
}
```

При условии, что `Control` свойство `null`, новый `CaptureElement` создается и `InitializeAsync` вызывается метод, который использует `MediaCapture` API, чтобы предоставить Предварительный просмотр потока из камеры. `SetNativeControl` Попытку назначить ссылку, чтобы затем вызывается метод `CaptureElement` экземпляр `Control` свойство. `CaptureElement` Управления предоставляет `Tapped` событие, которое обрабатывается `OnCameraPreviewTapped` метод, чтобы остановить и запустить предварительный просмотр видео при его выборе элемента. `Tapped` Событий подписана при пользовательское средство отрисовки для нового элемента Xamarin.Forms, а подписка отменена только при присоединении элемента модуля подготовки отчетов к изменениям.

> [!NOTE]
> Очень важно для остановки и удаления объектов, которые обеспечивают доступ к камере в приложении UWP. Невыполнение этого требования может повлиять на другие приложения, которые пытаются получить доступ к камере устройства. Дополнительные сведения см. в разделе [камеры предварительного просмотра](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создать пользовательское средство отрисовки для пользовательского элемента управления Xamarin.Forms, который используется для отображения потока предварительного просмотра видео с камеры в устройстве. Элементы управления Xamarin.Forms настраиваемого пользовательского интерфейса должен быть производным от [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класс, который используется для размещения макетов и элементов управления на экране.


## <a name="related-links"></a>Связанные ссылки

- [CustomRendererView (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
