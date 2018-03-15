---
title: "Доступных сборок"
description: "Доступных сборок в Xamarin.iOS, Xamarin.Android и Xamarin.Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/13/2018
ms.openlocfilehash: 09df3b5e203281070e21b8c18ee4ff42245c0e46
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="available-assemblies"></a>Доступных сборок

_Доступных сборок в Xamarin.iOS, Xamarin.Android и Xamarin.Mac_

Xamarin.iOS, Xamarin.Android и Xamarin.Mac все поставляются с различных сборок. Так же, как Silverlight является расширенной подмножество системной сборки .NET, Xamarin платформы также является расширенной подмножество несколько Silverlight и сборками рабочего стола .NET.

Xamarin платформы не совместимы с существующими сборками, скомпилированы для разных профиля ABI. Необходимо повторно компилировать исходный код для создания сборки, предназначенные для профиля (так же, как необходимо перекомпилировать предназначены для Silverlight и .NET 3.5 отдельно в исходном коде).

Xamarin.Mac приложения может быть скомпилирован в трех режимах: одна с помощью Xamarin курируемый Mobile Profile, .NET Framework 4.5 Xamarin.Mac, на котором можно выбирать существующих сборок настольные и не поддерживается, использующих API-Интерфейсы .NET содержится в системе моно Установка. Дополнительные сведения см. в разделе нашей [целевые платформы](~/mac/platform/target-framework.md) документации.


## <a name="net-standard-libraries"></a>Стандартные библиотеки .NET

В дополнение к Android, iOS и Mac привязок проекты могут использовать Xamarin [.NET стандартные библиотеки](~/cross-platform/app-fundamentals/net-standard.md).

## <a name="portable-class-libraries"></a>Переносимые библиотеки классов
 
Можно также использовать проектов Xamarin [переносимой библиотеки классов .NET](~/cross-platform/app-fundamentals/pcl.md), несмотря на то, что эта технология рекомендован к использованию пользу .NET Standard.

## <a name="supported-assemblies"></a>Поддерживаемых сборок

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|Совместимость API|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Включает CJK MidEast других, редко, Запад|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|Поставщик ADO.NET для SQLite; см. ограничения.|✓|✓|✓|
> |Mono.Data.Tds.dll|Поддержка протокола потока табличных данных; используется для [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) поддерживают в [System.Data](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Интерфейсы API шифрования.|✓|✓|✓|
> |monotouch.dll|Эта сборка содержит привязки к CocoaTouch API. Такая возможность доступна только в классическом проекты iOS.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|Объект OpenGL/OpenAL ориентированной API-интерфейсы, расширен для обеспечения поддержки устройства iPhone.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security. &#8203;Криптографии<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx) , с [удалены некоторые функциональные возможности](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Клиент полный oData.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net. &#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|Стек WCF, заданное на [Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx), а также типы из следующих пространств имен: <br />Система<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |Система. &#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx); частью [System.Data](~/ios/data-cloud/system.data.md) поддержки.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Основные веб-службы из профиля .NET 3.5, с удаленные компоненты сервера.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/en-us/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|Эта сборка содержит привязки к CocoaTouch API. Используется только в проектах единой операций ввода-вывода.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|Для компиляторов.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |Система. &#8203;Drawing.dll|System.Drawing API - интерфейс классический API. System.Drawing не поддерживается в единой API для Xamarin.Mac .NET 4.5 или мобильных платформ. Можно добавить поддержку System.Drawing iOS и OS X с помощью [sysdrawing coregraphics](https://github.com/mono/sysdrawing-coregraphics) библиотеки|✓| |✓|
