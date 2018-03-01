---
title: "Внедрение .NET в Java"
description: "Как использовать библиотеку Xamarin .NET в основе Java Android проект в машинном коде"
ms.topic: article
ms.prod: xamarin
ms.assetid: A489EEF3-1008-4257-BF63-FE21D8C23821
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 1a25f4bc39e39ce58a07ed399082bf13284c16e9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="embedding-net-in-java"></a>Внедрение .NET в Java

В некоторых случаях может потребоваться добавить в существующий проект Android native библиотека Xamarin .NET. Чтобы сделать это, можно использовать [Embeddinator 4000](https://mono.github.io/Embeddinator-4000/) , которая позволяет преобразовать библиотеку .NET в собственной библиотеки, которая может быть включена в машинном коде на языке Java Android приложение.

 
## <a name="requirements"></a>Требования

Чтобы использовать Embeddinator-4000 Java на Android, необходимо следующее:

-   **Android Studio** &ndash; [Android Studio 3.x](https://developer.android.com/studio/preview/index.html) должен быть установлен.

-   **Xamarin.Android** &ndash; [Xamarin.Android 7.4.99](https://jenkins.mono-project.com/view/Xamarin.Android/job/xamarin-android/lastSuccessfulBuild/Azure/) должен быть установлен.

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
