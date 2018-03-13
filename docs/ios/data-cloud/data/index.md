---
title: "доступ к данным iOS"
description: "Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных SQLite «встроенные» и доступа для хранения и извлечения данных стало проще благодаря платформе Xamarin. В этом документе показано, как получить доступ к базе данных SQLite."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 6dba862055cc266b2af1eaf87418fe7ebf5830c5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="ios-data-access"></a>доступ к данным iOS

_Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных SQLite «встроенные» и доступа для хранения и извлечения данных стало проще благодаря платформе Xamarin. В этом документе показано, как получить доступ к базе данных SQLite._

Xamarin.iOS поддерживает интерфейсы API доступа к базе данных, например:

-  Платформа ADO.NET.
-  SQLite NET сторонний библиотеки.

В этом руководстве общий обзор баз данных в целом перед описанием Настройка ADO.NET и SQLite.NET для доступа к базам данных SQLite в Xamarin.iOS приложения. 

Большая часть кода в этом документе полностью между различными платформами и будет выполняться на iOS или Android без изменения. Существуют два образца приложения, рассматриваются:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) — простых операций записывает результаты в текстовый отображения элемента управления;
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) — интегрирует операций с данными в небольших рабочее приложение, которое содержит список и изменяет простая структура данных.

Оба образца решения содержат iOS и Android образцы проектов приложений.

Приложения Xamarin.Forms чтения [работа с базами данных](~/xamarin-forms/app-fundamentals/databases.md) объясняет, как работать с SQLite библиотеки PCL с помощью Xamarin.Forms.

## <a name="sections"></a>Разделы

-  [Введение](introduction.md)
-  [Конфигурация](configuration.md)
-  [Использование ORM для SQLite.NET](using-sqlite-orm.md)
-  [Использование ADO.NET](using-adonet.md)
-  [Использование данных в приложении](using-data-in-an-app.md)


## <a name="summary"></a>Сводка

В этой главе описано доступ к данным в Xamarin.iOS, с помощью SQLite как компонент database engine. Базы данных можно найти в «напрямую» с помощью синтаксиса ADO.NET, или можно включить SQLite.NET ORM и выполнения операций с данными в C#.

Мы изучили двух выборок: один содержит код доступа к данным очень простого, выходные данные для текстового поля и простое приложение, которое включает в себя создания, чтения, обновления и удаления функциональные возможности. Мы также описана работа с потоками и использовании приложения с предварительно заполненную базу данных SQLite.

Дополнительные примеры доступа к данным на разных платформах см. наш [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) практический пример.

## <a name="related-links"></a>Связанные ссылки

- [DataAccess Basic (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (пример)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [операций ввода-вывода данных рецепты](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Доступ к данным Xamarin.Forms](~/xamarin-forms/app-fundamentals/databases.md)
