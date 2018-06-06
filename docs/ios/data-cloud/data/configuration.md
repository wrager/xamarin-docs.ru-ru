---
title: Настройка SQLite в Xamarin.iOS
description: В этом документе описывается определить расположение для файла базы данных в приложении Xamarin.iOS SQLite. Эти концепции относятся независимо от того, механизм доступа выбранных данных.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 57645c0bb947a60a3d74436b752210d1bac18124
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784385"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Настройка SQLite в Xamarin.iOS

Для использования в приложении Xamarin.iOS SQLite необходимо определить правильное расположение файла для файла базы данных.

## <a name="database-file-path"></a>Путь к файлу базы данных

Независимо от того, какой используется методом доступа к данным необходимо создать файл базы данных, прежде чем данные могут храниться с SQLite. В зависимости от того, для какой платформы он предназначен расположение файла будут отличаться. Для iOS среды класс можно использовать для создания допустимый путь, как показано в следующем фрагменте кода:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Существуют другие вещи, чтобы принять во внимание при выборе места для хранения файла базы данных. В iOS можно привести базу данных для резервной копии автоматически (или нет).

Если вы хотите использовать в другое место на каждой платформе межплатформенные приложения можно использовать директиву компилятора как показано для создания другой путь для каждой платформы:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Ссылаться на [работа с файловой системой](~/ios/app-fundamentals/file-system.md) для получения дополнительных сведений, на какие расположения файлов для использования в iOS. В разделе [построение межплатформенных приложений платформы](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Дополнительные сведения об использовании директивы компилятора написать код, предназначенный для каждой платформы.

## <a name="threading"></a>Потоки

Подключение к одной базе данных SQLite не следует использовать в нескольких потоках. Следите за тем открыть, использовать и затем закройте все подключения, создаваемые вами в том же потоке.

Чтобы убедиться, что код не пытается получить доступ к базе данных SQLite из нескольких потоков одновременно, вручную установить блокировку всякий раз, когда необходимо получить доступ к базе данных следующим образом:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Все базы данных access (операций чтения, записи, обновления, и т. д), должны быть помещены с ту же блокировку. Необходимо соблюдать осторожность, чтобы избежать взаимоблокировки, убедившись, что работа в предложении блокировки хранится простой и не выполнить вызов других методах, которые также может потребоваться блокировка!


## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [операций ввода-вывода данных рецепты](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
