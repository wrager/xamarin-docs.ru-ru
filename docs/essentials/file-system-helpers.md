---
title: Вспомогательные методы системы Xamarin.Essentials файла
description: Класс FileSystem содержит ряд вспомогательных методов для поиска кэша приложения и каталоги данных и открывать файлы в пакете приложения.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 871d164df982d1d170e8ba5bffd3bd6600a4cdda
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-file-system-helpers"></a>Вспомогательные методы системы Xamarin.Essentials файла

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**FileSystem** класс содержит ряд вспомогательных методов для поиска приложения кэша и каталоги данных и открывать файлы в пакете приложения.

## <a name="using-file-system-helpers"></a>Использование вспомогательных методов системных файлов

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Для получения каталога приложения для хранения **данные кэша**. Кэш данных может использоваться для любых данных, необходимо сохранять больше, чем временные данные, но не данные, необходимые для правильной работы.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Для получения diredctory верхнего уровня приложения для всех файлов, которые не являются файлами данных пользователя. Эти файлы архивируются синхронизации framework операционную систему. См. ниже особенностей реализации платформы.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Чтобы открыть файл, который объединяется в пакет приложения:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** — возвращает [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) текущего контекста.
- **AppDataDirectory** — возвращает [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) текущий контекст и являются резервное копирование с использованием [Autu резервного копирования](https://developer.android.com/guide/topics/data/autobackup.html) запуск на API 23 и более поздних версий.

Добавьте все файлы в **активы** папку в папке Android проекта и пометить как действие при построении **AndroidAsset** на работу с `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** — возвращает [библиотеки и кэши](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) каталога.
- **AppDataDirectory** — возвращает [библиотеки](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) каталог, в котором резервной копии, созданной iTunes и iCloud.

Добавьте все файлы в **ресурсов** папки в iOS проекта и пометить как действие при построении **BundledResource** на работу с `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** — возвращает [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) каталога...
- **AppDataDirectory** — возвращает [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) каталог резервного копирования в облако.

Добавьте любой файл в корневой папке в проекте UWP и пометьте как действие при построении **содержимого** на работу с `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Исходный код файла вспомогательные методы системы](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Документация по API системных файлов](xref:Xamarin.Essentials.FileSystem)
