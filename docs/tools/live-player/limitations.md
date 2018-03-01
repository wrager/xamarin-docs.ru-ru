---
title: "Ограничения"
description: "Некоторые ограничения Xamarin Live Player"
ms.topic: article
ms.prod: xamarin
ms.assetid: 36A1531E-630A-4B7C-A333-4E67E5DC023C
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 08/04/2017
ms.openlocfilehash: d551c0a82fb8f970ce5a00ec9e64ac7f49c81a44
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Ограничения

![Функция предварительного просмотра](~/media/shared/preview.png)

## <a name="device-requirements"></a>Требования к устройству
Приложение Xamarin Live Player поддерживает следующие устройства:

### <a name="ios"></a>iOS
- iOS 9.0 или более поздней версии.
- Процессор ARM64.
- Проверьте [App Store](https://itunes.apple.com/us/app/xamarin-live-player/id1228841832?mt=8) список поддерживаемых устройств.

### <a name="android"></a>Android
- Android 4.2 или более поздней версии.
- ARM-v7a, ARM v8a, ARM64-v8a x 86 или x86_64 процессора.


## <a name="limitations"></a>Ограничения

Существуют некоторые ограничения на задачи, которые могут запускаться Xamarin Live Player, включая следующие пункты:

### <a name="android"></a>Android
- Android пользовательские интерфейсы, разработанные с помощью AXML файлы в настоящее время не поддерживаются.

### <a name="ios"></a>iOS
- Не поддерживаются некоторые возможности раскадровки iOS.
- файлы XIB операций ввода-вывода не поддерживаются.

### <a name="xamarinforms"></a>Xamarin.Forms
- Пользовательские модули подготовки отчетов не поддерживаются.
- Эффекты не поддерживаются.
- Пользовательские элементы управления с привязываемыми свойствами пользовательских не поддерживаются.
- Внедренные ресурсы не поддерживаются (т. е. внедрение изображений или других ресурсов в PCL).

### <a name="misc"></a>Прочее
- Ограниченная поддержка для отражения (в настоящее время влияет на некоторые популярные NuGets, такие как SQLite и Json.NET). Другие NuGets по-прежнему поддерживаются.
- Некоторые системные классы не могут быть переопределены (например, нельзя реализовать подкласс).
- Некоторые компоненты платформы, которые требуют подготовки не может работать в приложении Xamarin Live Player (тем не менее он настроен для выполнения распространенных операций, например доступа к коллекции фотографий).
- Пользовательских целей и шагов построения игнорируются. Например средства, такие как Fody не может быть включено.

> [!WARNING]
> **Примечание:** Xamarin Live Player не работает с Xamarin Studio

Сообщите ли дополнительные проблемы на [bugzilla](https://aka.ms/live-player-report-issue).


## <a name="related-links"></a>Связанные ссылки

- [Устранение неполадок](~/tools/live-player/troubleshooting.md)
- [Установка](~/tools/live-player/install.md)
