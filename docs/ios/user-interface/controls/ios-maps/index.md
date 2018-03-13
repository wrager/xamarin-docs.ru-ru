---
title: "Карты"
description: "операций ввода-вывода включает MapKit, сопоставляет платформу, которая позволяет легко добавить в приложение. С помощью карты пакета приложений iOS можно добавить интерактивные карты, поддерживающих функции например панорамирования и масштабирования, показывающий расположение пользователя и графических слоев на карте. В этой статье подробно некоторые функции карты Kit, показывающий, как воспользоваться ими для сборки географической возможностей в приложение"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 540a459be24296c8446c2136773ddde59f9d4dd7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="maps"></a>Карты

_операций ввода-вывода включает MapKit, сопоставляет платформу, которая позволяет легко добавить в приложение. С помощью карты пакета приложений iOS можно добавить интерактивные карты, поддерживающих функции например панорамирования и масштабирования, показывающий расположение пользователя и графических слоев на карте. В этой статье подробно некоторые функции карты Kit, показывающий, как воспользоваться ими для сборки географической возможностей в приложение_

Карты — это стандартная функция всех современных мобильных операционных систем. iOS обеспечивает поддержку сопоставления изначально на платформе комплект средств для карты. С помощью комплекта карты приложения можно легко добавить широкие, интерактивных карты. Эти сопоставления можно настраивать различными способами, например добавление заметок для пометки расположений на карту и наложенной графики произвольных фигур. Комплект средств для карты даже имеет встроенную поддержку для отображения текущего расположения устройства.

## <a name="adding-a-map"></a>Добавление карты

Добавление карты к приложения выполняется путем добавления `MKMapView` экземпляра, на просмотр иерархии, как показано ниже:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` — `UIView` подкласс, в котором отображается карта. Путем добавления карты с помощью приведенного выше кода выводятся интерактивной карты:

 ![](images/00-map.png "Пример карты")

## <a name="map-style"></a>Стиль карты

 `MKMapView` поддерживает 3 различных стилей карт. Чтобы применить стиль карты, просто установите `MapType` значение из свойства `MKMapType` перечисления:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Следующий снимок экрана Показать доступные стили другой карте:

 ![](images/01-mapstyles.png "На этом снимке экрана Показать доступные стили другой карте")

## <a name="panning-and-zooming"></a>Панорамирования и масштабирования

 `MKMapView` включает поддержку карты интерактивные функции, такие как:

-  Изменение масштаба через жест жестом сжатия
-  Панорамирование через жест сдвиг


Эти функции можно включить или отключить, просто задав `ZoomEnabled` и `ScrollEnabled` свойства `MKMapView` экземпляра, где значение по умолчанию имеет значение true, если для обоих. Например отображать статические карту, просто установите соответствующие свойства значение false:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Расположение пользователя

Помимо взаимодействие с пользователем `MKMapView` также имеет встроенную поддержку для отображения расположения устройства. Это делается с помощью *расположение основных* framework. Чтобы получить доступ к расположению пользователя, необходимо запрашивать у пользователя. Чтобы сделать это, создайте экземпляр `CLLocationManager` и вызвать `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Обратите внимание, что в версиях iOS до 8.0, попытка вызова `RequestWhenInUseAuthorization` приведет к ошибке. Не забудьте проверить версию iOS до выполнения этого вызова, если планируется поддержка версий до 8.

Доступ к расположению пользователя также требует модификации **Info.plist**. Необходимо задать следующие ключи, связанные с данными о расположении:

- **NSLocationWhenInUseUsageDescription** для доступа к расположению пользователя во время его взаимодействия с приложением.
- **NSLocationAlwaysUsageDescription** для доступа приложения к расположению пользователя в фоновом режиме.

Можно добавить эти ключи, открыв **Info.plist** и выбрав *источника* в нижней части редактора.

После обновления **Info.plist** и запрашивает пользователя о разрешения на доступ к их расположение, можно показать расположение пользователя на карте, задав `ShowsUserLocation` значение true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Предупреждение о доступе к разрешить расположение")
 
## <a name="annotations"></a>Заметки

 `MKMapView` также поддерживает отображение изображения, известные как примечания на карте. Это могут быть пользовательских образов или системные ПИН-коды различных цветов. Например, на следующем рисунке показан карты с обоих ПИН-кода и пользовательского образа:

 ![](images/03-annotations.png "На этом снимке экрана показано сопоставление с обоих ПИН-код и пользовательский образ")

### <a name="adding-an-annotation"></a>Добавление заметки

Заметки сам состоит из двух частей:

-  `MKAnnotation` Объекта, включая модели данных о заметки, например названия и расположение заметки.
-  `MKAnnotationView` , Который содержит изображение, отображаемое и при необходимости выноски, который отображается при нажатии кнопки заметки.


Комплект средств для карты использует шаблон делегирования операций ввода-вывода для добавления заметок к схеме, где `Delegate` свойство `MKMapView` устанавливается с экземпляром `MKMapViewDelegate`. Реализация этот делегат, который отвечает за возврат `MKAnnotationView` для заметки.

Чтобы добавить заметку, сначала заметки добавляется путем вызова `AddAnnotations` на `MKMapView` экземпляр:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Когда расположение заметки становится видимым на карте, `MKMapView` будет вызывать его делегата `GetViewForAnnotation` метода `MKAnnotationView` для отображения.

Например, следующий код возвращает предоставляемой системой `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Повторное использование заметок

Для экономии памяти, `MKMapView` позволяет представления заметки, помещаемых в пул для повторного использования, аналогично тому, как ячейки таблицы используются повторно. Получение представления заметки из пула выполняется с помощью вызова `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Отображение выноски

