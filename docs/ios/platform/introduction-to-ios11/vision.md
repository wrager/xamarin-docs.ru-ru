---
title: "Концепция Framework"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: fb1b5b11ef0117a40267f805621797c3aee04810
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="vision-framework"></a>Концепция Framework

Концепция framework добавляет ряд новый образ, функциях для iOS 11, включая:

- [Обнаружение прямоугольника](#rectangles)
- [Обнаружение лицевой стороны](#faces)
- Машинного обучения анализ образов (описано в [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Обнаружение штрих-кода
- Анализ выравнивание изображения
- Определение текста
- Период обнаружения
- Обнаружение объектов и отслеживания

![Фотографии с обнаружил три прямоугольника](vision-images/found-rectangles-tiny.png) ![Фотографии с двумя фрагменты обнаружил](vision-images/xamarin-home-faces-tiny.png)

Прямоугольник обнаружения и обнаружения лицевой стороны более подробно рассмотрены ниже.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Обнаружение прямоугольника

[VisionRects пример](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) показано, как обрабатывать изображения и Рисование прямоугольников, обнаруженных на нем.

### <a name="1-initialize-the-vision-request"></a>1. Инициализировать запрос концепции

В `ViewDidLoad`, создайте `VNDetectRectanglesRequest` ссылающийся `HandleRectangles` метод, который будет вызываться в конце каждого запроса:

`MaximumObservations` Также можно задать свойства, в противном случае он будет по умолчанию 1 и будет возвращаться только один результат.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Начало обработки концепции

Следующий код запускает обработку запроса. В **VisionRects** примера, этот код выполняется после пользователь выбрал изображения:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Этот обработчик передает `ciImage` для платформы концепции `VNDetectRectanglesRequest` , созданного на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Обрабатывать результаты обработки концепции

После завершения обнаружения прямоугольник платформа выполняет `HandleRectangles` метод, о которой показан ниже:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отобразить результаты

`OverlayRectangles` Метод в **VisionRectangles** образец имеет три функции:

- Подготовка к просмотру исходного изображения
- Рисование прямоугольника, чтобы указать, где обнаружен каждого из них, и
- Добавление текстовую метку для каждого прямоугольника с помощью CoreGraphics.

Представление [источника образца](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) для метода CoreGraphics.

![Фотографии с обнаружил три прямоугольника](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Обнаружение прямоугольник чаще всего лишь первым шагом в цепочке операций, таких как с [в этом примере CoreMLVision](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), где прямоугольники передаются в модель CoreML анализируемый рукописные цифр.


<a name="faces" />

## <a name="face-detection"></a>Обнаружение лицевой стороны

[Пример VisionFaces](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) работает таким же образом, чтобы **VisionRectangles** образца с помощью другого запроса класса концепции.

### <a name="1-initialize-the-vision-request"></a>1. Инициализировать запрос концепции

В `ViewDidLoad`, создайте `VNDetectFaceRectanglesRequest` ссылающийся `HandleRectangles` метод, который будет вызываться в конце каждого запроса.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Начало обработки концепции

Следующий код запускает обработку запроса. В **VisionFaces** этот код выполняется после пользователь выбрал образ образца:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Этот обработчик передает `ciImage` для платформы концепции `VNDetectFaceRectanglesRequest` , созданного на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Обрабатывать результаты обработки концепции

После завершения обнаружения лиц на обработчик выполняет `HandleRectangles` метод, который выполняет обработку ошибок и отображает границы обнаруженных гарнитуры и вызывает `OverlayRectangles` для отрисовки ограничивающие прямоугольники в исходное изображение:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отобразить результаты

`OverlayRectangles` Метод в **VisionFaces** образец имеет три функции:

- Подготовка к просмотру исходного изображения
- Рисование прямоугольника для каждой грани обнаружена, и
- Добавление текстовой метки для каждой грани, с помощью CoreGraphics.

Представление [источника образца](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) для метода CoreGraphics.

![Фотографии с двумя фрагменты обнаружил](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Платформа концепции включает дополнительные возможности для обнаружения черт глаза и лицу. Используйте `VNDetectFaceLandmarksRequest` тип, который будет возвращать `VNFaceObservation` результатов, как и шаг 3 выше, но с дополнительными `VNFaceLandmark` данных.


## <a name="related-links"></a>Связанные ссылки

- [Прямоугольники Vision (пример)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Концепция гарнитуры (пример)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Усовершенствованные возможности образ Core - фильтры, системы, концепции и более (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/510/)
