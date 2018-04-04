---
title: Изменения платформы iOS дополнительных 10
description: В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для iOS 10.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 33852ef62bd00368ef6544d07e60dd6de4c3b7d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="additional-ios-10-frameworks-changes"></a>Изменения платформы iOS дополнительных 10

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для iOS 10._

## <a name="av-foundation-framework-additions"></a>Дополнения Framework AV Foundation

Платформа AVFoundation включает следующие улучшения:

- В iOS 10 разработчику больше не нужно реализовывать различные [AVPlayerItem](https://developer.xamarin.com/api/type/AVFoundation.AVPlayerItem/) поведения, основанный на типе содержимого. Просто установите `Rate` свойство и AVFoundation определит, когда недостаточно содержимое доступно для воспроизведения без остановился.
- Новый [AVCapturePhotoOutput](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureFileOutput/) класс заменяет устаревшие `AVCaptureStillImageOutput` класса и предоставляет единый метод для обработки всех рабочих процессов фотографии, предоставляя расширенного управления и наблюдение за процессом отслеживания и Поддержка новых возможностей, таких как Live фотографии и формат RAW отслеживания.
- Новый `AVPlayerLooper` класс упрощает цикла данного фрагмента носителя во время воспроизведения.
- `AVAssetDownloadURLSession` Класс позволяет для загрузки и последующего воспроизведения FairPlay зашифрованы потоков HLS.
- По умолчанию [AVCaptureSession](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureSession/) класс автоматически поддерживает записи расширенных цвет, широким аппаратных устройств, поддерживающих ее. В разделе Apple [Справочник совместимости устройства iOS](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) для получения дополнительных сведений.

## <a name="avkit-additions"></a>AVKit дополнения

AVKit framework теперь включает новый `UpdatesNowPlayingInfoCenter` свойство, указывающее, когда панель сведений игральной теперь должны быть обновлены.

## <a name="core-data-enhancements"></a>Усовершенствования основных данных

iOS 10 включает следующие усовершенствования для платформы данных основных компонентов:

- [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) объектов с хранилищами данных SQLite в режиме с упреждающей Записью журнала поддержки создания нового запроса компонентов, где управляемого объекта контексты Майкрософт можно прикрепить к версии конкретной базы данных для следующей выборки и Ошибка транзакции.
- Корневой [NSManagedObjectContext](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectContext/) объектов поддерживает параллельных сбой и выборка без сериализации.
- [NSPersistentStoreCoordinator](https://developer.xamarin.com/api/type/CoreData.NSPersistentStoreCoordinator/) класс поддерживается пул хранилищ данных SQLite.
- Были добавлены несколько новых удобных методов `NSManagedObject` упрощая процесс для выполнения выборки и создавать подклассы.
- С помощью высокоуровневые `NSPersistenceContainer` ссылка `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.xamarin.com/api/type/CoreData.NSManagedObjectModel/) и других ресурсов настройки основных данных.

Дополнительные сведения см. в разделе Apple [Framework справочные сведения об основных данных](https://developer.apple.com/reference/coredata).

## <a name="core-image-enhancements"></a>Основные усовершенствования изображения

iOS 10 вносит следующие усовершенствования для образа Core framework:

- Разработчику возможность обрабатывать изображения в цветовом пространстве вне контекста образ Core рабочее цветовое пространство путем преобразования в и из него цветовое пространство до и после обработки.
- Формат RAW изображения теперь поддерживается для устройств iOS, использующие A8 или A9 ЦП. Образ Core теперь обеспечивает поддержку для декодирования образы НЕОБРАБОТАННЫЕ либо из встроенных iSight камеры или с камеры сторонних производителей. Используйте `FilterWithImageData` или `FilterWithImageURL` методы [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) класса для обработки НЕОБРАБОТАННЫХ изображения.
- Были внесены некоторые улучшения производительности отрисовки `UIImage` визуализации (когда поддерживаемый образ Core изображения магазинов) в `UIImageView` объектов. 
- `UIImage` объекты с тегами расширенных гамма будет отображаться как расширенных охвата цвет в `UIImageView` объектов на устройствах iOS, которые поддерживает широкую цветовую.
- Основного образа ядра кода могут запросить определенный пиксель выходных форматов.
- `ImageWithExtent` Метод [CIFilter](https://developer.xamarin.com/api/type/CoreImage.CIFilter/) класс может использоваться для вставки пользовательской обработки в операцию фильтрации. Образ Core будет от фильтров при обработке изображения для вывода данный обратный вызов или отображения.

Кроме того были добавлены следующие новые фильтры образа ядра:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Дополнения движения Core

Новые для iOS 10 движения Core framework включает pedometer события, которые включить приложение для быстрого, получения уведомлений в режиме реального времени приостановки и возобновления отслеживания во время работы пользователя. Используйте [CMPedometer](https://developer.xamarin.com/api/type/CoreMotion.CMPedometer/) для регистрации событий pedometer основной или фоновой.

## <a name="foundation-enhancements"></a>Усовершенствования Foundation

В платформе Foundation для iOS 10 были внесены следующие улучшения:

- Используйте новую [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) класс форматирование локализованных измерения для отображения для конечного пользователя.
- Используйте новую [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) класса, чтобы дата и время вычисления интервал, например длительность, для сравнения интервалов и тестирование для пересечения интервал.
- Используйте новую [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) класса для преобразования между различных единиц из мер (единица Измерения) или выполняют вычисления над значениями в разных UOMs.

- Используйте новую [NSUnit](https://developer.apple.com/reference/foundation/nsunit) и [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) классы для представления определенных UOMs.
- Были добавлены несколько новых свойств [NSLocal](https://developer.apple.com/reference/foundation/nslocale) класса для получения информации и отображение доступных форматов.

## <a name="gamekit-enhancements"></a>Усовершенствования GameKit

Для платформы GameKit на iOS 10 были внесены следующие улучшения:

- **Приложения Game Center** устаревшими и удалены из операций ввода-вывода. Если приложение использует GameKit, он _должен_ предоставить свой собственный интерфейс для отображения GameKit функции, такие как списки лидеров и т. д. 
- Реализован новый тип учетной записи только для iCloud [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) класса.
- Новый [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) класс предоставляет обобщенный решения для управления постоянного хранилища данных на Game Center. `GKGameSession` поддерживает список проигрывателей и приложение отвечает за реализацию как и когда участника дата хранится, извлечь или обмениваются проигрыватели. Во многих случаях игровые сеансы можно заменить существующих совпадений, основанных, в режиме реального времени соответствует игры постоянного сохранения методы.

## <a name="gameplaykit-enhancements"></a>Усовершенствования GameplayKit

Для платформы GameplayKit на iOS 10 были внесены следующие улучшения:

- Используйте новую [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) класса для обеспечения высокой производительности, выглядящих естественного пути.
- Создание процедурных шума был добавлен и может использоваться для улучшения реалистичности в поиск естественных текстур, добавьте реалистичности движение камеры, а также способствует увеличению полнофункциональной игровой среды.
- Используйте Пространственные секционирования для секционирования данных игровой среды, для эффективного поиска.
- Новый Монте-Карло специалист по разработке стратегий ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) была добавлена для вычисления исчерпывающим возможности перемещения.
- Поддержка трехмерного представления будет добавлен существующий агент и поведения путь поиска, с помощью нового [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) классы.
- Новый [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) и [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) объединение GameplayKit и проще, чем когда-либо SpriteKit создание классов.
- Был добавлен новый API дерева принятия решений ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения AI создания игры.

## <a name="healthkit-enhancements"></a>Усовершенствования HealthKit

Для платформы HealthKit на iOS 10 были внесены следующие улучшения:

- Были добавлены новые разделы метаданных для типов погоды (такие как `HKWeatherConditionClear` и `HKWeatherConditionCloudy`) и тренировки типы (такие как `HKWorkoutActivityTypeFlexibility` и `HKWorkoutActivityTypeWheelchairRunPace`) были добавлены.
- Новый `HKCDADocument` был добавлен класс для представления клинических архитектура документа (CDA) в формате документа.
- Используйте новую [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) класс, чтобы указать `ActivityType` и `LocationType` для тренировки.
- Новый [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) и `WheelchairUse` метод [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) класс были добавлены для работы с инвалидных связанных данных о работоспособности.

## <a name="homekit-enhancements"></a>Усовершенствования HomeKit

Для платформы HomeKit на iOS 10 были внесены следующие улучшения:

- Были добавлены новые службы и характеристики.
- Можно настроить iPad должен использоваться в качестве концентратора HomeKit для предоставления удаленного доступа периферийных устройств, выполнения триггеров автоматизации и включить общие разрешения пользователя.
- Добавлена поддержка стандартные камеры и дверного звонка.
- Для аксессуаров были предоставлены контекста и конфигураций.

См. в нашем [введение в HomeKit](~/ios/platform/homekit.md) Дополнительные сведения см.

## <a name="metal-enhancements"></a>Усовершенствования системы

Для исходного framework в iOS 10 были внесены следующие улучшения:

- Теперь можно использовать трехмерных приложениях и играх _тесселяции_ эффективно визуализации сложного изображения и geometry через графический Процессор.
- Обеспечивают точное управление выделения ресурсов для оптимизации производительности системы на основе приложений с помощью ресурсов кучи и Memoryless целевых буферов визуализации.
- Используйте специализацию функции для создания коллекции оптимизирован материал и светлой сочетание функций для сцены.

Чтобы получить дополнительные сведения см. в разделе Apple [руководство по программированию исходного](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

## <a name="modelio-enhancements"></a>Усовершенствования ModelIO

Для платформы ModelIO на iOS 10 были внесены следующие улучшения:

 - Теперь поддерживается формат файла долл. США.
 - Подпись поля расстояние, добавлена поддержка [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) класса.
 - Используйте новую `MDLLightProbeIrradianceDataSource` класс, помогающий при размещении Light проверки.
 - Используйте новую `MDLMaterialPropertyGraph` класс для поддержки среды выполнения изменения модели.

## <a name="photos-enhancements"></a>Усовершенствования фотографий

Для платформы фотографии на iOS 10 были внесены следующие улучшения:

- Используйте [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) и [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) классов, чтобы воспользоваться преимуществами нового образа ядра процессора для выполнения изменения.
- Интерактивного редактирования фото теперь доступен для приложений, поддерживающих платформу фотографии и редактирования расширения фотографий (для использования внутри фотографии и камеры приложений).
- Используйте новую [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) класс применять изменения к видео и по-прежнему содержимое Live фотографий.

## <a name="replaykit-enhancements"></a>Усовершенствования ReplayKit

Для платформы ReplayKit на iOS 10 были внесены следующие улучшения:

- Используйте [RPScreenRecorder](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [RPBroadcastActivityViewController](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) и [RPBroadcastController](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) классы для поддержки передачи широковещательных сообщений из записанных содержимое с помощью сторонних производителей сайты.
- Расширения пользовательского интерфейса широковещательной рассылки и отправить вещания требуются для поддержки службы широковещательных ReplayKit сторонних производителей в приложении.

## <a name="scenekit-enhancements"></a>Усовершенствования SceneKit

Для платформы SceneKit на iOS 10 были внесены следующие улучшения:

- [SCNCamera](https://developer.xamarin.com/api/type/SceneKit.SCNCamera/) класс может предоставить больше реалистичности с помощью функций HDR и эффекты. Используйте адаптивной раскрытия для создания автоматического эффекты или используйте виньетирование, цветной каймы и градации цвета для добавления fillmatic эффектов в игру.
- SceneKit теперь включает новую систему физически основе отрисовки (PBR) для более точные результаты при использовании более простые средства разработки.
- Используйте новую [SCNLightingModelPhysicallyBased](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) заливки модели продукта широкий спектр реалистичной заливки эффекты при этом не только три основные свойства (`Diffuse`, `Metalness` и `Roughness`).
- С момента PBR заливки работает лучше всего с освещение на основе среды, используйте `LightingEnvironment` свойств на основе образа освещения всей сцены.
- Используйте `IESProfileURL` свойство для импорта приспособления реального мира, определяющие освещение на основе реальных значений как интенсивности (в люмен) и температуры цвета (в градусах Кельвина).
- Функции камеры PBR и HDR обеспечивают лучшие результаты, чем методы традиционной отрисовки, и в результате SceneKit теперь выполняет все вычисления цвета в линейном цветовом пространстве (с помощью цветового P3 отображает цвет всей устройства).
- Цвет SceneKit теперь соответствует все цвета, считывая данные профиля цвета.
- SceneKit интерпретирует значения компонента цвета в линейной цветовое пространство RGB для всех типов шейдера.
- Оба линейной цвет пространства визуализации и расширенными цвет можно отключить, задав `SCNDisableLinearSpaceRendering` и `SCNDisableWideGamut` ключи в приложении `Info.plist`.
- Построение primates произвольный многоугольник (или загрузить из файлов или создано программным путем) для указания geometry с новым [SCNGeometryPrimitiveTypePolygon](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) класса.
- Поскольку SceneKit считывает и настроить цвета данных профиля в изображения текстуры, используйте средства каталоги для всех образов чтобы убедиться, что эти сведения предоставляются.

## <a name="spritekit-enhancements"></a>Усовершенствования SpriteKit

Для платформы SpriteKit на iOS 10 были внесены следующие улучшения:

- Пользовательские шейдеров может предоставить атрибуты (`SKAttribute`), можно настроить отдельно с каждого узла, который использует шейдер, указав значение атрибута (`SKAttributeValue`).
- Tilemaps теперь поддерживают фигур квадрат, шестиугольник и изометрическая плитки для 2D, 2,5 D и прокрутка стороны игр с помощью `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` и `SKTileSet` классы.
- Используйте новую `SKWarpGeometry` класса необходимо растянуть или исказить [SKSpriteNode](https://developer.xamarin.com/api/type/SpriteKit.SKSpriteNode/) или [SKEffectNode](https://developer.xamarin.com/api/type/SpriteKit.SKEffectNode/) подготовки к просмотру. Новый [SKAction](https://developer.xamarin.com/api/type/SpriteKit.SKAction/) класс может использоваться для переходов между warp эффектов анимации.
- [SKView](https://developer.xamarin.com/api/type/SpriteKit.SKView/) класс предоставляет новые методы для предоставления тонкую настройку времени и способа сцены визуализируется.

## <a name="scrollview-enhancements"></a>Усовершенствования ScrollView

ScrollView управления в iOS 10.3 были внесены следующие улучшения:

- `UIScrollView` Теперь включают `IndexDisplayMode` свойства для управления отображением индекс при прокрутке пользователем как `UIScrollViewIndexDisplayMode` из:
    - `Automatic` -Отображение контролируется Операционной системой.
    - `AlwaysHidden` -Отображение всегда является скрытым.

В разделе [iOSTenThree пример](https://developer.xamarin.com/samples/monotouch/iOS10/iOSTenThree) для использования.

## <a name="uikit-enhancements"></a>Усовершенствования UIKit

Для платформы UIKit на iOS 10 были внесены следующие улучшения:

- Новый [UIPasteboard](https://developer.xamarin.com/api/type/UIKit.UIPasteboard/) API предоставляет новые возможности (например, время существования ограничениями) и будет автоматически объявляют совместимых типов содержимого для распространенных типов класса.
- Поддержка новых полностью интерактивный, основанных на объектах, прерываемых анимации был добавлен и могут быть связаны с помощью жестов. Pleas см. в разделе Apple [ссылка протокола UIViewAnimating](https://developer.apple.com/reference/uikit/uiviewanimating), [ссылку на класс UIViewPropertyAnimator](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider протокола ссылка](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [Ссылку на класс UICubicTimingParameters](https://developer.apple.com/reference/uikit/uicubictimingparameters) и [UISpringTimingParameter ссылку на класс](https://developer.apple.com/reference/uikit/uispringtimingparameters) для получения дополнительной информации.
- Новый `UIPreviewInteraction` и `UIPreviewInteractionDelegate` разрешить приложению разработчика для предоставления пользовательского интерфейса для операций считывания и pop.
- Новый `UIAccessibilityCustomRotor` класс позволяет приложению предоставлять пользовательские контекстные набор функциональных возможностей вспомогательных технологий, таких как голоса через.
- Используйте `UIAccessibilityIsAssistiveTouchRunning` и `UIAccessibilityAssistiveTouchStatusDidChangeNotification` символов, чтобы определить, включена ли AssistiveTouch.
- Используйте `UIAccessibilityHearingDevicePairedEar` и `UIAccessibilityHearingDevicePairedEarDidChangeNotification` символов, чтобы получить состояние любого пару помогает слуха периферийные устройства.
- Для поддержки динамического типа подписей для текстовых полей и текстовых полей используйте новый `PreferredFontForTextStyle` метод `UIFont` класса.
- Чтобы решить, если элемент должен обновить его шрифта при устройства `UIContentSizeCategory` изменения, используйте `AdjustsFontForContentSizeCategory` свойство `UIContentSizeCategoryAdjusting` делегата.
- `OpenURL` Метод `UIApplication` класса вызывается асинхронно и теперь поддерживает обработчик завершения, который вызывается после завершения активное действие.
- Инициировать CloudKit совместное использование и изменение его свойств, с использованием нового `UICloudSharingController` и `UICloudSharingControllerDelegate` классы.
- Воспользоваться преимуществами предварительно загруженными ячейки для прокрутки повышают `UICollectionViews` с новым `UICollectionViewDataSourcePrefetching` делегата.
- Разработчик теперь можно управлять внешним видом эмблемы для вкладки панели элементов (например, цвет текста и фона).
- Обновление элемента управления теперь поддерживается во всех представлений прокрутки и прокрутки представления подклассов (такие как `UICollectionView`).

## <a name="webkit-enhancements"></a>Усовершенствования WebKit

Для платформы WebKit на iOS 10 были внесены следующие улучшения:

- Peek и поддержки pop будет добавлен `WKWebView` класса. Используйте `ShouldPreviewElement` метод, чтобы определить, должен ли отображаться Предварительный просмотр для данной веб-просмотра.


## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Новые возможности iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
