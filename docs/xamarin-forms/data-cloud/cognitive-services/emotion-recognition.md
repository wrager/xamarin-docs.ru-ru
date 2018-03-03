---
title: "С помощью API эмоций — распознавания эмоций"
description: "API эмоций — принимает выражения лица изображение в качестве входных данных и возвращает степени достоверности во множестве эмоций для каждой грани в образе. В этой статье описывается использование API эмоций — для распознавания эмоций — оценка приложения Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>С помощью API эмоций — распознавания эмоций

_API эмоций — принимает выражения лица изображение в качестве входных данных и возвращает степени достоверности во множестве эмоций для каждой грани в образе. В этой статье описывается использование API эмоций — для распознавания эмоций — оценка приложения Xamarin.Forms._

![](~/media/shared/preview.png "Этот API является в настоящее время в предварительной версии")

> [!NOTE]
> API эмоций — находится на стадии предварительной версии. Может критически важных изменений в API до выпуска окончательной версии.

## <a name="overview"></a>Обзор

API эмоций — позволяет выявить anger, contempt, disgust, опасаясь, счастье, нейтральная, sadness и категории "неожиданное", выражения лица. Эти эмоций, глобально и что взаимодействуют через те же основные выражения лица. А также возвратить результат эмоций — для выражения лица, API эмоций — также Возвращает ограничивающий прямоугольник для обнаруженных лица, с помощью API начертания. Если пользователь уже вызван API лицевой стороны, пользователь может отправить лицевой стороны прямоугольника как необязательные входные данные. Обратите внимание, что для использования API эмоций — необходимо получить ключ API. Это можно сделать в [Приступая к работе бесплатно](https://www.microsoft.com/cognitive-services/sign-up) на сайте microsoft.com.

Распознавание эмоций — могут выполняться через клиентскую библиотеку, а также через REST API. Эта статья посвящена выполнения распознавания эмоций — через [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) клиентскую библиотеку, которую можно загрузить из NuGet.

API эмоций — также можно использовать для распознавания выражения лица людей в видео и возвращает сводку по их эмоций. Дополнительные сведения см. в разделе [эмоций — в видео](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) на сайте microsoft.com.

Дополнительные сведения об API эмоций — см. в разделе [документации по API эмоций —](https://www.microsoft.com/cognitive-services/emotion-api/documentation) на сайте microsoft.com.

## <a name="performing-emotion-recognition"></a>Выполнение эмоций распознавания

Распознавание эмоций — достигается за счет передачи API эмоций — поток изображений. Размер файла изображения не должен превышать 4 МБ и поддерживаемые форматы файлов, JPEG, GIF, PNG и BMP. В образе выявляемых начертания размер диапазона — 36 x 36 в 4096 x 4096 пикселей. Все фрагменты за пределами этого диапазона не может быть найден.

В следующем примере кода показан процесс распознавания эмоций:

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

`EmotionServiceClient` Для выполнения распознавания эмоций — с помощью ключа API эмоций —, переданного в качестве аргумента необходимо создать экземпляр `EmotionServiceClient` конструктор.

`RecognizeAsync` Метод, который вызывается в `EmotionServiceClient` экземпляра, загружает изображение в API эмоций — в виде `Stream`. API эмоций — будут отправлены ключ API при вызове этой операции. Ошибки при отправке допустимый ключ API приведет к `Microsoft.ProjectOxford.Common.ClientException` вызываемом с сообщение исключение, указывающее на то, что был отправлен недопустимый ключ API.

`RecognizeAsync` Метод будет возвращать `Emotion` массива, при условии, что фрагмент был распознан. Для каждого изображения максимальное число лиц, которые могут быть обнаружены 64 и лиц ранжируются по размеру лицевой стороны прямоугольника в порядке убывания. При обнаружении не начертания пустой `Emotion` возвращается массив.

При интерпретации результатов из API эмоций —, обнаруженных эмоций должен интерпретироваться как эмоций с самая высокая оценка как оценки нормализуются на сумму в один. Таким образом в примере приложения отображались распознаваемым эмоций с высший показатель для наибольшего грани обнаруженных в образе, как показано на следующем снимке экрана:

![](emotion-recognition-images/emotion-recognition.png "Распознавание эмоций")

## <a name="summary"></a>Сводка

В этой статье описано, как использовать API эмоций — для распознавания эмоций — оценка приложения Xamarin.Forms. API эмоций — принимает выражения лица изображение в качестве входных данных и возвращает уверенность в том в наборе эмоций для каждой грани в образе.


## <a name="related-links"></a>Связанные ссылки

- [API эмоций — документации](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [TODO Когнитивных службы (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [API эмоций —](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
