---
title: HttpClient и селектор реализации SSL/TLS для операций ввода-вывода и macOS
description: Стек HttpClient и SSL/TLS селектор реализация определяет HttpClient и SSL/TLS реализации, который будет использоваться в приложении iOS, tvOS или macOS Xamarin.
ms.prod: xamarin
ms.assetid: 12101297-BB04-4410-85F0-A0D41B7E6591
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 9de2c97933bd33111a751be51e06dffe09794f15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782272"
---
# <a name="httpclient-and-ssltls-implementation-selector-for-iosmacos"></a>HttpClient и селектор реализации SSL/TLS для операций ввода-вывода и macOS

**HttpClient реализация селектора** для Xamarin.iOS, Xamarin.tvOS и Xamarin.Mac элементов управления, который `HttpClient` реализации для использования. Можно переключиться к реализациям операций ввода-вывода, tvOS или macOS собственного транспортов (`NSUrlSession` или `CFNetwork`, в зависимости от операционной системы). Плюсом является двоичные файлы TLS 1.2 поддержки, меньше и быстрее загружает; Недостатком является то, что требуется для выполнения асинхронных операций для выполнения цикла событий.

Проекты должны ссылаться на **System.Net.Http** сборки.

> [!WARNING]
> **Апрель, 2018** — из-за повышения уровня безопасности, включающие соответствие стандартам PCI основные поставщиков облачных служб и веб-серверов требуется остановить, поддерживающей протокол TLS версий старше версии 1.2.  Xamarin проекты, созданные в предыдущих версиях Visual Studio по умолчанию для использования более ранних версий TLS.
>
> Чтобы продолжить работу с этих серверов и служб, приложений **следует обновить проекты Xamarin с `NSUrlSession` показано ниже, затем повторное построение и повторного развертывания приложения** для пользователей.

<a name="Selecting-a-HttpClient-Stack" />

### <a name="selecting-a-httpclient-stack"></a>При выборе стека HttpClient

Чтобы настроить `HttpClient` , используемого приложения:

1. Дважды щелкните **имя проекта** в **обозревателе решений** чтобы открыть параметры проекта.
2. Переключитесь в **построения** параметров проекта (например, **iOS построения** для приложения Xamarin.iOS).
3. Из **реализацию HttpClient** раскрывающийся список, выберите `HttpClient` тип в качестве одного из следующих действий: **NSUrlSession** (рекомендуется), **CFNetwork**, или  **Управляемые**.

[![Выберите управляемый, CFNetwork или NSUrlSession реализацию HttpClient](http-stack-images/http-xs-sml.png)](http-stack-images/http-xs.png#lightbox)

> [!TIP]
> Для поддержки TLS 1.2 `NSUrlSession` рекомендуется использовать параметр.

<a name="NSUrlSession" />

### <a name="nsurlsession"></a>NSUrlSession

`NSURLSession`-На основе обработчик основан на собственный `NSURLSession` framework, доступные в iOS 7 и более поздних. 
**Это рекомендуемый параметр.**

#### <a name="pros"></a>Преимущества

- Он использует собственные интерфейсы API для повышения производительности и меньший размер исполняемого файла.
- Поддержка последней стандартах, таких как TLS 1.2.

#### <a name="cons"></a>Недостатки

- Требуется iOS 7 или более поздней версии.
- Некоторые `HttpClient` функции и параметры будут недоступны.

<a name="CFNetwork" />

### <a name="cfnetwork"></a>CFNetwork

`CFNetwork`-На основе обработчик основан на собственный `CFNetwork` framework, доступные в iOS 6 и более поздних.

#### <a name="pros"></a>Преимущества

- Он использует собственные интерфейсы API для повышения производительности и меньший размер исполняемого файла.
- Поддержка новых стандартов, таких как TLS 1.2.

#### <a name="cons"></a>Недостатки

- Требуется iOS 6 или более поздней версии.
- Недоступно на watchOS.
- Некоторые функции HttpClient и параметры недоступны.

<a name="Managed" />

### <a name="managed"></a>Управляемый

Обработчик управляемый код является полностью управляемым HttpClient обработчик, который будет отправлен с предыдущую версию Xamarin.

#### <a name="pros"></a>Преимущества

- Она содержит набор с Microsoft .NET и более ранних версий Xamarin функций, наиболее подходящие.

#### <a name="cons"></a>Недостатки

- Он не полностью интегрирована с ОС Apple и ограничена TLS 1.0. Не может быть возможность подключения к безопасным веб-серверам или облачные службы в будущем.
- Он обычно гораздо медленнее в таких вещей, как шифрование собственных API.
- Она требует более управляемого кода, создавая таким образом приложение большего размера распространяемого.

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

## <a name="ssltls-implementation"></a>Реализация SSL/TLS

Протокол SSL (Secure Socket Layer) и его последователь TLS (протокол TLS), обеспечивают поддержку для протокола HTTP и другими сетевыми подключениями через `System.Net.Security.SslStream`. Xamarin.iOS, Xamarin.tvOS или Xamarin.Mac в `System.Net.Security.SslStream` реализация будет вызывать Apple стандартной реализации SSL/TLS, вместо использования управляемого реализацию, предоставляемую моно. Реализация собственного Apple поддерживает TLS 1.2.

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
