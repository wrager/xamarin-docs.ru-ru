---
title: "Пошаговое руководство. Использование фона местоположения"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: ba460bee067162f8e42f84f230f93cb1cf98ba98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-background-location"></a>Пошаговое руководство. Использование фона местоположения

В этом примере мы собираемся сборку iOS расположение приложения, которое выводит сведения о нашем текущее расположение: широты, долготы и другие параметры на экране. Это приложение будет показано правильно выполнять обновления расположение, пока приложение находится в активном или Backgrounded.

В этом пошаговом руководстве описывается какой-либо ключ backgrounding понятия, включая регистрации приложения в качестве приложения необходимости фона, приостановка обновления пользовательского интерфейса при backgrounded приложения и работа с `WillEnterBackground` и `WillEnterForeground` `AppDelegate` методы .

## <a name="application-set-up"></a>Настройка приложения


1. Во-первых, создайте новый **iOS > приложения > одного представления приложения (C#)**. Она вызывается _расположение_ и убедитесь, что выбраны iPad и iPhone.

1. Расположение приложения, рассматриваются в качестве фона необходимые приложения в iOS. Зарегистрировать приложение в качестве расположения приложения путем редактирования **Info.plist** файл проекта.

    В обозревателе решений дважды щелкните **Info.plist** файл, чтобы открыть его и перейдите к нижней части списка. Установите флажок как **Включение фоновых режимов** и **обновления расположения** флажки.


    В Visual Studio для Mac она будет выглядеть примерно следующим образом:

    [![](location-walkthrough-images/image7.png "Установите флажок, Включение фоновых режимов и флажки обновления расположения")](location-walkthrough-images/image7.png)

    В Visual Studio **Info.plist** должен быть обновлен вручную, добавив следующие пары ключ значение:

        ```csharp
        <key>UIBackgroundModes</key>
        <array>
            <string>location</string>
        </array>
        ```

1. Теперь, когда зарегистрировать приложение, оно может получить данные о местоположении с устройства. В iOS `CLLocationManager` класс используется для доступа к информации о местонахождении и могут вызывать события, которые предоставляют обновления расположения.

1. В коде, создайте новый класс с именем `LocationManager` , представляет собой единое место для различных экранов и код, чтобы подписаться на обновления расположения. В `LocationManager` класса, создать экземпляр `CLLocationManager` вызывается `LocMgr`:

```csharp
        public class LocationManager
        {
          protected CLLocationManager locMgr;

          public LocationManager (){
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
              locMgr.RequestAlwaysAuthorization (); // works in background
              //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
               locMgr.AllowsBackgroundLocationUpdates = true;
            }
          }

          public CLLocationManager LocMgr{
            get { return this.locMgr; }
          }
        }
```

    The code above sets a number of properties and permissions on the [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) class:

    - `PausesLocationUpdatesAutomatically` — Это логическое значение, которое может быть установлено в зависимости от того, разрешено ли система для приостановки расположение обновлений. На некоторых устройствах по умолчанию будет `true`, что может привести к устройству появлялись фонового обновления расположения после 15 минут.
    - `RequestAlwaysAuthorization` -Следует передавать этот метод, чтобы предоставить пользователю приложения параметр, чтобы разрешить расположение должен осуществляться в фоновом режиме. `RequestWhenInUseAuthorization` Можно также передавать, если нужно дать пользователю возможность разрешить расположение доступны только в том случае, если приложение находится на переднем плане.
    - `AllowsBackgroundLocationUpdates` — Это логическое свойство, представленные в iOS 9, можно задать, чтобы разрешить приложению получать обновления место при приостановке.

    > [!IMPORTANT]
> **Предупреждение**: iOS 8 (и выше), также необходимо вносить изменения в **Info.plist** из файла пользователя в запросе авторизации.

1. Добавьте раздел `NSLocationAlwaysUsageDescription` или `NSLocationWhenInUseUsageDescription` строкой, которая будет отображаться для пользователя в оповещение, которое запрашивает доступ к папке данных.

1. требует iOS 9, при использовании `AllowsBackgroundLocationUpdates` **Info.plist** включает ключ `UIBackgroundModes` со значением `location`. После выполнения шага 2 в данном пошаговом руководстве, это должно уже был в файле Info.plist.


1. Внутри `LocationManager` класса, создайте метод с именем `StartLocationUpdates` следующим кодом. Этот код показывает, как запуск Получает расположение обновления из `CLLocationManager`:

    ```csharp
        if (CLLocationManager.LocationServicesEnabled) {
          //set the desired accuracy, in meters
          LocMgr.DesiredAccuracy = 1;
          LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
          {
              // fire our custom Location Updated event
              LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
          };
          LocMgr.StartUpdatingLocation();
        }
        ```

    There are several important things happening in this method. First, we perform a check to see if the application has access to location data on the device. We verify this by calling `LocationServicesEnabled` on the `CLLocationManager`. This method will return **false** if the user has denied the application access to location information.

1. Next, tell the location manager how often to update. `CLLocationManager` provides many options for filtering and configuring location data, including the frequency of updates. In this example, set the `DesiredAccuracy` to update whenever the location changes by a meter. For more information on configuring location update frequency and other preferences, refer to the [CLLocationManager Class Reference](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) in the Apple documentation.

1. Finally, call `StartUpdatingLocation` on the `CLLocationManager` instance. This tells the location manager to get an initial fix on the current location, and to start sending updates

So far, the location manager has been created, configured with the kinds of data we want to receive, and has determined the initial location. Now the code needs to render the location data to the user interface. We can do this with a custom event that takes a `CLLocation` as an argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Далее необходимо подписаться на обновления расположения из `CLLocationManager`и инициировать пользовательские `LocationUpdated` событие, если новые данные о местоположении становится доступной, расположение в качестве аргумента передается в. Чтобы сделать это, создайте новый класс **LocationUpdateEventArgs.cs**. Этот код, доступный в основное приложение и возвращает местоположение устройства, когда происходит событие.

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Пользовательский интерфейс

1. Используйте конструктор операций ввода-вывода на экран, который будет отображать сведения о расположении сборки. Дважды щелкните **Main.storyboard** файл, чтобы начать.

    На экране, чтобы действовать как метки-заполнители для информации место раскадровки, перетащите несколько меток. В этом примере существует метки для широты, долготы, высота над уровнем моря, курс и скорости.

    Макет должен выглядеть следующим образом:

    ![](location-walkthrough-images/image8.png "Пример структуры пользовательского интерфейса в конструкторе iOS")

1. В панели ввода решений дважды щелкните `ViewController.cs` файла и изменить его, чтобы создать новый экземпляр LocationManager и вызовите `StartLocationUpdates`на нем.
  Измените код, чтобы иметь следующий вид:

        #region Computed Properties
        public static bool UserInterfaceIdiomIsPhone {
                    get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
                }

        public static LocationManager Manager { get; set;}
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        // As soon as the app is done launching, begin generating location updates in the location manager
            Manager = new LocationManager();
            Manager.StartLocationUpdates();
        }

        #endregion

    Расположение обновления будет запущен при запуске приложения, несмотря на то, что данные не будут отображаться.

