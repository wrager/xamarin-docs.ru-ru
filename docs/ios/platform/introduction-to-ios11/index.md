---
title: "Введение в iOS 11"
description: "Повторите iOS новый 11 API-интерфейсы, с помощью Xamarin."
ms.topic: article
ms.prod: xamarin
ms.assetid: 22C38EA6-6DA9-4B92-B41B-814E589033F6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/19/2016
ms.openlocfilehash: b379a0b35e676849c4d321c5f49b9d611c521d44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-ios-11"></a>Введение в iOS 11

_Повторите iOS новый 11 API-интерфейсы, с помощью Xamarin._

> [!NOTE]
> Прежде чем начать, просмотрите [Приступая к работе](get-started.md) страницу на наличие инструкций по установке Xcode 9.

![Пример ARKit](images/arkit.png) ![Размещение объектов AR](images/arkit2.png) ![Пример CoreML](images/coreml.png) ![Пример MapKit](images/mapkit.png) ![Пример прямоугольники концепции](images/vision1.png) ![Пример гарнитуры концепции](images/vision2.png) ![Перетаскивание пример](images/drag-drop.png) ![Перетаскивание пример](images/drag-drop2.png) ![Пример SiriKit](images/sirikit.png)

iOS 11 включает множество совершенно новой функции и улучшения в различных платформ:

## <a name="preparing-your-app-for-ios-11updating-your-appindexmd"></a>[Подготовка приложения для iOS 11](updating-your-app/index.md)

Архитектура обновления, новые визуальные изменения и процесс Connect обновленные iTunes для iOS 11 добавлены Apple. Используйте это руководство, чтобы убедиться в том, что приложения Xamarin.iOS готов для нового выпуска.

## <a name="arkitarkitindexmd"></a>[ARKit](arkit/index.md)

ARKit позволяет дополнить реальности на iOS, позволяя пользователям взаимодействовать с внешним миром через камеры устройства.
С помощью Xamarin, можно также использовать [ARKit с UrhoSharp](arkit/urhosharp.md).

## <a name="coremlcoremlmd"></a>[CoreML](coreml.md)

Моделей машинного обучения можно интегрировать в приложения iOS 11 с CoreML. Платформа CoreML предоставляет простой API-Интерфейс для включения в проекты приложений, чтобы разрешить проблемы для анализа существующих моделей непосредственно в приложение.

## <a name="corenfccorenfcmd"></a>[CoreNFC](corenfc.md)

iPhone 7 и более новых устройств могут считывать тегов рядом с полем связи действия (NFC), для включения приложений для обнаружения с тегами продуктов, мест или действия в мире вокруг них.

## <a name="drag-and-dropdrag-and-dropmd"></a>[Перетаскивание](drag-and-drop.md)

Платформа перетаскивание обеспечивает поддержку всего операций ввода-вывода для перемещения данных по сенсорный ввод. На iPad можно перетащить как внутри, так и от других приложений; Хотя на iPhone, можно перетаскивать только в пределах одного приложения. Поддерживается для многих типов настройки, включая сложные типы данных, анимации и обработка мультисенсорные жесты.

## <a name="mapkitmapkitmd"></a>[MapKit](mapkit.md)

MapKit имеет ряд улучшений, включая поддержку автоматического маркера, группирование и добавление компаса к представлению.

## <a name="pdfkit"></a>PDFKit

PDFKit теперь доступен на iOS 11, переводя создания PDF и возможностей в приложениях для редактирования.

## <a name="sirikitsirikitmd"></a>[SiriKit](sirikit.md)

Siri теперь поддерживает еще больше взаимодействия, включая списки и примечания и другие расширения, такие как приложения альтернативные имена.

## <a name="visionvisionmd"></a>[Vision](vision.md)

Предоставляет функции обработки и анализа различные изображения для iOS, включая лицевой стороны обнаружения и распознавания, моделей CoreML, новые интерфейсы API обнаружения штрих-кода, текста и период обнаружения и общими обнаружения объекта и отслеживания.

## <a name="samples"></a>Примеры

Мы сотрудничаем с C# [образцы](https://developer.xamarin.com/samples/ios/iOS11/) можно приступить к работе:

* [Образец ARKit](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
* [ARKit размещение объектов](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
* [ARKit и UrhoSharp](arkit/urhosharp.md)
* [Образец CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreML)
* [Пример распознавания CoreML изображения](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition)
* [CoreML с Azure пользовательскую модель.](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
* [Пример средства чтения CoreNFC тег](https://developer.xamarin.com/samples/monotouch/ios11/NFCTagReader/)
* [Перетаскиванием таблиц представлений](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView)
* [Drag & Drop представление коллекции](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
* [Пользовательское представление перетаскивания.](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCustomView)
* [Перетащите DragBoard & сброса образца](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropDragBoard)
* [Образец MapKit](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample)
* [Образец SiriKit](https://developer.xamarin.com/samples/monotouch/ios11/SiriKitSample/)
* [Обновленный framework образец фото](https://developer.xamarin.com/samples/monotouch/ios11/SamplePhotoApp/)
* [Концепции и образец CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision)
* [Образец обнаружения прямоугольники концепции](https://developer.xamarin.com/samples/monotouch/ios11/VisionRects)
* [Образец обнаружения гарнитуры концепции](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces)
* [Образец PDKFit мини-приложения](https://developer.xamarin.com/samples/monotouch/ios11/PDFAnnotationWidgetsAdvanced)
* [Образец PDFKit водяной знак](https://developer.xamarin.com/samples/monotouch/ios11/PDFDocumentWatermark)

## <a name="more-information"></a>Дополнительные сведения

Дополнительные сведения о iOS 11 на сайте Apple [новые возможности iOS 11](https://developer.apple.com/ios/) документации.


## <a name="related-links"></a>Связанные ссылки

- [Новые возможности iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Образцы Xamarin iOS 11](https://developer.xamarin.com/samples/ios/iOS11/)
