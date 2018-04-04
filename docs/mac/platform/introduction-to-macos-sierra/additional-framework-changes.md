---
title: Дополнительные macOS изменения Сьерра Framework
description: В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для macOS Сьерра.
ms.prod: xamarin
ms.assetid: CA701269-D11E-4DE3-89C1-58EF8993A482
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a1bc12629a84e9a06cc80882d141bf6a0c2f0c52
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="additional-macos-sierra-framework-changes"></a>Дополнительные macOS изменения Сьерра Framework

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих инфраструктур для macOS Сьерра._

<a name="Accelerate-Framework-Enhancements" />

## <a name="accelerate-framework-enhancements"></a>Ускорение усовершенствования Framework

Ускорение методология macOS Сьерра были внесены следующие усовершенствования:

- Добавлены Quadrature (целочисленный математического анализа).
- Добавлены базовые функции для создания нейронных сетей.
- Добавлены геометрические функциях предиката для проверки на наличие таких вещей, как пересечения двух геометрических объектов.

<a name="AppKit-Framework-Enhancements" />

## <a name="appkit-framework-enhancements"></a>Усовершенствования Appkit Framework

Платформа AppKit для macOS Сьерра были внесены следующие усовершенствования:

- Несколько усовершенствований `NSCollectionView` такие как:
    - **Свертываемых** -пользователь может свернуть раздел представление коллекции в одну строку горизонтальной.
    - **С плавающей запятой заголовки** -верхние и нижние колонтитулы, теперь могут быть перемещается (в макет потока) с помощью такой же API, что [UICollectionView](https://developer.apple.com/reference/uikit/uicollectionview) в iOS.
    - **Прокручиваемый фон представления** -фон представления коллекции теперь можно указать значение для прокрутки вместе с содержимым.
- Оптимизирован и расширенных передачи отложенное представление макета.
- API-Интерфейс и перетащите теперь содержит новый `NSFilePromiseProvider` и `NSFilePromiseReceiver` классы для поддержки перетащите или косяк.
- Несколько конструкторов удобства были добавлены к существующим элементам управления:
    -  `NSButton` включает новые конструкторы для создания кнопок, флажков и переключателей.
    -  `NSTextField` включает новые конструкторы для создания метки без переноса и упаковки, редактируемые текстовые поля и атрибутами метки.
    -  `NSSegmentedControl` включает новые конструкторы для создания сегментированных элементов управления из группы меток или изображений.
    -  `NSSlider` включает новые конструкторы для создания горизонтальный линейный ползунки.
    -  `NSImageView` включает новые конструкторы для создания представления нередактируемые изображения из данного `NSImage`.
-  Новый `NSGridView` был добавлен в коллекцию представлений sub в сетку с переменной размера строки и столбцы, которые динамически скрыто или показано автоматический макет.

<a name="AVFoundation-Framework-Enhancements" />

## <a name="avfoundation-framework-enhancements"></a>Усовершенствования AVFoundation Framework

Платформа AVFoundation для macOS Сьерра были внесены следующие усовершенствования:

- В macOS, приложение больше не нужно реализовывать различные [AVPlayerItem](https://developer.apple.com/reference/avfoundation/avplayeritem) поведения, основанный на типе содержимого. Просто установите `Rate` свойство и AVFoundation определит, когда недостаточно содержимое доступно для воспроизведения без остановился.
- Новый `AVPlayerLooper` класс упрощает цикла данного фрагмента носителя во время воспроизведения.
- `AVAssetDownloadURLSession` Класс позволяет для загрузки и последующего воспроизведения FairPlay зашифрованы потоков HLS.

<a name="Core-Data-Framework-Enhancements" />

## <a name="core-data-framework-enhancements"></a>Усовершенствования Framework основных данных

Платформа данных основных компонентов для macOS Сьерра были внесены следующие усовершенствования:

- Корневой [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов поддерживает параллельных сбой и выборка без сериализации.
- [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) класс поддерживается пул хранилищ данных SQLite.
- [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) объектов с хранилищами данных SQLite в режиме с упреждающей Записью журнала поддержки создания нового запроса компонентов, где управляемого объекта контексты Майкрософт можно прикрепить к версии конкретной базы данных для следующей выборки и Ошибка транзакции.
- С помощью высокоуровневые `NSPersistenceContainer` ссылка `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) и других ресурсов настройки основных данных.
- Были добавлены несколько новых удобных методов `NSManagedObject` упрощая процесс для выполнения выборки и создавать подклассы.

Дополнительные сведения см. в разделе Apple [Framework справочные сведения об основных данных](https://developer.apple.com/reference/coredata).

<a name="Core-Image-Framework-Enhancements" />

## <a name="core-image-framework-enhancements"></a>Усовершенствования Framework Core изображения

Основной среде изображения для macOS Сьерра были внесены следующие усовершенствования:

- `ImageWithExtent` Метод [CIFilter](https://developer.apple.com/reference/coreimage/cifilter) класс может использоваться для вставки пользовательской обработки в операцию фильтрации. Образ Core будет от фильтров при обработке изображения для вывода данный обратный вызов или отображения.
- Теперь приложение может обрабатывать изображения в цветовом пространстве вне контекста образ Core рабочее цветовое пространство путем преобразования в и из него цветовое пространство до и после обработки.
- Образ Core ядра могут запросить конкретный выходной формат пикселей.
- Были добавлены следующие новые фильтры образа: `CINinePartTitled`, `CINinePartStretched`, `CIHueSaturationValueGradient`, `CIEdgePreserveUpsampleFilter` и `CIClamp`.

<a name="Foundation-Framework-Enhancements" />

## <a name="foundation-framework-enhancements"></a>Усовершенствования Framework Foundation

Платформа Foundation для macOS Сьерра были внесены следующие усовершенствования:

- Используйте [NSDimentions](https://developer.apple.com/reference/foundation/nsdimension) масс API-Интерфейс для представления, преобразование и отображение многие из наиболее распространенных физические единицы такие как длина, скорости, продолжительность и температуры.
- Используйте [NSISO8601DateFormatter](https://developer.apple.com/reference/foundation/nsiso8601dateformatter) класса для синтаксического анализа и создания ISO 8601 отформатированные даты.
- Используйте новую [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) класса, чтобы дата и время вычисления интервал, например длительность, для сравнения интервалов и тестирование для пересечения интервал.
- Используйте [NSPersonNameComponentsFormatter](https://developer.apple.com/reference/foundation/nspersonnamecomponentsformatter) класс, чтобы проанализировать элементы указания имени пользователя из строки.
- Используйте новую [NSURLSessionTaskMetrics](https://developer.apple.com/reference/foundation/nsurlsessiontaskmetrics) для получения метрик для URL-адреса, сетевые подключения сеанса.

Дополнительные сведения см. в разделе Apple [Foundation заметки о выпуске для v10.12 OS X и iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/RN-Foundation-OSX10.12/index.html).

<a name="GameKit-Framework-Enhancements" />

## <a name="gamekit-framework-enhancements"></a>Усовершенствования GameKit Framework

Платформа GameKit для macOS Сьерра были внесены следующие усовершенствования:

- **Приложения Game Center** устаревшими и удалены из macOS. Если приложение использует GameKit, он _должен_ предоставить свой собственный интерфейс для отображения GameKit функции, такие как списки лидеров и т. д. 
- Реализован новый тип учетной записи только для iCloud [GKCloudPlayer](https://developer.apple.com/reference/gamekit/gkcloudplayer) класса.
- Новый [GKGameSession](https://developer.apple.com/reference/gamekit/gkgamesession) класс предоставляет обобщенный решения для управления постоянного хранилища данных на Game Center. `GKGameSession` поддерживает список проигрывателей и приложение является ответственным формой реализации, как и когда хранится Дата участника, извлечь или обмениваются проигрыватели. Во многих случаях игровые сеансы можно заменить существующих совпадений, основанных, в режиме реального времени соответствует игры постоянного сохранения методы.

<a name="GamePlayKit-Framework-Enhancements" />

## <a name="gameplaykit-framework-enhancements"></a>Усовершенствования GamePlayKit Framework

Платформа GamePlayKit для macOS Сьерра были внесены следующие усовершенствования:

- Создание процедурных шума был добавлен и может использоваться для улучшения реалистичности в поиск естественных текстур, добавьте реалистичности движение камеры, а также способствует увеличению полнофункциональной игровой среды.
- Используйте Пространственные секционирования для секционирования данных игровой среды, для эффективного поиска.
- Новый Монте-Карло специалист по разработке стратегий ([GKMonteCarloStrategist](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) была добавлена для вычисления исчерпывающим возможности перемещения.
- Был добавлен новый API дерева принятия решений ([GKDecisionTree](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [GKDecisionNode](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения AI создания игры.
- Поддержка трехмерного представления будет добавлен существующий агент и поведения путь поиска, с помощью нового [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) классы.
- Используйте новую [GKMeshGraph](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) класса для обеспечения высокой производительности, выглядящих естественного пути.
- Новый [GKScene](https://developer.apple.com/reference/gameplaykit/gkscene) и [GKSKNodeComponent](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) объединение GameplayKit и проще, чем когда-либо SpriteKit создание классов.

<a name="Metal-Framework-Enhancements" />

## <a name="metal-framework-enhancements"></a>Усовершенствования исходного Framework

Платформа исходного состояния системы для macOS Сьерра были внесены следующие усовершенствования:

- Теперь можно использовать трехмерных приложениях и играх _тесселяции_ эффективно визуализации сложного изображения и geometry через графический Процессор.
- Используйте специализацию функции для создания коллекции оптимизирован материал и светлой сочетание функций для сцены.
- Обеспечивают точное управление выделения ресурсов для оптимизации производительности системы на основе приложений с помощью ресурсов кучи и Memoryless целевых буферов визуализации.

Чтобы получить дополнительные сведения см. в разделе Apple [руководство по программированию исходного](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221).

<a name="ModelIO-Framework-Enhancements" />

## <a name="model-io-framework-enhancements"></a>Улучшения модели Framework ввода-вывода

К структуре модели ввода-вывода для macOS Сьерра были внесены следующие усовершенствования:

- Теперь поддерживается формат файла долл. США.
- Используйте новую `MDLMaterialPropertyGraph` класс для поддержки среды выполнения изменения модели.
- Подпись поля расстояние, добавлена поддержка [MDLVoxelArray](https://developer.apple.com/reference/modelio/mdlvoxelarray) класса.
- Используйте новую `MDLLightProbeIrradianceDataSource` класс, помогающий при размещении Light проверки.

<a name="Photos-Framework-Enhancements" />

## <a name="photos-framework-enhancements"></a>Усовершенствования Framework фотографий

Платформа фотографии для macOS Сьерра были внесены следующие усовершенствования:

- Интерактивного редактирования фото теперь доступен для приложений, поддерживающих платформу фотографии и редактирования расширения фотографий (для использования внутри фотографии и камеры приложений).
- Используйте новую [PHLivePhotoEditingContext](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) класс применять изменения к видео и по-прежнему содержимое Live фотографий.
- Используйте [CIImageProcessorInput](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) и [CIImageProcessorOutput](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) классов, чтобы воспользоваться преимуществами нового образа ядра процессора для выполнения изменения.
- Для поддержки динамической фотографии, [PHLivePhoto](https://developer.apple.com/reference/photos/phlivephoto) и [PHLivePhotoView](https://developer.apple.com/reference/photosui/phlivephotoview) классы были перенесены из iOS macOS.

<a name="SceneKit-Framework-Enhancements" />

## <a name="scenekit-framework-enhancements"></a>Усовершенствования SceneKit Framework

Платформа SceneKit для macOS Сьерра были внесены следующие усовершенствования:

- Теперь включает новую систему физически основе отрисовки (PBR) для более точные результаты при использовании более простые средства разработки.
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

<a name="Security-Framework-Enhancements" />

## <a name="security-framework-enhancements"></a>Улучшения безопасности Framework

Модель безопасности для macOS Сьерра были внесены следующие усовершенствования:

- `SecKey` Модернизированный и интерфейса и единой для всех платформ (iOS, tvOS, watchOS и macOS).

<a name="SpriteKit-Framework-Enhancements" />

## <a name="spritekit-framework-enhancements"></a>Усовершенствования SpriteKit Framework

Платформа SpriteKit для macOS Сьерра были внесены следующие усовершенствования:

- Tilemaps теперь поддерживают фигур квадрат, шестиугольник и изометрическая плитки для 2D, 2,5 D и прокрутка стороны игр с помощью `SKTileMapMode`, `SKTileGroup`, `SKTileGroupRule` и `SKTileSet` классы.
- Используйте новую `SKWarpGeometry` класса необходимо растянуть или исказить [SKSpriteNode](https://developer.apple.com/reference/spritekit/skspritenode) или [SKEffectNode](https://developer.apple.com/reference/spritekit/skeffectnode) подготовки к просмотру. Новый [SKAction](https://developer.apple.com/reference/spritekit/skaction) класс может использоваться для переходов между warp эффектов анимации.
- Пользовательские шейдеров может предоставить атрибуты (`SKAttribute`), можно настроить отдельно с каждого узла, который использует шейдер, указав значение атрибута (`SKAttributeValue`).
- [SKView](https://developer.apple.com/reference/spritekit/skview) класс предоставляет новые методы для предоставления тонкую настройку времени и способа сцены визуализируется.

<a name="New-Frameworks" />

## <a name="new-frameworks"></a>Новым платформам

MacOS Сьерра были добавлены следующие платформы:

- **Способы Framework** -эта платформа позволяет приложению просмотрите взаимодействия (например, расположение или действий пользователя) и выполните действия, основанные на этих данных.
- **SafariServices Framework** -эта платформа позволяет разрабатывать приложения расширения для обозревателя Safari (например, содержимого препятствия) для iOS и macOS приложению.

## <a name="related-links"></a>Связанные ссылки

- [Примеры Mac](https://developer.xamarin.com/samples/mac/)
- [Новые возможности OS X 10.12](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
