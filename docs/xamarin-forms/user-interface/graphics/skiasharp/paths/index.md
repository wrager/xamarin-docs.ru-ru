---
title: SkiaSharp строки и пути
description: Используйте SkiaSharp для рисования линий и графики пути
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: 2f9941305f165ec04e5fc80e3c41e3150a21a9b7
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2018
---
# <a name="skiasharp-lines-and-paths"></a>SkiaSharp строки и пути

_Используйте SkiaSharp для рисования линий и графики пути_

[Предыдущего раздела](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) продемонстрировал, SkiaSharp `SKCanvas` класс содержит несколько методов для рисования прямоугольники, эллипсы и прямоугольников со скругленными углами. Этот раздел и дальнейших разделах рассматриваются различные классы, для подключения, создание и Подготовка к просмотру *контуры*.

Путь графики является наиболее общий способ рисования линий и кривых в SkiaSharp. Этот раздел описывает использование `SKPath` для рисования прямых линий и использовать коллекцию очень мала прямых линий (называется *ломаной линии*) для рисования кривых, которые вы можете определить математически. Ниже будут рассматриваются различные виды поддерживается кривых `SKPath`.

Примеры программ в этом разделе отображаются под заголовком **строки и пути** на домашней странице [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программы и в [ **Пути** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths) папку решения.

## <a name="lines-and-stroke-capslinesmd"></a>[Линии и концы штрихов](lines.md)

Сведения об использовании SkiaSharp для рисования линий с ограничениями различных штриха.

## <a name="path-basicspathsmd"></a>[Основные сведения о пути](paths.md)

Просмотр объектов SkiaSharp SKPath для объединения линий и кривых.

## <a name="the-path-fill-typesfill-typesmd"></a>[Типы заполнения пути](fill-types.md)

Обнаружение различных эффектов возможных типов заполнения SkiaSharp пути.

## <a name="polylines-and-parametric-equationspolylinesmd"></a>[Ломаные линии и параметрические уравнения](polylines.md)

Используйте SkiaSharp к просмотру любой строки, которые можно определить с помощью параметрических уравнений.

## <a name="dots-and-dashesdotsmd"></a>[Точки и тире](dots.md)

Мастер Рисование пунктирная и пунктирных линий в SkiaSharp особенностей.

## <a name="finger-paintingfinger-paintmd"></a>[Рисование пальцами](finger-paint.md)

Пальцы используются для закрашивания на холсте.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
