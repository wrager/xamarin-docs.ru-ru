---
title: "Работа с представлениями коллекции"
description: "В этой статье описывается проектирование и работа с представлениями коллекции внутри приложения Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f0201e114f55e0610aceb68f98fae60a801afc68
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-collection-views"></a>Работа с представлениями коллекции

_В этой статье описывается проектирование и работа с представлениями коллекции внутри приложения Xamarin.tvOS._

Представления коллекций позволяют группу содержимое будет отображаться с использованием произвольного макеты. С помощью встроенной поддержки, они позволяют легко создавать макеты вид сетки или линейного, поддерживая пользовательских макетов.

[ ![](collection-views-images/collection01.png "Пример представления коллекции")](collection-views-images/collection01.png)

Представление коллекции поддерживает коллекцию элементов с помощью делегата и источником данных для обеспечения взаимодействия с пользователем и содержимое коллекции. Так как представление коллекции основан на подсистемы разметки, которое не зависит от самого представления, предоставляя структуру можно легко изменить представление представление коллекции данных на лету.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>О представлениях коллекции

Как упоминалось выше, представления коллекции (`UICollectionView`) управляет упорядоченную коллекцию элементов, а также предоставляет настраиваемые макеты этих элементов. Представления коллекций работает таким же образом, для представления таблиц (`UITableView`), за исключением того, они могут использовать макеты элементы присутствуют в не только один столбец.

При использовании представления коллекции в tvOS, приложение отвечает за предоставление данных, связанные с коллекцией, используя в качестве источника данных (`UICollectionViewDataSource`). Просмотр данных коллекции можно при необходимости быть, организованная в нужную группу (разделов).

Представление коллекции представляет отдельные элементы на экране с помощью ячейки (`UICollectionViewCell`), обеспечивающий представление заданного элемента данных из коллекции (например, изображения и название).

При необходимости можно добавить дополнительные представления представление коллекции презентацию в качестве заголовков и нижних колонтитулов для разделов и ячеек. Представление коллекции макета несет ответственность за определение размещения из этих представлений, а также отдельные ячейки.

Представление коллекции может отвечать на действия пользователя, с помощью делегата (`UICollectionViewDelegate`). Этот делегат также отвечает за определение, если данная ячейка можно получить фокус, если ячейка была выделена или если отмечен. В некоторых случаях делегата определяет размер отдельных ячеек.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Макеты коллекции

Ключевой особенностью представление коллекции является разделение данные, которые он представления и его макет. Макет представления коллекции (`UICollectionViewLayout`) отвечает за предоставление организации и расположение ячейки (и любые дополнительные представления) в представлении коллекции на экране презентации.

Отдельные ячейки создаются путем представления коллекции из присоединенного источника данных и затем упорядочиваются и отображаются с указанным макетом представления коллекции.

При создании представления коллекции обычно предоставляет разметки представления коллекции. Однако разметки представления коллекции можно изменить в любое время и на экране представления данных представления коллекции автоматически обновляется с помощью предоставленных нового макета.

Макет представления коллекции предоставляет несколько методов, которые можно использовать для анимации перехода между двумя разными макетами (по умолчанию выполняется без анимации). Кроме того коллекция макеты можно работать с распознавателей жестов для дальнейшей анимации взаимодействие с пользователем, который приводит к изменению в макете.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Создание ячеек и дополнительных представлений

Источник данных для представления коллекции не только отвечает за предоставление резервное коллекции элементов, а также данные, используемые для отображения содержимого ячейки.

Так как коллекция представлений были предназначены для обработки больших наборов элементов, отдельные ячейки можно из очереди и использовать повторно, чтобы предотвратить переполнение ограничения оперативной памяти. Существует два метода для вывода представлений:

- `DequeueReusableCell` — Создает или возвращает ячейку данного типа (как указано в раскадровке приложения).
- `DequeueReusableSupplementaryView` — Создает или возвращает дополнительные представления данного типа (как указано в раскадровке приложения).

