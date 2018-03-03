---
title: "Общие сведения о разработке игр с CocosSharp"
description: "Этот составной пошаговом руководстве показано, как создать простой двумерную игры с использованием CocosSharp. Рассматриваются общие игры принципы программирования, например, графики, ввод и физических."
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 3b1bb45ab87c85dff42b4f7ea5297eb3596b81a5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>Общие сведения о разработке игр с CocosSharp

_Этот составной пошаговом руководстве показано, как создать простой двумерную игры с использованием CocosSharp. Рассматриваются общие игры принципы программирования, например, графики, ввод и физических._

Ядро CocosSharp 2D игры предоставляет технологии для создания кросс платформенных игр. Полный список поддерживаемых платформ см. [CocosSharp wiki на GitHub](https://github.com/mono/CocosSharp/wiki). Этот учебник будет использовать C# образцы кода, несмотря на то, что CocosSharp, также будет полностью функционален F #.

Предоставляемые ядро CocosSharp [MonoGame framework](http://www.monogame.net/), который сам является кросс платформенных, с аппаратным ускорением API, предоставляя графики, управление состоянием аудио, игры, ввод и конвейера содержимого для импорта ресурсов. CocosSharp представляет собой уровень абстракции эффективное, хорошо подходят для игр 2D. Кроме того больше игр выполнить собственные оптимизацию за пределами их основных библиотек игры при увеличении сложности. Иными словами CocosSharp предоставляет сочетание простоте использования и производительность, позволяя разработчикам быстро приступить к работе без ограничения игры размером или сложностью.

В первой части этого фокус Пошаговое руководство по созданию пустой проект.  Во второй части описывается сохранение всех наших логику игры. 

В конце данного пошагового руководства будет создан простой игры, где игрока цель — слайд ракетка по горизонтали относительно попытку сохранить шарик попала за пределами экрана. Каждый показатель приведет к увеличению оценка игрока на один пункт.

![](images/image1.png "Каждый показатель приведет к увеличению оценка игрока на один пункт")

# <a name="walkthrough-parts"></a>Части пошагового руководства

* [Часть 1 – Создание проекта CocosSharp](~/graphics-games/cocossharp/first-game/part1.md)
* [Часть 2 — реализация BouncingGame](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>Связанные ссылки

- [Игр (пример)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Завершенный проект (пример)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [PCL CocosSharp в NuGet](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [Документация по CocosSharp API](http://developer.xamarin.comhttps://developer.xamarin.com/api/namespace/CocosSharp/)
