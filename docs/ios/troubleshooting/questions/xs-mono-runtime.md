---
title: "Как задать переменные среды выполнения моно проектов iOS в Xamarin Studio?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1176CEA9-C7F1-411B-8F1A-99374E8AFF33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/31/2017
ms.openlocfilehash: 6032ea89aa54719cc4b0fdde67e67f1ec8fb183b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-set-mono-runtime-environment-variables-for-ios-projects-in-xamarin-studio"></a>Как задать переменные среды выполнения моно проектов iOS в Xamarin Studio?

Если необходимо задать переменные среды любой среде выполнения для Mono, они могут задаваться в **параметры проекта > запустить > Общие** страницы.

Примечание: Переменные среды сбора мусора для SGen (МОНО\_GC\_PARAMS) набор таким образом будет использоваться только при запуске из Xamarin Studio. При запуске приложения с устройства параметры Sgen будет игнорироваться. 

Чтобы окончательно задать переменную среды для приложения, необходимо добавить в качестве аргумента дополнительных mtouch (для всех соответствующих конфигураций):

```csharp
   --setenv=NAME=VALUE
```

Чтобы просмотреть переменные среды, которые могут быть установлены, обратитесь к странице моно man: [http://docs.go-mono.com/?link=man%3amono (1)](http://docs.go-mono.com/?link=man%3amono(1)) см. раздел: `ENVIRONMENT VARIABLES`

![](xs-mono-runtime-images/environment-variables.jpg "Задание переменных среды для проекта")
