---
title: Общий доступ к коду
description: Основные понятия приложения
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 4cca202627d1b901e00532c92598ffddd48b4853
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="sharing-code"></a>Общий доступ к коду

В этом разделе содержится руководство для некоторых наиболее распространенных задач действия или концепции, разработчики должны иметь в виду при разработке приложений для мобильных устройств.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Общие сведения о совместное использование кода](code-sharing.md)

Дополнительные сведения о различных кода, параметры, доступные для проектов Xamarin, включая переносимой библиотеки классов (PCLs), общих проектов и стандартные библиотеки .NET для управления доступом.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Переносимые библиотеки классов](~/cross-platform/app-fundamentals/pcl.md)

Проекты переносимой библиотеки классов позволяют создавать и распространять сборки, содержащие общий код на нескольких платформах. Для создания переносимой библиотеки классов (или «PCL») сначала выбрать какие платформы целевого, а затем написать код для подмножество .NET Framework, которая доступна в профиле, определенные для этих платформ. В этом документе описывается создание и использование PCLs с помощью Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Общие проекты](~/cross-platform/app-fundamentals/shared-projects.md)

Общие проекты позволяют писать общий код, который ссылается на несколько проектов различных приложений. Код компилируется как часть каждого ссылающийся проект и могут включать директивы компилятора для внедрения функциональных возможностей платформой базы общего кода. Здесь рассматривается работа общие проекты и как создать и использовать их с проектами Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

Стандартная .NET — это новая возможность для совместное использование кода несколькими платформами. Он работает сходным образом в переносимой библиотеки классов; Код построена на основе конкретной версии (в настоящее время 1.0 до версии 1.6) и может быть использовано в других проектах, которые поддерживают этот уровень или более поздней версии. Проекты .NET standard, поддерживаются в Xamarin Studio 6.2, Visual Studio для Windows и Visual Studio для Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Проекты NuGet: Многоплатформенного библиотек для совместного использования кода](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Пакеты NuGet можно создавать автоматически из проектов PCL или .NET standard. и общие проекты, которые могут быть упакованы в пакеты NuGet «заманить и подменить», с помощью отдельный тип проекта NuGet. В этом разделе описывается создание пакетов NuGet для каждого сценария совместного использования кода.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Создание вручную пакетов NuGet для Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Советы по созданию пакеты NuGet, которые работают с платформе Xamarin.
