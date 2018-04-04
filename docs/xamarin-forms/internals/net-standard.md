---
title: Поддержка 2.0 standard .NET в Xamarin.Forms
description: В этой статье описывается порядок преобразования Xamarin.Forms приложению использовать стандартные .NET версии 2.0.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 8685f1e10b5094e6f58e8efea51e6dd216dfa000
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Поддержка 2.0 standard .NET в Xamarin.Forms

_В этой статье описывается порядок преобразования Xamarin.Forms приложению использовать стандартные .NET версии 2.0._

Стандартная .NET представляет собой спецификацию API .NET, которые должны быть доступны во всех реализациях .NET. Он упрощает совместное использование кода для приложений для настольных компьютеров, мобильных приложений и игр и облачным службам, путем приведения идентичные интерфейсы API для различных платформ. Сведения о платформах, поддерживаемых в .NET Standard см. в разделе [реализации поддержки .NET](/dotnet/standard/net-standard#net-implementation-support/).

.NET стандартные библиотеки являются заменой для переносимой библиотеки классов (PCL). Тем не менее библиотеки, ориентированном на .NET Standard PCL по-прежнему и называется переносимую библиотеку Классов на основе .NET Standard. Некоторые профили PCL сопоставляются с версии платформы .NET Standard и профилей, которые имеют сопоставления, типы двух библиотек смогут ссылаются друг на друга. Дополнительные сведения см. в разделе [PCL совместимости](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4 позволяет приложениям Xamarin.Forms целевой .NET Standard 2.0, заменив PCL библиотека .NET Standard 2.0. Это можно сделать следующим образом:

- Убедитесь, [.NET Core 2.0](https://www.microsoft.com/net/download/core) установлен.
- Обновление решения Xamarin.Forms, чтобы использовать Xamarin.Forms 2.4 или более поздней версии.
- Добавьте в решение, ориентированном на .NET 2.0 стандартной библиотеке .NET Standard.
- Удаление класса, который добавляется в библиотеку .NET Standard.
- Добавьте пакет NuGet Xamarin.Forms 2.4 (или более поздней) в библиотеку .NET Standard.
- В проектах платформы добавьте ссылку на библиотеку .NET Standard и удалите ссылку на проект переносимой библиотеки Классов, содержащий логику пользовательского интерфейса Xamarin.Forms.
- Скопируйте файлы в проект переносимой библиотеки Классов .NET стандартной библиотеки.
- Удалите проект PCL, который содержит логику пользовательского интерфейса Xamarin.Forms.


## <a name="related-links"></a>Связанные ссылки

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Варианты общего доступа к коду](~/cross-platform/app-fundamentals/code-sharing.md)
