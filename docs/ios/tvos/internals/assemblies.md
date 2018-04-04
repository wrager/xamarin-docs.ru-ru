---
title: Сборки
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
ms.openlocfilehash: 7d0ee27cfa2ae153ef481f943402f5fcfc5d04e4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="assemblies"></a>Сборки

## <a name="supported-assemblies"></a>Поддерживаемых сборок

Это список сборок, поддерживаемых Xamarin для Xamarin.tvOS приложений. Подробный список этих приведена ниже.  Включает некоторые важные пропусков `System.EnterpriseServices`, стек ASP.NET и Windows.Forms.

|Assembly|Добавлено|Совместимость API|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|Для компиляторов.|
|Mono.Data.Sqlite.dll|1.2|Поставщик ADO.NET для SQLite; в разделе [ограничения](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|Поддержка протокола потока табличных данных; используется для [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) поддерживают в [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Интерфейсы API шифрования.|
|monotouch.dll|1.0|Эта сборка содержит [C# привязки к CocoaTouch API](https://developer.xamarin.com/api/root/ios-unified/).|
|mscorlib.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|OpenTK.dll|1.0|Объект OpenGL/OpenAL ориентированного API-интерфейсы, [расширены и поддерживают устройства iPhone](https://developer.xamarin.com/api/namespace/OpenGLES/).|
|System.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Data.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx), [с некоторыми ограничениями](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Клиент полный oData.|
|System.Drawing;|1.0|System.Drawing API - интерфейс классический API.<br />_System.Drawing не поддерживается в единой API для Xamarin.Mac .NET 4.5 или мобильных платформ._|
|System.Json.dll|1.1|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.Runtime.Serialization.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.dll|1.1|[WCF](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) стека, заданное на [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|
|System.ServiceModel.Web.dll|?|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <ul><li>Система</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); частью [System.Data](https://docs.microsoft.com/xamarin/ios/data-cloud/system.data) поддержки.|
|System.Web.Services|1.1|[Базовых веб-служб](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services) профиля .NET 3.5, с удаленные компоненты сервера.|
|System.Xml.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|
|System.Xml.Linq.dll|1.0|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|

<a name="Summary" />

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов

Помимо привязки Mac можно использовать Xamarin.tvOS [переносимой библиотеки классов .NET](~/cross-platform/app-fundamentals/pcl.md).



## <a name="related-links"></a>Связанные ссылки

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
