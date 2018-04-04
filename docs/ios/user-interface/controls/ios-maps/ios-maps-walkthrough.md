---
title: Заметки и наложения
description: Эта статья содержит пошаговое руководство, показывающий, как пользоваться возможностями пометку и наложение комплект средств для карты. В этом примере показано добавление карты для приложения, который отображает пометку и наложение в расположении конференции 2013 развивать Xamarin.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9defbade6fafefb26d87e88665c491b3a559c1ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Заметки и наложения — Пошаговое руководство

Приложение, которое мы собираемся в данном пошаговом руководстве показан ниже:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Пример приложения MapKit")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
Можно найти полный код в **MapsWalkthroughComplete** папку внутри [карты демонстрационный пример](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Давайте начнем с создания нового **iOS пустой проект**и присвоить ей соответствующие имя. Мы начнем путем добавления кода в нашей контроллера представления для отображения MapView, а затем создать новые классы для наших MapDelegate и пользовательские заметки. Для этого выполните следующие действия:

## <a name="viewcontroller"></a>ViewController


1. Добавьте следующие пространства имен для `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Добавить `MKMapView` переменную в класс экземпляра вместе с `MapDelegate` экземпляра. Мы создадим `MapDelegate` вскоре:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. На контроллере `LoadView` метод, добавьте `MKMapView` и присвойте ему значение `View` свойства контроллера:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Далее мы добавим код для инициализации карты в "ViewDidLoad'' метод.

1. В `ViewDidLoad` добавьте код, чтобы задать тип карты конкретное расположение пользователя и разрешить изменение масштаба и сдвиг:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Добавьте код для центрирования карты и задать его область.

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Создать новый экземпляр `MapDelegate` и назначьте его `Delegate` из `MKMapView`. Опять же, мы будем implcodeent `MapDelegate` вскоре:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. Начиная с версии iOS 8 следует запросе авторизации пользователя, чтобы использовать их расположения, так что давайте добавьте это в нашем примере. Во-первых, определите `CLLocationManager` переменной уровня класса:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. В `ViewDidLoad` метод, мы хотим проверить, если устройство, работающим с приложением iOS 8, а также их в случае мы будет запрашивать авторизацию, при использовании приложения:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Наконец, нужно изменить **Info.plist** файл, чтобы уведомить пользователей причину запроса их расположение. В **источника** меню **Info.plist**, добавьте следующий раздел:
    
    `NSLocationWhenInUseUsageDescription` 
    
    и строка: 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs-класс для пользовательских примечаний


1. Мы будем использовать настраиваемый класс для заметки вызывается `ConferenceAnnotation`. Добавьте следующий класс в проект:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - Добавление пометку и наложение

1. С `ConferenceAnnotation` на месте можно добавить его к схеме. В `ViewDidLoad` метод `ViewController`, добавить заметку координату центра карты:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Нам также нужно иметь наложение гостиницы. Добавьте следующий код для создания `MKPolygon` с использованием координат для предоставленных гостиницы и добавьте его к схеме, вызов `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Это завершает код в `ViewDidLoad`. Теперь нам нужно реализовать наш `MapDelegate` класса для обработки Создание заметки и наложения представления соответственно.


## <a name="mapdelegate"></a>MapDelegate

1. Создайте класс с именем `MapDelegate` , наследуемый от `MKMapViewDelegate` и включать `annotationId` переменной для использования в качестве идентификатора повторного использования для заметки:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Мы иметь только одну аннотацию здесь повторное использование кода, не является строго обязательным, но рекомендуется включить его.

1. Реализуйте `GetViewForAnnotation` метод, чтобы получить о `ConferenceAnnotation` с помощью **conference.png** изображений, включенных в этом пошаговом руководстве:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Когда пользователь нажимает на заметку, мы хотим отображать изображение Город Остине. Добавьте следующие переменные для `MapDelegate` для образа и его для просмотра:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Далее, чтобы изображение при касании заметки, реализовать `DidSelectAnnotation` метод следующим образом:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Чтобы скрыть изображение, когда пользователь отменяет выделение заметки, коснувшись в любом другом месте на карте, реализовать `DidSelectAnnotationView` метод следующим образом:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Теперь у нас есть код для заметки на месте. Осталось является добавление кода для `MapDelegate` можно создать представление для наложения гостиницы.

1. Добавьте следующие реализацию `GetViewForOverlay` для `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Запустите приложение. Теперь у нас есть интерактивной карты с пользовательской заметкой и наложение! Коснитесь заметку, а изображение Остине отображается, как показано ниже:

 [![](ios-maps-walkthrough-images/01-map-image.png "Коснитесь заметку, а изображение Остине")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели способ добавления заметки к схеме, а также способы добавления наложение для заданного многоугольника. Мы также было показано, как добавить заметку для анимации изображения по карте поддержка сенсорного ввода.


## <a name="related-links"></a>Связанные ссылки

- [Демонстрация карты (пример)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md)
