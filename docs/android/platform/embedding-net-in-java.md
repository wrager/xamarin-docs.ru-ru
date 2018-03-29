---
title: Внедрение .NET в Java
description: Как использовать библиотеку Xamarin .NET в основе Java Android проект в машинном коде
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/28/2018
ms.openlocfilehash: f0da12d739c6003257d3acf9ccefdec7e36f5349
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2018
---
# <a name="embedding-net-in-java"></a>Внедрение .NET в Java

В некоторых случаях может потребоваться добавить в существующий проект Android native библиотека Xamarin .NET. Чтобы сделать это, можно использовать [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) , которая позволяет преобразовать библиотеку .NET в собственной библиотеки, которая может быть включена в машинном коде на языке Java Android приложение.

 
## <a name="requirements"></a>Требования

Чтобы использовать Embeddinator-4000 Java на Android, необходимо следующее:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) должен быть установлен.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) должен быть установлен.

-   **Java Developer Kit** &ndash; [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) должен быть установлен.

-   **Моно** &ndash; [5.0 моно](http://www.mono-project.com/download/) должен быть установлен.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio можно использовать для редактирования и компиляции кода C#.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Visual Studio для Mac можно использовать для редактирования и компиляции кода C#.

-----

 
## <a name="using-the-embeddinator-4000"></a>С помощью Embeddinator-4000

Для использования библиотеки .NET в собственный проект Android, выполните следующие действия:

1.  Создание проекта библиотеки Android в C#.

2.  Установите Embeddinator-4000 через NuGet.

3.  Запустите Embeddinator на библиотеку Android сборки.

4.  Используйте созданный файл AAR в проекте Java в Android Studio.

Эти шаги описаны подробно в [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/getting-started-java-android.html) документации.
