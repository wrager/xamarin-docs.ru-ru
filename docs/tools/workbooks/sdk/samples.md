---
title: Пример интеграции
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 62d8cf14d6deb9c59297b708221dec10ca009f7f
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="sample-integrations"></a>Пример интеграции

В разделе [приемника Кухня] [ KitchenSink] образец рабочий пример интеграции. Просто построить `KitchenSink.sln` в Visual Studio для Mac или Visual Studio и откройте `KitchenSink.workbook`.

[![Снимок экрана интеграции Кухня приемника](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

В образце приемника Кухня показаны оба вида понятия:

* Представление части демонстрируется использование `RepresentationManager` для улучшения визуализации с помощью встроенных представлений.
* `Person` Объекта и его связанные JavaScript модуля подготовки отчетов демонстрируют использование `ISerializableObject` без прохода через представление поставщика.

См. также [SkiaSharp] [ skiasharp] реальных пример интеграции, который использует существующую [представления](~/tools/workbooks/sdk/representations.md) предоставляемые Xamarin книги к просмотру ее типы.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