Перед вызовом любого из этих методов, необходимо зарегистрировать класс раскадровки или `.xib` файл, используемый для создания представления ячейки в представлении коллекции. Пример:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Где `typeof(CityCollectionViewCell)` предоставляет класс, который поддерживает представление и `CityViewDatasource.CardCellId` содержит идентификатор, используемый при ячейки (или представления) удален из очереди.

После ячейки удален из очереди, можно настроить его с данными для элемента, который он является числом, представляющим и вернуться в представление коллекции для отображения.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>О контроллерах представление коллекции

Контроллер представление коллекции (`UICollectionViewController`) — это специализированный контроллер представление (`UIViewController`), предоставляющий следующие особенности:

- Он отвечает за загрузку представление коллекции из его раскадровки или `.xib` файл и при создании экземпляра представления. Если создать в коде, автоматически создается новая, ненастроенная представления коллекции.
- После загрузки представление коллекции контроллер пытается загрузить его источника данных и делегат из раскадровки или `.xib` файла. Если ни одна не доступна, устанавливается как источником.
- Гарантирует, что данные загружаются до как представление коллекции заполняется при первом отображении и перезагружает и снимите выбор на каждом из последующих.

Кроме того, контроллер представление коллекции обеспечивает переопределяемых методов, которые можно использовать для управления жизненным циклом представления коллекции, такие как `AwakeFromNib` и `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Представления коллекций и раскадровки

Для работы с представлением коллекции в приложении Xamarin.tvOS проще всего добавить в соответствующую раскадровку. Как краткий пример мы собираемся создавать пример приложения, представляющего изображение, заголовок и кнопка выбора. Если пользователь, нажмите кнопку "выбрать", представления коллекции будет отображаться, пользователи могут выбрать новое изображение. При выборе изображения представление коллекции закрывается, и отображается новое изображение и заголовок.

Давайте выполните следующие действия.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

    
1. Запуск нового **tvOS единое представление приложения** в Visual Studio для Mac.
1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его в конструкторе iOS.
1. Добавить представление изображения, метки кнопки на существующие и настроить их на следующий вид: 

    [ ![](collection-views-images/collection02.png "Пример макета")](collection-views-images/collection02.png)
1. Назначьте **имя** представление изображения и метки в **вкладку графического** из **свойства обозревателя**. Пример: 

    [ ![](collection-views-images/collection03.png "Имя параметра")](collection-views-images/collection03.png)
1. Затем перетащите раскадровку контроллер представление коллекции: 

    [ ![](collection-views-images/collection04.png "Контроллер представление коллекции")](collection-views-images/collection04.png)
1. Перетаскивание с помощью кнопки на контроллер представление коллекции и выберите **Push** из всплывающего окна: 

    [ ![](collection-views-images/collection05.png "Выберите из контекстного меню Push")](collection-views-images/collection05.png)
1. При запуске приложения станет представление коллекции будет показывать всякий раз, когда пользователь нажимает кнопку.
1. Выберите представление коллекции и введите следующие значения в **вкладка «Макет»** из **свойства обозревателя**: 

    [ ![](collection-views-images/collection06.png "Обозреватель свойств")](collection-views-images/collection06.png)
1. Определяет размер отдельных ячеек и границы между ячейками и внешнего края представление коллекции.
1. Выберите контроллер представление коллекции и задайте его класс `CityCollectionViewController` в **вкладку графического**: 

    [ ![](collection-views-images/collection07.png "Задайте класс CityCollectionViewController")](collection-views-images/collection07.png)
1. Выберите представление коллекции и задайте его класс `CityCollectionView` в **вкладку графического**: 

    [ ![](collection-views-images/collection08.png "Задайте класс CityCollectionView")](collection-views-images/collection08.png)
1. Выберите ячейку представления коллекции и задайте его класс `CityCollectionViewCell` в **вкладку графического**: 

    [ ![](collection-views-images/collection09.png "Задайте класс CityCollectionViewCell")](collection-views-images/collection09.png)
1. В **вкладку графического** убедитесь, что **макета** — `Flow` и **направления прокрутки** — `Vertical` для представления коллекции: 

    [ ![](collection-views-images/collection10.png "Вкладка мини-приложения")](collection-views-images/collection10.png)
1. Выберите ячейку представления коллекции и задайте его **удостоверение** для `CityCell` в **вкладку графического**: 

    [ ![](collection-views-images/collection11.png "Задать удостоверение для CityCell")](collection-views-images/collection11.png)
1. Сохраните изменения.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Запуск нового **tvOS единое представление приложения** в Visual Studio.
1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его в конструкторе iOS.
1. Добавить представление изображения, метки кнопки на существующие и настроить их на следующий вид: 

    [ ![](collection-views-images/collection02vs.png "Настройка макета")](collection-views-images/collection02vs.png)
1. Назначьте **имя** представление изображения и метки в **вкладку графического** из **свойства обозревателя**. Пример: 

    [ ![](collection-views-images/collection03vs.png "Обозреватель свойств")](collection-views-images/collection03vs.png)
1. Затем перетащите раскадровку контроллер представление коллекции: 

    [ ![](collection-views-images/collection04vs.png "Контроллер представление коллекции")](collection-views-images/collection04vs.png)
1. Перетаскивание с помощью кнопки на контроллер представление коллекции и выберите **Push** из всплывающего окна: 

    [ ![](collection-views-images/collection05vs.png "Выберите из контекстного меню Push")](collection-views-images/collection05vs.png)
1. При запуске приложения станет представление коллекции будет показывать всякий раз, когда пользователь нажимает кнопку.
1. Выберите представление коллекции и в **вкладка «Макет»** из **свойства обозревателя** введите **ширина** как _361_ и  **Высота** как _256_ 
1. Определяет размер отдельных ячеек и границы между ячейками и внешнего края представление коллекции.
1. Выберите контроллер представление коллекции и задайте его класс `CityCollectionViewController` в **вкладку графического**: 

    [ ![](collection-views-images/collection07vs.png "Задайте класс CityCollectionViewController")](collection-views-images/collection07vs.png)
1. Выберите представление коллекции и задайте его класс `CityCollectionView` в **вкладку графического**: 

    [ ![](collection-views-images/collection08vs.png "Задайте класс CityCollectionView")](collection-views-images/collection08vs.png)
1. Выберите ячейку представления коллекции и задайте его класс `CityCollectionViewCell` в **вкладку графического**: 

    [ ![](collection-views-images/collection09vs.png "Задайте класс CityCollectionViewCell")](collection-views-images/collection09vs.png)
1. В **вкладку графического** убедитесь, что **макета** — `Flow` и **направления прокрутки** — `Vertical` для представления коллекции: 

    [ ![](collection-views-images/collection10vs.png "Вкладка указанное мини-приложения")](collection-views-images/collection10vs.png)
1. Выберите ячейку представления коллекции и задайте его **удостоверение** для `CityCell` в **вкладку графического**: 

    [ ![](collection-views-images/collection11vs.png "Задать удостоверение для CityCell")](collection-views-images/collection11vs.png)
1. Сохраните изменения.
    

-----

Если был выбран, мы `Custom` для представления коллекции **макета**, можно было указать настраиваемый макет. Компания Apple предоставляет встроенный `UICollectionViewFlowLayout` и `UICollectionViewDelegateFlowLayout` , можно легко представить данные в макете сетки (они используются `flow` стиля макета). 

Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Предоставление данных для представления коллекции

Теперь, когда есть наши представления коллекции (и коллекции View Controller) добавлены в наши раскадровку, мы должны предоставлять данные для коллекции. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Модель данных

Во-первых мы собираемся создать модель данных, содержащий изображение, заголовок и флаг, разрешающий города, чтобы выбрать имя файла.

Создание `CityInfo` класса и сделать его выглядеть следующим образом:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Ячейку представления коллекции

Теперь нам необходимо определить способ представления данных для каждой ячейки. Изменить `CityCollectionViewCell.cs` файла (создан автоматически из файла раскадровки) и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Для нашего приложения tvOS мы будет отображаться изображение, а также необязательный заголовок. Если не удается заданного города, мы затемнения представление изображения, используя следующий код:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Ячейка, содержащая изображения переходит в режим с фокусом пользователем, мы хотим использовать встроенные эффект фокусировки на нем путем установки следующее свойство:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Дополнительные сведения о навигации и фокус см. в разделе нашей [работа с навигации и фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленного и контроллеров Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) документации.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Поставщик данных представления коллекции

С нашей модели данных, созданной и наши определенные макет ячеек давайте создадим источника данных для представления нашей коллекции. Источник данных будет обеспечивает не только резервное копирование данных, но также вывода ячеек для отображения отдельных ячеек на экране.

Создание `CityViewDatasource` класса и сделать его выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Позволяет просмотреть этот класс подробно. Во-первых, наследуют от `UICollectionViewDataSource` и предоставлять сокращенный идентификатор ячейки (, назначенные в конструктор iOS):

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Далее мы обеспечивают хранение для сбора данных и предоставить класс для заполнения данных.

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Затем мы переопределить `NumberOfSections` метод и возвращают число разделов (группы элементов), просматривать коллекции имеет. В этом случае имеется только один:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Далее мы возвращается количество элементов в нашей коллекции, используя следующий код:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Наконец мы dequeue ячейку для повторного использования при запросе представление коллекции с помощью следующего кода:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

После мы получаем ячейке представления коллекции нашей `CityCollectionViewCell` типа, мы его заполнение данного элемента.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Реагирование на события пользователей

Поскольку мы хотим пользователя, чтобы иметь возможность выбрать элемент из коллекции, необходимо предоставить делегат представление коллекции для обработки этого взаимодействия. И необходимо обеспечить возможность нашей вызывающего представления знать, какой элемент пользователь выбрал.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Делегат приложения

Мы необходим способ для выбранного элемента из представления коллекции содержат обратные связи у вызывающего представления. Мы будем использовать пользовательское свойство в нашем `AppDelegate`. Изменить `AppDelegate.cs` файл и добавьте следующий код:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Это свойство определяет и задает Город по умолчанию, которая изначально будет отображаться. Более поздней версии мы будем использовать это свойство для отображения выбранных пользователем и разрешение select необходимо изменить.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Делегат представление коллекции

Добавьте новый `CityViewDelegate` класса в проект и сделайте его выглядеть следующим образом:


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим этот класс. Во-первых, наследуют от `UICollectionViewDelegateFlowLayout`. Причина, по которой мы наследовать от этого класса и не `UICollectionViewDelegate` является использование встроенной `UICollectionViewFlowLayout` представляет нашей элементы и не является типом пользовательского макета.

Далее мы возвращаем размер для отдельных элементов, с помощью следующего кода:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Затем мы решите, если данная ячейка может получить фокус, используя следующий код: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Мы проверяем, имеет ли данного фрагмента данных резервного его `CanSelect` значение флага `true` и возвращают это значение. Дополнительные сведения о навигации и фокус см. в разделе нашей [работа с навигации и фокус](~/ios/tvos/app-fundamentals/navigation-focus.md) и [Siri удаленного и контроллеров Bluetooth](~/ios/tvos/platform/remote-bluetooth.md) документации.

Наконец мы можем ответить пользователю при выборе элемента с помощью следующего кода:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Здесь мы устанавливаем `SelectedCity` свойство нашей `AppDelegate` элементом, что пользователь выбрал и мы закрыть контроллер представление коллекции, вернуться к представлению, вызвавшую нам. Мы еще не определили `ParentController` свойство наши представления коллекции еще мы сделаем то далее.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Настройка представления коллекции

Теперь нам нужно изменить наши представления коллекции и назначить наш источник данных и делегата. Изменить `CityCollectionView.cs` файл (созданный нам автоматически из наших раскадровки) и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Во-первых, мы предоставляем ярлык для доступа к нашей `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Далее представлена ярлыком для представления коллекции источник данных и свойства для доступа к коллекции View Controller (используется нашей выше делегата для закрытия коллекции, когда пользователь выбирает):

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Затем мы используем следующий код для инициализации представления коллекции и назначать наш класс ячейки, источник данных и делегат:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Наконец мы хотим заголовке на изображения только если он выделен пользователя (фокус). Для этого с помощью следующего кода:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Мы устанавливаем transparence предыдущего элемента теряет фокусировку значение нуль (0) и transparence следующего элемента быть сфокусирован на 100%. Эти перехода получить также анимации.


