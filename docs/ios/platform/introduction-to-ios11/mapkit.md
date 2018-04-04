---
title: Новые возможности в MapKit на iOS 11
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 3b1ac8015a86292f00f26133277ead2f211dca33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Новые возможности в MapKit на iOS 11

iOS 11 MapKit добавляет следующие новые функции:

- [Заметка кластеризации](#clustering)
- [Компас кнопки](#compass)
- [Масштаб представления](#scale)
- [Кнопка отслеживания пользователя](#user-tracking)

![Карта, отображающая кластеризованный маркеры и компас кнопки](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Автоматическое группирование маркеры при сохранении масштабирования

Образец [MapKit образца «Tandm»](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) показано, как реализовать новые заметки iOS 11 кластеров.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Создание `MKPointAnnotation` подкласс

Класс заметки точка представляет каждый маркер на карте. Они могут быть добавлены по отдельности с помощью `MapView.AddAnnotation()` или из массива, используя `MapView.AddAnnotations()`.

Классы точки заметки не имеют визуальное представление, они требуются только для представления данных, связанного с маркером (самое главное, `Coordinate` свойство, которое является его широту и долготу на карте) и любые пользовательские свойства:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Создание `MKMarkerAnnotationView` подкласс одного маркеров

Представление маркера заметки — визуальное представление каждого примечания и реализуется с помощью свойств, например:

- **MarkerTintColor** — цвет маркера.
- **GlyphText** — отображение текста в маркере.
- **GlyphImage** — задает изображение, отображаемое в маркере.
- **DisplayPriority** — определяет z порядке (наложения поведение) при переполнении карты с маркерами. Используйте один из `Required`, `DefaultHigh`, или `DefaultLow`.

Чтобы обеспечить поддержку автоматического кластеризации, необходимо также задать:

- **ClusteringIdentifier** — определяет, какие маркеры получить компактными группами. Можно использовать тот же идентификатор для всех маркеров или использовать разные идентификаторы для управления способом, группируются вместе.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Создать `MKAnnotationView` для представления кластеров маркеров

При представлении заметки, который представляет собой кластер маркеры _удалось_ быть простой образ, пользователи ожидают, что приложению предоставлять визуальные подсказки сколько маркеры были сгруппированы вместе.

[Пример кода](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) использует CoreGraphics число маркеров в кластере, а также представление графа круг долю каждого типа маркера.

Также следует задать:

- **DisplayPriority** — определяет z порядке (наложения поведение) при переполнении карты с маркерами. Используйте один из `Required`, `DefaultHigh`, или `DefaultLow`.
- **CollisionMode** — `Circle` или `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Регистрация классов представления

Когда создается представление элемента управления карты и добавленными к представлению, регистрация типы представлений заметки включить автоматическое поведение кластеризации, как карты развернуто и выхода из системы:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Визуализации карты!

При отображении карты заметки маркеры кластеризованный или к просмотру в зависимости от масштаба. При изменении масштаба маркеров анимирование кластеры и из них.

![Симулятор, показывающая кластеризованный маркеров на карте](mapkit-images/cyclemap-sml.png)

Ссылаться на [сопоставляет раздел](~/ios/user-interface/controls/ios-maps/index.md) Дополнительные сведения по отображению данных с MapKit.

<a name="compass" />

## <a name="compass-button"></a>Компас кнопки

iOS 11 дает возможность извлечь компас из карты и вывести ее в другом месте в представлении. В разделе [Tandm пример приложения](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) в качестве примера.

Кнопка «Создать», который выглядит как компаса (включая динамической анимации при изменении ориентации карты), и отображает его на другой элемент управления.

![Компаса кнопки в панели навигации](mapkit-images/compass-sml.png)

В следующем примере кода создается компаса кнопка и отображает его на панели навигации:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Свойство можно использовать для управления видимостью компас по умолчанию в представлении карты.

<a name="scale" />

## <a name="scale-view"></a>Масштаб представления

Добавить масштаб в любом другом месте представления с помощью `MKScaleView.FromMapView()` метод для получения экземпляра масштабированного представления для добавления в другом месте в иерархии представления.

![Масштаб представления накладывается на карте](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Свойство можно использовать для управления видимостью компас по умолчанию в представлении карты.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Кнопка отслеживания пользователя

Кнопка отслеживания пользователя на карте отображается текущее расположение пользователя. Используйте `MKUserTrackingButton.FromMapView()` метод, чтобы получить экземпляр кнопки, примените изменения форматирования и добавить в другом месте в иерархии представления.

![Расположение кнопки пользователя, накладывается на карте](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>Связанные ссылки

- [Образец MapKit «Tandm»](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Что такое New в MapKit (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/237/)
