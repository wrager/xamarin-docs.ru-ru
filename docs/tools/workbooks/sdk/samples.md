---
title: "Пример интеграции"
ms.topic: article
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: f71da0f522c6c028981637a9797c3836063516f0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="sample-integrations"></a>Пример интеграции

## <a name="sample-integrations"></a>Пример интеграции

В разделе [приемника Кухня] [ KitchenSink] образец рабочий пример интеграции. Просто построить `KitchenSink.sln` в Visual Studio для Mac или Visual Studio и откройте `KitchenSink.workbook`.

[![Снимок экрана интеграции Кухня приемника](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

В образце приемника Кухня показаны оба вида понятия:

* Представление части демонстрируется использование `RepresentationManager` для улучшения визуализации с помощью встроенных представлений.
* `Person` Объекта и его связанные JavaScript модуля подготовки отчетов демонстрируют использование `ISerializableObject` без прохода через представление поставщика.

См. также [SkiaSharp] [ skiasharp] реальных пример интеграции, который использует существующую [представления](~/tools/workbooks/sdk/representations.md) предоставляемые Xamarin книги к просмотру ее типы.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks