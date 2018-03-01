---
title: "Ошибка конструктора с RegisterServicePort iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8042ea895de3a623279c9d5f3a5309b990489773
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="ios-designer-error-with-registerserviceport"></a>Ошибка конструктора с RegisterServicePort iOS

## <a name="sample-error"></a>Пример ошибки
> System.AggregateException: Произошла одна или несколько ошибок---> System.SystemException: RegisterServicePort (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): возвращаемые ядра: -308 (-308): (ipc и mig) server прервано

## <a name="explanation"></a>Объяснение
Ошибки, связанные с `RegisterServicePort` и аналогичные сообщения об ошибках как выше обычно являются проблемы с шпионского ПО или вредоносных программ на компьютере. Попробуйте [комментарий в этот отчет об ошибке](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) Дополнительные сведения и ссылки [Apple форум обсуждений](https://discussions.apple.com/thread/5596008) по удалению заражения. 

Для диагностики проблемы, откройте приложение macOS **консоли** и удалите каждый файл внутри **диагностические отчеты пользователя** раздел [http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy). Затем запустите Visual Studio для Mac и попытаться использовать конструктор. Если создание файлов журнала отображается в этом разделе после конструктора не удалось инициализировать, сохраните их для нас для анализа.  

Обратите внимание, что этот файл является наиболее важным для проверки: 
> /usr/lib/libimckit.dylib

Независимо от того, приведенные выше результаты, если этот файл существует, упомянутых выше шпионского ПО или вредоносных программ проблема заключается в установлена на компьютере.  

Инструкции по удалению шпионского ПО или вредоносные программы имеет следующую ссылку: [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

