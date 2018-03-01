---
title: "Выделение круговой области на карте"
description: "В этой статье объясняется, как добавление циклические наложения на карту, чтобы выделить круговой области карты."
ms.topic: article
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0eef31c5b9a93154b1038ffa63ee560bd738fe6b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Выделение круговой области на карте

_В этой статье объясняется, как добавление циклические наложения на карту, чтобы выделить круговой области карты._

## <a name="overview"></a>Обзор

Наложение находится многоуровневого график на карте. Перекрытия поддерживает рисование графического содержимого, которая масштабируется с картой, как оно развернуто. Следующих снимках экрана Показать результат сложения циклические наложения к схеме:

![](circle-map-overlay-images/screenshots.png)

Когда [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) элемент управления отрисовывается приложением Xamarin.Forms в iOS `MapRenderer` создается экземпляр класса, который в свою очередь создает собственный `MKMapView` элемента управления. На платформе Android `MapRenderer` класс создает экземпляр собственного `MapView` элемента управления. В универсальной платформы Windows (UWP), `MapRenderer` класс создает экземпляр собственного `MapControl`. Процесс подготовки отчета можно считать преимущества реализации настроек карты платформой путем создания пользовательского средства визуализации для `Map` на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Map) Xamarin.Forms пользовательской карты.
1. [Использовать](#Consuming_the_Custom_Map) пользовательской карты с помощью Xamarin.Forms.
1. [Настройка](#Customizing_the_Map) карты путем создания пользовательского средства отрисовки карты для каждой платформы.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") необходимо инициализировать и настроить перед использованием. Дополнительные сведения см. в разделе [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Сведения о настройке карты с помощью пользовательского средства отрисовки см. в разделе [Настройка ПИН-код карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Создание пользовательской карты

Создание `CustomCircle` классом, имеющим `Position` и `Radius` свойства:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Создайте подкласс [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) класс, который добавляет свойство с типом `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Использование пользовательской карты

Использовать `CustomMap` управления, объявив его экземпляр в экземпляре страницы XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Можно также использовать `CustomMap` управления, объявив его экземпляр в экземпляре страницы C#:

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

Инициализация `CustomMap` управления при необходимости:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

Добавляет эту инициализацию [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) и `CustomCircle` экземпляров для пользовательской карты и помещает представление карты с [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) метод, который изменяет положение и масштаб Уровень сопоставления, создав [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) из [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) и [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Настройка карты

Пользовательское средство отрисовки теперь должны добавляться в каждый проект приложения Добавление карты циклические наложения.

#### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить циклические наложения:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

Этот метод выполняет следующую конфигурацию, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- `MKMapView.OverlayRenderer` Задано значение соответствующего делегата.
- Круг создается путем задания статического `MKCircle` , указывающий центра круга, а радиус окружности в метрах.
- Круг добавляется в схему путем вызова `MKMapView.AddOverlay` метод.

Затем реализуйте `GetOverlayRenderer` метод для настройки обработки наложения:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` и `OnMapReady` методы для добавления циклические наложения:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

`OnElementChanged` Вызовы метода `MapView.GetMapAsync` метод, который возвращает базовый `GoogleMap` привязывается к представлению, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms. Один раз `GoogleMap` экземпляр доступен `OnMapReady` метод будет вызван, где круга создается путем создания экземпляра `CircleOptions` , указывающий центра круга, а радиус окружности в метрах. Круг затем добавляется в схему путем вызова `NativeMap.AddCircle` метод.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Создание пользовательского средства визуализации для универсальной платформы Windows

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить циклические наложения:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }
            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        ...
    }
}
```

Этот метод выполняет следующие операции, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- Положение круг и radius извлекаются из `CustomMap.Circle` свойство и передается `GenerateCircleCoordinates` метод, который приводит к возникновению Широта и долгота координат для периметра окружности.
- Координаты периметра окружности преобразуются в `List` из `BasicGeoposition` координаты.
- Круг создается путем создания экземпляра `MapPolygon` объекта. `MapPolygon` Класс используется для отображения многоточечного фигуры на карте, установив его `Path` свойства `Geopath` , содержащий координаты фигуры.
- К просмотру многоугольника на карте, добавляя его в `MapControl.MapElements` коллекции.

## <a name="summary"></a>Сводка

В этой статье описывается добавление на карту, чтобы выделить круговой области карты циклические наложения.


## <a name="related-links"></a>Связанные ссылки

- [Циклическая Ovlerlay карты (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Настройка ПИН-код карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