1. Теперь, когда получает обновления расположения, обновление экрана с сведения о расположении. Следующий метод получает расположение из наших `LocationUpdated` событий и отображает его в пользовательском Интерфейсе:

        #region Public Methods
        public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
        {
            // Handle foreground updates
            CLLocation location = e.Location;

            LblAltitude.Text = location.Altitude + " meters";
            LblLongitude.Text = location.Coordinate.Longitude.ToString ();
            LblLatitude.Text = location.Coordinate.Latitude.ToString ();
            LblCourse.Text = location.Course.ToString ();
            LblSpeed.Text = location.Speed.ToString ();

            Console.WriteLine ("foreground updated");
        }

        #endregion


Нам по-прежнему необходимо подписаться на `LocationUpdated` событие в нашей AppDelegate и вызовите новый метод для обновления пользовательского интерфейса. Добавьте следующий код в `ViewDidLoad,` сразу же после `StartLocationUpdates` вызова:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Теперь после запуска приложения, он должен выглядеть следующим образом:

[![](location-walkthrough-images/image5.png "Запустите пример приложения")](location-walkthrough-images/image5.png)

## <a name="handling-active-and-background-states"></a>Работа с регионами активный и фона

1. Приложение вывод расположение обновлений, когда оно находится на переднем плане и active. Чтобы продемонстрировать, что произойдет, если приложение входит в фоновом режиме, необходимо переопределить `AppDelegate` изменений состояния методы, которые отслеживают приложения, чтобы приложение записывает в консоль при переходе от переднего плана и фона:

        public override void DidEnterBackground (UIApplication application)
        {
          Console.WriteLine ("App entering background state.");
        }

        public override void WillEnterForeground (UIApplication application)
        {
          Console.WriteLine ("App will enter foreground");
        }

    Добавьте следующий код в `LocationManager` постоянно печать новое местоположение выходных данных приложения, чтобы проверить сведения о расположении данных по-прежнему доступны в фоновом режиме:

        public class LocationManager
        {
          public LocationManager ()
          {
            ...
            LocationUpdated += PrintLocation;
          }
          ...

          //This will keep going in the background and the foreground
          public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
            CLLocation location = e.Location;
            Console.WriteLine ("Altitude: " + location.Altitude + " meters");
            Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
            Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
            Console.WriteLine ("Course: " + location.Course);
            Console.WriteLine ("Speed: " + location.Speed);
          }
        }

1. Один оставшиеся проблемы с кодом: попытка обновить пользовательский Интерфейс при backgrounded приложения iOS причина будет завершит его. Когда приложение выходит в фоновом режиме, код должен отменить подписку на обновления расположения и остановить обновление пользовательского интерфейса.

    iOS предоставляет нам уведомления во время работы приложения о переход в другое приложение состояний. В этом случае мы можно подписаться на `ObserveDidEnterBackground` уведомления.

    В следующем фрагменте кода показано, как использовать уведомления для представления, в том, когда для остановки обновления пользовательского интерфейса. Это будет отправлена `ViewDidLoad`:

        UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
          Manager.LocationUpdated -= HandleLocationChanged;
        });

    Когда приложение запущено, результат будет выглядеть примерно следующим образом:

    ![](location-walkthrough-images/image6.png "Пример вывода расположение в консоли")

1. Приложение выводит расположение обновлений на экране при работе на переднем плане и продолжает выводят данные в окно выходных данных приложения при работе в фоновом режиме.

Остается только одна проблема необработанных: экрана запускает обновления пользовательского интерфейса при первой загрузке приложения, но нет возможности узнать, когда приложение повторно вошел переднего плана. Если backgrounded приложение переводится обратно на переднем плане, не будет возобновить обновления пользовательского интерфейса.

Чтобы устранить эту проблему, вложите вызов для запуска обновления пользовательского интерфейса внутри еще одно уведомление, которое инициируется, когда приложение входит в активном состоянии:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Теперь пользовательский Интерфейс будет начать обновление, если приложение запускается в первый, и обновление приложения любое время возобновления возобновляет на переднем плане.

В этом пошаговом руководстве мы создали приложение правильно, поддержкой фона iOS, которое выводит данные о местоположениях для экрана и окна вывода приложения.


## <a name="related-links"></a>Связанные ссылки

- [Расположение (часть 4) (пример)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Ссылка на Framework Core расположение](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
