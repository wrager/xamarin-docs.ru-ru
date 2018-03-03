---
title: "Безопасности транспортного уровня (TLS)"
description: "Включение TLS 1.2 для проектов Xamarin на Android, iOS и Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/10/2017
ms.openlocfilehash: 5237ed35116e5f8983df579d0ab68363996fb06f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="transport-layer-security-tls"></a>Безопасности транспортного уровня (TLS)

_Включение TLS 1.2 для проектов Xamarin на Android, iOS и Mac_

С помощью последней версии [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) важно для обеспечения сетевого взаимодействия приложений являются безопасными.

> [!NOTE]
> Xamarin освобождает с момента [февраля 2017 г](https://releases.xamarin.com/stable-release-cycle-9/) использовать TLS 1.2 в новых проектах по умолчанию.

Теперь доступна поддержка TLS 1.2 в:

* Моно 4.8 (включает в себя [поддержка TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support))
* Xamarin.iOS
* Xamarin.Mac
* Xamarin.Android (требуется Android 5.0 или более поздней версии)

Проекты должны ссылаться на **System.Net.Http** сборки. 

## <a name="updating-to-tls-12"></a>Обновление для TLS 1.2

В этом разделе описываются некоторые параметры конфигурации для сетевого взаимодействия в проектах Xamarin обновите ваш _существующие_ приложений, чтобы воспользоваться преимуществами более безопасного протокола.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Эти параметры можно найти в **параметры проекта > Параметры Android** и щелкнув **Дополнительно** кнопки: 

[![Настройка HttpClient и TLS в Visual Studio](transport-layer-security-images/properties-vs-sml.png)](transport-layer-security-images/properties-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
Эти параметры можно найти в **свойства проекта > Параметры сборки > Дополнительно** вкладки:

[![Настройка HttpClient и TLS в Visual Studio и Xamarin Studio для Mac](transport-layer-security-images/properties-xs-sml.png)](transport-layer-security-images/properties-xs.png)

-----


### <a name="httpclient-implementation"></a>Реализация HttpClient

Разработчики Xamarin всегда были возможность использовать собственный сетевые классы в коде, однако также используется параметр, который определяет, какой сетевой стек `HttpClient` классы. Это предоставляет знакомый API .NET, который имеет преимущества скорости и безопасности собственной платформы.

Доступны следующие варианты.

- **Управляемого стека** — функцию моно сеть или
- **Собственный стек** — различных сетевых API-интерфейсов, предоставляемых базовой платформы (Android, iOS или macOS).

Управляемого стека обеспечивает наивысший уровень совместимости с существующим кодом .NET, однако он может работать медленнее и больший размер исполняемого файла.

Собственные параметры могут выполняться быстрее, имеют более высокий уровень безопасности (включая TLS 1.2), но не обеспечивают функциональные возможности и параметры `HttpClient` класса.


### <a name="ssltls-implementation"></a>Реализация SSL/TLS

Параметры проекта также позволяют выбрать, какую реализацию SSL/TLS для поддержки:

- **Моно/управляемый код** — TLS 1.1 на Android, TLS 1.0 на iOS и macOS.
- **Собственный** — TLS 1.2 на Android, iOS и macOS.

По умолчанию новые проекты Xamarin собственную реализацию, которая поддерживает TLS 1.2 (что рекомендуется для всех проектов), однако можно переключиться обратно для управляемого кода, если необходимо для обеспечения совместимости.

> [!IMPORTANT]
> **Моно/управляемый код** параметр будет удален в [будущих выпусков](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/).
>
> Собственный режим рекомендуется.

# <a name="platform-specific-details"></a>Сведения о конкретной платформы

Сводка выше описание параметрами уровня проекта для класса HttpClient и SSL/TLS реализации проектов Xamarin. Реализация HttpClient также может быть задана динамически в коде и IOS, существует два варианта собственного для выбора.

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS и Mac**](~/cross-platform/macios/http-stack.md)


# <a name="summary"></a>Сводка

По возможности следует использовать Transport Layer Security (TLS) 1.2.
Новые приложения теперь по умолчанию для этой конфигурации, однако может потребоваться обновить параметры в существующие приложения в соответствии с инструкциями в этой статье.

## <a name="related-links"></a>Связанные ссылки

- [Безопасность транспорта приложения](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android среды](~/android/deploy-test/environment.md)
- [Цикл Xamarin 9 (февраля 2017 г.)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Википедия)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Заметки о выпуске моно 4.8 - поддерживает TLS 1.2](http://www.mono-project.com/docs/about-monohttps://developer.xamarin.com/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler и WebRequestHandler описано](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/en-us/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/en-us/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/en-us/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-клиента (пример)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
