---
title: Безопасность транспорта приложения в Xamarin.iOS
description: Безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения.
ms.prod: xamarin
ms.assetid: F8C5E444-2D05-4D9B-A2EF-EB052CD6F007
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 71632da89c6a276b427b36f91eb343ab0a5c515b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784326"
---
# <a name="app-transport-security-in-xamarinios"></a>Безопасность транспорта приложения в Xamarin.iOS

_Безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения._

В этой статье приведены изменения системы безопасности, которые принудительно обеспечивает безопасность транспорта приложения на приложение iOS 9 и [это означает для проектов Xamarin.iOS](#xamarinsupport), включают [параметры конфигурации ATS](#config) и он рассматривается как [отказаться от ATS](#optout) ATS при необходимости. Поскольку ATS включен по умолчанию, все подключения к Интернету небезопасные вызовет исключение в приложениях iOS 9 (если она явно разрешен).


## <a name="about-app-transport-security"></a>Сведения о безопасности транспорта приложения

Как уже говорилось выше, ATS гарантирует, соответствуют всем каналам связи в iOS 9 и OS X El Capitan безопасное подключение, советы и рекомендации, чтобы предотвратить случайное раскрытие конфиденциальных сведений непосредственно через приложение или библиотеку, которая является Использование.

Для существующих приложений реализовать `HTTPS` протокола, когда это возможно. Для новых приложений Xamarin.iOS, следует использовать `HTTPS` исключительно при взаимодействии с Интернет-ресурсов. Кроме того общий API связи должен быть зашифрован с помощью TLS версии 1.2 с безопасной пересылки.

Подключение, выполненное с [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) или [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) будет использоваться ATS по умолчанию в приложениях, собранных для iOS 9 и OS X 10.11 (El Capitan).

## <a name="default-ats-behavior"></a>Поведение по умолчанию ATS

Поскольку ATS включена по умолчанию в приложениях, собранных для iOS 9 и OS X 10.11 (El Capitan), все подключения, использующие [NSUrlConnection](https://developer.xamarin.com/api/type/Foundation.NSUrlConnection/), [CFUrl](https://developer.xamarin.com/api/type/CoreFoundation.CFUrl/) или [NSUrlSession](https://developer.xamarin.com/api/type/Foundation.NSUrlSession/) будут защищены ATS требования безопасности. Если эти требования не подходят для подключений, произойдет сбой с исключением.

### <a name="ats-connection-requirements"></a>Требования к соединению ATS

ATS будет обеспечивают следующие требования для всех подключений к Интернету.

- Шифры все соединения должны использовать пересылку (PFS). См. список допустимых шифрами ниже.
- Протокол Transport Layer Security (TLS), должен быть версии 1.2 или более поздней.
- Для всех сертификатов, необходимо использовать по крайней мере SHA256 отпечаток пальца с 2048 бит или больше ключ RSA, или 256 бит или больше ключ Elliptic кривых (ECC).

Еще раз поскольку ATS включена по умолчанию в iOS 9, любая попытка подключения не соответствует требованиям приведет к формированию исключения. 

<a name="ATS-Compatible-Ciphers" />

### <a name="ats-compatible-ciphers"></a>Шифры совместимые ATS

Следующий тип шифрования безопасной пересылки, принимаются ATS защищенным каналам связи:

- `TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA`
- `TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256`
- `TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA`

Дополнительные сведения о работе с классами связи Интернет iOS см. в разделе Apple [ссылку на класс NSURLConnection](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSURLConnection_Class/index.html#//apple_ref/doc/uid/TP40003755), [ссылки CFURL](https://developer.apple.com/library/prerelease/ios/documentation/CoreFoundation/Reference/CFURLRef/index.html#//apple_ref/doc/uid/20001206) или [NSURLSession ссылку на класс ](https://developer.apple.com/library/prerelease/ios/documentation/Foundation/Reference/NSURLSession_class/index.html#//apple_ref/doc/uid/TP40013435).

<a name="xamarinsupport" />

## <a name="supporting-ats-in-xamarinios"></a>Поддержка ATS в Xamarin.iOS

Поскольку ATS включен по умолчанию в iOS 9 и OS X El Capitan, если приложения Xamarin.iOS, любой библиотеки или службы, которую она использует производит подключение к Интернету, необходимо выполнить некоторое действие или подключений к приведет к созданию исключения.

Для существующего приложения Apple предлагает поддерживаете `HTTPS` протокола как можно быстрее. Если вы либо невозможно, так как вы подключаетесь к сторонним веб-службы, которая не поддерживает `HTTPS` или если поддержка `HTTPS` будет крайне неэффективно, вы можете отказаться от ATS. В разделе [Opting масштабированием ATS](#optout) ниже, в разделе для получения дополнительных сведений.

Для нового приложения Xamarin.iOS, следует использовать `HTTPS` исключительно при взаимодействии с Интернет-ресурсов. Опять же, может возникнуть ситуация (например, с помощью веб-службы сторонних производителей) где это невозможно, и вам потребуется выключить ATS.

Кроме того ATS обеспечивает высокого уровня API связи с помощью TLS версии 1.2 с безопасной пересылки. . В разделе [требования к соединению ATS](#ATS-Connection-Requirements) и [совместимый шифры ATS](#ATS-Compatible-Ciphers) разделах выше для получения дополнительных сведений.

Не может быть знакомы с TLS ([Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security)) он является преемником SSL ([Secure Socket Layer](https://en.wikipedia.org/wiki/Transport_Layer_Security)) и предоставляет коллекцию протоколов шифрования для обеспечения безопасности через Сетевые подключения.

Уровень TLS управляется веб-службы для использования, поэтому за пределами элемента управления приложения. Как `HttpClient` и `ModernHttpClient` будет использовать самый высокий уровень шифрования TLS, поддерживаемые сервером.

В зависимости от сервера, что идет (особенно если это сторонняя служба), может потребоваться отключить пересылку (PFS) или выберите более низкого уровня TLS. В разделе [Настройка параметров ATS](#Configuring-ATS-Options) ниже, в разделе для получения дополнительных сведений.

> [!IMPORTANT]
> Безопасность транспорта приложения не применяется к с помощью приложений Xamarin **HTTPClient управляемых реализаций**. Он применяется для соединений с помощью CFNetwork **реализации HTTPClient** или **NSURLSession HTTPClient реализации** только.

### <a name="setting-the-httpclient-implementation"></a>Параметр реализации HTTPClient

Чтобы задать HTTPClient реализацию, используемую приложением iOS, дважды щелкните **проекта** в **обозревателе решений** Открытие **параметры проекта**. Перейдите к **сборка iOS** и выберите тип нужного клиента в разделе **HttpClient реализацию** раскрывающегося списка:

![](ats-images/client01.png "Задание параметров построения iOS")


#### <a name="managed-handler"></a>Управляемый обработчик.

Управляемый код обработчика является полностью управляемым обработчик HttpClient, будет отправлен с предыдущими версиями Xamarin.iOS и обработчик по умолчанию.

Преимущества:

- Это наиболее совместимый с Microsoft .NET и старую версию Xamarin.

Недостатки:

- Он не полностью интегрирована с операций ввода-вывода (например, это обязательно TLS 1.0).
- Это обычно гораздо медленнее, чем собственных API.
- Она требует более управляемого кода и создает большего приложения.

#### <a name="cfnetwork-handler"></a>Обработчик CFNetwork

Обработчик CFNetwork основе основан на собственный `CFNetwork` framework.

Преимущества:

- Использует собственный API для повышения производительности и меньшего размера исполняемый файл.
- Добавляет поддержку для новых стандартов, таких как TLS 1.2.

Недостатки:

- Требуется iOS 6 или более поздней версии.
- Не доступен из watchOS.
- Некоторые функции HttpClient и параметры недоступны.

#### <a name="nsurlsession-handler"></a>Обработчик NSUrlSession

Обработчик NSUrlSession основе основан на собственный `NSUrlSession` API.

Преимущества:

- Использует собственный API для повышения производительности и меньшего размера исполняемый файл.
- Добавляет поддержку для новых стандартов, таких как TLS 1.2.

Недостатки:

- Требуется iOS 7 или более поздней версии.
- Некоторые функции HttpClient и параметры недоступны. 

## <a name="diagnosing-ats-issues"></a>Диагностика проблем ATS

При попытке подключения к Интернету напрямую или с веб-страниц в iOS 9, может получить ошибку в виде:

> Безопасность транспорта приложения заблокировал текстом HTTP (http://www.-the-blocked-domain.com) загрузки ресурсов, так как он не защищен. Временные исключения можно настроить через файл Info.plist приложения.

В iOS9 безопасность транспорта приложения (ATS) обеспечивает безопасное соединение между Интернет-ресурсов (например, приложение серверных) и приложения. Кроме того, ATS требует взаимодействия с использованием `HTTPS` протокола и высокого уровня API связи с помощью TLS версии 1.2 с безопасной пересылки.

Поскольку ATS включена по умолчанию в приложениях, собранных для iOS 9 и OS X 10.11 (El Capitan), все подключения, использующие `NSURLConnection`, `CFURL` или `NSURLSession` будет зависеть от ATS требования безопасности. Если эти требования не подходят для подключений, произойдет сбой с исключением.

Также предоставляет Apple [TLSTool пример приложения](https://developer.apple.com/library/mac/samplecode/sc1236/Introduction/Intro.html#//apple_ref/doc/uid/DTS40014927-Intro-DontLinkElementID_2) , можно скомпилировать (или при необходимости перекодировать Xamarin и C#) и используется для диагностики проблем ATS/TLS. См. в разделе [Opting масштабированием ATS](#optout) информацию о решении этой проблемы.


<a name="config" />

## <a name="configuring-ats-options"></a>Настройка параметров ATS

Некоторые функции ATS можно настроить, задав значения для определенных разделов в вашем приложении **Info.plist** файла. Доступны следующие разделы для управления ATS (_с отступом, чтобы показать, как вложенные_):

```csharp
NSAppTransportSecurity
    NSAllowsArbitraryLoads
    NSAllowsArbitraryLoadsInWebContent
    NSExceptionDomains
    <domain-name-for-exception-as-string>
        NSExceptionMinimumTLSVersion
        NSExceptionRequiresForwardSecrecy
        NSExceptionAllowsInsecureHTTPLoads
        NSRequiresCertificateTransparency
        NSIncludesSubdomains
        NSThirdPartyExceptionMinimumTLSVersion
        NSThirdPartyExceptionRequiresForwardSecrecy
        NSThirdPartyExceptionAllowsInsecureHTTPLoads
```

Каждый ключ имеет следующий тип и значение.

- **NSAppTransportSecurity** (`Dictionary`) — содержит все ключи параметров и значений для ATS.
- **NSAllowsArbitraryLoads** (`Boolean`) — Если `YES` ATS будет отключена для любого домена **не** в `NSExceptionDomains`. Параметры безопасности будет использоваться для указанных доменов.
- **NSAllowsArbitraryLoadsInWebContent** (`Boolean`) — Если `YES` позволит веб-страницы правильно загрузить во время защиты безопасности транспорта Apple (ATS) все еще включена для остальной части приложения.
- **NSExceptionDomains** (`Dictionary`) — это совокупность доменов и параметры безопасности, которые ATS следует использовать для данного домена.
- **< Domain-name-for-exception-as-string >** (`Dictionary`)-коллекцию исключений для данного домена (например) `www.xamarin.com`).
- **NSExceptionMinimumTLSVersion** (`String`)-минимальной TLS, либо как `TLSv1.0`, `TLSv1.1` или `TLSv1.2` (используется по умолчанию).
- **NSExceptionRequiresForwardSecrecy** (`Boolean`) — Если `NO` не использовать шифр прямой безопасности домена. Значение по умолчанию — `YES`.
- **NSExceptionAllowsInsecureHTTPLoads** (`Boolean`) — Если `NO` (по умолчанию), все связи с этим доменом должен находиться в `HTTPS` протокола.
- **NSRequiresCertificateTransparency** (`Boolean`) — Если `YES` домена Secure Sockets Layer (SSL) должны содержать допустимые прозрачности данные. Значение по умолчанию — `NO`.
- **NSIncludesSubdomains** (`Boolean`) — Если `YES` эти параметры переопределяют все поддомены этого домена. Значение по умолчанию — `NO`.
- **NSThirdPartyExceptionMinimumTLSVersion** (`String`)-TLS, используемой при домена служба третьей стороны вне контроля разработчика.
- **NSThirdPartyExceptionRequiresForwardSecrecy** (`Boolean`) — Если `YES` сторонних производителей домена требуется пересылку (PFS).
- **NSThirdPartyExceptionAllowsInsecureHTTPLoads** (`Boolean`) — Если `YES` ATS позволит не безопасный обмен данными с доменами сторонних производителей.

<a name="optout" />

### <a name="opting-out-of-ats"></a>Выбор Out ATS

При высокой Apple предлагает использовать `HTTPS` протокола и безопасное подключение к Интернету на основе сведений, возможны ситуации, которые не всегда возможно. Например, при обмене данными с веб-службы сторонних производителей или через Интернет, доставленных рекламы в вашем приложении.

Если приложение Xamarin.iOS необходимо создать запрос к небезопасным доменом, следующие изменения в свое приложение **Info.plist** файла приведет к отключению безопасности по умолчанию, которые ATS применяет для данного домена:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSExceptionDomains</key>
    <dict>
        <key>www.the-domain-name.com</key>
        <dict>
            <key>NSExceptionMinimumTLSVersion</key>
            <string>TLSv1.0</string>
            <key>NSExceptionRequiresForwardSecrecy</key>
            <false/>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

В среде Visual Studio для Mac дважды щелкните `Info.plist` файла в **обозревателе решений**, переключитесь в **источника** просмотра и добавления выше ключи:

[![](ats-images/ats01.png "В представлении исходный файл Info.plist")](ats-images/ats01.png#lightbox)


Если приложению необходима для загрузки и отображения веб-содержимого с узлов, не является безопасной, добавьте следующий в свое приложение **Info.plist** файла, чтобы разрешить веб-страницы правильно загрузить во время защиты безопасности транспорта Apple (ATS) все еще включена для остальных приложения:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key> NSAllowsArbitraryLoadsInWebContent</key>
    <true/>
</dict>
```

При необходимости можно внести следующие изменения в свое приложение **Info.plist** файл, чтобы полностью отключить ATS для всех доменов и обмен данными через Интернет:

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

В среде Visual Studio для Mac дважды щелкните `Info.plist` файла в **обозревателе решений**, переключитесь в **источника** просмотра и добавления выше ключи:

[![](ats-images/ats02.png "В представлении исходный файл Info.plist")](ats-images/ats02.png#lightbox)

> [!IMPORTANT]
> Если приложению требуется подключение небезопасных веб-сайт, необходимо выполнить **всегда** введите домен, как исключение с помощью `NSExceptionDomains` вместо отключения ATS полностью с помощью `NSAllowsArbitraryLoads`. `NSAllowsArbitraryLoads` следует использовать только в чрезвычайных ситуациях аварийного.




Опять же, отключение ATS следует _только_ использоваться в качестве последнего средства, если переключение на безопасные соединения является нецелесообразным или недоступны.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье появился безопасности транспорта приложения (ATS) и описывается способ, тем самым безопасной связи с Интернетом. Во-первых мы узнали изменения, которые требуется ATS для Xamarin.iOS приложение, работающее на iOS 9. Затем мы узнали, управляющий ATS функции и параметры. Наконец мы узнали, отказ от ATS Xamarin.iOS приложения.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
