---
title: "Где найти файл .dSYM symbolicate журналы аварийного завершения операций ввода-вывода"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 223e1977bb6229760d6428fdca2f5372b1e25c23
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Где найти файл .dSYM symbolicate журналы аварийного завершения операций ввода-вывода

При построении приложений iOS из visual studio, .dSYM файл, можно использовать для отчетов о сбоях symbolicate заканчивается на узле сборки по пути:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Обратите внимание, что `~/Library` папка является скрытой по умолчанию в средстве поиска, поэтому, если требуется использовать Finder **Go > перейдите в папку** меню и введите: `~/Library/Caches/Xamarin/mtbs/builds/` открыть папку.  

В качестве альтернативы можно отобразить `~/Library` папки с помощью **Показать параметры представления** панели домашней папки. При выборе домашней папке на боковой панели в Finder и воспользоваться меню поиска **представление > Показать параметры представления** (или cmd-j), затем появится флажок для **показывать папку библиотеки**.


### <a name="see-also"></a>См. также
- Отчеты об аварийном завершении расширенные действия для symbolicating iOS: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Функции приводит к сбою журналах приложения iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
