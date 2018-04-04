---
title: Стек HttpClient и селектор реализации SSL/TLS для Android
description: Селекторы стека HttpClient и реализации SSL/TLS определить будет использоваться в приложениях Xamarin.Android реализацию класса HttpClient и SSL/TLS.
ms.prod: xamarin
ms.assetid: D7ABAFAB-5CA2-443D-B902-2C7F3AD69CE2
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 2bc9b2a454b306f0794ef3704daa7e0fe6d04ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-android"></a>Стек HttpClient и селектор реализации SSL/TLS для Android

_Селекторы стека HttpClient и реализации SSL/TLS определить будет использоваться в приложениях Xamarin.Android реализацию класса HttpClient и SSL/TLS._

## <a name="overview"></a>Обзор

Xamarin.Android предоставляет два поля со списком, определяющих параметры TLS для приложения Android. Одно поле со списком будут определять `HttpMessageHandler` будет использовать при создании экземпляра `HttpClient` , пока объект другого определяет, какая реализация TLS будут использоваться веб-запросов.

> [!NOTE]
> Проекты должны ссылаться на **System.Net.Http** сборки.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Параметры для стека HttpClient находятся в параметрах проекта для проекта Xamarin.Android. Щелкните **параметры Android** , а затем щелкните на **Дополнительно** кнопки. При этом отобразится **Android Дополнительно** диалоговое окно, которое имеет два поля со списком: для реализации HttpClient и для реализации SSL/TLS:


