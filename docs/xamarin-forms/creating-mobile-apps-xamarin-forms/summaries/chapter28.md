---
title: "Сводка Глава 28. Расположение и карты"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: F6E20077-687C-45C4-A375-31D4F49BBFA4
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d7a75ce0303030d53315b5e698fc604a910c969c
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="summary-of-chapter-28-location-and-maps"></a>Сводка Глава 28. Расположение и карты

Поддерживает Xamarin.Forms [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) элемент, производный от `View`. Из-за специальные требования к платформе связанные с использованием карт, они реализованы в отдельную сборку, **Xamarin.Forms.Maps**и включают в себя другое пространство имен: `Xamarin.Forms.Maps`.

## <a name="the-geographic-coordinate-system"></a>Используется географическая система координат

Географической системы координат определяет положение на объект сферическую (или почти сферической) как Земли. Координаты состоит из двух *широты* и *долготы* выраженное в углов.

Вызывается круг высококачественных `equator` находится ровно посредине между двумя полюса, через которые логически расширяет поверхности Земли в направлении оси.

### <a name="parallels-and-latitude"></a>Parallels и широта

Угол измеряемый северу или югу от экватора от центра Земли метки строк равно широты, известный как *parallels*. Эти диапазон на экватора от 0 до 90 градусов в Северной и южно полюса. По соглашению latitudes севернее экватора будут положительных и югу от экватора они отрицательные значения.

### <a name="longitude-and-meridians"></a>Долгота и меридианов

Половины круги отлично от северного к южному полюсу строк равно долготы также называются *меридианов*. Это относительно меридиана в по Гринвичу, Англия. По соглашению долгот к востоку от главного меридиана положительные значения от 0 до 180 градусов, и долгот западу от главного меридиана это отрицательные значения от 0 к &ndash;180 градусов.

### <a name="the-equirectangular-projection"></a>Равнопромежуточная проекция

Любой карты плоской земли вводит искажения. Если все строки широты и долготы являются прямыми, и если равны различия в широты и долготы углы соответствуют одинаковые расстояния на карте, то результатом является *равнопромежуточная проекция*. На этой карте искажает областей ближе к полюсам, так как по горизонтали растягивается.

### <a name="the-mercator-projection"></a>Проекция Меркатора

Популярный *проекцию Меркатора* пытается компенсировать горизонтальной Растяжение, распределяя также этих областей по вертикали. Это приводит к области со сходной полюса расположения значительно больше, чем это действительно, но любой локальной сети достаточно близко соответствует фактическая область карты.

### <a name="map-services-and-tiles"></a>Службы карты и плитки

Карта службы используют разновидность проекцию Меркатора вызывается `Web Mercator`. Службы карты доставлять растрового изображения плиток клиента на основе расположения и масштаба.

## <a name="getting-the-users-location"></a>Получение расположения пользователя

Xamarin.Forms `Map` классы не включают средства для получения географического расположения пользователя, но это желательно часто при работе с картами, поэтому службы зависимостей необходимо его обработать.

### <a name="the-location-tracker-api"></a>Расположение отслеживания API

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) решение содержит код для отслеживания расположения API. [ `GeographicLocation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/GeographicLocation.cs) Структура инкапсулирует широты и долготы. [ `ILocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/ILocationTracker.cs) Интерфейс определяет два метода для запуска и приостановка отслеживания расположения и событие, если доступно новое расположение.

#### <a name="the-ios-location-manager"></a>Диспетчер расположение iOS

Реализация iOS `ILocationTracker` — [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/LocationTracker.cs) класс, который делает использование iOS [ `CLLocationManager` ](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/).

#### <a name="the-android-location-manager"></a>Диспетчер Android расположение

Реализация Android `ILocationTracker` — [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/LocationTracker.cs) класс, который использует Android [ `LocationManager` ](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/) класса.

#### <a name="the-windows-runtime-geo-locator"></a>Локатор geo среды выполнения Windows

