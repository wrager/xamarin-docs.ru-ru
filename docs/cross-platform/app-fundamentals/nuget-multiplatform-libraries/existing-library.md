---
title: "Создание NuGet из существующих проектов библиотеки"
ms.topic: article
ms.prod: xamarin
ms.assetid: EDAC3E5E-DB7D-40A9-AE28-45C52ADA854E
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/20/2017
ms.openlocfilehash: 44a97114e7d325a1fa196d2c9828855ad1a30c94
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="creating-a-nuget-from-existing-library-projects"></a>Создание NuGet из существующих проектов библиотеки

Библиотеки Классов, в существующий или .NET Standard, можно превратить NuGets через **параметры проекта** окна:

1. Щелкните правой кнопкой мыши на проект библиотеки в **Pad решения** и выберите **параметры**.

2. Последовательно выберите пункты **пакет NuGet > метаданных** статьи и ввести все [необходимую информацию](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) в **Общие** вкладки:

  [ ![](existing-library-images/existing-metadata-sml.png "Введите необходимые метаданные")](existing-library-images/existing-metadata.png)

3. При необходимости [добавить дополнительные метаданные](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md) в **сведения** вкладки.

4. После настройки метаданных можно правой кнопкой мыши проект и выберите **создать пакет NuGet** и **.nupkg** будут сохранены в файл пакета NuGet **/bin/** папка (отладки или выпуска, в зависимости от конфигурации).

  ![](existing-library-images/create-nuget-package.png "Выберите в контекстном меню создания пакетов NuGet")

5. Для создания пакета NuGet на _каждого_ построение или развертывание, перейдите к **пакет NuGet > построения** раздел и делений **создать пакет NuGet при сборке проекта**:

    [ ![](existing-library-images/existing-tickbox-sml.png "Чтобы создать пакет NuGet деления")](existing-library-images/existing-tickbox.png)

> [!NOTE]
> Построение NuGet пакета может замедлить процесс построения. Если этот флажок не установлен, вы можете по-прежнему создать пакет NuGet вручную в любое время, в контекстном меню проекта (показано в шаге 4).

## <a name="verifying-the-output"></a>Проверка выходных данных

Пакеты NuGet также являются ZIP-файлов, поэтому можно проверить внутренняя структура созданного пакета.

На этом снимке экрана показано содержимое на основе PCL NuGet — включается только в одну сборку PCL:

![](existing-library-images/nuget-output.png "Файлы, содержащиеся в пакет NuGet.")


## <a name="related-links"></a>Связанные ссылки

- [Структуры метаданных](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