[![Параметры Android в Visual Studio](http-stack-images/tls07-vs-sml.png)](http-stack-images/tls07-vs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Выбор HttpClient стека

Этот параметр проекта управляет которой `HttpMessageHandler` реализация будет использоваться при каждом `HttpClient` экземпляра объекта. По умолчанию это управляемый `HttpClientHandler`.

[![Android HttpClient реализации со списком в Visual Studio](http-stack-images/tls04-vs-sml.png)](http-stack-images/tls04-vs.png#lightbox) 


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Параметры для стека HttpClient находятся в параметрах проекта для проекта Xamarin.Android. Щелкните **сборки > Android построения** параметры и нажмите кнопку **Общие** вкладки:

[![Visual Studio для Android параметры Mac](http-stack-images/tls07-xs-sml.png)](http-stack-images/tls07-xs.png#lightbox)

## <a name="httpclient-stack-selector"></a>Выбор HttpClient стека

Этот параметр проекта управляет которой `HttpMessageHandler` реализация будет использоваться при каждом `HttpClient` экземпляра объекта. По умолчанию это управляемый `HttpClientHandler`.

![Android HttpClient реализации со списком в Visual Studio для Mac](http-stack-images/tls04-xs.png )

-----

### <a name="managed-httpclienthandler"></a>Управляемый (HttpClientHandler)

Управляемый обработчик является полностью управляемым HttpClient обработчик, который будет отправлен с предыдущими версиями Xamarin.Android.

#### <a name="pros"></a>Преимущества:

- Это наиболее совместимой (компоненты) с MS .NET и более ранних версий Xamarin.

#### <a name="cons"></a>Недостатки:

- Он не полностью интегрирована с операционной Системой (например) ограничено TLS 1.0).
- Это обычно гораздо медленнее, (например) шифрование) чем собственный API.
- Она требует более управляемый код, создание более крупных приложений.

### <a name="androidclienthandler"></a>AndroidClientHandler

AndroidClientHandler — новый обработчик, который делегирует в машинный код Java/OS вместо реализации все данные в управляемом коде.

#### <a name="pros"></a>Преимущества:

- Используйте собственный API для повышения производительности и меньший размер исполняемого файла.
- Поддержка последние стандарты, например. TLS 1.2.

#### <a name="cons"></a>Недостатки:

- Требуется Android 5.0 или более поздней версии.
- Некоторые функции HttpClient и параметры недоступны.

### <a name="choosing-a-handler"></a>Выбор обработчик

Выбор между `AndroidClientHandler` и `HttpClientHandler` зависит от требований приложения. `AndroidClientHandler` является хорошим выбором, если применяются все из следующих действий:

-   Требуется поддержка TLS 1.2 +.
-   Целевое приложение Android 5.0 (API 21) или более поздней версии.
-   Требуется TLS 1.2 + Поддержка `HttpClient`.
-   Не требуется поддержка TLS 1.2 + `WebClient`.

`HttpClientHandler` является хорошим выбором, если вам требуется TLS 1.2 + поддерживает, но должны поддерживать версии Android, более ранних, чем Android 5.0. Это также хорошо подходит, если требуется TLS 1.2 + Поддержка `WebClient`.

Начиная с Xamarin.Android 8.3 `HttpClientHandler` скучных SSL, по умолчанию используется (`btls`) как базовый поставщик TLS. Поставщик скучных SSL TLS обеспечивает следующие преимущества:

-   Он поддерживает TLS 1.2.
-   Он поддерживает все версии Android.
-   Он поддерживает TLS 1.2 для обоих `HttpClient` и `WebClient`.

Недостаток использования скучных SSL в качестве базового поставщика TLS, его можно увеличить размер результирующей APK (добавляет дополнительный размер APK на поддерживаемых ABI около 1 МБ).

Начиная с Xamarin.Android 8.3, TLS поставщика по умолчанию является скучных SSL (`btls`). Если вы не хотите использовать скучных SSL, можно вернуться к исторических управляемую реализацию протокола SSL, задав `$(AndroidTlsProvider)` свойства `legacy` (Дополнительные сведения о настройке свойств сборки см. в разделе [процессе сборки](~/android/deploy-test/building-apps/build-process.md)).


### <a name="programatically-using-androidclienthandler"></a>Программно с помощью `AndroidClientHandler`

`Xamarin.Android.Net.AndroidClientHandler` — `HttpMessageHandler` Реализацию специально для Xamarin.Android.
Экземпляры этого класса будет использовать собственный `java.net.URLConnection` реализацию для всех подключений HTTP. Теоретически это обеспечит увеличение производительности HTTP и меньшего размера APK.

Этот фрагмент кода приведен пример того, как явно для одного экземпляра `HttpClient` класса:

```csharp
// Android 5.0 or higher, Xamarin.Android 6.1 or higher
HttpClient client = new HttpClient(new Xamarin.Android.Net.AndroidClientHandler ());
```

> [!NOTE]
> Основное устройство Android должны поддерживать протокол TLS 1.2 (т. е. Android 5.0 и более поздние версии)


## <a name="ssltls-implementation-build-option"></a>Параметр построения реализации SSL/TLS

Этот параметр проекта управляет базовой библиотеки TLS будут использоваться все веб-запроса и `HttpClient` и `WebRequest`. По умолчанию выбран TLS 1.2:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Поле со списком реализации TLS/SSL в Visual Studio](http-stack-images/tls06-vs.png)](http-stack-images/tls05-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Поле со списком реализации TLS/SSL в Visual Studio для Mac](http-stack-images/tls06-xs.png)](http-stack-images/tls05-xs.png#lightbox)

-----

Пример:

```csharp
var client = new HttpClient();
```

Если реализация HttpClient было задано значение **управляемый** и реализация TLS задано значение **собственного TLS 1.2 +**, то `client` объект автоматически использовать управляемые `HttpClientHandler` и TLS 1.2 (предоставленный библиотекой BoringSSL) для его HTTP-запросов.

Тем не менее если **реализацию HttpClient** равно `AndroidHttpClient`, затем все `HttpClient` объектов будет использовать базовый класс Java `java.net.URLConnection` и не затрагиваются **реализации TLS/SSL** значение. `WebRequest` объекты будут использовать библиотеку BoringSSL.

## <a name="other-ways-to-control-ssltls-configuration"></a>Другие способы управления конфигурацией SSL/TLS

Существует три способа, что приложение Xamarin.Android можно контролировать параметры TLS:

1. Выберите библиотеку HttpClient по умолчанию и внедрения TLS в параметрах проекта.
2. Программно с помощью `Xamarin.Android.Net.AndroidClientHandler`.
3. Объявите переменные среды (необязательно).

Доступны три варианта является рекомендуемый подход следует использовать для объявления по умолчанию параметры проекта Xamarin.Android `HttpMessageHandler` и TLS для всего приложения. Затем, при необходимости, программным образом создавать `Xamarin.Android.Net.AndroidClientHandler` объекты. Эти параметры описаны выше.

Третий вариант &ndash; использование переменных среды &ndash; описанный ниже.

### <a name="declare-environment-variables"></a>Объявите переменные среды

Существует две переменные среды, которые связаны с использованием TLS в Xamarin.Android:

-   `XA_HTTP_CLIENT_HANDLER_TYPE` &ndash; Значение по умолчанию объявляет эту переменную среды `HttpMessageHandler` , будет использоваться приложением. Пример:

    ```csharp
    XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
    ```

-   `XA_TLS_PROVIDER` &ndash; Эта переменная среды будет объявлять TLS библиотеку, в которой будет использоваться, либо `btls`, `legacy`, или `default` (который является таким же, как пропуск этой переменной):

    ```csharp
    XA_TLS_PROVIDER=btls
    ```

Эта переменная среды, устанавливается путем добавления _файла среды_ в проект. Файл среды — это файл текстовый формат Unix с действием построения **AndroidEnvironment**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Снимок экрана AndroidEnvironment действие построения в Visual Studio.](http-stack-images/tls03-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![Снимок экрана AndroidEnvironment построить действие в Visual Studio для Mac.](http-stack-images/tls03-xs.png)

-----

См. в разделе [Xamarin.Android среды](~/android/deploy-test/environment.md) руководство Дополнительные сведения о переменных среды и Xamarin.Android.


## <a name="related-links"></a>Связанные ссылки

- [Протокол TLS](~/cross-platform/app-fundamentals/transport-layer-security.md)
