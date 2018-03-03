---
title: "Проекты NuGet (Nugetizer 3000)"
description: "Автоматически создайте пакеты NuGet, чтобы совместное использование кода между платформами, с помощью «3000 Nugetizer\"!"
ms.topic: article
ms.prod: xamarin
ms.assetid: F0A5A9BB-86CD-44C9-8EE8-74D1E5E74A30
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 11/22/2017
ms.openlocfilehash: 66bf9c215e3d30687fa8037220b8b35409ca285d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="nuget-projects-nugetizer-3000"></a>Проекты NuGet (Nugetizer 3000)

_Автоматически создайте пакеты NuGet, чтобы совместное использование кода между платформами, с помощью «3000 Nugetizer"!_

Можно автоматически создавать пакеты NuGet для совместного использования кода на платформах с помощью _Nugetizer 3000_. Это делает имеется возможность создания пакетов NuGet с существующие проекты библиотеки или путем создания нового **проекта библиотеки многоплатформенных**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
Nugetizer 3000 входит в состав Visual Studio для Mac 6.2.
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
<a name="to-use-the-nugetizer-3000-in-visual-studio-please-download-and-run-the-vsix-installerhttpbitlynugetizer-2017"></a>Для использования Nugetizer 3000 в Visual Studio, см. [Загрузите и запустите установщик VSIX](http://bit.ly/nugetizer-2017).
-----



[ ![](images/mulitplatform-library-sml.png "Создать новое окно многоплатформенных библиотеки")](images/mulitplatform-library.png)

После настройки каждой сборки проекта выводит полный пакет NuGet, который используется для внутренних целей совместного использования кода с другими приложениями или отправлены [NuGet.org](https://www.nuget.org).

Существует три варианта использования этой функции:

- [Имеющиеся проекты библиотеки](existing-library.md)

  Создайте пакет NuGet из существующих проектов PCL (или .NET Standard).

- [Создание нового проекта многоплатформенного библиотеки](single-codebase.md)

  Создайте новую библиотеку для совместного использования кода через NuGet, с помощью PCL или .NET Standard.

- [Создание новых проектов библиотекой для определенной платформы](platform-specific.md)

  Создайте новую библиотеку и NuGet, включая специфический для платформы код для iOS и Android и использует общий проект для хранения общего кода и проекты под конкретные платформы для поддержки функциональных возможностей конкретного iOS или Android.

Ссылаться на [руководство метаданных](metadata.md) Подробнее об обязательных и необязательных метаданных, которые необходимо добавить в любой пакет NuGet.


## <a name="further-nuget-information"></a>Дополнительные сведения о NuGet

Дополнительные сведения о [вручную создание NuGets для Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md) и [включить пакет NuGet в приложении](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough).

Корпорации Майкрософт [документации по NuGet](https://docs.microsoft.com/nuget/) содержит более подробные сведения о **.nupkg** формат и использования пакетов NuGet в Visual Studio.

Обсуждение разработки для проектов пакета NuGet (так) NuGetizer 3000) можно найти в [репозитории NuGet GitHub](https://github.com/NuGet/Home/wiki/NuGetizer-3000).


## <a name="related-links"></a>Связанные ссылки

- [Варианты использования NuGetizer 3000](https://github.com/NuGet/Home/wiki/NuGetizer-Core-Scenarios)
- [Вручную создайте пакеты NuGet для Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)
- [Документации по NuGet](https://docs.microsoft.com/nuget/)
- [Заметки о выпуске](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#NuGetizer_3000)
