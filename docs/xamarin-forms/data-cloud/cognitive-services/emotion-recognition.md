---
title: Распознавание эмоций — с помощью API начертание шрифта
description: API начертания принимает выражения лица изображение в качестве входных данных и возвращает данные, включая степени достоверности во множестве эмоций для каждой грани в образе. В этой статье объясняется, как использовать API-Интерфейс лицевой стороны для распознавания эмоций — оценка приложения Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 49e53425dbaf3aadd74d02ab030929e3311c7c8c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>Распознавание эмоций — с помощью API начертание шрифта

_API начертания принимает выражения лица изображение в качестве входных данных и возвращает данные, включая степени достоверности во множестве эмоций для каждой грани в образе. В этой статье объясняется, как использовать API-Интерфейс лицевой стороны для распознавания эмоций — оценка приложения Xamarin.Forms._

## <a name="overview"></a>Обзор

Начертания API можно выполнять обнаружение эмоций — обнаружение anger, contempt, disgust, опасаясь, счастье, нейтральный, sadness и категории "неожиданное", в выражения лица. Эти эмоций, глобально и что взаимодействуют через те же основные выражения лица. А также возвратить результат эмоций — для выражения лица, API лицевой стороны можно также Возвращает ограничивающий прямоугольник для обнаруженных гарнитуры. Обратите внимание, что ключ API, которые должны быть получены для использования API начертания. Это можно сделать в [повторите служб Когнитивных](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Распознавание эмоций — могут выполняться через клиентскую библиотеку, а также через REST API. Эта статья посвящена выполнения распознавания эмоций — через [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) клиентскую библиотеку, которую можно загрузить из NuGet.

API лицевой стороны можно также использовать для распознавания выражения лица людей в видео и может возвращать Сводка их эмоций. Дополнительные сведения см. в разделе [как анализировать видео в режиме реального времени](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Дополнительные сведения об API-Интерфейсе начертания. в разделе [API лицевой](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Выполнение эмоций распознавания

Распознавание эмоций достигается за счет передачи поток изображений в API начертания. Размер файла изображения не должен превышать 4 МБ и поддерживаемые форматы файлов, JPEG, GIF, PNG и BMP.

В следующем примере кода показан процесс распознавания эмоций:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

`FaceServiceClient` Необходимо создать экземпляр, чтобы выполнять распознавание эмоций, с ключом API лицевой и конечной точки, передаваемые в качестве аргументов `FaceServiceClient` конструктор.

> [!NOTE]
> Необходимо использовать один регион при вызовах API лицевой стороны как можно использовать для получения ключей подписки. Например, если вы приобрели подписку ключи из `westus` , конечную точку обнаружения начертания, будет ли области `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

`DetectAsync` Метод, который вызывается в `FaceServiceClient` экземпляра, загружает изображение в API лицевой стороны в виде `Stream`. Ключ API будут отправлены в API лицевой стороны, при вызове этой операции. Ошибки при отправке допустимый ключ API приведет к `Microsoft.ProjectOxford.Face.FaceAPIException` вызываемом с сообщение исключение, указывающее на то, что был отправлен недопустимый ключ API.

`DetectAsync` Метод будет возвращать `Face` массива, при условии, что фрагмент был распознан. Каждая возвращенная начертания содержит прямоугольник, указывая его расположение, в сочетании с ряда необязательно начертания атрибутов, которые задаются `faceAttributes` аргумент `DetectAsync` метод. При обнаружении не начертания пустой `Face` возвращается массив.

При интерпретации результатов из API начертания, обнаруженных эмоций должен интерпретироваться как эмоций с самая высокая оценка как оценки нормализуются на сумму в один. Таким образом в примере приложения отображались распознаваемым эмоций с высший показатель для наибольшего грани обнаруженных в образе, как показано на следующем снимке экрана:

![](emotion-recognition-images/emotion-recognition.png "Распознавание эмоций")

## <a name="summary"></a>Сводка

В этой статье описано, как использовать API лицевой стороны для распознавания эмоций — оценка приложения Xamarin.Forms. API начертания принимает выражения лица изображение в качестве входных данных и возвращает данные, включая уверенность в наборе эмоций для каждой грани в образе.

## <a name="related-links"></a>Связанные ссылки

- [Сталкиваются API](/azure/cognitive-services/face/overview/).
- [TODO Когнитивных службы (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
