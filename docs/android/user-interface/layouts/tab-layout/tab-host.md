---
title: "Вкладка макета с TabHost"
description: "В этой статье приводятся общий обзор TabHost, старые API используется для создания макетов с вкладками в приложении Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/25/2017
ms.openlocfilehash: ff61ca0a2bca466da3e33c93a17944915328b70c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="tab-layout-with-tabhost"></a>Вкладка макета с TabHost

_В этой статье приводятся общий обзор TabHost, старые API используется для создания макетов с вкладками в приложении Xamarin.Android._

<a name="Overview" />

## <a name="overview"></a>Обзор

> [!NOTE]
> **Примечание:** `TabHost` является старый API, который является устаревшим Google. Разработчики могут создавать приложения с вкладками, с помощью [панели действий](~/android/user-interface/controls/action-bar.md). `ActionBar` Доступна в все версии Android. Он впервые появился в Android 3.0 (API уровня 11) и обратно была перенесена в ОС Android 2.2 (API уровня 8) и Android 2.3 (API уровня 10) в [V7 AppCompat библиотеки](http://developer.android.com/tools/support-library/features.html#v7-appcompat), которая доступна для Xamarin.Android через [Xamarin Библиотека поддержки Android - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) пакета.

`TabHost` Является более старые, исходное API для создания пользователя с вкладками interfacesIt является наилучшим образом подходит для приложений Xamarin.Android, должны поддерживать Android 2.2 и Android 2.3 и не может использовать **ActionBarSherlock**.
Следующие пять компонентов — все это с `TabHost` API:

-  **TabActivity** &ndash; это специальное представление, которое выступает в роли контейнера для `TabHost` (как описано ниже).

-  **TabHost** &ndash; это контейнер для пользовательского интерфейса с вкладками. Он содержит два дочерних элемента: список меток вкладок и их `FrameLayout` , отображающий содержимое вкладок.

-  **TabWidget** &ndash; этот элемент управления отображает список меток вкладок, один для каждой вкладки в `TabHost` . Каждая вкладка в `TabHost` представлены `TabHost.TabSpec` объекта. Этот объект содержит метаданные для каждой вкладки. При выборе вкладки, `TabHost` отвечает на выбор, отображая на соответствующую вкладку.

-  **FrameLayout** &ndash; должен иметь пользовательский Интерфейс с вкладками `FrameLayout` содержащихся в словаре `TabHost`. Он используется для отображения содержимого для вкладки.

-  **Действия или представлений** &ndash; при выборе вкладки отображаются действия или представления в `FrameLayout`.

На следующей диаграмме показан как все эти компоненты связаны друг с другом:

![Диаграмма, иллюстрирующая макет кадра в TabWidget в TabHost](tab-host-images/image03.png)

Содержимое вкладок может быть действий или представления. Представления, сравнительно простым и простой, но может привести к большой объем co habitating несвязанный код в действии. Это приведет к снижению разделение о проблемах и класс перегруженными, трудно поддерживать. В отличие от этого действия требуют системных ресурсов, а также позволяют более Модульный подход с логикой для каждой вкладки, содержащийся в собственный отдельный класс.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье описывается высокоуровневая компоненты более старых `TabHost` API для Android и как они связаны друг с другом.



## <a name="related-links"></a>Связанные ссылки

- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabHost.TabSpec](https://developer.xamarin.com/api/type/Android.Widget.TabHost+TabSpec/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet AppCompat v7 библиотеку поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека v7 совместимости приложений](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
