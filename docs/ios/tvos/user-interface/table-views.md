---
title: Работа с tvOS представления таблиц в Xamarin
description: В этой статье описывается проектирование и работа с представлениями таблицы и таблицы Просмотр контроллеров внутри приложения Xamarin.tvOS.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8c74c2cc7598f50e57a6a450823e2b0ebca4b537
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789571"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Работа с tvOS представления таблиц в Xamarin

_В этой статье описывается проектирование и работа с представлениями таблицы и таблицы Просмотр контроллеров внутри приложения Xamarin.tvOS._

В tvOS представление таблицы представляется в виде отдельного столбца Прокрутка строк, которые при необходимости могут быть организованы в группы или разделов. При необходимости для эффективного отображения большой объем данных для пользователя в снимите флажок, чтобы понять, как следует использовать таблицу представления.

Представления таблиц обычно отображаются в одной из сторон [разделенное представление](~/ios/tvos/user-interface/split-views.md) как навигация с подробными сведениями выбранного элемента, отображаемого на противоположной стороне:

[![](table-views-images/intro01.png "Пример представления таблицы")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>О представлениях таблиц

Объект `UITableView` отображает один столбец прокручиваемый строк в виде иерархического списка данных, которые при необходимости могут быть организованы в группы или разделов: 

[![](table-views-images/table01.png "Выбранный элемент")](table-views-images/table01.png#lightbox)

Apple имеет следующие рекомендации для работы с таблицами:

- **Учитывать ширину быть** -попробуйте найти правильный баланс в вашей ширина таблицы. Если таблица является слишком большой, он может быть трудно сканирования на расстоянии и можно извлечь из доступных области содержимого. Если таблица является слишком узкий, это может привести к данные будут усечены или wrap, еще раз это может быть сложно для пользователя, из которого выполняется чтение в комнате.
- **Показать содержимое таблицы быстро** — для больших списков данных отложенной загрузки содержимого и запустить с информацией, как только таблицы, представленные пользователю. Если таблица принимает длинный, чтобы загрузить, пользователь может утратить интерес в приложении или задержек, которые он заблокирован вверх.
- **Уведомить пользователя из Long содержимого загружает** — Если время загрузки большой таблицы неизбежные, присутствует [индикатор выполнения или индикатор активности](~/ios/tvos/user-interface/progress-indicators.md) , чтобы они знали, приложение не защищенном месте.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Ячейка типов табличных представлений

Объект `UITableViewCell` используется для представления отдельных строк данных в табличном представлении. Apple определено несколько типов ячейки таблицы по умолчанию:

- **По умолчанию** — это тип создает параметр изображения слева от ячейки и по левому краю заголовка справа. 
- **Подзаголовок** — это тип, создает по левому краю заголовок на первой строке и меньшую по левому краю подзаголовок на следующей строке.
- **Значение 1** -этот тип представляет заголовок по левому краю с более светлым цветной, по правому краю подзаголовок в той же строке.
- **Значение 2** -этот тип представляет заголовок по правому краю с светлее цветной, по левому краю подзаголовок в той же строке.

Все типы ячейки представление таблицы по умолчанию также поддерживают графические элементы, такие как индикаторы раскрытия или флажки. 

Кроме того, можно определить **настраиваемый** представление в виде ячейки таблицы и присутствует _ячейки прототип_, либо создать в конструкторе интерфейс или с помощью кода.

Apple имеет следующие рекомендации по работе с ячейками таблицы представления.

- **Избегайте обрезки текста** -сохранить отдельные строки текста короткое, чтобы они не оказаться усеченное. Для пользователя, для анализа в для комнаты жестко усеченное слов или фраз.
- **Следует учитывать состояние строки фокуса** — так как строки становится больше, с использованием более скругленные углы в фокусе, необходимо протестировать внешнего вида вашей ячеек во всех состояниях. Изображения или текста могут стать обрезаны или искать неверные в состоянии фокуса.
- **Для редактирования таблиц только в случае необходимости используйте** -перемещение или удаление строк из таблицы потребует больше времени на tvOS операций ввода-вывода. Необходимо тщательно решить, если этот компонент будет добавлять или будет отвлекать внимание от tvOS приложения.
- **Создание настраиваемых ячейки типов в обычном** — пока встроенные типы ячейки представление таблицы прекрасно подходят для многих ситуациях, рассмотрите возможность создания пользовательских типов ячейки стандартным сведения для более полного управления и лучше представить данные в пользователь.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Работа с представлениями таблицы

Самый простой способ работы с представлениями таблицы в приложении Xamarin.tvOS — для создания и изменения их внешний вид в конструкторе интерфейса.

Чтобы начать работу, выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
    
1. В Visual Studio для Mac, создание нового проекта приложения tvOS и выберите **tvOS** > **приложения** > **одного представления приложения** и нажмите кнопку  **Далее** кнопки: 

    [![](table-views-images/table02.png "Выберите приложение, одно представление")](table-views-images/table02.png#lightbox)
1. Введите **имя** приложения и нажмите кнопку **Далее**: 

    [![](table-views-images/table03.png "Введите имя для приложения")](table-views-images/table03.png#lightbox)
1. Либо измените **имя проекта** и **имя решения** или примите значения по умолчанию и нажмите кнопку **создать** кнопку, чтобы создать новое решение: 

    [![](table-views-images/table04.png "Имя проекта и имя решения")](table-views-images/table04.png#lightbox)
1. В **Pad решения**, дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе iOS: 

    [![](table-views-images/table05.png "Файл Main.storyboard")](table-views-images/table05.png#lightbox)
1. Выберите и удалите **представление по умолчанию контроллер**: 

    [![](table-views-images/table06.png "Выберите и удалите контроллер представление по умолчанию")](table-views-images/table06.png#lightbox)
1. Выберите **разбиение View Controller** из **элементов** и перетащите его на поверхность разработки.
1. По умолчанию, вы сможете получить [разделенное представление](~/ios/tvos/user-interface/split-views.md) с **навигации View Controller** и **контроллер представление таблицы** в левой части и **View Controller** в правую часть. Это предлагается использовать Apple табличного представления в tvOS: 

    [![](table-views-images/table08.png "Добавить представление с разделением")](table-views-images/table08.png#lightbox)
1. Необходимо будет выбрать каждой части представления таблицы и назначьте его пользовательский **имя класса** в **мини-приложение** вкладке **свойства обозревателя** доступа к нему позже в C# код. Например **таблицы View Controller**: 

    [![](table-views-images/table09.png "Назначьте имя класса")](table-views-images/table09.png#lightbox)
1. Необходимо создать пользовательский класс для **контроллер представление таблицы**, **представление таблицы** , а также **ячеек прототип**. Visual Studio для Mac добавит пользовательских классов в дереве проекта, создаются как: 

    [![](table-views-images/table10.png "Пользовательские классы в дереве проекта")](table-views-images/table10.png#lightbox)
1. Затем выберите представление таблицы в области конструктора и при необходимости измените его свойств. Например, число **ячеек прототип** и **стиль** (обычный или сгруппированные): 

    [![](table-views-images/table11.png "Вкладка мини-приложения")](table-views-images/table11.png#lightbox)
1. Для каждого **ячейки прототип**, выделите его и назначить уникальный **идентификатор** в **мини-приложение** вкладке **свойства обозревателя**. Этот шаг является _очень важным_ как этот идентификатор понадобится позже при заполнении таблицы. Например `AttrCell`: 

    [![](table-views-images/table12.png "Вкладка мини-приложения")](table-views-images/table12.png#lightbox)
1. Можно также выбрать для представления ячейки как один из [по умолчанию ячейки типов табличных представлений](#Table-View-Cell-Types) через **стиль** раскрывающийся список или присвойте ей значение **настраиваемый** и используйте область конструктора, чтобы макет ячейки путем перетаскивания в другие графические элементы пользовательского интерфейса из **элементов**: 

    [![](table-views-images/table13.png "Макет ячеек")](table-views-images/table13.png#lightbox)
1. Назначить уникальный **имя** для каждого элемента пользовательского интерфейса в макете прототип ячейки в **мини-приложение** вкладке **свойства обозревателя** , они будут доступны позже в коде C#: 

    [![](table-views-images/table14.png "Присвойте имя")](table-views-images/table14.png#lightbox)
1. Повторите то же самое для всех ячеек прототип в табличном представлении.
1. Затем назначить пользовательские классы для остальной части проекта пользовательского интерфейса, макет в представлении сведений и назначить уникальный **имена** для каждого элемента пользовательского интерфейса в области сведений просмотрите, чтобы вы могли обращаться к ним в C# также. Пример. 

    [![](table-views-images/table15.png "В макет пользовательского интерфейса")](table-views-images/table15.png#lightbox)
1. Сохраните изменения в раскадровку.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. В Visual Studio, создание нового проекта приложения tvOS и выберите **tvOS** > **одно представление приложение** и введите имя для вашего приложения. Нажмите кнопку **хорошо** кнопку, чтобы создать новое решение: 

    [![](table-views-images/table02-vs.png "Выберите приложение, одно представление")](table-views-images/table02-vs.png#lightbox)
1. В **обозревателе решений**, дважды щелкните `Main.storyboard` файл, чтобы открыть его в конструкторе iOS: 

    [![](table-views-images/table05-vs.png "Файл Main.storyboard")](table-views-images/table05-vs.png#lightbox)
1. Выберите и удалите **представление по умолчанию контроллер**: 

    [![](table-views-images/table06-vs.png "Выберите и удалите контроллер представление по умолчанию")](table-views-images/table06-vs.png#lightbox)
1. Выберите **разбиение View Controller** из **элементов** и перетащите его на поверхность разработки: 

    [![](table-views-images/table07-vs.png "Контроллер разделение представления")](table-views-images/table07-vs.png#lightbox)
1. По умолчанию, вы сможете получить [разделенное представление](~/ios/tvos/user-interface/split-views.md) с **навигации View Controller** и **контроллер представление таблицы** в левой части и **View Controller** в правую часть. Это предлагается использовать Apple табличного представления в tvOS: 

    [![](table-views-images/table08-vs.png "Макет пользовательского интерфейса")](table-views-images/table08-vs.png#lightbox)
1. Необходимо будет выбрать каждой части представления таблицы и назначьте его пользовательский **имя класса** в **мини-приложение** вкладке **свойства обозревателя** доступа к нему позже в C# код. Например **таблицы View Controller**: 

    [![](table-views-images/table09-vs.png "Вкладка мини-приложения")](table-views-images/table09-vs.png#lightbox)
1. Необходимо создать пользовательский класс для **контроллер представление таблицы**, **представление таблицы** , а также **ячеек прототип**. Visual Studio для Mac добавит пользовательских классов в дереве проекта, создаются как: 

    [![](table-views-images/table10-vs.png "Пользовательские классы в дереве проекта")](table-views-images/table10-vs.png#lightbox)
1. Затем выберите представление таблицы в области конструктора и при необходимости измените его свойств. Например, число **ячеек прототип** и **стиль** (обычный или сгруппированные): 

    [![](table-views-images/table11-vs.png "Вкладка мини-приложения")](table-views-images/table11-vs.png#lightbox)
1. Для каждого **ячейки прототип**, выделите его и назначить уникальный **идентификатор** в **мини-приложение** вкладке **свойства обозревателя**. Этот шаг является _очень важным_ как этот идентификатор понадобится позже при заполнении таблицы. Например `AttrCell`: 

    [![](table-views-images/table12-vs.png "Назначьте идентификатор")](table-views-images/table12-vs.png#lightbox)
1. Можно также выбрать для представления ячейки как один из [по умолчанию ячейки типов табличных представлений](#Table-View-Cell-Types) через **стиль** раскрывающийся список или присвойте ей значение **настраиваемый** и используйте область конструктора, чтобы макет ячейки путем перетаскивания в другие графические элементы пользовательского интерфейса из **элементов**: 

    [![](table-views-images/table13-vs.png "В раскрывающемся списке стилей")](table-views-images/table13-vs.png#lightbox)
1. Назначить уникальный **имя** для каждого элемента пользовательского интерфейса в макете прототип ячейки в **мини-приложение** вкладке **свойства обозревателя** , они будут доступны позже в коде C#: 

    [![](table-views-images/table14-vs.png "Вкладка мини-приложения")](table-views-images/table14-vs.png#lightbox)
1. Повторите то же самое для всех ячеек прототип в табличном представлении.
1. Затем назначить пользовательские классы для остальной части проекта пользовательского интерфейса, макет в представлении сведений и назначить уникальный **имена** для каждого элемента пользовательского интерфейса в области сведений просмотрите, чтобы вы могли обращаться к ним в C# также. Пример. 

    [![](table-views-images/table15.png "В макет пользовательского интерфейса")](table-views-images/table15.png#lightbox)
1. Сохраните изменения в раскадровку.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Разработка модели данных

Упрощают работу с данными, которые облегчают отображает представление таблицы и для упрощения предоставления подробной информации (как пользователь выбрал или выделяет строки в представлении таблицы), создайте пользовательский класс или классы в качестве модели данных для получения сведений .

Рассмотрим в качестве примера приложение бронирования путешествия, которое содержит список **города**, каждый, содержащий уникальный список **сеанса** , пользователь может выбрать. Пользователь получит возможность пометить притяжения как *избранные*, выберите, чтобы получить *направлениях* для притяжения и *бронирования авиабилетов* до заданного города.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Создание модели данных для **притяжения**, щелкните правой кнопкой мыши имя проекта в **Pad решения** и выберите **добавить** > **новый файл...** . Введите `AttractionInformation` для **имя** и нажмите кнопку **New** кнопки: 

[![](table-views-images/data01.png "Введите AttractionInformation имени")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Создание модели данных для **притяжения**, щелкните правой кнопкой мыши имя проекта в **обозревателе решений** и выберите **добавить** > **новый элемент ...** . Выберите **класса** и введите `AttractionInformation` для **имя** и нажмите кнопку **добавить** кнопки: 

[![](table-views-images/data01-vs.png "Выберите класс и введите AttractionInformation имени")](table-views-images/data01-vs.png#lightbox)

-----

Изменить `AttractionInformation.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Этот класс предоставляет свойства для хранения сведений о данной **притяжения**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

После этого щелкните правой кнопкой мыши имя проекта в **Pad решения** еще раз и выберите **добавить** > **новый файл...** . Введите `CityInformation` для **имя** и нажмите кнопку **New** кнопки: 

[![](table-views-images/data02.png "Введите CityInformation имени")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

После этого щелкните правой кнопкой мыши имя проекта в **обозревателе решений** еще раз и выберите **добавить** > **новый элемент...** . Введите `CityInformation` для **имя** и нажмите кнопку **добавить** кнопки: 

[![](table-views-images/data02-vs.png "Введите CityInformation имени")](table-views-images/data02-vs.png#lightbox)

-----

Изменить `CityInformation.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Этот класс содержит все сведения о назначении **Город**, коллекцию **сеанса** для такого города и предоставляет два вспомогательных метода (`AddAttraction`) для упрощения добавления сеанса для Город.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Просмотр источников данных таблицы

Каждое представление таблицы требуется источник данных (`UITableViewDataSource`) для предоставления данных для таблицы и формировать необходимые строки как предусмотренного в табличном представлении.

В приведенном выше примере щелкните правой кнопкой мыши имя проекта в **обозревателе решений**выберите **добавить** > **новый файл...**  и вызывает его `AttractionTableDatasource` и нажмите кнопку **New** кнопку, чтобы создать. Далее следует изменить `AttractionTableDatasource.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Давайте рассмотрим несколько частей класса подробно.

Во-первых мы определения константу для хранения уникальный идентификатор ячейки прототип (это тот же идентификатор, назначенный в конструкторе интерфейса выше), добавить ярлык View Controller таблицы и создания хранилища данных:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Далее мы сохранить контроллер представление таблицы, а затем постройте и заполнить нашему источнику данных (с использованием модели данных, определенного выше) при создании класса:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Для примера `PopulateCities` метод просто создает объекты модели данных в памяти, однако они легко считываться из базы данных или веб-службы в реальном приложении:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections` Метод возвращает количество секций в таблице:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Для **Plain** со стилем таблицы представления, всегда возвращают значение 1.

`RowsInSection` Метод возвращает количество строк в текущем разделе:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Опять же, для **Plain** представления таблиц возвращает общее число элементов в источнике данных.

`TitleForHeader` Метод возвращает заголовок для заданного раздела:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Для **Plain** таблицы представления типа, не указывайте заголовок (`""`).

Наконец, при запросе таблицы представления, создать и заполнить ячейки прототип с помощью `GetCell` метод: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Дополнительные сведения о работе с `UITableViewDatasource`, см. в разделе Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) документации.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Делегат представление таблицы

Каждое представление таблицы требует делегата (`UITableViewDelegate`) реагировать на взаимодействие с пользователем или другие системные события, в таблице.

В приведенном выше примере щелкните правой кнопкой мыши имя проекта в **обозревателе решений**выберите **добавить** > **новый файл...**  и вызывает его `AttractionTableDelegate` и нажмите кнопку **New** кнопку, чтобы создать. Далее следует изменить `AttractionTableDelegate.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Давайте рассмотрим несколько разделов этого класса в сведениях.

Во-первых мы создадим ярлык для контроллера представления таблицы при создании класса:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Затем, при выборе строки (пользователь щелкает Touch в области на удаленном Apple) необходимо пометить **притяжения** представленной выбранной строки в список избранного:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Далее, когда пользователь выделяет строку (передав его с помощью Apple удаленного Touch области фокуса) необходимо предоставить сведения о **притяжения** представляемое данной строкой, в разделе "Сведения" представление нашей разбиение контроллер:

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow` Метод вызывается для каждой строки, который должен получить фокус в табличном представлении. Вернуть `true` Если строка получает фокус, в противном случае возвращает `false`. В случае в этом примере мы создали пользовательский `AttractionHighlighted` событие, которое будет вызвано для каждой строки при получении фокуса.

Дополнительные сведения о работе с `UITableViewDelegate`, см. в разделе Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) документации.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Представление ячейки таблицы

Для каждой ячейки прототипа, добавлены в представление таблицы в конструкторе интерфейса, вы создали пользовательский экземпляр представление ячейки таблицы (`UITableViewCell`) для заполнения новой ячейки (строка), как он создан.

Для примера приложения, дважды щелкните `AttractionTableCell.cs` файл, чтобы открыть его для редактирования и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Этот класс предоставляет хранилище для объекта модели данных притяжения (`AttractionInformation` как указано выше) отображается в данной строке:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI` Метод заполняет пользовательского интерфейса мини-приложений (которые были добавлены прототип ячейки в конструкторе интерфейса) при необходимости:

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Дополнительные сведения о работе с `UITableViewCell`, см. в разделе Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) документации.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Контроллер представление таблицы

Контроллер представление таблицы (`UITableViewController`) управляет представление таблицы, которая была добавлена в раскадровку через интерфейс конструктора.

Для примера приложения, дважды щелкните `AttractionTableViewController.cs` файл, чтобы открыть его для редактирования и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Давайте подробнее рассмотрим этот класс. Во-первых, мы создали ярлыки для упрощения доступа к представлению таблицы `DataSource` и `TableDelegate`. Мы будем использовать их позже для обмена данными между таблицы представления в левой части разделенного представления и Просмотр подробностей в правом.

Наконец, при представлении загружается в память, мы создаем экземпляры `AttractionTableDatasource` и `AttractionTableDelegate` (как созданных ранее) и присоединить их к представлению таблицы.

Дополнительные сведения о работе с `UITableViewController`, см. в разделе Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) документации.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Полное извлечение все вместе

Как уже говорилось в начале этого документа, представления таблиц обычно отображаются в одной из сторон [разделенное представление](~/ios/tvos/user-interface/split-views.md) как навигация с подробными сведениями выбранного элемента, отображаемого на противоположной стороне. Пример: 

[![](table-views-images/intro01.png "Запустите образец приложения")](table-views-images/intro01.png#lightbox)

Так как это стандартный шаблон в tvOS, давайте взглянем на заключительных этапах для объединения все, что и у левой и правой сторон представление с разделением, взаимодействуют друг с другом.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Подробное представление

Для примера приложения движения, представленных выше пользовательский класс (`AttractionViewController`) определяется для стандартного представления контроллера, представленных в правой части разделенное представление как представление сведений:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Здесь мы предоставили **притяжения** (`AttractionInformation`) производится отображается как свойство и создание `UpdateUI` метод, который заполняет мини-приложения пользовательского интерфейса добавляются в представление в конструкторе интерфейса.

Мы определили ярлык на контроллер представление Split (`SplitView`), мы будем использовать для обмена данными изменения в представлении (`AcctractionTableView`).

Настраиваемые действия (события) и, наконец, были добавлены три `UIButton` экземпляры, созданные в конструкторе интерфейса, разрешить пользователю пометить притяжения как _избранные_, получить _направлениях_ для притяжения и _бронирования авиабилетов_ до заданного города.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Представление контроллер навигации

Поскольку контроллер представление таблицы, вложенные в контроллере представление навигации в левой части разделенного представления, View Controller навигации был назначен пользовательский класс (`MasterNavigationController`) в конструкторе интерфейса и определяются следующим образом:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Еще раз этот класс просто определяет несколько клавиш для упрощения взаимодействия через две стороны View Controller разбиение:

* `SplitView` -Представляет собой ссылку на разбиение View Controller (`MainSpiltViewController`), которому принадлежит контроллер представления навигации.
* `TableController` — Возвращает контроллер представление таблицы (`AttractionTableViewController`), будет представлено в виде представления сверху в контроллере представления навигации.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Разбиение View Controller

Так как представление контроллер разбиение базовый наше приложение, мы создали пользовательский класс (`MasterSplitViewController`) для него в конструкторе интерфейса и применяются следующие определения:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Во-первых, мы создавать ярлыки для **сведения** сторона разделенное представление (`AttractionViewController`) и **Master** стороны (`MasterNavigationController`). Опять же это упрощает для обмена данными между двумя сторонами позже.

Далее при представлении с разделением загружается в память, мы присоединения View Controller разбиение обеим сторонам разделенное представление и реагировать на пользователя, выделение притяжения в табличном представлении (`AttractionHighlighted`), отображая новое притяжения в **подробные сведения**  сторона представление с разделением.

См. в разделе [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) образец приложения для полной реализации представления таблиц в одном разделенном представлении.

## <a name="table-views-in-detail"></a>Представления таблиц подробно

Поскольку tvOS основываются на iOS, представления и таблицы таблицы Просмотр контроллеров разработаны и ведут себя таким же образом. Более подробные сведения о работе с представлением таблицы в приложение Xamarin, см. в разделе iOS [работа с таблицами и ячейками](~/ios/user-interface/controls/tables/index.md) документации.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован проектирование и работа с представлениями таблицы внутри приложения Xamarin.tvOS. И представил пример работы с представлением таблицы в представление с разделением, который широко распространенный способ табличного представления в приложении tvOS.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
