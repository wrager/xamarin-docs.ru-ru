---
title: SkiaSharp строки и пути
description: В этой статье объясняется, как использовать SkiaSharp для рисования линий и контуры в приложениях Xamarin.Forms и это демонстрируется с примерами кода.
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: a922f7ec91624e20a9afedb8063328bdb7a4d42d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243757"
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
