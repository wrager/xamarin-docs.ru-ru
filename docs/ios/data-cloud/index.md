---
title: "Данные и облачные службы"
description: "Руководства по стабилизации и развертыванию"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 5814936289164f14a07b33c219ad1fd01b00473b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="data-and-cloud-services"></a>Данные и облачные службы


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Доступ к данным](~/ios/data-cloud/data/index.md)

Большинство приложений имеют некоторые требования, чтобы сохранить данные на устройстве локально. Если объем данных элементарно мал, это обычно требуется базы данных и уровнем данных в приложении для управления доступом к базе данных. iOS имеет ядро базы данных Sqlite «встроенные» и доступ к данным стало проще благодаря платформе Xamarin которая поставляется с поставщиком данных SQLite.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

API-Интерфейс хранилища iCloud в iOS 5 позволяет приложениям сохранять пользовательские документы и данные приложения в центральном расположении и доступа к указанным элементам из всех пользовательских устройств.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

CloudKit framework упрощает разработку приложений, iCloud доступ. Сюда входят методы для получения данных приложений и средств права, а также возможность безопасного хранения сведений о приложении. Этот комплект предоставляет пользователям слоя анонимности, разрешив доступ к приложениям с их идентификаторами iCloud без совместного использования личных сведений.