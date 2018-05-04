---
title: Начало работы с C#
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: 1313d7156a1fd75fd40e2aff65404aef5ab023bb
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-c"></a>Начало работы с C#

## <a name="requirements"></a>Требования

Чтобы использовать внедрения .NET с C, необходимо иметь промежуточных компьютера Mac и Windows:

### <a name="macos"></a>macOS

* macOS 10.12 (Сьерра) или более поздней версии
* Xcode 8.3.2 или более поздней версии
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 или более поздней версии
* Visual Studio 2015 или более поздней версии

## <a name="installing-net-embedding-from-nuget"></a>Установка .NET внедрение из NuGet

Выполните следующие [инструкции](~/tools/dotnet-embedding/get-started/install/install.md) для установки и настройки внедрения .NET для проекта.

Вызова команды, которые нужно настроить будет выглядеть (с разными номерами версий и пути):

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Создание

### <a name="output-files"></a>Выходные файлы

Если все пойдет хорошо, появится следующий результат:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Поскольку `--compile` флаг был передан средству, внедрение .NET следует также скомпилированных выходные файлы в общей библиотеки, расположенные рядом с созданные файлы **libmanaged.dylib** файла macOS и **managed.dll** в Windows.

Для использования общей библиотеки, можно включить **managed.h** файл заголовка C, который предоставляет C объявления, соответствующих соответствующее управляемая библиотека API-интерфейсов и выполните компоновку с упомянутых ранее скомпилированные общей библиотеки.

## <a name="further-reading"></a>Дополнительные сведения

* [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
