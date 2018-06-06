---
title: Установка .NET внедрения
description: В этом документе описывается установка внедрения .NET. Обсуждается запуска средств вручную, как создавать привязки автоматически, как использовать пользовательские целевые объекты MSBuild и необходимые действия после построения.
ms.prod: xamarin
ms.assetid: 47106AF3-AC6E-4A0E-B30B-9F73C116DDB3
author: chamons
ms.author: chhamo
ms.date: 4/18/2018
ms.openlocfilehash: 057a1f3f662b2dbe2f8aee277505e1d6e8798084
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793799"
---
# <a name="installing-net-embedding"></a>Установка .NET внедрения

## <a name="installing-net-embedding-from-nuget"></a>Установка .NET внедрение из NuGet

Выберите **Добавить > Добавление пакетов NuGet...**  и установить **Embeddinator 4000** из диспетчера пакетов NuGet:

![Диспетчер пакетов NuGet](images/visualstudionuget.png)

Будет выполнена установка **Embeddinator 4000.exe** и **objcgen** в **пакетов/Embeddinator-4000/tools** каталога.

Как правило последний выпуск Embeddinator-4000 должен быть выбран для загрузки. Поддержка Objective-C требует 0,4 или более поздней версии.

## <a name="running-manually"></a>Запуск вручную

Теперь, когда установлены NuGet средств можно запустить вручную.

- Откройте командную строку (Windows) или терминалов (macOS)
- Перейдите в каталог в корневом каталоге решения
- Инструментарий устанавливается в:
    - **. / пакетов, Embeddinator-4000. [Версия] / Сервис/objcgen** (Objective-C)
    - **. / пакетов, Embeddinator-4000. [VERSION]/tools/Embeddinator-4000.exe** (Java/C) 
- На macOS **objcgen** можно запустить напрямую. 
- В Windows **Embeddinator 4000.exe** можно запустить напрямую.
- На macOS **Embeddinator 4000.exe** необходимо запускать с **моно**: 
    - `mono ./packages/Embeddinator-4000.[VERSION]/tools/Embeddinator-4000.exe`

Каждый вызов команды потребуется ряд параметров, перечисленных в документации по конкретной платформы.

## <a name="automatic-binding-generation"></a>Автоматическая привязка поколения

Существует два подхода для автоматического запуска .NET внедрение часть процесса построения.

- Пользовательские целевые объекты MSBuild
- Действия после построения

Хотя в этом документе описываются оба, используя пользовательский целевой объект MSBuild является предпочтительным. Шаги построения POST не будет обязательно будет запущен при построении из командной строки.

### <a name="custom-msbuild-targets"></a>Пользовательские целевые объекты MSBuild

Чтобы настроить построение с помощью целевых объектов MSbuild, сначала создайте **Embeddinator 4000.targets** файла рядом с вашей csproj примерно следующего содержания:

```xml
<Project DefaultTargets="Build" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <Target Name="RunEmbeddinator" AfterTargets="AfterBuild" Inputs="$(OutputPath)/$(AssemblyName).dll" Outputs="$(IntermediateOutputPath)/Embeddinator/$(AssemblyName).framework/$(AssemblyName)">
        <Exec Command="" />
    </Target>
</Project>
```

Здесь текст команды должны присваиваться один из вызовов внедрения .NET, перечисленные в документации по конкретной платформы.

Для использования этой цели:

- Закройте проект в Visual Studio или Visual Studio 2017 г. для Mac
- Откройте csproj библиотеки в текстовом редакторе
- Добавьте следующую строку внизу выше последней `</Project>` строки:

```xml
 <Import Project="Embeddinator-4000.targets" />
```

- Повторно откройте проект

### <a name="post-build-steps"></a>Действия после построения

Шаги, чтобы добавить действие после сборки для запуска .NET внедрение зависят от интегрированной среды разработки.

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

В Visual Studio для Mac перейдите к **параметры проекта > сборки > команд Custom** и добавьте **после построения** шаг.

Настройка команды, перечисленные в документации по конкретной платформы.

> [!NOTE]
> Убедитесь, что для использования номера версии, установленные из NuGet

Если вы собираетесь выполнять текущей работы над проектом C#, можно также добавить пользовательскую команду для очистки **вывода** каталог до выполнения внедрения .NET:

```shell
rm -Rf '${SolutionDir}/output/'
```

![Действие настраиваемого построения](images/visualstudiocustombuild.png)

> [!NOTE]
> Созданную привязку будут помещены в папку, указанную в `--outdir` или `--out` параметра. Имя созданного привязки может меняться, как некоторые платформы имеют ограничения на имена пакетов.

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Фактически мы определим то же самое, но будут немного другими меню в Visual Studio 2017 г. Команды оболочки также могут немного отличаться.

Последовательно выберите пункты **параметры проекта > события построения** и введите команду, перечислены в документации по конкретной платформы в **Командная строка события после построения** поле. Пример:

![Встраивание в Windows .NET](images/visualstudiowindows.png)
