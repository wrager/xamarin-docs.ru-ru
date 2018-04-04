---
title: Стек HttpClient и селектор реализации SSL/TLS для операций ввода-вывода и macOS
description: Стек HttpClient и селектор реализации SSL/TLS определяет, будет использоваться приложением Xamarin iOS, tvOS или macOS реализацию класса HttpClient и SSL/TLS.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 06/12/2017
ms.openlocfilehash: ba9eb6a062ce91db5f1597de6f9a2b01ad18a367
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="httpclient-stack-and-ssltls-implementation-selector-for-iosmacos"></a>Стек HttpClient и селектор реализации SSL/TLS для операций ввода-вывода и macOS

## <a name="httpclient-stack-selector"></a>Выбор HttpClient стека

Доступны для Xamarin.iOS, Xamarin.tvOS и Xamarin.Mac: Определяет, что `HttpClient` реализации для использования. Значение по умолчанию по-прежнему будет HttpClient, который работает на основе `HttpWebRequest`, а теперь при необходимости выберите нужный к реализациям операций ввода-вывода, tvOS или macOS собственного транспортов (`NSUrlSession` или `CFNetwork` в зависимости от операционной системы). Плюсом двоичных файлов меньшего размера и быстрее осуществлять загрузку, недостатком является то, что требуется для выполнения асинхронных операций для выполнения цикла событий.

Проекты должны ссылаться на **System.Net.Http** сборки.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>При выборе стека HttpClient

Настройка HttpClient, используется в приложении:

1. Дважды щелкните **имя проекта** в **обозревателе решений** чтобы открыть параметры проекта.
2. Переключитесь в **построения** параметров проекта (например, **iOS построения** для приложения Xamarin.iOS).
3. Из **реализацию HttpClient** раскрывающийся список, выберите тип HttpClient в качестве одного из следующих: **управляемое**, **CFNetwork** или **NSUrlSession**.

