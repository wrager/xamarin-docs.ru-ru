---
title: Создание новой библиотеки многоплатформенного для NuGet
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 85403ee2ab8b5c1433e0e070b3af22fa42cd5efa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-new-multiplatform-library-for-nuget"></a>Создание новой библиотеки многоплатформенного для NuGet

Создание проекта библиотеки многоплатформенных, использующего PCL или .NET Standard означает, что полученный NuGet можно добавить к любому проекту .NET, которая поддерживает профиль, включая проекты ASP.NET и настольных приложений, с помощью WinForms, WPF или UWP.

Библиотека может содержать только код, поддерживаемый выбранного профиля PCL или .NET Standard, а также другие NuGets, добавляются.
Это подходит для бизнес-логики и алгоритмы, которые могут быть выражены полностью в библиотеке базовых классов .NET.

Одна сборка создается и встроены в пакет NuGet.

Если в дальнейшем функциональность платформой [можно добавлять проекты под конкретные платформы](#add-platforms).

## <a name="steps-to-create-a-multiplatform-library-nuget"></a>Инструкции по созданию NuGet многоплатформенного библиотеки

1. Выберите **файл > новое решение** (или щелкните правой кнопкой мыши существующее решение и выберите **Добавить > Новый проект**).

2. Выберите **многоплатформенных библиотеки** из **многоплатформенных > Библиотека** раздела:

  [![](single-codebase-images/mulitplatform-library-sml.png "Настройка параметров библиотеки несколькими платформами для единой базой кода")](single-codebase-images/mulitplatform-library.png#lightbox)

3. Введите **имя** и **описание**и выберите **один для всех платформ**:

  [![](single-codebase-images/single-configure-sml.png "Настройка параметров библиотеки несколькими платформами для единой базой кода")](single-codebase-images/single-configure.png#lightbox)

4. Завершите работу мастера. Единая библиотека проект создается в решении.

5. Щелкните правой кнопкой мыши новый проект библиотеки, а затем выберите **параметры**. **Сборки > Общие** позволяет **требуемой версии .NET Framework** задаваемый — выберите профиль .NET Переносимых или версии .NET Standard:

  [![](single-codebase-images/single-choose-type-sml.png "Выберите для типа библиотеки PCL или .NET Standard")](single-codebase-images/single-choose-type.png#lightbox)

6. Кроме того, в **параметры проекта** откройте **пакет NuGet > метаданных** статьи и введите [необходимые метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) (а также любые необязательные метаданные):

  [![](single-codebase-images/single-metadata-sml.png "Введите необходимые метаданные")](single-codebase-images/single-metadata.png#lightbox)

7. Правой кнопкой мыши проект библиотеки и выберите **создать пакет NuGet** (или построение или развертывание решения) и **.nupkg** будут сохранены в файл пакета NuGet **/bin/** папка (отладки или выпуска, в зависимости от конфигурации):

  ![](single-codebase-images/create-nuget-package.png "Файл пакета NuGet сохраняются в папке bin отладки или выпуска, в зависимости от конфигурации")


## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлов, поэтому можно проверить внутренняя структура созданного пакета.

На этом снимке экрана показано содержимое на основе PCL NuGet — включается только в одну сборку PCL:

![](single-codebase-images/nuget-output.png "Файлы, содержащиеся в пакет NuGet.")

<a name="add-platforms" />

## <a name="adding-platform-specific-code"></a>Добавление кода для конкретной платформы

PCL-проектов и проектов на основе .NET Standard не могут содержать ссылки на конкретные платформы (например, iOS или Android функциональные возможности).

Если существующий проект PCL или .NET стандартного проекта необходимо расширить для включения кода под конкретную платформу, это можно сделать, щелкнув правой кнопкой мыши проект и выбрав **Добавить > добавить реализацию платформы...** :

[![](single-codebase-images/add-later-sml.png "Добавление платформы реализация меню")](single-codebase-images/add-later.png#lightbox)

В решение можно добавить один или несколько проектов платформы и при необходимости существующей библиотеки PCL или .NET Standard можно преобразовать в общий проект:

[![](single-codebase-images/add-later-platforms-sml.png "Добавьте параметры платформы, например iOS, Android и общий проект")](single-codebase-images/add-later-platforms-sml.png#lightbox)

После преобразования в общий проект, посетите **параметры проекта > пакета NuGet > ссылочные сборки**
[раздел](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md) и убедитесь, что все обязательные профили выбраны (, чтобы NuGet по-прежнему совместимы с проектами, который ранее использовался в).


## <a name="related-links"></a>Связанные ссылки

- [Руководство по метаданным](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
