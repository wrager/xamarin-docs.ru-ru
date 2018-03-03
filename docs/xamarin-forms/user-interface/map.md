---
title: "Карта"
description: "Xamarin.Forms использует схему собственного API-интерфейсов для каждой платформы."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 98e6cade952e656046c6c0981a0b73ff0894c9d6
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="map"></a>Карта

_Xamarin.Forms использует схему собственного API-интерфейсов для каждой платформы._

Xamarin.Forms.Maps использует схему собственного API-интерфейсов для каждой платформы. Это обеспечивает быстрый и привычных maps пользователей, но есть некоторые этапы настройки требуются для соответствия каждой требованиям платформы конкретного API.
После настройки `Map` управления работает так же, как и любой другой элемент Xamarin.Forms в общий код.

* [Сопоставляет инициализации](#Maps_Initialization) — с помощью `Map` требуется дополнительный код инициализации во время запуска.
* [Конфигурация платформы](#Platform_Configuration) -каждой платформы требуется настройка для карт для работы.
* [С помощью схемы на языке C#](#Using_Maps) -отображение сопоставляет и контакты, с помощью C#.
* [С помощью схемы на языке XAML](#Using_Xaml) -отображения карты с XAML.

Элемент управления карты используется в [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) пример, который приведен ниже.

 [ ![Карты в образце MobileCRM](map-images/maps-zoom-sml.png "пример элемента управления карты")](map-images/maps-zoom.png "пример элемента управления карты")

Функциональные возможности карты можно улучшить путем создания [сопоставить пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Инициализация карты

При добавлении карты в приложении Xamarin.Forms **Xamarin.Forms.Maps** имеет отдельный пакет NuGet, следует добавить на каждый проект в решении.
На Android это также имеет зависимость от GooglePlayServices (другой NuGet), который загружается автоматически при добавлении Xamarin.Forms.Maps.

После установки пакета NuGet, требуется некоторый код инициализации в каждом проекте приложения *после* `Xamarin.Forms.Forms.Init` вызова метода. Для iOS используйте следующий код:

```csharp
Xamarin.FormsMaps.Init();
```

В Android необходимо передать те же параметры, что `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Для среды выполнения Windows (WinRT) и универсальной платформы Windows (UWP) используйте следующий код:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Добавьте этот вызов в следующих файлах для каждой платформы:

-  **iOS** -AppDelegate.cs файл `FinishedLaunching` метод.
-  **Android** -MainActivity.cs файл `OnCreate` метод.
-  **WinRT и UWP** -MainPage.xaml.cs файл `MainPage` конструктор.

После добавления пакета NuGet и вызвать метод инициализации в пределах каждого applcation `Xamarin.Forms.Maps` интерфейсы API, которые могут использоваться в коде общих PCL или общий проект.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Конфигурация платформы

Дополнительные действия по настройке необходимы на некоторых платформах, прежде чем будет отображать карту.

### <a name="ios"></a>iOS

На iOS 7 карты управлять «просто работает», если как `FormsMaps.Init()` выполнен вызов.

Для iOS 8 два ключа должны быть добавлены к **Info.plist** файла: [ `NSLocationAlwaysUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) и [ `NSLocationWhenInUseUsageDescription` ](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26). Ниже приведен XML-представление - следует обновить `string` значения в том, как приложение использует сведения о расположении:

```xml
<key>NSLocationAlwaysUsageDescription</key>
    <string>Can we use your location</string>
<key>NSLocationWhenInUseUsageDescription</key>
    <string>We are using your location</string>
```

**Info.plist** записи также могут добавляться в **источника** представления при редактировании **Info.plist** файла:

![Info.plist для iOS 8](map-images/ios8-map-permissions.png "операции Info.plist требуется iOS 8")


### <a name="android"></a>Android

Для использования [Google Maps API v2](https://developers.google.com/maps/documentation/android/) Android, необходимо создать ключ API и добавьте его в проект Android.
Следуйте инструкциям в документе Xamarin на [получения ключа Google Maps API v2](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
После выполнения этих инструкций, вставьте ключ API в **Properties/AndroidManifest.xml** файл (Просмотр исходного кода и поиска или обновить следующий элемент):

```xml
<meta-data android:name="com.google.android.maps.v2.API_KEY"
            android:value="AbCdEfGhIjKlMnOpQrStUvWValueGoesHere" />
```

Без действительного ключа API управления карты будет отображаться как серые поля на Android.

> [!NOTE]
> **Примечание**: помните, что необходимо создать другой ключ, с помощью файла ключей, используемого для подписания версии любого приложения, который загружается в магазине Google Play. Ключ, созданная для разработки и отладка не будет работать и загрузить из Google Play приложение будет нарушили Отображение карты. Также следует помнить, повторное создание ключа if приложения **имя пакета** изменения.

Также необходимо включить соответствующие разрешения, щелкнув правой кнопкой мыши проект Android и выбрав **параметры > сборки > приложения Android** и отсчет следующее:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

На снимке экрана ниже показаны некоторые из них:

![Разрешения, необходимые для Android](map-images/android-map-permissions.png "необходимые разрешения для Android")

Последние две являются обязательными, так как приложения требуют подключения к сети для загрузки данных карты. Узнайте, как Android [разрешений](http://developer.android.com/reference/android/Manifest.permission.html) для получения дополнительных сведений.

### <a name="windows-runtime-and-universal-windows-platform"></a>Среда выполнения Windows и универсальной платформы Windows

Для работы с картами в среде выполнения Windows и универсальной платформы Windows необходимо создать маркер авторизации. Дополнительные сведения см. в разделе [запроса ключа проверки подлинности maps](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) на сайте MSDN.

Токен проверки подлинности должна задаваться в `FormsMaps.Init("AUTHORIZATION_TOKEN")` вызова метода, для проверки подлинности приложения с помощью Bing Maps.

<a name="Using_Maps" />

## <a name="using-maps"></a>С использованием карт

В разделе [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) в образце MobileCRM пример использования элемента управления карты в коде. Простой `MapPage` класс может выглядеть как - уведомления, новый `MapSpan` создается для размещения представление карты:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Тип схемы

Также можно изменить, задав содержимого карты `MapType` свойство, чтобы показать карту обычных улицы (по умолчанию), вспомогательные изображения или их сочетание.

```csharp
map.MapType == MapType.Street;
```

Допустимые `MapType` значения:

-  Гибридный
-  Вспомогательные
-  Улица (по умолчанию)


### <a name="map-region-and-mapspan"></a>Области карты и MapSpan

Как показано в приведенном выше фрагменте кода, предоставляющего `MapSpan` экземпляр конструктора карты задает исходное представление (точки центра и масштаб) сопоставления при его загрузке. `MoveToRegion` Метода в классе карты можно изменить уровень позиции и масштабирования карты. Существует два способа для создания нового `MapSpan` экземпляр:

-  **MapSpan.FromCenterAndRadius()** -статический метод для создания диапазон из `Position` и указав `Distance` .
-  **новый (MapSpan)** -конструктор, использующий `Position` и преобразование градусов, широты и долготы для отображения.


Чтобы изменить уровень масштаба карты, не изменяя расположение, создайте новый `MapSpan` с использованием текущего расположения из `VisibleRegion.Center` свойству элемента управления карты. Объект `Slider` может использоваться для управления масштабом карты следующим образом (Однако масштабирование непосредственно в элементе управления карты не может обновить в настоящее время значение ползунка):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [ ![Карты с zoom](map-images/maps-zoom-sml.png "Масштаб карты управления")](map-images/maps-zoom.png "Масштаб карты элементов управления")

### <a name="map-pins"></a>Булавки

Можно пометить местоположения на карте с `Pin` объектов.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` может быть присвоено одно из следующих значений, которые могут повлиять на способ отображения ПИН-кода в (в зависимости от платформы):

-  Универсальный
-  Место
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>С помощью Xaml

Maps также может быть размещен в макетах Xaml, как показано в этом фрагменте кода.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion` И `Pins` можно задать в коде с помощью `MyMap` ссылки (или все, что называется карты). Обратите внимание, что дополнительный `xmlns` определения пространства имен, необходимые для ссылки на элементы управления Xamarin.Forms.Maps.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Сводка

Xamarin.Forms.Maps является отдельной NuGet, которые необходимо добавить в каждый проект в решение Xamarin.Forms. Как и некоторые действия по настройке iOS, Android, WinRT и UWP требуется дополнительный код инициализации.

После настройки сопоставляет API можно использовать для отображения карты с маркерами ПИН-код в нескольких строк кода. Карты можно улучшить с [пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Связанные ссылки

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Пользовательское средство отрисовки карты](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
