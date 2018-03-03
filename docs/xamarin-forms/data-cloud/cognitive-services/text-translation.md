---
title: "Перевод текста с помощью преобразователя API"
description: "API-Интерфейс Microsoft Translator можно использовать для преобразования речи и текста с помощью API-интерфейса REST. В этой статье описывается использование API преобразования текста Microsoft для перевода текста с одного языка на другой в приложении Xamarin.Forms."
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>Перевод текста с помощью преобразователя API

_API-Интерфейс Microsoft Translator можно использовать для преобразования речи и текста с помощью API-интерфейса REST. В этой статье описывается использование API преобразования текста Microsoft для перевода текста с одного языка на другой в приложении Xamarin.Forms._

## <a name="overview"></a>Обзор

Транслятор API состоит из двух компонентов:

- Перевод текста REST API для перевод текста с одного языка на другой язык текста. API-Интерфейс автоматически определяет язык текста, который был отправлен до его преобразования.
- Перевод речи REST API для ведения протокола речи с одного языка в текст другого языка. API-Интерфейс также сочетает в себе возможности преобразования текста в речь оперировать переведенный текст обратно.

Эта статья посвящена перевод текста с одного языка на другой с помощью API преобразования текста.

Использование API преобразования текста необходимо получить ключ API. Это можно сделать, следуя указаниям в разделе [Приступая к работе](http://docs.microsofttranslator.com/text-translate.html) на [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

Дополнительные сведения об API Microsoft Translator см. в разделе [документации Microsoft Translator](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview) на сайте microsoft.com.

## <a name="authentication"></a>Проверка подлинности

Каждый запрос к API преобразования текста требуется маркер доступа JSON Web Token (JWT), который можно получить от службы маркеров когнитивных служб в `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Маркер можно получить, выполнив запрос POST в службу маркера указание `Ocp-Apim-Subscription-Key` заголовок, содержащий ключ API, как его значение.

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

Маркер доступа должен быть указан в каждый API преобразования текста вызывать как `Authorization` префиксом со строкой заголовка `Bearer`, как показано в следующем примере кода:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Дополнительные сведения о службе маркеров когнитивных служб см. в разделе [API проверки подлинности маркеров](http://docs.microsofttranslator.com/oauth-token.html) на [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

## <a name="performing-text-translation"></a>Выполняет перевод текста

Перевод текста можно получить, сделав запрос GET к `Translate` API по `https://api.microsofttranslator.com/v2/http.svc/Translate`. В примере приложения `TranslateTextAsync` метод вызывает процесс преобразования текста:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` Метод приводит к возникновению ошибки URI запроса и извлекает токен доступа от службы маркеров. Затем отправляется запрос перевод текста `Translate` API, который возвращает XML-ответ, содержащий результат. Выполняется синтаксический анализ XML-ответа и перевод результат возвращается в вызывающий метод для отображения.

Дополнительные сведения об интерфейсах API REST перевод текста см. в разделе [образец кода](http://docs.microsofttranslator.com/text-translate.html#/default) на [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="configuring-text-translation"></a>Настройка преобразования текста

Процесс преобразования текстового можно настроить путем указания параметров запроса HTTP. Существуют принудительные и необязательные параметры, с помощью следующего метода, включая принудительное параметры, которые необходимо задать:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Этот метод устанавливает текст, которые будут преобразованы и язык перевода текста. Список языков, поддерживаемых Microsoft Translator см. в разделе [языков](https://www.microsoft.com/translator/languages.aspx) на сайте microsoft.com.

> [!NOTE]
> Если приложению нужно знать, какой язык, текст находится в, `Detect` API можно вызвать для определения языка текстовую строку.

Дополнительные сведения о принудительном и необязательные параметры в разделе [API преобразования текста](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) на [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="sending-the-request"></a>Идет отправка запроса

`SendRequestAsync` Метод выполняет запрос GET к REST API преобразования текста и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Этот метод создает запрос GET, добавляя токен доступа для `Authorization` префиксом со строкой заголовка `Bearer`. Затем отправляется запрос GET `Translate` API с URL-АДРЕСЕ запроса задания текста перевода и язык перевода текста. Ответ считывается и возвращается вызывающему методу.

`Translate` API отправит код состояния HTTP 200 (ОК) в ответе, что запрос допустим, указывающая, что запрос выполнен успешно, и что запрашиваемые данные находятся в ответе. Список возможных ошибках см. в разделе ответные сообщения в [получить перевод](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) на [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="processing-the-response"></a>Обработка ответа

API ответ возвращается в формате XML. Следующий XML-данных показано сообщение обычно успешный ответ:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

В примере приложения XML-ответа разбивается на `XDocument` экземпляр со значением корневой XML возвращается в вызывающий метод для отображения, как показано на следующем снимке экрана:

![](text-translation-images/text-translation.png "Перевод текста на немецкий")

## <a name="summary"></a>Сводка

В этой статье описано, как использовать API преобразования текста Microsoft для перевод текста с одного языка на другой язык в приложении Xamarin.Forms текст. Помимо перевод текста, API-Интерфейс Microsoft Translator также можно переписать речи с одного языка в текст другого языка.



## <a name="related-links"></a>Связанные ссылки

- [Документация Microsoft Translator](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [Использование веб-службы RESTful](~/xamarin-forms/data-cloud/consuming/rest.md)
- [TODO Когнитивных службы (пример)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [API преобразования текста](http://docs.microsofttranslator.com/text-translate.html)
