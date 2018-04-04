---
title: С помощью Microsoft Speech API распознавания речи
description: API речи Microsoft является облачной API, который предоставляет алгоритмы обработки языка. В этой статье описывается использование REST API распознавания речи Microsoft для преобразования текста в приложении Xamarin.Forms аудио.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 81e645749d239f8964047e92255e786c9b35409d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>С помощью Microsoft Speech API распознавания речи

_API речи Microsoft является облачной API, который предоставляет алгоритмы обработки языка. В этой статье описывается использование REST API распознавания речи Microsoft для преобразования текста в приложении Xamarin.Forms аудио._

## <a name="overview"></a>Обзор

API речи Microsoft состоит из двух компонентов:

- Распознавание речи API для преобразования в текст произносимых слов. Распознавание речи может выполняться через API-Интерфейс REST, клиентская библиотека или библиотека службы.
- Преобразование текста в речь API для преобразования текста в речь. Преобразование текста в речь выполняется через REST API.

Эта статья посвящена выполняет распознавание речи через REST API. Хотя библиотеки клиента и службы поддерживают возвращение частичные результаты, API-интерфейса REST может возвращать только один распознавания результат, не все частичные результаты.

Необходимо получить ключ API для использования API Microsoft речи. Это можно сделать в [повторите служб Когнитивных](https://azure.microsoft.com/try/cognitive-services/).

Дополнительные сведения о Microsoft Speech API см. в разделе [документации по API речи Microsoft](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Проверка подлинности

Каждый запрос к API-интерфейса REST Microsoft речи требуется маркер доступа JSON Web Token (JWT), который можно получить от службы маркеров когнитивных служб в `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Маркер можно получить, выполнив запрос POST в службу маркера указание `Ocp-Apim-Subscription-Key` заголовок, содержащий ключ API, как его значение.

В следующем примере кода показано, как запросить доступ маркера от службы маркеров:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Маркер доступа, возвращаемый, являющийся текст в кодировке Base64, имеет время отключения 10 минут. Таким образом образец приложения обновляет токен доступа минутам 9.

Маркер доступа должен быть указан в каждой REST API Microsoft речи вызывать как `Authorization` префиксом со строкой заголовка `Bearer`, как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Действительный маркер доступа передается API-интерфейса REST Microsoft речи приведет к ошибке ответов 403.

## <a name="performing-speech-recognition"></a>Выполняет распознавание речи

Распознавание речи достигается путем создания запроса POST для `recognition` API по `https://speech.platform.bing.com/speech/recognition/`. Один запрос не может содержать более 10 секунд аудио и длительность общего запроса не может превышать 14 секунд.

Звуковое содержимое должны находиться в тексте запроса в формате wav POST.

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

В каждом проекте платформой как данные wav PCM, записи звука и `RecognizeSpeechAsync` использует метод `PCLStorage` пакет NuGet, чтобы открыть звуковой файл в виде потока. Созданный URI запроса распознавания речи и маркер доступа, извлекается из службы маркеров. Отправляется запрос распознавания речи `recognition` API, который возвращает ответ JSON, содержащий результат. Результат возвращается в вызывающий метод для отображения десериализуется ответ JSON.

### <a name="configuring-speech-recognition"></a>Настройка распознавания речи

Процесс распознавания речи может настраиваться путем указания параметров запроса HTTP:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Основной конфигурации, выполненные `GenerateRequestUri` метод заключается в установке языка звукового содержимого. Список поддерживаемых языков см. в разделе [поддерживаемые языки ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>Идет отправка запроса

`SendRequestAsync` Метод выполняет запрос POST к REST API Microsoft речи и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Этот метод создает запрос POST по:

- Перенос аудиопотока в `StreamContent` экземпляр, который предоставляет содержимое HTTP на основе потока.
- Установка `Content-Type` заголовок запроса на `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Добавление маркера доступа для `Authorization` префиксом со строкой заголовка `Bearer`.

Затем отправляется запрос POST `recognition` API. Ответ считывается и возвращается вызывающему методу.

`recognition` API отправит код состояния HTTP 200 (ОК) в ответе, что запрос допустим, указывающая, что запрос выполнен успешно, и что запрашиваемые данные находятся в ответе. Список возможных ошибках см. в разделе [Устранение неполадок](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Обработка ответа

API ответ возвращается в формате JSON, содержащего распознанный текст содержащийся в `name` тег. Приведенные ниже данные JSON показано сообщение обычно успешный ответ:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

В примере приложения, ответ JSON десериализуется в `SpeechResult` экземпляра, результат возвращается в вызывающий метод для отображения, как показано на следующем снимке экрана:

![](speech-recognition-images/speech-recognition.png "Распознавание речи")

## <a name="summary"></a>Сводка

В этой статье описано, как использовать API-интерфейса REST Microsoft речи для преобразования текста в приложении Xamarin.Forms аудио. Помимо выполнения распознавания речи, Microsoft Speech API также можно преобразовывать текста в речь.

## <a name="related-links"></a>Связанные ссылки

- [Документация Microsoft Speech API](/azure/cognitive-services/speech/home/).
- [Использование веб-службы RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO Когнитивных службы (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
