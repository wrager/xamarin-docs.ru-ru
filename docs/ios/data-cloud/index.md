---
title: Данные и облачные службы
description: Руководства по стабилизации и развертыванию
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 81aebe5fd7431e578b75c5b61e1d2c92ce546909
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="data-and-cloud-services"></a>Данные и облачные службы


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных Sqlite «встроенные» и доступ к данным стало проще благодаря платформе Xamarin которая поставляется с поставщиком данных SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

API-Интерфейс хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложения в центральном расположении и доступа к указанным элементам из всех пользовательских устройств.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit framework упрощает разработку приложений, iCloud доступ. Сюда входят методы для получения данных приложений и средств права, а также возможность безопасного хранения сведений о приложении. Этот комплект предоставляет пользователям слоя анонимности, разрешив доступ к приложениям с их идентификаторами iCloud без совместного использования личных сведений.