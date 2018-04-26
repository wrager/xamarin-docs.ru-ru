---
title: Безопасности транспортного уровня (TLS) 1.2
description: Включение TLS 1.2 для проектов Xamarin на Android, iOS и Mac
ms.prod: xamarin
ms.assetid: 399F71C6-16A4-4ABC-B30D-AF17D066A5FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2018
ms.openlocfilehash: 6205e8633ccdd2c1e568e7de8103c38eb9edbc2f
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="transport-layer-security-tls-12"></a>Безопасности транспортного уровня (TLS) 1.2

С помощью последней версии [ _Transport Layer Security_ (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security) важно для обеспечения сетевого взаимодействия приложений являются безопасными.

> [!WARNING]
> **Апрель, 2018** — из-за повышения уровня безопасности, включающие соответствие стандартам PCI основные поставщиков облачных служб и веб-серверов требуется остановить, поддерживающей протокол TLS версий старше версии 1.2.  Xamarin проекты, созданные в предыдущих версиях Visual Studio по умолчанию для использования более ранних версий TLS.
>
> Чтобы продолжить работу с этих серверов и служб, приложений **следует обновить проекты Xamarin на расположенные ниже, а затем повторно постройте и повторного развертывания приложения** для пользователей.

Проекты должны ссылаться на **System.Net.Http** сборки и настроен, как показано ниже.

## <a name="update-android-to-tls-12"></a>Обновление Android для TLS 1.2

Обновление **реализацию HttpClient** и **реализации SSL/TLS** для включения безопасности TLS 1.2.

> [!NOTE]
> Требуется Android 5.0 или более поздней версии.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Эти параметры можно найти в **свойства проекта > Параметры Android** и щелкнув **Дополнительно** кнопки:

[![Настройка HttpClient и TLS в Visual Studio](transport-layer-security-images/android-win-sml.png)](transport-layer-security-images/android-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Эти параметры можно найти в **параметры проекта > сборки > Android построения** вкладки:

[![Настройка HttpClient и TLS в Visual Studio для Mac](transport-layer-security-images/android-mac-sml.png)](transport-layer-security-images/android-mac.png#lightbox)

-----

## <a name="update-ios-to-tls-12"></a>Обновите iOS до TLS 1.2

Обновление **HttpClient реализацию** параметр, чтобы включить безопасность TSL 1.2.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Этот параметр можно найти в **свойства проекта > iOS построения**:

[![Настройка HttpClient и TLS в Visual Studio](transport-layer-security-images/ios-win-sml.png)](transport-layer-security-images/ios-win.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Этот параметр можно найти в **параметры проекта > сборки > iOS построения** вкладки:

[![Настройка HttpClient в Visual Studio для Mac](transport-layer-security-images/ios-mac-sml.png)](transport-layer-security-images/ios-mac.png#lightbox)

-----

## <a name="update-macos-to-tls-12-in-visual-studio-for-mac"></a>Обновление macOS TLS 1.2 в Visual Studio для Mac

Обновление **реализацию HttpClient** в диалоговом окне **параметры проекта > сборки > построения Mac** вкладку, чтобы включить безопасность TSL 1.2:

[![Настройка HttpClient в Visual Studio для Mac](transport-layer-security-images/macos-mac-sml.png)](transport-layer-security-images/macos-mac.png#lightbox)

## <a name="alternative-configuration-options"></a>Альтернативная конфигурация параметров

В этом разделе рассматриваются альтернативные подходы к конфигурации поддерживается TLS 1.2, показанном выше.
Разработчики приложений могут попробовать этих вариантов, только если они понимаете риски использования разные уровни поддержки TLS.

### <a name="httpclient-implementation"></a>Реализация HttpClient

Разработчики Xamarin всегда были возможность использовать собственный сетевые классы в коде, однако также используется параметр, который определяет, какой сетевой стек `HttpClient` классы. Это предоставляет знакомый API .NET, который имеет преимущества скорости и безопасности собственной платформы.

Доступны следующие варианты.

- **Управляемого стека** — функцию моно сеть или
- **Собственный стек** — различных сетевых API-интерфейсов, предоставляемых базовой платформы (Android, iOS или macOS).

Управляемого стека обеспечивает наивысший уровень совместимости с существующим кодом .NET, однако он может работать медленнее и больший размер исполняемого файла.

Собственные параметры могут выполняться быстрее, имеют более высокий уровень безопасности (включая TLS 1.2), но не обеспечивают функциональные возможности и параметры `HttpClient` класса.

### <a name="ssltls-implementation-android"></a>Реализация SSL/TLS (Android)

Параметры проекта для Android также позволяют выбрать, какая реализация SSL/TLS для поддержки:

- **Моно или управляемые** — TLS 1.1 на Android
- **Собственный** — TLS 1.2 на Android.

По умолчанию новые проекты Xamarin собственную реализацию, которая поддерживает TLS 1.2 (что рекомендуется для всех проектов), однако можно переключиться обратно для управляемого кода, если необходимо для обеспечения совместимости.

> [!IMPORTANT]
> **Моно/управляемый код** параметр был [удалены из iOS и Mac](https://developer.xamarin.com/releases/ios/xamarin.ios_10/xamarin.ios_10.8/) параметры проекта.
>
> Для операций ввода-вывода и платформ Mac всегда используется собственный вариант.

## <a name="platform-specific-details"></a>Сведения о конкретной платформы

Сводка выше описание параметрами уровня проекта для класса HttpClient и SSL/TLS реализации проектов Xamarin. Реализация HttpClient также можно динамически задать в коде. Эти руководства платформой для получения дополнительной информации см.:

- [**Android**](~/android/app-fundamentals/http-stack.md)
- [**iOS и Mac**](~/cross-platform/macios/http-stack.md)


## <a name="summary"></a>Сводка

По возможности следует использовать Transport Layer Security (TLS) 1.2.
Необходимо обновить параметры в существующие приложения в соответствии с инструкциями в этой статье затем повторное построение и повторно развернуть для ваших клиентов.

## <a name="related-links"></a>Связанные ссылки

- [Безопасность транспорта приложения](~/ios/app-fundamentals/ats.md)
- [Xamarin.Android среды](~/android/deploy-test/environment.md)
- [Цикл Xamarin 9 (февраля 2017 г.)](https://releases.xamarin.com/stable-release-cycle-9/)
- [TLS (Википедия)](https://en.wikipedia.org/wiki/Transport_Layer_Security)
- [Заметки о выпуске моно 4.8 - поддерживает TLS 1.2](http://www.mono-project.com/docs/about-mono/releases/4.8.0/#tls-12-support)
- [BoringSSL](https://boringssl.googlesource.com/boringssl/)
- [HttpClient, HttpClientHandler и WebRequestHandler описано](https://blogs.msdn.microsoft.com/henrikn/2012/08/07/httpclient-httpclienthandler-and-webrequesthandler-explained/)
- [System.Net.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)
- [System.Net.HttpClientHandler](https://msdn.microsoft.com/library/system.net.http.httpclienthandler(v=vs.118).aspx)
- [System.Net.HttpMessageHandler](https://msdn.microsoft.com/library/system.net.http.httpmessagehandler(v=vs.118).aspx)
- [System.Net.HttpWebRequest](https://msdn.microsoft.com/library/system.net.httpwebrequest(v=vs.110).aspx)
- [System.Net.WebClient](https://msdn.microsoft.com/library/system.net.webclient(v=vs.110).aspx)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [java.net.URLConnection](http://developer.android.com/reference/java/net/URLConnection.html)
- [Foundation.CFNetwork](https://developer.xamarin.com/api/type/CoreFoundation.CFNetwork/)
- [Foundation.NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/)
- [System.Net.WebRequest](https://msdn.microsoft.com/library/system.net.webrequest(v=vs.110).aspx)
- [HTTP-клиента (пример)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