Как упоминалось ранее, заметки можно включить вывод выноски. Чтобы отобразить выноску, просто установите `CanShowCallout` значение true в `MKAnnotationView`. Это приводит к заголовок заметки, отображаются при касании заметка, как показано:

 ![](images/04-callout.png "Отображение заголовка заметки")

### <a name="customizing-the-callout"></a>Настройка выноски

Также можно настроить выноски представления в левой и правой периферийных устройств, как показано ниже:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Этот код вызывает следующие выноски.

 ![](images/05-callout-accessories.png "Пример выноски")

Для обработки пользователя, коснувшись правой стандартную программу, реализовать `CalloutAccessoryControlTapped` метод в `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Перекрытия

Другой способ слой графики на карте использует перекрытия. Перекрытия поддерживает рисование графического содержимого, которая масштабируется с картой, как оно развернуто. операций ввода-вывода поддерживает несколько типов наложений, включая:

-  Многоугольники - обычно используется для выделения некоторые области на карте.
-  Многоугольника - часто возникает при отображении маршрут.
-  Круги - используется для выделения круговой области карты.


Кроме того можно создать специальные наложения для отображения произвольных геометрических объектов с помощью детального, специализированных код прорисовки. Например лепестковой погоде будет хорошим кандидатом для пользовательских наложения.

#### <a name="adding-an-overlay"></a>Добавление наложение

Как и заметки, добавление наложение включает 2 частей:

-  Создание модели объекта для наложения и его добавления в `MKMapView` .
-  Создание представления для наложения в `MKMapViewDelegate` .


Модель для наложения может быть любым `MKShape` подкласс. Включает Xamarin.iOS `MKShape` подклассов многоугольников, ломаных линий и круги, через `MKPolygon`, `MKPolyline` и `MKCircle` классы соответственно.

Например, следующий код используется для добавления `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Это представление для наложения `MKOverlayView` экземпляр, возвращаемый `GetViewForOverlay` в `MKMapViewDelegate`. Каждый `MKShape` имеет соответствующий `MKOverlayView` , которому известны как отобразить заданной фигуры. Для `MKPolygon` есть `MKPolygonView`. Аналогичным образом `MKPolyline` соответствует `MKPolylineView`, а также для `MKCircle` есть `MKCircleView`.

Например, в следующем коде возвращается `MKCircleView` для `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Круг выводятся на карте, как показано.

 ![](images/06-circle-overlay.png "Круг, отображаемых на карте")

## <a name="local-search"></a>Сайт локального поиска

операций ввода-вывода включает локального поиска API с комплекта карты, позволяющий асинхронную выполняет поиск точки в указанном географическом регионе.

Для выполнения поиска с локального приложения необходимо выполнить следующие действия:

1.  Создание `MKLocalSearchRequest` объекта.
1.  Создание `MKLocalSearch` объекта из `MKLocalSearchRequest` .
1.  Вызовите `Start` метод `MKLocalSearch` объекта.
1.  Получить `MKLocalSearchResponse` объекта с помощью обратного вызова.


Локальный поиск сам интерфейс API не располагает интерфейсом пользователя. Он даже не требует карты для использования. Тем не менее чтобы сделать Практическое применение локального поиска, приложению требуется предоставляют способ указания запрос поиска и отображения результатов. Кроме того поскольку результаты, содержащие данные о местоположении, он будет часто смысл отобразить их на карте.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Добавление локального поиска пользовательского интерфейса

Примите условия поиска можно выполнить с `UISearchBar`, что предоставляемые `UISearchController` и отображает результаты в таблице.

Следующий код добавляет `UISearchController` (имеет свойство панель поиска) в `ViewDidLoad` метод `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>Обновление результатов поиска

`SearchResultsUpdater` Действует как посредник между `searchController`в строке поиска и результаты поиска. 

В этом примере мы сначала необходимо создать метод поиска в `SearchResultsViewController`. Для этого необходимо создать `MKLocalSearch` объекта и его использования для поиска `MKLocalSearchRequest`, результаты возвращаются в обратный вызов, переданный `Start` метод `MKLocalSearch` объекта. Затем результаты возвращаются в `MKLocalSearchResponse` объект, содержащий массив `MKMapItem` объектов:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

Затем в наших `MapViewController` мы создадим пользовательскую реализацию `UISearchResultsUpdating`, которая назначается `SearchResultsUpdater` свойство нашей `searchController` в [Добавление пользовательского интерфейса локального поиска](#Adding_a_Local_Search_UI) раздела:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Выше реализации добавляет заметки к схеме при выборе элемента из результатов, как показано ниже:

 ![](images/08-search-results.png "Заметки добавляются на карту при выборе элемента из результатов")
 
 > [!IMPORTANT]
> **Примечание** `UISearchController` была реализована в iOS 8. Если вы хотите поддерживать более ранних, чем это устройства, то необходимо будет использовать `UISearchDisplayController`.



## <a name="summary"></a>Сводка

В этой статье проверить *карты* *Kit* платформа для iOS. Во-первых, хотя рассматривались как `MKMapView` класс позволяет интерактивной карты должны быть включены в приложение. Затем показано, как настроить сопоставления с помощью заметок и наложения. Наконец, он проверяет возможности локального поиска, которые были добавлены к пакету карты с iOS 6.1, описывает использование выполняют запросы на основе расположения интересные и добавить их на карту.

## <a name="related-links"></a>Связанные ссылки

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (пример)](https://developer.xamarin.com/samples/monotouch/MapDemo)