Реализация среды выполнения Windows `ILocationTracker` — [ `LocationTracker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/LocationTracker.cs) класс, который используется в UWP [ `Geolocator` ](https://msdn.microsoft.com/library/windows/apps/br225534).

### <a name="display-the-phones-location"></a>Отобразить расположение телефона

[ **WhereAmI** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/WhereAmI) образец использует средство отслеживания расположения для отображения расположения телефона в тексте и на карте equirectangular.

### <a name="the-required-overhead"></a>Требуется издержки

Является обязательным для некоторого объема служебных данных **WhereAmI** по использованию регистрации расположение. Во-первых, все проекты в **WhereAmI** решения должны иметь ссылки на соответствующие проекты в **Xamarin.FormsBook.Platform**и каждый **WhereAmI** проекта необходимо вызвать метод `Toolkit.Init` метод.

Некоторые дополнительные издержки от платформы, в форме разрешения расположения является обязательным.

#### <a name="location-permission-for-ios"></a>Расположение разрешение для iOS

Для операций ввода-вывода **info.plist** файл должен содержать элементы, содержащие текст вопроса запрос у пользователя, чтобы разрешить получение расположения этого пользователя.

#### <a name="location-permissions-for-android"></a>Разрешения расположения для Android

Android приложений, получающих расположения пользователя должен иметь разрешения на ACCESS_FILE_LOCATION в файле AndroidManifest.xml.

#### <a name="location-permissions-for-the-windows-runtime"></a>Разрешения расположения для среды выполнения Windows

Приложение Windows или Windows Phone должен иметь `location` помечены возможность устройства в файл Package.appxmanifest.

## <a name="working-with-xamarinformsmaps"></a>Работа с Xamarin.Forms.Maps

В процессе с помощью участвуют несколько требований к `Map` класса.

### <a name="the-nuget-package"></a>Пакет NuGet

**Xamarin.Forms.Maps** NuGet библиотеку необходимо добавить в решение приложения. Номер версии должен быть таким же, как **Xamarin.Forms** в настоящее время установлен пакет.

### <a name="initializing-the-maps-package"></a>Инициализация пакета карты

Проекты приложений необходимо вызвать `Xamarin.FormsMaps.Init` метод после вызова `Xamarin.Forms.Forms.Init`.

### <a name="enabling-map-services"></a>Включение служб карты

Поскольку `Map` можно получить расположения пользователя, приложение должно получить разрешения для пользователя, как было описано выше в разделе:

#### <a name="enabling-ios-maps"></a>Включение iOS сопоставляет

Приложения iOS с помощью `Map` требуется две строки в файле info.plist.

#### <a name="enabling-android-maps"></a>Включение Android сопоставляет

Для использования служб Google карты требуется ключ авторизации. Этот ключ будет вставлена в **AndroidManifest.xml** файла. Кроме того **AndroidManifest.xml** файла требуется `manifest` теги, участвующих в получении расположения пользователя.

#### <a name="enabling-windows-runtime-maps"></a>Сопоставляет Включение среды выполнения Windows

Приложения среды выполнения Windows требуется ключ авторизации с помощью Bing Maps. Этот ключ передается в качестве аргумента для `Xamarin.FormsMaps.Init` метод. Приложение необходимо также включить для служб обнаружения расположения.

### <a name="the-unadorned-map"></a>Без каких-либо карты

[ **MapDemos** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28/MapDemos) Образец состоит из [MapsDemoHomePage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml) файла и [MapsDemoHomePage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapDemosHomePage.xaml.cs) файла кода, который допускает переход различных программ демонстрации.

[BasicMapPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/BasicMapPage.xaml) файла показан способ отображения [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) представления. По умолчанию он отображает города Рим, но карты можно управлять со стороны пользователя.

Чтобы отключить горизонтальной и вертикальной прокрутки, установите [ `HasScrollEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasScrollEnabled/) свойства `false`. Чтобы отключить масштабирование, задайте [ `HasZoomEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.HasZoomEnabled/) для `false`. Эти свойства могут не работать на всех платформах.

### <a name="streets-and-terrain"></a>Улиц и рельефа

Можно отобразить разных типа карт, задав `Map` свойство [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) типа [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/), перечисление с три члена:

- [`Street`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Street/), значение по умолчанию
- [`Satellite`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Satellite/)
- [`Hybrid`](https://developer.xamarin.com/api/field/Xamarin.Forms.Maps.MapType.Hybrid/)

[MapTypesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypesPage.xaml) файла показан способ использования типа "переключатель" для выбора типа карты. Он использует [ `RadioButtonManager` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioButtonManager.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) на основе библиотеки и класс [MapTypeRadioButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapTypeRadioButton.xaml) файл.

### <a name="map-coordinates"></a>Координаты карты

Программу можно получить текущую область, `Map` отображение через [ `VisibleRegion` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.VisibleRegion/) свойство. Это свойство является *не* поддерживаемый привязываемые свойства, и отсутствует механизм уведомлений для указания его изменении, поэтому программы, отслеживать свойства, которые, возможно, следует использовать таймер для этой цели.

`VisibleRegion` относится к типу [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/), класс с четыре свойства только для чтения:

- [`Center`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Center/) типа [`Position`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/)
- [`LatitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LatitudeDegrees/) Тип `double`, указывающее высоту отображаемую область карты
- [`LongitudeDegrees`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.LongitudeDegrees/) Тип `double`, указывающее ширину отображаемую область карты
- [`Radius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.MapSpan.Radius/) Тип [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/), указывающее размер наибольшего круговой области, видимым на карте

`Position` и `Distance` оба структуры. `Position` Определяет два свойства только для чтения, настроенных с помощью [ `Position` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Position.Position/p/System.Double/System.Double/):

- [`Latitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Latitude/)
- [`Longitude`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Position.Longitude/)