## <a name="configuring-the-collection-view-controller"></a>Настройка контроллера представления коллекции

Теперь нужно сделать Окончательная настройка для наших представления коллекции и разрешить на контроллере, чтобы задать свойство, которое мы определили, представление коллекции может быть закрыт после пользователь выбирает элемент.

Изменить `CityCollectionViewController.cs` файла (создаваемый автоматически из наших раскадровки) и сделать его выглядеть следующим образом:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Помещение их все Together 

Теперь, когда у нас есть все объединить частей для заполнения и управления наших представление коллекции нам нужно отредактируйте окончательного нашей главное представление для объединения все.

Изменить `ViewController.cs` файла (создаваемый автоматически из наших раскадровки) и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Следующий код сначала отображает выбранный элемент из `SelectedCity` свойство `AppDelegate` и отображение его, когда пользователь сделал выбор из представления коллекции:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Тестирование приложения

Все на месте Если построение и запуск приложения, основное представление отображается с Город по умолчанию:

[ ![](collection-views-images/run01.png "Главный экран")](collection-views-images/run01.png)

Если пользователь щелкнуть **выберите представление** будет отображаться кнопка представление коллекции:

[ ![](collection-views-images/run02.png "Представление коллекции")](collection-views-images/run02.png)

Все города с его `CanSelect` свойство `false` отображается серым цветом, и пользователь не сможет установить фокус на него. Когда пользователь выделяет элемент (сделать его фокус) отображается заголовок, и они могут использовать эффект фокусировки, чтобы тонкости наклона изображения в трехмерном пространстве.

