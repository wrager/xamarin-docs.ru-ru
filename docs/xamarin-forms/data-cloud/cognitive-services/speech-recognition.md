---
title: "С помощью Bing речи API распознавания речи"
description: "API речи Bing является облачной API, который предоставляет алгоритмы обработки языка. В этой статье описывается использование API REST распознавания речи Bing для преобразования текста в приложении Xamarin.Forms аудио."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>С помощью Bing речи API распознавания речи

_API речи Bing является облачной API, который предоставляет алгоритмы обработки языка. В этой статье описывается использование API REST распознавания речи Bing для преобразования текста в приложении Xamarin.Forms аудио._

![](~/media/shared/preview.png "Этот API является в настоящее время в предварительной версии")

> [!NOTE]
> API речи Bing находится на стадии предварительной версии. Может критически важных изменений в API до выпуска окончательной версии.

## <a name="overview"></a>Обзор

API речи Bing состоит из двух компонентов:

- Распознавание речи API для преобразования в текст произносимых слов. Распознавание речи может выполняться через API-Интерфейс REST, клиентская библиотека или библиотека службы.
- Преобразование текста в речь API для преобразования текста в речь. Преобразование текста в речь выполняется через REST API.

Эта статья посвящена выполняет распознавание речи через REST API. Хотя библиотеки клиента и службы поддерживают возвращение частичные результаты, API-интерфейса REST может возвращать только один распознавания результат, не все частичные результаты.

Необходимо получить ключ API для использования API распознавания речи Bing. Это можно сделать в [Приступая к работе бесплатно](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) на сайте microsoft.com.

Дополнительные сведения об API Bing речи см. в разделе [документации речь Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) на сайте microsoft.com.

## <a name="authentication"></a>Проверка подлинности

Каждый запрос к API REST распознавания речи Bing требуется маркер доступа JSON Web Token (JWT), который можно получить от службы маркеров когнитивных служб в `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Маркер можно получить, выполнив запрос POST в службу маркера указание `Ocp-Apim-Subscription-Key` заголовок, содержащий ключ API, как его значение.

В следующем примере кода показано, как запросить доступ маркера от службы маркеров:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Маркер доступа, возвращаемый, являющийся текст в кодировке Base64, имеет время отключения 10 минут. Таким образом образец приложения обновляет токен доступа минутам 9.

Маркер доступа должен быть указан в каждый API REST Bing распознавания речи вызывать как `Authorization` префиксом со строкой заголовка `Bearer`, как показано в следующем примере кода:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Передайте действительный маркер доступа к REST API распознавания речи Bing приведет к ошибке ответов 403.

## <a name="performing-speech-recognition"></a>Выполняет распознавание речи

Распознавание речи достигается путем создания запроса POST для `recognize` API по `https://speech.platform.bing.com/recognize`. Один запрос не может содержать более 10 секунд аудио и длительность общего запроса не может превышать 14 секунд.

Звуковое содержимое должны находиться в тексте запроса в формате wav POST. Сведения о поддерживаемых кодеков см. в разделе [поддерживается кодеки](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) на сайте microsoft.com.

В примере приложения `RecognizeSpeechAsync` метод вызывает процесс распознавания речи:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

В каждом проекте платформой как данные wav PCM, записи звука и `RecognizeSpeechAsync` использует метод `PCLStorage` пакет NuGet, чтобы открыть звуковой файл в виде потока. Созданный URI запроса распознавания речи и маркер доступа, извлекается из службы маркеров. Отправляется запрос распознавания речи `recognize` API, который возвращает ответ JSON, содержащий результат. Результат возвращается в вызывающий метод для отображения десериализуется ответ JSON.

### <a name="configuring-speech-recognition"></a>Настройка распознавания речи

Можно настроить процесс распознавания речи путем указания параметров запроса HTTP. Существуют принудительные и необязательные параметры, с помощью следующего метода, включая принудительное параметры, которые необходимо задать:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

Основной конфигурации, выполненные `GenerateRequestUri` метод заключается в установке языка звукового содержимого. Список поддерживаемых языков см. в разделе [поддерживается язык и региональные стандарты ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) на сайте microsoft.com.

Дополнительные сведения о возможных значениях для принудительного параметров см. в разделе [необходимые параметры](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) на сайте microsoft.com. Сведения о дополнительных параметрах см. в разделе [необязательные параметры](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) на сайте microsoft.com.

### <a name="sending-the-request"></a>Идет отправка запроса

`SendRequestAsync` Метод выполняет запрос POST к REST API распознавания речи Bing и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Этот метод создает запрос POST по:

- Перенос аудиопотока в `StreamContent` экземпляр, который предоставляет содержимое HTTP на основе потока.
- Установка `Content-Type` заголовок запроса на `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Добавление маркера доступа для `Authorization` префиксом со строкой заголовка `Bearer`.

Затем отправляется запрос POST `recognize` API. Ответ считывается и возвращается вызывающему методу.

`recognize` API отправит код состояния HTTP 200 (ОК) в ответе, что запрос допустим, указывающая, что запрос выполнен успешно, и что запрашиваемые данные находятся в ответе. Список возможных ошибках см. в разделе [сообщения об ошибках](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) на сайте microsoft.com.

### <a name="processing-the-response"></a>Обработка ответа

API ответ возвращается в формате JSON, содержащего распознанный текст содержащийся в `name` тег. Приведенные ниже данные JSON показано сообщение обычно успешный ответ:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

В примере приложения, ответ JSON десериализуется в `SpeechResult` экземпляра, результат возвращается в вызывающий метод для отображения, как показано на следующем снимке экрана:

![](speech-recognition-images/speech-recognition.png "Распознавание речи")

Сведения о значениях каждого тега JSON см. в разделе [ответов распознавания речи](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) на сайте microsoft.com.

## <a name="summary"></a>Сводка

В этой статье описано, как использовать API REST распознавания речи Bing для преобразования текста в приложении Xamarin.Forms аудио. Помимо выполнения распознавания речи, API речь Bing также можно преобразовывать текста в речь.



## <a name="related-links"></a>Связанные ссылки

- [Документация речь Bing](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Использование веб-службы RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO Когнитивных службы (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Распознавание речи Bing REST API](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