[![Выберите управляемый, CFNetwork или NSUrlSession реализацию HttpClient](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

<a name="Managed" />

### <a name="managed-default"></a>Управляемый (по умолчанию)

Обработчик управляемый код является полностью управляемым HttpClient обработчик, который будет отправлен с предыдущую версию Xamarin.

#### <a name="pros"></a>Преимущества:

 - Она содержит набор с Microsoft .NET и более ранних версий Xamarin функций, наиболее подходящие.

#### <a name="cons"></a>Недостатки:

 - Он не полностью интегрирована с ОС Apple и ограничена TLS 1.0.
 - Он обычно гораздо медленнее в таких вещей, как шифрование собственных API.
 - Она требует более управляемого кода, создавая таким образом приложение большего размера распространяемого.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

Обработчик основе CFNetwork основан на собственный `CFNetwork` framework, доступные в iOS 6 и более поздних.

#### <a name="pros"></a>Преимущества:

 - Он использует собственные интерфейсы API для повышения производительности и меньший размер исполняемого файла.
 - Поддержка новых стандартов, таких как TLS 1.2.

#### <a name="cons"></a>Недостатки:

 - Требуется iOS 6 или более поздней версии.
 - Недоступно на watchOS.
 - Некоторые функции HttpClient и параметры недоступны.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

Обработчик основе NSURLSession основан на собственный `NSURLSession` framework, доступные в iOS 7 и более поздних.

#### <a name="pros"></a>Преимущества:

 - Он использует собственные интерфейсы API для повышения производительности и меньший размер исполняемого файла.
 - Поддерживает последние стандарты, такие как TLS 1.2.

#### <a name="cons"></a>Недостатки:

 - Требуется iOS 7 или более поздней версии.
 - Некоторые функции HttpClient и параметры недоступны.

### <a name="programmatically-setting-the-httpmessagehandler"></a>Программной настройке HttpMessageHandler

Помимо конфигурации на уровне проекта, показанном выше, можно также создать экземпляр `HttpClient` и вставляют нужного `HttpMessageHandler` через конструктор, как показано в следующих фрагментах кода:

```csharp
// This will use the default message handler for the application; as
// set in the Project Options for the project.
HttpClient client = new HttpClient();

// This will create an HttpClient that explicitly uses the CFNetworkHandler
HttpClient client = new HttpClient(new CFNetworkHandler());

// This will create an HttpClient that explicitly uses NSUrlSessionHandler
HttpClient client = new HttpClient(new NSUrlSessionHandler());
```

Это дает возможность использовать другой `HttpMessageHandler` из что объявляется в **параметры проекта** диалогового окна.

<a name="New-SSL-TLS-implementation-build-option" />
<a name="Selecting-a-SSL-TLS-implementation" />
<a name="Apple-TLS" />

## <a name="ssltls-implementation-build"></a>Сборки для реализации SSL/TLS

Протокол SSL (Secure Socket Layer) и его последователь TLS (протокол TLS), обеспечивают поддержку для протокола HTTP и другими сетевыми подключениями через `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS или Xamarin.Mac в `System.Net.Security.SslStream` реализация будет вызывать Apple стандартной реализации SSL/TLS, вместо использования управляемого реализацию, предоставляемую моно. Реализация собственного Apple поддерживает TLS 1.2.

<a name="Mono" />

> [!WARNING]
> **Моно/управляемый код** поставщика TLS ограничена SSL v3 и TLS v1. Этот поставщик TLS рекомендуется к использованию и больше не доступен для Xamarin.iOS приложений. 

<a name="App-Transport-Security" />

## <a name="app-transport-security"></a>Безопасность транспорта приложения

Apple _безопасность транспорта приложения_ (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения. ATS гарантирует, соответствуют всем каналам связи безопасное подключение, советы и рекомендации, тем самым предотвращая случайного раскрытия конфиденциальной информации, непосредственно через приложение или библиотеки, которая его использует.

Поскольку ATS включена по умолчанию в приложениях, собранных для iOS 9, tvOS 9 и OS X 10.11 (El Capitan) и более поздних версиях все подключения, использующие `NSUrlConnection`, `CFUrl` или `NSUrlSession` будет зависеть от ATS требования безопасности. Если соединения не отвечает этим требованиям, произойдет сбой с исключением.

С учетом выбранных стека HttpClient и реализации SSL/TLS, может потребоваться внести изменения в приложение для правильной работы с ATS.

Чтобы узнать больше о ATS, см. в разделе нашей [руководство по безопасности транспорта приложения](~/ios/app-fundamentals/ats.md).

## <a name="known-issues"></a>Известные проблемы

В этом разделе будут рассмотрены известные проблемы с поддержкой TLS в Xamarin.iOS.

### <a name="project-failed-to-load-with-error-requested-value-appletls-wasnt-found"></a>Проект не удалось загрузить с ошибкой «Запрошенное значение не было найдено AppleTLS»

Xamarin.iOS 9.8 появились некоторые новые параметры содержится **.csproj** файла для приложения Xamarin.iOS. Эти изменения могут вызвать проблемы при открытии проекта с более ранними версиями Xamarin.iOS. Снимке экрана ниже приведен пример сообщения об ошибке, которое может отображаться в этом сценарии.

![Снимок экрана: произошла ошибка при попытке загрузить проект, запросу не найдено значение для прежних версий](http-stack-images/tlserror-xs.png)

Эта ошибка вызвана появлением `MtouchTlsProvider` файл проекта в Xamarin.iOS 9.8 параметров. Если он не является возможность обновления для Xamarin.iOS 9.8 (или более поздней версии), работа решением является изменение вручную **.csproj** файла приложения, удалить `MtouchTlsprovider` элемент, а затем сохраните файл измененного проекта.

Ниже приведен пример того, что `MtouchTlsProvider` параметра может выглядеть как внутри **.csproj** файла:

```xml
<MtouchTlsProvider>Default</MtouchTlsProvider>
```

## <a name="related-links"></a>Связанные ссылки

- [Протокол TLS](~/cross-platform/app-fundamentals/transport-layer-security.md)
- [Безопасность транспорта приложения](~/ios/app-fundamentals/ats.md)