Когда пользователь щелкает выберите изображение, представление коллекции закрывается, и основное представление отображается повторно с использованием нового образа:

[ ![](collection-views-images/run03.png "Новый образ на начальном экране")](collection-views-images/run03.png)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Создание пользовательской структуры и переупорядочение элементов

Одно из основных возможностей с помощью представления коллекции является возможность создания пользовательских макетов. Так как tvOS наследует от операций ввода-вывода, процесс создания пользовательских макетов одинаково. См. в разделе нашей [введение к представлениям коллекции](~/ios/user-interface/controls/uicollectionview.md) Дополнительные сведения см.

Недавно добавлен в коллекцию представлений для iOS 9 была возможность легко включить изменения порядка элементов в коллекции. Опять же, поскольку tvOS 9 — это подмножество iOS 9, для этого их таким же образом. См. в разделе нашей [изменения представления коллекции](~/ios/user-interface/controls/uicollectionview.md) документа для получения дополнительных сведений.


<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован проектирование и работа с представлениями коллекции внутри приложения Xamarin.tvOS. Во-первых он рассматривается все элементы, входящие в состав представления коллекции. Далее показано, как разрабатывать и реализовывать представления коллекции с помощью раскадровки. Наконец обеспечивается ссылки на сведения Создание пользовательских макетов и изменение порядка элементов.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
