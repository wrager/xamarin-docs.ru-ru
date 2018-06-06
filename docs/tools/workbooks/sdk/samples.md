---
title: Пример интеграции
description: Этот документ представлены ссылки на образцы, демонстрирующие интеграций Xamarin книги. Связанные примеры работы с представление подготовки к просмотру и SkiaSharp.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 75d88af4c294a35d45f05724eb96ce822cddf126
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793977"
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