`Distance` предназначена для предоставления расстояние независимые единицы путем преобразования между метрики и единиц на английском языке. Объект `Distance` значение можно создать несколькими способами:

- [`Distance` Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Distance.Distance/p/System.Double/) с расстояние в метрах
- [`Distance.FromMeters`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMeters/p/System.Double/) статический метод
- [`Distance.FromKilometers`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromKilometers/p/System.Double/) статический метод
- [`Distance.FromMiles`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Distance.FromMiles/p/System.Double/) статический метод

Значение можно найти три свойства:

- [`Meters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Meters/) типа `double`
- [`Kilometers`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Kilometers/) типа `double`
- [`Miles`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Distance.Miles/) типа `double`

[MapCoordinatesPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml) файл содержит несколько `Label` элементы для отображения `MapSpan` сведения. [MapCoordinatesPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MapCoordinatesPage.xaml.cs) файл кода программной части использует таймер для хранения сведений, обновлено, так как пользователь управляет карты.

### <a name="position-extensions"></a>Положение расширения

Новую библиотеку для этой книги с именем [ **Xamarin.FormsBook.Toolkit.Maps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit.Maps) содержит типы, зависящие от карты, но независимая от платформы. [ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Класс имеет `ToString` метод `Position`и метод для вычисления расстояния между двумя `Position` значения.

### <a name="setting-an-initial-location"></a>Настройка начальной местоположения

Можно вызвать [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion/p/Xamarin.Forms.Maps.MapSpan/) метод `Map` программно задать расположение и масштабирования уровня на карте. Аргумент имеет тип `MapSpan`. Можно создать `MapSpan` с помощью одного из следующих:

- [`MapSpan` Конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.MapSpan.MapSpan/p/Xamarin.Forms.Maps.Position/System.Double/System.Double/) с `Position`и диапазон широты и долготы
- [`MapSpan.FromCenterAndRadius`](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.FromCenterAndRadius/p/Xamarin.Forms.Maps.Position/Xamarin.Forms.Maps.Distance/) с `Position` и radius

Можно также создать `MapSpan` из существующей, с помощью методов [ `ClampLatitude` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.ClampLatitude/p/System.Double/System.Double/) или [ `WithZoom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.MapSpan.WithZoom/p/System.Double/).

[WyomingPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml) файла и [WyomingPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/WyomingPage.xaml.cs) файл кода демонстрируется использование `MoveToRegion` метод для отображения состояния Вайоминг.

Также можно использовать [ `Map` конструктор](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Maps.Map.Map/p/Xamarin.Forms.Maps.MapSpan/) с `MapSpan` объекта, чтобы инициализировать расположение карты. [XamarinHQPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/XamarinHQPage.xaml) файле показано, как это сделать полностью в XAML для отображения Xamarin главный офис в Сан-Франциско.

### <a name="dynamic-zooming"></a>Динамическое изменение масштаба

Можно использовать `Slider` динамически Масштаб карты. [RadiusZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml) файла и [RadiusZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/RadiusZoomPage.xaml.cs) файл кода программной части показано изменение радиус карты на основе `Slider` значение.

[LongitudeZoomPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml) файла и [LongitudeZoomPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LongitudeZoomPage.xaml.cs) файл кода программной части показывают Альтернативным подходом, лучше работает на Android, но оба подхода отлично подходит для Windows платформы.

### <a name="the-phones-location"></a>Расположение телефона

[ `IsShowingUser` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.IsShowingUser/) Свойство `Map` работает немного по-разному на трех платформах как [ShowLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ShowLocationPage.xaml) файла показан:

- На iOS синяя точка обозначает расположение телефона но необходимо вручную перейти существует
- В Android отображается значок, что при передаче переходит карту, чтобы расположение телефона
- UWP похож на iOS, но иногда автоматически переходит к месту расположения

**MapDemos** проект пытается имитировать Android подход, определяя на основе значок кнопки, на основе [MyLocationButton.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml) файла и [MyLocationButton.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/MyLocationButton.xaml.cs) файл кода программной части.

[GoToLocationPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml) файла и [GoToLocationPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GoToLocationPage.xaml.cs) файл кода программной части используйте эту кнопку, чтобы перейти к местоположению телефона.

### <a name="pins-and-science-museums"></a>ПИН-коды и музеи обработки и анализа

Наконец `Map` класс определяет [ `Pins` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.Pins/) свойство типа `IList<Pin>`. [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Класс определяет четыре свойства:

- [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Label/) Тип `string`— обязательное свойство
- [`Address`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Address/) Тип `string`, необязательное понятное адрес
- [`Position`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Position/) Тип `Position`, указывающее, где отображаются ПИН-код на карте
- [`Type`](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Pin.Type/) Тип [ `PinType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.PinType/), перечисления, который не используется

**MapDemos** проект содержит файл [ScienceMuseums.xml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Data/ScienceMuseums.xml), перечисляя музеи обработки и анализа в Соединенных Штатах и [ `Locations` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Locations.cs) и [ `Site` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/Site.cs) классы для десериализации данных.

[ScienceMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml) файла и [ScienceMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/ScienceMuseumsPage.xaml.cs) контакты отображения файла кода для этих музеи обработки и анализа в схеме. При нажатии кнопки ПИН-код, он отображает адреса и веб-сайта для музей.

### <a name="the-distance-between-two-points"></a>Расстояние между двумя точками

[ `PositionExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs) Класс содержит [ `DistanceTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit.Maps/Xamarin.FormsBook.Toolkit.Maps/PositionExtensions.cs#L88) метод с упрощенной Вычисление расстояния между двумя географических регионах.

Это используется в [LocalMuseumsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml) файла и [LocalMuseumsPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/LocalMuseumsPage.xaml.cs) файл кода для отображения музей расстояние от расположения пользователя также:

[![Тройной экрана странице локальной музеи](images/ch28fg28-small.png "расстояние до местоположения")](images/ch28fg28-large.png#lightbox "расстояние до местоположения")

Программа также показано, как динамически ограничить количество ПИН-кодов, исходя из расположения карты.

## <a name="geocoding-and-back-again"></a>Геокодирования и обратно

[ **Xamarin.Forms.Maps** ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) сборки также содержит [ `Geocoder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Geocoder/) класса [ `GetPositionsForAddressAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync/p/System.String/) метод, который преобразует адрес текст в ноль или более возможных географического положения и другой метод [ `GetAddressesForPositionAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync/p/Xamarin.Forms.Maps.Position/) , преобразует в обратном направлении.

[GeocoderRoundTrip.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml) файла и [GeocoderRoundTrip.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter28/MapDemos/MapDemos/MapDemos/GeocoderRoundTripPage.xaml.cs) эта функция позволяет продемонстрировать файл кода программной части.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 28 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch28-Aug2016.pdf)
- [Глава 28 образцы](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter28)
- [Элемент управления картой](~/xamarin-forms/user-interface/map.md)
