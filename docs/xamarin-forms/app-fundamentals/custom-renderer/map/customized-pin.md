---
title: Настройка ПИН-код карты
description: В этой статье показано, как создать пользовательское средство отрисовки для элемента управления карты, который отображает собственного карты с настроенные ПИН-код и настраиваемое представление данных ПИН-код для каждой платформы.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 04d3d99a5d85dd77c93e9b926e8952cc3d8a771e
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-map-pin"></a>Настройка ПИН-код карты

_В этой статье показано, как создать пользовательское средство отрисовки для элемента управления карты, который отображает собственного карты с настроенные ПИН-код и настраиваемое представление данных ПИН-код для каждой платформы._

Каждый Xamarin.Forms представление имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) визуализируется в Xamarin.Forms приложение в iOS, `MapRenderer` создается экземпляр класса, который в свою очередь создает собственный `MKMapView` элемента управления. На платформе Android `MapRenderer` класс создает экземпляр собственного `MapView` элемента управления. В универсальной платформы Windows (UWP), `MapRenderer` класс создает экземпляр собственного `MapControl`. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) и соответствующие собственные элементы управления, которые реализуют:

![](customized-pin-images/map-classes.png "Связь между элементом управления картой и реализации собственные элементы управления")

Процесс подготовки отчета можно использовать для реализации настроек платформой путем создания пользовательского средства визуализации для [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Map) Xamarin.Forms пользовательской карты.
1. [Использовать](#Consuming_the_Custom_Map) пользовательской карты с помощью Xamarin.Forms.
1. [Создание](#Creating_the_Custom_Renderer_on_each_Platform) пользовательское средство отрисовки карты на каждой платформе.

Каждый элемент теперь обсуждаются в свою очередь, чтобы реализовать `CustomMap` модуля подготовки отчетов, отображающий собственного карту с настроенные ПИН-код и настраиваемое представление данных ПИН-код для каждой платформы.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) необходимо инициализировать и настроить перед использованием. Дополнительные сведения см. на веб-сайте [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Создание пользовательской карты

Элемент управления пользовательской карты можно создать путем создания подкласса [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) класса, как показано в следующем примере кода:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Элемента управления в проекте библиотеки .NET Standard и определяет API для пользовательской карты. Обеспечивает доступ к пользовательской карте `CustomPins` свойство, которое представляет коллекцию `CustomPin` объекты, которые будут отображены элементом управления собственного карты на каждой платформе. `CustomPin` В следующем примере кода показан класс:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Этот класс определяет `CustomPin` как наследующий свойства [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) класса и добавления `Url` свойство.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Использование пользовательской карты

`CustomMap` Элемент управления может ссылаться на языке XAML в проекте библиотеки .NET Standard объявление пространства имен для его расположения и с помощью префикса пространства имен в элементе управления пользовательской карты. В следующем примере кода показан способ `CustomMap` управления могут быть использованы XAML-страницы:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` Префикс пространства имен можно присвоить любое имя. Тем не менее `clr-namespace` и `assembly` значения должны соответствовать данные пользовательской карты. После объявления пространства имен, префикс используется для ссылки на пользовательские карты.

В следующем примере кода показан способ `CustomMap` управления могут быть использованы на странице C#:

```csharp
public class MapPageCS : ContentPage
{
  public MapPageCS ()
  {
    var customMap = new CustomMap {
      MapType = MapType.Street,
      WidthRequest = App.ScreenWidth,
      HeightRequest = App.ScreenHeight
    };
    ...

    Content = customMap;
  }
}
```

`CustomMap` Экземпляр будет использоваться для отображения собственного карты на обеих платформах. Он имеет [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) свойство задает стиль отображения [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), с возможными значениями, определяются в [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) перечисления. Для iOS и Android, ширина и высота карты устанавливается через свойства `App` класса, инициализированный в проекты под конкретные платформы.

Расположение карты и закрепления содержит, инициализируются, как показано в следующем примере кода:

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Инициализация добавляет пользовательский ПИН-код и помещает представление карты с [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) метод, который изменяет положение и масштаб карты, создав [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) из [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) и [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Возможность добавления пользовательского средства отрисовки каждого проект приложения для настройки собственного карты элементов управления.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского средства визуализации для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `MapRenderer` класс, который подготавливает к просмотру пользовательской карты.
1. Переопределить `OnElementChanged` метода, который отображает пользовательской карты и написания логики для ее настройки. Этот метод вызывается, когда создается соответствующий пользовательской карты Xamarin.Forms.
1. Добавить `ExportRenderer` класс пользовательское средство отрисовки, чтобы указать, что он будет использоваться для отрисовки пользовательской карты Xamarin.Forms атрибут. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Это необязательно для предоставления пользовательского средства отрисовки в каждом проекте платформы. Если пользовательское средство отрисовки не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для базового класса элемента управления.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](customized-pin-images/solution-structure.png "Обязанности проекта CustomMap пользовательского модуля подготовки отчетов")

`CustomMap` Элемент управления отрисовывается классами платформой модуля подготовки отчетов, которые являются производными от `MapRenderer` класса для каждой платформы. Это приведет к каждой `CustomMap` устанавливаются готовится к просмотру с помощью элементов управления от платформы, как показано на следующем снимке экрана:

![](customized-pin-images/screenshots.png "CustomMap на каждой платформе")

`MapRenderer` Предоставляемых классами `OnElementChanged` метод, который вызывается при создании пользовательской карты Xamarin.Forms для отображения соответствующего неуправляемого элемента управления. Этот метод принимает `ElementChangedEventArgs` параметра, который содержит `OldElement` и `NewElement` свойства. Эти свойства представляют собой элемент Xamarin.Forms, модуль подготовки отчетов *было* присоединен и элемент Xamarin.Forms, модуль подготовки отчетов *—* присоединенного, соответственно. В примере приложения `OldElement` свойство будет `null` и `NewElement` свойство будет содержать ссылку на `CustomMap` экземпляра.

Переопределенная версия `OnElementChanged` метод в каждом классе платформой модуля подготовки отчетов — это место для выполнения настройки собственного элемента управления. Типизированную ссылку на собственном элементе управления, используемого на платформе можно получить с помощью `Control` свойство. Кроме того, можно получить ссылку на элемент управления Xamarin.Forms, который готовится к просмотру с помощью `Element` свойство.

Необходимо соблюдать осторожность при подписке на обработчики событий в `OnElementChanged` метода, как показано в следующем примере кода:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Собственный элемент управления должен быть настроен и обработчики событий подписаться только в том случае, если новый элемент Xamarin.Forms подключено пользовательское средство отрисовки. Аналогичным образом все обработчики событий, которые были подписаны на должно быть отменена только элемент, модуль подготовки отчетов, к которому присоединена изменения. Этот подход поможет вам создать пользовательское средство отрисовки, не страдает от утечки памяти.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя типа пользовательского элемента управления Xamarin.Forms готовится к просмотру и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматриваются реализации каждого пользовательского средства отрисовки платформой класса.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

Следующих снимках экрана показана карты, до и после настройки:

![](customized-pin-images/map-layout-ios.png "До и после настройки элемента управления карты")

Для операций ввода-вывода вызывается ПИН-код *заметки*, и может быть пользовательский образ или системные ПИН-код для различных цветов. Заметки можно включить вывод *выноски*, какие сведения будут отображаться в ответ на выбор заметки. Отображает выноски `Label` и `Address` свойства `Pin` экземпляре, с необязательным влево и вправо периферийных устройств представления. На снимке экрана выше, представлении слева периферийных устройств является изображение monkey с идет справа аксессуаров представление *сведения* кнопки.

В следующем примере кода показано пользовательское средство отрисовки для платформы iOS.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` Метод выполняет следующие конфигурации [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) экземпляра, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Свойству `GetViewForAnnotation` метод. Этот метод вызывается, когда [расположение заметки становится видимым на карте](#Displaying_the_Annotation)и используется для настройки предварительного заметки для отображения.
- Обработчики событий для `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, и `DidDeselectAnnotationView` зарегистрированных событий. Эти события возникают при пользователь [касается правой аксессуаров в выноски](#Tapping_on_the_Right_Callout_Accessory_View)и когда пользователь [выбирает](#Selecting_the_Annotation) и [отменяет выделение](#Deselecting_the_Annotation) заметки, соответственно. События имеют отменена только в том случае, если элемент модуль подготовки отчетов подключен к изменения.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Отображение заметки

`GetViewForAnnotation` Метод вызывается, когда расположение заметки становится видимым на карте и используется для настройки предварительного заметки для отображения. Заметка состоит из двух частей:

- `MkAnnotation` — включает в себя заголовок, подзаголовок и расположение заметки.
- `MkAnnotationView` — содержит изображение для представления заметки и при необходимости выноски, который отображается при нажатии кнопки заметки.

`GetViewForAnnotation` Метод принимает `IMKAnnotation` , содержащее данных заметок и возвращает `MKAnnotationView` для отображения на карте и показано в следующем примере кода:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Этот метод гарантирует, что заметка будет отображаться в виде пользовательского образа, а не как системные ПИН-код, а это при касании заметки выноски будут отображены, включающий дополнительное содержимое, слева и справа от заголовка заметки и адрес . Это выполняется следующим образом:

1. `GetCustomPin` Метод вызывается для возвращения данных пользовательского ПИН-код для заметки.
1. Для экономии памяти, представление заметки заносится в пул для повторного использования при вызове [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Класс расширяет `MKAnnotationView` класса `Id` и `Url` свойства, которые соответствуют свойствам идентичными в `CustomPin` экземпляра. Новый экземпляр `CustomMKAnnotationView` создается, при условии, что заметки `null`:
  - `CustomMKAnnotationView.Image` Свойству изображения, которое будет представлять заметки на карте.
  - `CustomMKAnnotationView.CalloutOffset` Свойству `CGPoint` , указывающий, что будет по центру выноски выше заметки.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` Свойству изображение monkey, которое будет отображаться слева от заголовка заметки и адрес.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` Свойству *сведения* кнопки, которая будет отображаться в правой части заголовка заметки и адрес.
  - `CustomMKAnnotationView.Id` Свойству `CustomPin.Id` свойство, возвращенное `GetCustomPin` метод. Это позволяет заметку, которая будет идентифицироваться так, чтобы был [выноски можно выполнить дальнейшую настройку](#Selecting_the_Annotation)при необходимости.
  - `CustomMKAnnotationView.Url` Свойству `CustomPin.Url` свойство, возвращенное `GetCustomPin` метод. URL-адрес будет выполнен переход, если пользователь [нажимает кнопку, отображенных в представлении аксессуаров правой выноски](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Свойству `true` выноски отображается при касании заметки.
1. Заметка возвращается для отображения на карте.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>При выборе заметки

Когда пользователь нажимает на заметку, `DidSelectAnnotationView` активируется события, которые в свою очередь выполняет `OnDidSelectAnnotationView` метод:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Этот метод расширяет существующий выноски (содержащий влево и вправо аксессуаров представления), добавляя `UIView` экземпляр, содержащий изображение логотипа Xamarin, при условии, что у выбранного аннотации его `Id` свойства `Xamarin`. Это позволяет выполнять сценарии, где для другие примечания могут отображаться различные выноски. `UIView` Экземпляра будет отображаться по центру сверху существующих выноски.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Коснувшись представление правой аксессуаров выноски

Когда пользователь нажимает на *сведения* кнопки в представлении аксессуаров правой выноски `CalloutAccessoryControlTapped` активируется события, которые в свою очередь выполняет `OnCalloutAccessoryControlTapped` метод:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Этот метод открывает веб-браузер и переходит к адресу, сохраненному в `CustomMKAnnotationView.Url` свойство. Обратите внимание, что адрес был определен при создании `CustomPin` коллекции в проекте библиотеки .NET Standard.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>При снятии флажка заметки

Если заметка отображается и пользователь нажимает на карте, `DidDeselectAnnotationView` активируется события, которые в свою очередь выполняет `OnDidDeselectAnnotationView` метод:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Этот метод гарантирует, что при существующей выноски не выбран, расширенные части выноски (изображение логотипа Xamarin) также прекращается, отображаемых и его ресурсы будут освобождены.

Дополнительные сведения о настройке `MKMapView` см. в разделе [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

Следующих снимках экрана показана карты, до и после настройки:

![](customized-pin-images/map-layout-android.png "До и после настройки элемента управления карты")

В Android вызывается ПИН-код *маркер*, и может быть пользовательский образ или маркер системных цветов. Может показывать маркеры *информационное окно*, какие сведения будут отображаться в ответ на пользователя, коснувшись маркера. В окне сведений отображаются `Label` и `Address` свойства `Pin` экземпляром и можно настроить для включения другого содержимого. Только одно окно сведения могут быть отображены за один раз.

В следующем примере кода показано пользовательское средство отрисовки для платформы Android.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

При условии, что пользовательское средство отрисовки подключен как новый элемент Xamarin.Forms `OnElementChanged` вызовы метода `MapView.GetMapAsync` метод, который возвращает базовый `GoogleMap` , привязанного к представлению. Один раз `GoogleMap` экземпляр доступен `OnMapReady` переопределение будет вызываться. Этот метод регистрирует обработчик событий для `InfoWindowClick` событие, которое возникает при [нажатии информационное окно](#Clicking_on_the_Info_Window)и подписка отменена только в том случае, если элемент модуль подготовки отчетов подключен к изменения. `OnMapReady` Также переопределить вызовы `SetInfoWindowAdapter` метод, чтобы указать, что `CustomMapRenderer` экземпляра класса будет предоставлять методы для настройки окна сведения.

`CustomMapRenderer` Класс реализует `GoogleMap.IInfoWindowAdapter` интерфейс [Настройка окна сведений](#Customizing_the_Info_Window). Этот интерфейс указывает, что должны быть реализованы следующие методы:

- `public Android.Views.View GetInfoWindow(Marker marker)` — Этот метод вызывается для возвращения пользовательских сведений окна для маркера. Если он возвращает `null`, отрисовка окна по умолчанию будет использоваться. Если он возвращает `View`, затем, `View` будут располагаться внутри рамки окна сведений.
- `public Android.Views.View GetInfoContents(Marker marker)` — Этот метод вызывается для возвращения `View` с содержимым окна сведения и будет вызываться только если `GetInfoWindow` возвращает `null`. Если он возвращает `null`, отрисовки содержимого окна сведений по умолчанию будет использоваться.

В примере приложения настроена только содержимое окна сведений и поэтому `GetInfoWindow` возвращает метод `null` Чтобы включить эту возможность.

#### <a name="customizing-the-marker"></a>Настройка маркера

Значок, используемый для представления маркера можно настраивать путем вызова `MarkerOptions.SetIcon` метод. Это можно сделать путем переопределения `CreateMarker` метод, который вызывается для каждого `Pin` добавляются на карту:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Этот метод создает новый `MarkerOption` экземпляра для каждого `Pin` экземпляр. После задания позиции, метку и адрес маркера, задается ее значок `SetIcon` метод. Этот метод принимает `BitmapDescriptor` объект, содержащий данные, необходимые для подготовки к просмотру значок с `BitmapDescriptorFactory` класс, предоставляющий вспомогательные методы для упрощения создания `BitmapDescriptor`.

Дополнительные сведения об использовании `BitmapDescriptorFactory` класс, чтобы настроить маркер см. в разделе [Настройка маркер](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Настройка окна сведений

Когда пользователь нажимает на маркер, `GetInfoContents` метод выполняется, при условии, что `GetInfoWindow` возвращает `null`. В следующем примере кода показан `GetInfoContents` метод:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Этот метод возвращает `View` с содержимым окна сведения. Это выполняется следующим образом:

- Объект `LayoutInflater` извлекается экземпляр. Это используется для создания макета XML-файл в его соответствующий `View`.
- `GetCustomPin` Метод вызывается для возвращения данных пользовательского ПИН-код для окна сведения.
- `XamarinMapInfoWindow` Макета увеличивается, если `CustomPin.Id` свойства равен `Xamarin`. В противном случае `MapInfoWindow` завышенными макета. Это позволяет использовать сценарии, где макетов окон измененными данными могут отображаться различные маркеров.
- `InfoWindowTitle` И `InfoWindowSubtitle` ресурсы извлекаются из увеличенную макета и их `Text` свойств устанавливаются соответствующие данные на `Marker` экземпляра условии, что ресурсы не `null`.
- `View` Возвращается экземпляр для отображения на карте.

> [!NOTE]
> Информационное окно не является активной `View`. Вместо этого будет преобразовать Android `View` в статическом растрового изображения и отображать, как изображения. Это означает, что при информационное окно может отвечать на событие click, он не может отвечать на любые события сенсорного экрана или жестов, и отдельные элементы управления в окне «сведения» не может ответить на свои собственные события щелчка.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Щелкнув в окне «сведения»

Когда пользователь щелкает в окне сведений `InfoWindowClick` активируется события, которые в свою очередь выполняет `OnInfoWindowClick` метод:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Этот метод открывает веб-браузер и переходит к адресу, сохраненному в `Url` из извлеченного `CustomPin` экземпляра для `Marker`. Обратите внимание, что адрес был определен при создании `CustomPin` коллекции в проекте библиотеки .NET Standard.

Дополнительные сведения о настройке `MapView` см. в разделе [Maps API](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Создание пользовательского средства визуализации для универсальной платформы Windows

Следующих снимках экрана показана карты, до и после настройки:

![](customized-pin-images/map-layout-uwp.png "До и после настройки элемента управления карты")

На UWP вызывается ПИН-код *значок схемы*, и может быть пользовательского образа или образа по умолчанию, определенная системой. Значок схемы может показывать `UserControl`, которое отображается в ответ на пользователя, нажав на значок карты. `UserControl` Может отображать любое содержимое, включая `Label` и `Address` свойства `Pin` экземпляра.

В следующем примере кода показано пользовательское средство отрисовки UWP.

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

`OnElementChanged` Метод выполняет следующие операции, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms:

- Он удаляет `MapControl.Children` коллекции для удаления всех существующих элементов интерфейса пользователя из схемы, перед регистрацией обработчик событий для `MapElementClick` события. Это событие возникает, когда пользователь нажимает кнопку или щелкает `MapElement` на `MapControl`и подписка отменена только в том случае, если элемент модуль подготовки отчетов подключен к изменения.
- Каждый ПИН-код в `customPins` коллекция отображается в правильном географическом расположении на карте, следующим образом:
  - Расположение для ПИН-код создается как `Geopoint` экземпляра.
  - Объект `MapIcon` создается экземпляр, представляющий ПИН-код.
  - Изображение, используемое для представления `MapIcon` задается путем присвоения `MapIcon.Image` свойство. Тем не менее изображение значка карты не всегда гарантированно будет показано, как может быть скрыт других элементов на карте. Таким образом, карты значка `CollisionBehaviorDesired` свойству `MapElementCollisionBehavior.RemainVisible`, чтобы убедиться, что оно остается видимым.
  - Расположение `MapIcon` задается путем присвоения `MapIcon.Location` свойство.
  - `MapIcon.NormalizedAnchorPoint` Является свойство Приблизительное расположение указатель мыши на изображение. Если это свойство сохраняет значение по умолчанию (0,0), который представляет в верхний левый угол изображения, изменения масштаба карты может привести к изображения, указывающий другое расположение.
  - `MapIcon` Добавляется экземпляр `MapControl.MapElements` коллекции. Это приведет к значок карты, отображаемая на `MapControl`.

> [!NOTE]
> При использовании одного изображения для несколько значков карты `RandomAccessStreamReference` экземпляр должен быть объявлен на уровне приложения или страницы для достижения оптимальной производительности.

#### <a name="displaying-the-usercontrol"></a>Отображение пользовательского элемента управления

Когда пользователь нажимает на значок карты `OnMapElementClick` выполнения метода. Этот метод показан в следующем примере кода:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Этот метод создает `UserControl` экземпляр, который отображает сведения о ПИН-код. Это выполняется следующим образом:

- `MapIcon` Извлекается экземпляр.
- `GetCustomPin` Метод вызывается для возврата данных пользовательского ПИН-кода, которое будет отображаться.
- Объект `XamarinMapOverlay` создается экземпляр для отображения данных пользовательского ПИН-код. Этот класс является пользовательского элемента управления.
- Географическое расположение, в которой отображается `XamarinMapOverlay` экземпляра `MapControl` создается как `Geopoint` экземпляра.
- `XamarinMapOverlay` Добавляется экземпляр `MapControl.Children` коллекции. Эта коллекция содержит элементы пользовательского интерфейса XAML, которые будут отображаться на карте.
- Географическое расположение `XamarinMapOverlay` экземпляра на карте устанавливается путем вызова `SetLocation` метод.
- Относительное расположение на `XamarinMapOverlay` экземпляром, который соответствует в указанное расположение, устанавливается путем вызова `SetNormalizedAnchorPoint` метод. Это гарантирует, что изменяет уровень масштаба карты результата в `XamarinMapOverlay` экземпляр всегда отображается в правильном расположении.

Кроме того, если сведения о ПИН-код уже отображается на карте, нажав на карте удаляет `XamarinMapOverlay` экземпляра из `MapControl.Children` коллекции.

#### <a name="tapping-on-the-information-button"></a>Нажимая кнопку "Сведения"

Когда пользователь нажимает на *сведения* кнопку в `XamarinMapOverlay` пользовательский элемент управления `Tapped` активируется события, которые в свою очередь выполняет `OnInfoButtonTapped` метод:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Этот метод открывает веб-браузер и переходит к адресу, сохраненному в `Url` свойство `CustomPin` экземпляра. Обратите внимание, что адрес был определен при создании `CustomPin` коллекции в проекте библиотеки .NET Standard.

Дополнительные сведения о настройке `MapControl` см. в разделе [карты и общие сведения о расположении](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) на сайте MSDN.

## <a name="summary"></a>Сводка

В этой статье показано, как создать пользовательское средство отрисовки для `Map` элемента управления, позволяя разработчикам выполнять переопределить свои собственные настройки платформой собственного отрисовки по умолчанию. Xamarin.Forms.Maps предоставляет абстракцию кросс платформенных для отображения карты, используйте собственный API-интерфейсы для каждой платформы, чтобы получить быстрое и знакомые карту возникнуть карту для пользователей.


## <a name="related-links"></a>Связанные ссылки

- [Элемент управления карты](~/xamarin-forms/user-interface/map.md)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
- [API карт](~/android/platform/maps-and-location/maps/maps-api.md)
- [Настраиваемые ПИН-кода (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
