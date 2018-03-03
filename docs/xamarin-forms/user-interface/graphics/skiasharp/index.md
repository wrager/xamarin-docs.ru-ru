---
title: "С помощью SkiaSharp в Xamarin.Forms"
description: "Используйте SkiaSharp для двумерных изображений в приложениях Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: a49c442fcce31fb6b853359ddfafc9a43d0a2114
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>С помощью SkiaSharp в Xamarin.Forms

_Используйте SkiaSharp для двумерных изображений в приложениях Xamarin.Forms_

SkiaSharp — это система двумерной графики для .NET и C# с открытым исходным кодом Skia графического модуля, широко используются в продуктах Google. SkiaSharp можно использовать в приложениях Xamarin.Forms для рисования 2D векторной графики, точечные рисунки и текст. В разделе [2D Рисование](~/graphics-games/skiasharp/index.md) руководство по более общие сведения о библиотеке SkiaSharp и некоторые другие учебники.

В этом руководстве предполагается, что вы знакомы с программированием Xamarin.Forms.

## <a name="webinar"></a>Веб-семинар

[![](images/skiasharpwebinarscreen.png "SkiaSharp для веб-семинар Xamarin.Forms")](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)  
[Посмотрите вебинар «SkiaSharp для Xamarin.Forms»](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)

## <a name="skiasharp-preliminaries"></a>Предварительные действия SkiaSharp

SkiaSharp для Xamarin.Forms представлена в виде пакета NuGet. После создания решения Xamarin.Forms в Visual Studio или Visual Studio для Mac, можно использовать диспетчер пакетов NuGet для поиска **SkiaSharp.Views.Forms** пакета и добавить его в решение. При проверке **ссылки** раздел после добавления SkiaSharp каждого проекта, можно видеть, что различные **SkiaSharp** библиотеки были добавлены все проекты в решении.

Если приложение Xamarin.Forms предназначен для операций ввода-вывода, используйте страницу свойств проекта для изменения целевого объекта развертывания минимальное на iOS 8.0.

На любой странице C#, использующий SkiaSharp вам требуется включить `using` директивы [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) пространства имен, которое включает в себя все SkiaSharp классы, структуры и перечисления, которые будут использоваться в рисунки программирование. Следует также `using` директивы [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) пространство имен для классов, определенных в Xamarin.Forms. Это гораздо меньше пространства имен, с наиболее важных классов, [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). Этот класс является производным от Xamarin.Forms `View` класса и размещает графики SkiaSharp выходных данных.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` Пространство имен также содержит `SKGLView` класс, производный от `View` , но для отображения графики используется OpenGL. Для простоты в этом руководстве ограничится для `SKCanvasView`, но с помощью `SKGLView` вместо является очень похожа.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[Основы рисования в SkiaSharp](basics/index.md)

Некоторые из простой фигур графики, который можно нарисовать с SkiaSharp являются круги, эллипсы и прямоугольники. При отображении эти показатели, будет рассказано о SkiaSharp координаты, размеры и цвета.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[Строки и пути SkiaSharp](paths/index.md)

Графический контур — это последовательность соединенных прямых линий и кривых. Пути могут быть преобразованы в обводку, заполняется, или оба. В этом разделе включает многие аспекты рисования линии, включая штриха завершается, а соединения и пунктирных и пунктирные линии, но останавливается во всех остальных случаях кривой геометрических объектов.

## <a name="skiasharp-transformstransformsindexmd"></a>[Преобразование SkiaSharp](transforms/index.md)

Преобразования также позволяют графические объекты равномерно преобразуется, масштабировать, поворачивать, и синхронизована. В этой статье также показано, как можно использовать матрицу стандартные преобразования 3 x 3 для создания-аффинных преобразований и применение преобразований к пути.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[Пути и кривые SkiaSharp](curves/index.md)

Исследования пути продолжается с добавлением объектам путь кривых и использовать другие функции мощные путь. Вы увидите, как можно указать полный путь четкими текстовой строки, используйте путь эффекты и детализировать внутренние компоненты пути.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [SkiaSharp с веб-семинар Xamarin.Forms (видео)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
