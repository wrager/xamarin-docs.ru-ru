---
title: CocosSharp
description: Этот документ, ссылки на различные статьи, посвященные разработке игр с CocosSharp.
ms.prod: xamarin
ms.assetid: 5E72869D-3541-408B-AB64-D34C777AFB79
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: a188863cf57706e3f9dd6c8f4d2d3e60b2591e0b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="cocossharp"></a>CocosSharp

_CocosSharp — это библиотека для создания игр 2D, с помощью C# и F #. Это порт .NET популярных Cocos2D ядра._

## <a name="introduction-to-cocossharp"></a>Общие сведения о CocosSharp

Ядро CocosSharp 2D игры предоставляет технологии для создания кросс платформенных игр. Полный список поддерживаемых платформ см. [CocosSharp wiki на GitHub](https://github.com/mono/CocosSharp/wiki).
Эти руководства используйте C# образцы кода, несмотря на то, что CocosSharp, также будет полностью функционален F #.

Предоставляемые ядро CocosSharp [MonoGame framework](http://www.monogame.net/), который сам является кросс платформенных, с аппаратным ускорением API, предоставляя графики, управление состоянием аудио, игры, ввод и конвейера содержимого для импорта ресурсов.
CocosSharp представляет собой уровень абстракции эффективное, хорошо подходят для игр 2D.
Кроме того больше игр выполнить собственные оптимизацию за пределами их основных библиотек игры при увеличении сложности. Иными словами CocosSharp предоставляет сочетание простоте использования и производительность, позволяя разработчикам быстро приступить к работе без ограничения игры размером или сложностью.

Практические рассматривается создание простого CocosSharp кросс платформенных игр:

> [!Video https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Developing-Cross-platform-2D-Games-in-C-and-CocosSharp/player]

## <a name="bouncinggamegraphics-gamescocossharpbouncing-gamemd"></a>[BouncingGame](~/graphics-games/cocossharp/bouncing-game.md)

![BouncingGame](images/bouncing-game.png "BouncingGame")

Это руководство описывает BouncingGame, включая способы работы с содержимым игры, различные визуальные элементы, используемые для создания игр, добавление логику игры и многое другое.

## <a name="fruity-falls-gamegraphics-gamescocossharpfruity-fallsmd"></a>[Возвращается Fruity игры](~/graphics-games/cocossharp/fruity-falls.md)

![Снимок экрана игры приходится Fruity](images/fruity-falls.png "Fruity приходится игры экрана")

В данном руководстве описываются приходится Fruity игры, охватывающие CocosSharp общие и основные понятия разработки игр как физический, управления содержимым, состояния игры и разработки игр.  

## <a name="coin-time-gamegraphics-gamescocossharpcointimemd"></a>[Игра времени разработки](~/graphics-games/cocossharp/cointime.md)

![Время снимок экрана игры, таблетка](images/cointime.png "снимок экрана игры времени разработки")

Время разработки — полный platformer, игры для iOS и Android. Игра предназначена для того, чтобы собрать все монеты уровня и затем достичь дверца выхода избегая враги и другими препятствиями.

## <a name="drawing-geometry-with-ccdrawnodegraphics-gamescocossharpccdrawnodemd"></a>[Рисование геометрических с CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md)

![Фигуры, нарисованных при помощи CCDrawNode](images/ccdrawnode.png "фигур, нарисованных при помощи CCDrawNode")

CCDrawNode предоставляет методы для рисования простые объекты, такие как линий, кругов и треугольников.

## <a name="animating-with-ccactiongraphics-gamescocossharpccactionmd"></a>[Анимация с CCAction](~/graphics-games/cocossharp/ccaction.md)

![Анимация CCAction](images/ccaction.png "CCAction анимации")

`CCAction` является базовым классом, который может использоваться для анимации объектов CocosSharp. В настоящем руководстве описывается встроенные `CCAction` реализации для общих задач, например положение, масштабирования и поворота. На ней также рассматривается создание пользовательских реализаций путем наследования от `CCAction`.

## <a name="using-tiled-with-cocossharpgraphics-gamescocossharptiledmd"></a>[Использование Tiled с CocosSharp](~/graphics-games/cocossharp/tiled.md)

![Уровень в игру](images/tiled.png "уровень игры")

Мозаичное заполнение мощный, гибкий и сопоставляет отлаженное приложение для создания плитки ортогональных и изометрическая игр для устройств. CocosSharp предоставляет встроенной интеграции для копиями в собственном формате.

## <a name="entities-in-cocossharpgraphics-gamescocossharpentitiesmd"></a>[Сущности в CocosSharp](~/graphics-games/cocossharp/entities.md)

![Космическим кораблем из игры](images/entities.png "космическим кораблем из игры")

Шаблон сущности является мощным средством для организации кода игры. Он повышает удобочитаемость, делает код проще было обслуживать и использует функции, встроенные родители потомки.

## <a name="handling-multiple-resolutions-in-cocossharpgraphics-gamescocossharpresolutionsmd"></a>[Обработка нескольких разрешений в CocosSharp](~/graphics-games/cocossharp/resolutions.md)

![Сетка, представляющий разрешение экрана](images/resolutions.png "сетки, представляющий разрешение экрана")

В этом руководстве показано, как работать с CocosSharp для разработки игр, правильно отображаться на устройствах для различных разрешений экрана.

## <a name="cocossharp-content-pipelinegraphics-gamescocossharpcontent-pipelineindexmd"></a>[Конвейер содержимого CocosSharp](~/graphics-games/cocossharp/content-pipeline/index.md)

![XNB](images/content-pipeline.png "XNB")

Конвейеры содержимого часто используются в разработки игр для оптимизации содержимого и отформатировать его таким образом, чтобы ее можно было загрузить в определенным оборудованием или с помощью определенных платформ разработки игр.

## <a name="improving-frame-rate-with-ccspritesheetgraphics-gamescocossharpccspritesheetmd"></a>[Повышение частоты кадров с CCSpriteSheet](~/graphics-games/cocossharp/ccspritesheet.md)

![Дерево из CCSpriteSheet](images/ccspritesheet.png "дерева из CCSpriteSheet")

`CCSpriteSheet` предоставляет функциональные возможности для объединения и с помощью много файлов изображений в одну текстуру. Сокращение числа текстуры можно улучшить, время загрузки игры и частоты кадров.

## <a name="texture-caching-using-cctexturecachegraphics-gamescocossharptexture-cachemd"></a>[Кэширование текстуры с помощью CCTextureCache](~/graphics-games/cocossharp/texture-cache.md)

![Представление как CocosSharp кэширует изображения](images/texture-cache.png "представление как CocosSharp кэширует изображений")

В CocosSharp `CCTextureCache` класс предоставляет стандартный способ организации, кэширование и выгрузки содержимого. 

## <a name="2d-math-with-cocossharpgraphics-gamescocossharpmathmd"></a>[2D математические операции с CocosSharp](~/graphics-games/cocossharp/math.md)

![Поворот изображения](images/math.png "Поворот изображения")

В настоящем руководстве описывается 2D математических операций для разработки игр. Он использует CocosSharp показано, как выполнять типичные задачи разработки игр и объясняет математически этих задач.

## <a name="performance-and-visual-effects-with-ccrendertexturegraphics-gamescocossharpccrendertexturemd"></a>[Производительность и визуальные эффекты с CCRenderTexture](~/graphics-games/cocossharp/ccrendertexture.md)

![Sprite из игры](images/ccrendertexture.png "sprite из игры")

`CCRenderTexture` Класс предоставляет функциональные возможности для подготовки к просмотру несколько объектов, CocosSharp одну текстуру. После создания `CCRenderTexture` экземпляров может использоваться для эффективной визуализации графики и реализации визуальные эффекты.
