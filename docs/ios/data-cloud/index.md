---
title: Данные и облачных служб в приложениях Xamarin.iOS
description: Документ содержит ссылки на руководства, описывающие, как работать с локальными данными, iCloud и CloudKit в приложения Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: a733c1af34b577786a7e18eeafa13da4327dddc6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784264"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Данные и облачных служб в приложениях Xamarin.iOS

##  <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных Sqlite «встроенные» и доступ к данным стало проще благодаря платформе Xamarin которая поставляется с поставщиком данных SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

API-Интерфейс хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложения в центральном расположении и доступа к указанным элементам из всех пользовательских устройств.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit framework упрощает разработку приложений, iCloud доступ. Сюда входят методы для получения данных приложений и средств права, а также возможность безопасного хранения сведений о приложении. Этот комплект предоставляет пользователям слоя анонимности, разрешив доступ к приложениям с их идентификаторами iCloud без совместного использования личных сведений.