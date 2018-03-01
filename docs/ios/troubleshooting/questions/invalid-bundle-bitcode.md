---
title: "Ошибка при отправке в магазин приложений: «Недопустимый пакет - параметры, не может быть внедрен в bitcode обнаруживаются отправки»"
ms.topic: article
ms.prod: xamarin
ms.assetid: 137313FB-3D29-428B-93C1-5A05DC8F7C03
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b8ce643d392945e7e746c28b13063a2b0b7ebb48
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="error-when-submitting-to-app-store-invalid-bundle---options-not-allowed-to-be-embedded-in-bitcode-are-detected-in-the-submission"></a>Ошибка при отправке в магазин приложений: «Недопустимый пакет - параметры, не может быть внедрен в bitcode обнаруживаются отправки»

watchOS приложений и приложений, tvOS _требуют_ bitcode при подаче App Store. При создании и отправке watchOS и tvOS приложения с помощью Xcode 8.3 или более ранней версии, (с помощью уведомлений по электронной почте) может возникнуть следующая ошибка при попытке отправить в магазин приложений:

>Недопустимый пакет - не удается обработать приложение, поскольку параметры не может быть внедрен в bitcode обнаруживаются отправки. Вполне вероятно, что приложение не строится с помощью инструментов, предоставляемых в Xcode.

Решением этой проблемы является построение приложений с помощью Xcode 9 и последнюю версию Xamarin.iOS.
