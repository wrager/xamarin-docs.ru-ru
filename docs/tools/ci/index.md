---
title: "Введение в непрерывной интеграции с Xamarin"
description: "Непрерывная интеграция рекомендуется применять программного обеспечения в котором компилируется автоматизированной сборки и при необходимости проверяет приложения, когда добавляется или изменяется разработчиками в хранилище контроля версий проекта кода. В этой статье обсуждается основные принципы использования непрерывной интеграции и некоторые параметры, доступные для непрерывной интеграции с Xamarin проектами."
ms.topic: article
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: c28389479148829ee87eeda307915aacf7007b21
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Введение в непрерывной интеграции с Xamarin

_Непрерывная интеграция рекомендуется применять программного обеспечения в котором компилируется автоматизированной сборки и при необходимости проверяет приложения, когда добавляется или изменяется разработчиками в хранилище контроля версий проекта кода. В этой статье обсуждается основные принципы использования непрерывной интеграции и некоторые параметры, доступные для непрерывной интеграции с Xamarin проектами._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Введение в непрерывной интеграции](~/tools/ci/intro-to-ci.md)

В этом разделе рассматриваются различные компоненты, связанные с непрерывной интеграции и их связи. В нем изложены непрерывной интеграции среды, в которых рассматриваются в определенной в следующих разделах.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>Работа с непрерывной интеграции среды


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[С помощью приложения центра сборки с помощью Xamarin](/appcenter/build/xamarin/)

Сборка решения Xamarin.iOS и Xamarin.Android с центром приложения прямо из GitHub, Bitbucket или VSTS.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[С помощью TeamCity с помощью Xamarin](~/tools/ci/teamcity.md)

В этом руководстве описаны шаги, связанные с использованием TeamCity для компиляции мобильных приложений и их отправки в Xamarin Test Cloud.

###  <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[С помощью Jenkins, с помощью Xamarin](~/tools/ci/jenkins-walkthrough.md)

В этом руководстве показано, как настроить Jenkins как сервер непрерывной интеграции и автоматизации компиляции мобильных приложений, созданных с помощью Xamarin. Здесь описана процедура установки Jenkins в OS X, настроить его и установить задания для компиляции приложения Xamarin.iOS и Xamarin.Android, если изменения фиксируются в системе управления версиями.

