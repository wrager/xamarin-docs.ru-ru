---
title: "Android.Support.v7.AppCompat - ресурс не найден, соответствующий заданным именем: attr «android: actionModeShareDrawable»"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: f37d8439d5987a2f577aee741a2b2374990dae91
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - ресурс не найден, соответствующий заданным именем: attr «android: actionModeShareDrawable»

1. Убедитесь, что загрузить последние дополнения, а также Android 5.0 SDK через диспетчер Android SDK (API 21).

2. Убедитесь, что при компиляции приложения с compileSdkVersion значение 21. При необходимости можно задать targetSdkVersion также 21.

3. Если требуется предыдущей версии, такие как API 19, загрузите соответствующую версию найти на странице Nuget:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Примечание*: Если вручную установить через консоль диспетчера пакетов, убедитесь, что также установить ту же версию Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Ссылка на переполнение стека: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>См. также

- [Какие пакеты Android SDK следует установить?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

