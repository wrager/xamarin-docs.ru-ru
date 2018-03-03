---
title: "Выделение области на карте"
description: "В этой статье описывается добавление наложения многоугольника на карту, чтобы выделить области на карте. Многоугольники являются замкнутую фигуру и внутренние заполнено."
ms.topic: article
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 6c116565842537f24d92a6d100ab1636f25c2e12
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-region-on-a-map"></a>Выделение области на карте

_В этой статье описывается добавление наложения многоугольника на карту, чтобы выделить области на карте. Многоугольники являются замкнутую фигуру и внутренние заполнено._

## <a name="overview"></a>Обзор

Наложение находится многоуровневого график на карте. Перекрытия поддерживает рисование графического содержимого, которая масштабируется с картой, как оно развернуто. Следующих снимках экрана показано результат сложения наложения многоугольника на карту.

![](polygon-map-overlay-images/screenshots.png)

Когда [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) элемент управления отрисовывается приложением Xamarin.Forms в iOS `MapRenderer` создается экземпляр класса, который в свою очередь создает собственный `MKMapView` элемента управления. На платформе Android `MapRenderer` класс создает экземпляр собственного `MapView` элемента управления. В универсальной платформы Windows (UWP), `MapRenderer` класс создает экземпляр собственного `MapControl`. Процесс подготовки отчета можно считать преимущества реализации настроек карты платформой путем создания пользовательского средства визуализации для `Map` на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Map) Xamarin.Forms пользовательской карты.
1. [Использовать](#Consuming_the_Custom_Map) пользовательской карты с помощью Xamarin.Forms.
1. [Настройка](#Customizing_the_Map) карты путем создания пользовательского средства отрисовки карты для каждой платформы.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) необходимо инициализировать и настроить перед использованием. Дополнительные сведения см. в разделе [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

Сведения о настройке карты с помощью пользовательского средства отрисовки см. в разделе [Настройка ПИН-код карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Создание пользовательской карты

Создать подкласс [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) класс, который добавляет `ShapeCoordinates` свойство:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` Свойство будет сохранять коллекцию координаты, определяющие области, который будет выделен.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Инициализация задает набор координат широты и долготы, для определения области карты, который будет выделен. Позиционирует представление карты с [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) метод, который изменяет положение и масштаб карты, создав [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) из [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) и [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Настройка карты

Пользовательское средство отрисовки теперь должны добавляться в каждый проект приложения, чтобы добавить слой многоугольников карты.

#### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить слой многоугольников:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Этот метод выполняет следующую конфигурацию, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- `MKMapView.OverlayRenderer` Задано значение соответствующего делегата.
- Коллекция координаты широты и долготы извлекаются из `CustomMap.ShapeCoordinates` свойство и хранится в виде массива `CLLocationCoordinate2D` экземпляров.
- Многоугольника создается путем вызова статического `MKPolygon.FromCoordinates` метод, который указывает широту и долготу каждой точки.
- Добавляются многоугольника на карту путем вызова `MKMapView.AddOverlay` метод. Этот метод автоматически закрывает многоугольника путем рисования линии, соединяющей начальную и конечную точки.

Затем реализуйте `GetOverlayRenderer` метод для настройки обработки наложения:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` и `OnMapReady` методы для добавления дополнительного значка «многоугольник»:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` Метод получает коллекцию координаты широты и долготы из `CustomMap.ShapeCoordinates` свойства и сохраняет их в переменной-члена. Затем он вызывает `MapView.GetMapAsync` метод, который возвращает базовый `GoogleMap` привязывается к представлению, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms. Один раз `GoogleMap` экземпляр доступен `OnMapReady` метод будет вызван, где многоугольника создается путем создания экземпляра `PolygonOptions` объекта, указывающее широту и долготу каждой точки. Многоугольника затем добавляется в схему путем вызова `NativeMap.AddPolygon` метод. Этот метод автоматически закрывает многоугольника путем рисования линии, соединяющей начальную и конечную точки.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Создание пользовательского средства визуализации для универсальной платформы Windows

Создать подкласс `MapRenderer` класса и переопределить его `OnElementChanged` метод, чтобы добавить слой многоугольников:

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Этот метод выполняет следующие операции, при условии, что пользовательское средство отрисовки присоединяется новый элемент Xamarin.Forms.

- Коллекция координаты широты и долготы извлекаются из `CustomMap.ShapeCoordinates` свойство и он преобразуется в `List` из `BasicGeoposition` координаты.
- Многоугольника создается путем создания экземпляра `MapPolygon` объекта. `MapPolygon` Класс используется для отображения многоточечного фигуры на карте, установив его `Path` свойства `Geopath` , содержащий координаты фигуры.
- К просмотру многоугольника на карте, добавляя его в `MapControl.MapElements` коллекции. Обратите внимание, автоматически закрывается многоугольника путем рисования линии, соединяющей начальную и конечную точки.

## <a name="summary"></a>Сводка

В этой статье описывается добавление наложения многоугольника на карту, чтобы выделить области карты. Многоугольники являются замкнутую фигуру и внутренние заполнено.


## <a name="related-links"></a>Связанные ссылки

- [Многоугольника карты наложения (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Настройка закрепления карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
