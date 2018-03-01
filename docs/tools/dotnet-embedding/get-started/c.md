---
title: "Начало работы с C#"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ea8335348416dc00074d83e09b74521da7abcb66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-c"></a>Начало работы с C#


## <a name="requirements"></a>Требования

Для использования с C внедрения .NET необходимо иметь промежуточных компьютера Mac и Windows:

* macOS 10.12 (Сьерра) или более поздней версии
* Xcode 8.3.2 или более поздней версии

* Windows 7, 8, 10 или более поздней версии
* Visual Studio 2013 или более поздней версии

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Установка

Следующий шаг — Чтобы загрузить и установить средства внедрения .NET на компьютере.

По-прежнему доступны не двоичные сборок для генераторов C и Java, но ожидается в ближайшее время.

В качестве альтернативы можно построить его в нашем репозитории git в разделе [дополнение](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) документа инструкции.


## <a name="generation"></a>Создание

Чтобы создать код на языке C, вызовите средство внедрения .NET, передача правой флаги для языка C:

### <a name="windows"></a>Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Убедитесь, что для вызова из командной оболочки Visual Studio определенной версии Visual Studio вы будете для различных версий.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Выходные файлы

Если все пойдет хорошо, появится следующий результат:

```csharp
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

Поскольку `--compile` флаг был передан средству, внедрение .NET следует также скомпилированных выходные файлы в общей библиотеки, расположенные рядом с созданные файлы `libmanaged.dylib` файл на macOS, и `managed.dll` в Windows.

Для использования общей библиотеки, можно включить `managed.h` файл заголовка C, который предоставляет C объявления, соответствующих соответствующее управляемая библиотека API-интерфейсов и выполните компоновку с упомянутых ранее скомпилированные общей библиотеки.

## <a name="further-reading"></a>Дополнительные сведения

* [Ограничения Embeddinator](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
