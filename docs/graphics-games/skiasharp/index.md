---
title: 2D Рисование с SkiaSharp
description: В этом документе Обзор двумерные кросс платформенных Рисование с SkiaSharp. Ссылки на различные руководства, описывающие SkiaSharp и его различных API-интерфейсов.
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 962fe657f25976f9b5069f2d434e92f816d249ca
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783296"
---
# <a name="2d-drawing-with-skiasharp"></a>2D Рисование с SkiaSharp

SkiaSharp предоставляет возможности API C# это двумерной графики. Питание включено [библиотеки Skia Google](http://skia.org), той же библиотеке, лежащих в основе графических стеки Google Chrome, Firefox и Android.

[![](images/ide-sml.png "SkiaSharp предоставляет возможности API C# это двумерной графики")](images/ide.png#lightbox)

SkiaSharp является переносимой библиотеки и удобно поставляется как [пакет NuGet кросс платформенных](https://www.nuget.org/packages/SkiaSharp)и поддерживает следующие платформы готового: macOS, Xamarin.Android и Xamarin.iOS, а также рабочий стол Windows.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Общие сведения о SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Обзор основных понятий SkiaSharp и образец кода для отрисовки графики, текст, растровые изображения и использовать фильтры образа.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Учебники по SkiaSharp для Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Описание способов работы с кросс-графики платформы, подготовки к просмотру в Xamarin.Forms:

- [Основы рисования](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Рисование простых круга](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Интеграция с Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Пикселей и аппаратно независимых единицах](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Базовая анимация](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Интеграция текста и графики](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Основы растрового изображения](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Строки и пути](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Строки и пунктирные завершения отрезков](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Основные сведения о пути](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Типы заполнения путь](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Ломаных линий и параметрических уравнений](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Точек и тире](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Рисование пальцами](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Преобразования для преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Преобразование изменения масштаба](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Преобразования вращения](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Наклон преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Матрица преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Манипуляции сенсорного ввода](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Сходные без преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Трехмерного поворота](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Кривых и контуров](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Три способа нарисовать дугу](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Три типа кривых Безье](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Данные пути SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Обрезка изображения по границам области с помощью путей](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Эффекты пути](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Пути и текст](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Сведения о пути и перечисление](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Примечания для конкретных платформ](~/graphics-games/skiasharp/platform.md)

На этой странице описаны инструкции по SkiaSharp установке на различных платформах, включая iOS, Android, macOS и Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Документация по API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Можно просмотреть [документации по API](https://developer.xamarin.com/api/namespace/SkiaSharp/) для SkiaSharp на сайте.

## <a name="work-in-progress"></a>Выполняемая работа

SkiaSharp является незавершенным, мы совместное использование с нашего сообщества. Хотя привязку важные части Skia API, объем работы остается сделать. Мы используем стабильный API C, отображаются Skia, и мы планируем будет продолжаться, добавляющие нашей работе для привязок C Skia раскрывается полностью API-интерфейсам.

Чтобы помочь нам руководства наши усилия привязки, оставьте комментарии и предложения как проблемы в репозитории GitHub [ http://github.com/mono/SkiaSharp ](http://github.com/mono/SkiaSharp).
