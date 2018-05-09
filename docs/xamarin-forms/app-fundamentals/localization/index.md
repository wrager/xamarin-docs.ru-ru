---
title: Локализация Xamarin.Forms
description: Встроенные локализации .NET framework можно использовать для создания разных платформах многоязыковых приложений с помощью Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/13/2018
ms.openlocfilehash: 29624eeccc8542b3296774f6b77480b664bee478
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="localization"></a>Локализация

_Встроенные локализации .NET framework можно использовать для создания разных платформах многоязыковых приложений с помощью Xamarin.Forms._

## <a name="string-and-image-localizationtextmd"></a>[Строки и локализации изображения](text.md)

Встроенный механизм для локализации приложения использует .NET [RESX-файлы](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) и классы в `System.Resources` и `System.Globalization` пространства имен. RESX-файлы, содержащие переведенных строк внедряются в сборку Xamarin.Forms, а также класс, созданный компилятором, который предоставляет строго типизированный доступ к переводы. Переведенный текст может быть извлечен в коде.

## <a name="right-to-left-localizationright-to-leftmd"></a>[Локализация справа налево](right-to-left.md)

Направление потока имеет направление, в котором проверяются элементы пользовательского интерфейса на странице глаза. Локализация справа налево добавляет поддержку направлением потока справа налево Xamarin.Forms приложениям.
