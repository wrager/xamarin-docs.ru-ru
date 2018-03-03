---
title: "Изменения платформы дополнительных tvOS 10"
description: "В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для tvOS 10."
ms.topic: article
ms.prod: xamarin
ms.assetid: F771640A-F92E-4954-82D5-2D720434971E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 004abfffd3a100b7a25a9647fe233fd676f61143
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="additional-tvos-10-frameworks-changes"></a>Изменения платформы дополнительных tvOS 10

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для tvOS 10._


Помимо основных изменений для tvOS Apple внес изменения и усовершенствования несколько существующих инфраструктур в tvOS 10.

<a name="AV-Foundation-Framework" />

## <a name="av-foundation-framework-additions"></a>Дополнения Framework AV Foundation

Платформа AVFoundation включает следующие улучшения:

 - В tvOS 10 приложение больше не поддерживает разные [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) поведения, основанный на типе содержимого. Просто установите `Rate` свойство и AVFoundation определит, когда недостаточно содержимое доступно для воспроизведения без остановился.
 - Новый `AVPlayerLooper` класс упрощает цикла данного фрагмента носителя во время воспроизведения.

<a name="AVKit-Framework-Enhancements" />

## <a name="avkit-framework-enhancements"></a>Усовершенствования AVKit Framework

Платформа AVKit включает следующие улучшения:

 - Приложение теперь имеет контроль над пропуск поведение [AVPlayerViewController](https://developer.apple.com/reference/avkit/avplayerviewcontroller) , пропуская жест возможно перемещение к следующему элементу в список воспроизведения или предварительное внутри текущего элемента.

<a name="Core-Data-Enhancements" />

## <a name="core-data-enhancements"></a>Усовершенствования основных данных

tvOS 10 включает следующие усовершенствования для платформы данных основных компонентов:

 - Корневой [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов поддерживает параллельных сбой и выборка без сериализации.
 - [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) класс поддерживается пул хранилищ данных SQLite.
 - [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов с хранилищами данных SQLite в режиме с упреждающей Записью журнала поддержки создания нового запроса компонентов, где управляемого объекта контексты Майкрософт можно прикрепить к версии конкретной базы данных для следующей выборки и Ошибка транзакции.
 - С помощью высокоуровневые `NSPersistenceContainer` ссылка `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и других ресурсов настройки основных данных.
 - Были добавлены несколько новых удобных методов `NSManagedObject` упрощая процесс для выполнения выборки и создавать подклассы.

Дополнительные сведения см. в разделе Apple [Framework справочные сведения об основных данных](https://developer.apple.com/reference/coredata).

<a name="Core-Graphics-Enhancements" />

## <a name="core-graphics-enhancements"></a>Основные усовершенствования графики

tvOS 10 включает следующие усовершенствования для платформы графики основных компонентов:

 - Новый [CGColorConverterRef](https://developer.apple.com/reference/coregraphics/cgcolorconverterref) класс может использоваться для выполнения ряда преобразования цветов.

<a name="Core-Image-Enhancements" />

## <a name="core-image-enhancements"></a>Основные усовершенствования изображения

tvOS 10 вносит следующие усовершенствования для образа Core framework:

 - `ImageWithExtent` Метод [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) класс может использоваться для вставки пользовательской обработки в операцию фильтрации. Образ Core будет от фильтров при обработке изображения для вывода данный обратный вызов или отображения.
 - Теперь приложение может обрабатывать изображения в цветовом пространстве вне контекста образ Core рабочее цветовое пространство путем преобразования в и из него цветовое пространство до и после обработки.
 - Были внесены некоторые улучшения производительности отрисовки `UIImage` визуализации (когда поддерживаемый образ Core изображения магазинов) в `UIImageView` объектов. 
 - `UIImage` объекты с тегами расширенных гамма будет отображаться как расширенных охвата цвет в `UIImageView` объектов на устройствах iOS, которые поддерживает широкую цветовую.
 - Основного образа ядра кода могут запросить определенный пиксель выходных форматов.

Кроме того были добавлены следующие новые фильтры образа ядра:

 - `CINinePartTiled`
 - `CINinePartStretched`
 - `CIHueSaturationValueGradient`
 - `CIEdgePreserveUpsampleFilter`
 - `CIClamp`

<a name="Foundation-Enhancements" />

## <a name="foundation-enhancements"></a>Усовершенствования Foundation

В платформе Foundation для tvOS 10 были внесены следующие улучшения:

 - Используйте новую [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) класса, чтобы дата и время вычисления интервал, например длительность, для сравнения интервалов и тестирование для пересечения интервал.
 - Были добавлены несколько новых свойств [NSLocal](https://developer.apple.com/reference/foundation/nslocale) класса для получения информации и отображение доступных форматов.
 - Используйте новую [NSMeasuerment](https://developer.apple.com/reference/foundation/nsmeasurement) класса для преобразования между различных единиц из мер (единица Измерения) или выполняют вычисления над значениями в разных UOMs.
 - Используйте новую [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) класс форматирование локализованных измерения для отображения для конечного пользователя.
 - Используйте новую [NSUnit](https://developer.apple.com/reference/foundation/nsunit) и [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) классы для представления определенных UOMs.

<a name="GameKit-Enhancements" />

## <a name="gamekit-enhancements"></a>Усовершенствования GameKit

Для платформы GameKit на tvOS 10 были внесены следующие улучшения:

 - Реализован новый тип учетной записи только для iCloud [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) класса.
 - Новый [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) класс предоставляет обобщенный решения для управления постоянного хранилища данных на Game Center. `GKGameSession` поддерживает список проигрывателей и приложение является ответственным формой реализации, как и когда хранится Дата участника, извлечь или обмениваются проигрыватели. Во многих случаях игровые сеансы можно заменить существующих совпадений, основанных, в режиме реального времени соответствует игры постоянного сохранения методы.

<a name="GameplayKit-Enhancements" />

## <a name="gameplaykit-enhancements"></a>Усовершенствования GameplayKit

Для платформы GameplayKit на tvOS 10 были внесены следующие улучшения:

 - Создание процедурных шума был добавлен и может использоваться для улучшения реалистичности в поиск естественных текстур, добавьте реалистичности движение камеры, а также способствует увеличению полнофункциональной игровой среды.
 - Используйте Пространственные секционирования для секционирования данных игровой среды, для эффективного поиска.
 - Новый Монте-Карло специалист по разработке стратегий ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) была добавлена для вычисления исчерпывающим возможности перемещения.
 - Был добавлен новый API дерева принятия решений ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения AI создания игры.
 - Поддержка трехмерного представления будет добавлен существующий агент и поведения путь поиска, с помощью нового [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) классы.
 - Используйте новую [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) класса для обеспечения высокой производительности, выглядящих естественного пути.
 - Новый [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) и [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) объединение GameplayKit и проще, чем когда-либо SpriteKit создание классов.

<a name="Metal-Enhancements" />

## <a name="metal-enhancements"></a>Усовершенствования системы

Для исходного framework в tvOS 10 были внесены следующие улучшения:

 - Теперь можно использовать трехмерных приложениях и играх _тесселяции_ эффективно визуализации сложного изображения и geometry через графический Процессор.
 - Используйте специализацию функции для создания коллекции оптимизирован материал и светлой сочетание функций для сцены.
 - Обеспечивают точное управление выделения ресурсов для оптимизации производительности системы на основе приложений с помощью ресурсов кучи и Memoryless целевых буферов визуализации.

Чтобы получить дополнительные сведения см. в разделе Apple [руководство по программированию исходного](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="Metal-Performance-Shaders-Enhancements" />

## <a name="metal-performance-shaders-enhancements"></a>Улучшения производительности исходного шейдеров

В платформе шейдеров производительности системы в tvOS 10 были внесены следующие улучшения:

 - Были добавлены многие новые версии ядра шейдеров производительности системы структуру, которая позволяет приложению использовать преимущества оптимизированных высокой, параллельными данными вычислений, таких как операции нейронной сети и места преобразования цветов.

<a name="ModelIO-Enhancements" />

## <a name="modelio-enhancements"></a>Усовершенствования ModelIO

Для платформы ModelIO на tvOS 10 были внесены следующие улучшения:

 - Теперь поддерживается формат файла долл. США.
 - Используйте новую `MDLMaterialPropertyGraph` класс для поддержки среды выполнения изменения модели.
 - Подпись поля расстояние, добавлена поддержка [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) класса.
 - Используйте новую `MDLLightProbeIrradianceDataSource` класс, помогающий при размещении Light проверки.

<a name="SceneKit-Enhancements" />

## <a name="scenekit-enhancements"></a>Усовершенствования SceneKit

Для платформы SceneKit на tvOS 10 были внесены следующие улучшения:

 - SceneKit теперь включает новую систему физически основе отрисовки (PBR) для более точные результаты при использовании более простые средства разработки.
 - Используйте новую [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) заливки модели продукта широкий спектр реалистичной заливки эффекты при этом не только три основные свойства (`Diffuse`, `Metalness` и `Roughness`).
 - С момента PBR заливки работает лучше всего с освещение на основе среды, используйте `LightingEnvironment` свойств на основе образа освещения tan всей сцены.
 - Используйте `IESProfileURL` свойство для импорта приспособления реального мира, определяющие базовый освещения на реальных значений, таких как интенсивности (в люмен) и цвет температуры (в градусах Кельвина).
 - [SCNCamera](https://developer.apple.com/reference/scenekit/scncamera) класс может предоставить больше реалистичности с помощью функций HDR и эффекты. Используйте адаптивной раскрытия для создания автоматического эффекты или используйте виньетирование, цветной каймы и градации цвета для добавления filmatic эффектов в игру.
 - Функции камеры PBR и HDR обеспечивают лучшие результаты, чем методы традиционной отрисовки, и в результате SceneKit теперь выполняет все вычисления цвета в линейном цветовом пространстве (с помощью цветового P3 отображает цвет всей устройства).
 - Цвет SceneKit теперь соответствует все цвета, считывая данные профиля цвета.
 - SceneKit интерпретирует значения компонента цвета в линейной цветовое пространство RGB для всех типов шейдера.
 - Поскольку SceneKit считывает и настроить цвета данных профиля в изображения текстуры, используйте средства каталоги для всех образов чтобы убедиться, что эти сведения предоставляются.
 - Оба линейной цвет пространства визуализации и расширенными цвет можно отключить, задав `SCNDisableLinearSpaceRendering` и `SCNDisableWideGamut` ключи в приложении `Info.plist`.
 - Построение primates произвольный многоугольник (или загрузить из файлов или создано программным путем) для указания geometry с новым [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) класса.

<a name="SpriteKit-Enhancements" />

## <a name="spritekit-enhancements"></a>Усовершенствования SpriteKit

Для платформы SpriteKit на tvOS 10 были внесены следующие улучшения:

 - Tilemaps теперь поддерживают фигур квадрат, шестиугольник и изометрическая плитки для 2D, 2,5 D и прокрутка стороны игр с помощью `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` и `SKTileSet` классы.
 - Используйте новую `SKWarpGeometry` класса необходимо растянуть или исказить [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) или [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) подготовки к просмотру. Новый [SKAction](https://developer.apple.com/reference/spritekit/skaction) класс может использоваться для переходов между warp эффектов анимации.
 - Пользовательские шейдеров может предоставить атрибуты (`SKAttribute`), можно настроить отдельно с каждого узла, который использует шейдер, указав значение атрибута (`SKAttributeValue`).
 - [SKView](https://developer.apple.com/reference/spritekit/skview) класс предоставляет новые методы для предоставления тонкую настройку времени и способа сцены визуализируется.

<a name="UIKit-Enhancements" />

## <a name="uikit-enhancements"></a>Усовершенствования UIKit

Для платформы UIKit на tvOS 10 были внесены следующие улучшения:

 - Фокус API была расширена для поддержки фокус не представление элемента в дополнение к `UIViews`. Элементы, которые поддерживают фокус _должен_ реализовать `IUIFocusItem` интерфейса.
 - Новый `UIGraphicsRender` класс предоставляет объектно ориентированного метода создания растровых изображений или PDF-файлов из UIKit отрисовки или двухмерной графики и заменяет устаревшие `UIGraphicsBeginImageContext` метод.
 - `UIUserInterfaceStyle` Был добавлен класс, чтобы определить, какие темы пользовательского интерфейса (Темная или Светлая) в данный момент активна.
 - Добавлена поддержка нового полностью интерактивный, основанных на объектах, прерываемых анимации и фургон связываться с помощью жестов. Pleas см. в разделе Apple [ссылка протокола UIViewAnimating](https://developer.apple.com/reference/uikit/uiviewanimating), [ссылку на класс UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider протокола ссылка](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Ссылку на класс UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) и [UISpringTimingParameter ссылку на класс](https://developer.apple.com/reference/uikit/uispringtimingparameters) для получения дополнительной информации.
 - Новый `UIPreviewInteraction` и `UIPreviewInteractionDelegate` разрешить приложению приложения для предоставления пользовательского интерфейса для операций считывания и pop.
 - Новый `UIAccessibilityCustomRotor` класс позволяет приложению предоставлять пользовательские контекстные набор функциональных возможностей вспомогательных технологий, таких как голоса через.
 - Используйте `UIAccessibilityIsAssistiveTouchRunning` и `UIAccessibilityAssistiveTouchStatusDidChangeNotification` символов, чтобы определить, включена ли AssistiveTouch.
 - Используйте `UIAccessibilityHearingDevicePairedEar` и `UIAccessibilityHearingDevicePairedEarDidChangeNotification` символов, чтобы получить состояние любого пару помогает слуха периферийные устройства.
 - Новый [UIPasteboard](https://developer.apple.com/reference/uikit/uipasteboard) API предоставляет новые возможности (например, время существования ограничениями) и будет автоматически объявляют совместимых типов содержимого для распространенных типов класса.
 - Для поддержки динамического типа подписей для текстовых полей и текстовых полей используйте новый `PreferredFontForTextStyle` метод `UIFont` класса.
 - Чтобы решить, если элемент должен обновить шрифт при устройства `UIContentSizeCategory` изменения, используйте `AdjustsFontForContentSizeCategory` свойство `UIContentSizeCategoryAdjusting` делегировать.
 - Приложение теперь можно управлять внешним видом эмблемы для вкладки панели элементов, например цвет текста и фона.
 - Обновить элемент управления в теперь поддерживается во всех представлений прокрутки и прокрутки представления подклассов (такие как `UICollectionView`).
 - `OpenURL` Метод `UIApplication` класса вызывается асинхронно теперь поддерживает обработчик завершения, который вызывается после завершения открытия.
 - Инициировать CloudKit совместное использование и изменение его свойств, с использованием нового `UICloudSharingController` и `UICloudSharingControllerDelegate` классы.
 - Воспользоваться преимуществами предварительно загруженными ячейки для прокрутки повышают `UICollectionViews` с новым `UICollectionViewDataSourcePrefetching` делегата.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [Новые возможности tvOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
