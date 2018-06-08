---
title: Выделение маршрута на карте
description: В этой статье описывается добавление наложения ломаной линии на карту. Наложение ломаной линии — это ряд сегментов линии, которые обычно используются для отображения на карте маршрута или форме любой фигуры, необходимый.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 8b80f9569e9377ca76798911cda64d8c0ee28d93
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846647"
---
# <a name="highlighting-a-route-on-a-map"></a>Выделение маршрута на карте

_В этой статье описывается добавление наложения ломаной линии на карту. Наложение ломаной линии — это ряд сегментов линии, которые обычно используются для отображения на карте маршрута или форме любой фигуры, необходимый._

## <a name="overview"></a>Обзор

Наложение находится многоуровневого график на карте. Перекрытия поддерживает рисование графического содержимого, которая масштабируется с картой, как оно развернуто. Следующих снимках экрана показано результат сложения наложения ломаной линии на карту.

![](polyline-map-overlay-images/screenshots.png)

Когда [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) элемент управления отрисовывается приложением Xamarin.Forms в iOS `MapRenderer` создается экземпляр класса, который в свою очередь создает собственный `MKMapView` элемента управления. На платформе Android `MapRenderer` класс создает экземпляр собственного `MapView` элемента управления. В универсальной платформы Windows (UWP), `MapRenderer` класс создает экземпляр собственного `MapControl`. Процесс подготовки отчета можно считать преимущества реализации настроек карты платформой путем создания пользовательского средства визуализации для `Map` на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Map) Xamarin.Forms пользовательской карты.
1. [Использовать](#Consuming_the_Custom_Map) пользовательской карты с помощью Xamarin.Forms.
1. [Настройка](#Customizing_the_Map) карты путем создания пользовательского средства отрисовки карты для каждой платформы.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) необходимо инициализировать и настроить перед использованием. Дополнительные сведения см. на веб-сайте [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Сведения о настройке карты с помощью пользовательского средства отрисовки см. в разделе [Настройка ПИН-код карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Создание пользовательской карты

Создать подкласс [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) класс, который добавляет `RouteCoordinates` свойство:

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates` Свойство будет сохранять коллекцию координаты, определяющие изменяемый маршрут, который будет выделен.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Инициализация задает набор координат широты и долготы, для определения маршрута на карте, который будет выделен. Позиционирует представление карты с [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) метод, который изменяет положение и масштаб карты, создав [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) из [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) и [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Настройка карты

Пользовательское средство отрисовки теперь должны добавляться в каждый проект приложения для добавления наложения ломаной линии на карту.

#### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить наложения ломаной линии:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Этот метод выполняет следующую конфигурацию, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- `MKMapView.OverlayRenderer` Задано значение соответствующего делегата.
- Коллекция координаты широты и долготы извлекаются из `CustomMap.RouteCoordinates` свойство и хранится в виде массива `CLLocationCoordinate2D` экземпляров.
- Ломаной линии создается путем вызова статического `MKPolyline.FromCoordinates` метод, который указывает широту и долготу каждой точки.
- Ломаной линии добавляется в схему путем вызова `MKMapView.AddOverlay` метод.

Затем реализуйте `GetOverlayRenderer` метод для настройки обработки наложения:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` и `OnMapReady` методы для добавления в оверлей ломаной линии:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged` Метод получает коллекцию координаты широты и долготы из `CustomMap.RouteCoordinates` свойства и сохраняет их в переменной-члена. Затем он вызывает `MapView.GetMapAsync` метод, который возвращает базовый `GoogleMap` привязывается к представлению, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms. Один раз `GoogleMap` экземпляр доступен `OnMapReady` метод будет вызван, где ломаной линии создается путем создания экземпляра `PolylineOptions` объекта, указывающее широту и долготу каждой точки. Ломаной линии затем добавляется в схему путем вызова `NativeMap.AddPolyline` метод.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Создание пользовательского средства визуализации для универсальной платформы Windows

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить наложения ломаной линии:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
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

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Этот метод выполняет следующие операции, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- Коллекция координаты широты и долготы извлекаются из `CustomMap.RouteCoordinates` свойство и он преобразуется в `List` из `BasicGeoposition` координаты.
- Ломаной линии создается путем создания экземпляра `MapPolyline` объекта. `MapPolygon` Класс используется для отображения линии на карте, установив его `Path` свойства `Geopath` , содержащий координаты строки.
- К просмотру многоугольника на карте, добавляя его в `MapControl.MapElements` коллекции.

## <a name="summary"></a>Сводка

В этой статье описывается добавление наложения ломаной линии на карту для отображения на карте маршрута или форме любой фигуры, необходимый.


## <a name="related-links"></a>Связанные ссылки

- [Ovlerlay ломаной линии карты (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Настройка закрепления карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
