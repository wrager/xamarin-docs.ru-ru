---
title: Макеты с вкладками
description: Общие сведения о макетах с вкладками в Android
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/23/2017
ms.openlocfilehash: 4095944bb630637a2e761af18796dacdef17785c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-layouts"></a>Макеты с вкладками


## <a name="overview"></a>Обзор

Вкладки — шаблон распространенных пользовательских интерфейса в мобильных приложениях из-за их простоту и удобство использования. Они обеспечивают согласованность и простой способ перехода между экранов в приложении. Android имеет несколько API-интерфейса для интерфейсов с вкладками. 

-   **TabHost** &ndash; исходного API-интерфейс для создания с вкладками пользовательских интерфейсов. Объект `TabHost` мини-приложение будет добавлен в макете и действует как контейнер для представления с вкладками. Этот API, поскольку является устаревшей и использовать его не рекомендуется. 

-   **Панели действий** &ndash; входит новый набор API-интерфейса, появившихся в версии 3.0 Android (API уровня 11) с целью предоставления согласованного навигации и переключение представлений интерфейса. Которое было обратно перенесено в ОС Android 2.2 (API уровня 8) с [библиотеку поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; показывает current, следующей и предыдущей страницы `ViewPager`. `ViewPager` она доступна только через [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     Дополнительные сведения о `PagerTabStrip`, в разделе [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Панель инструментов** &ndash; `Toolbar` является компонентом панели действий, новая и более гибкие, который заменяет `ActionBar`. `Toolbar` доступна в Android без описания операций 5.0 или более поздней версии, и она доступна в старых версиях Android через [библиотеку поддержки Android версии 7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) пакет NuGet. 
    `Toolbar` сейчас компонент полосы рекомендуемые действия для использования в приложениях для Android.
    Дополнительные сведения см. в разделе [инструментов](~/android/user-interface/controls/tool-bar/index.md). 


Эти API визуально сильно отличаются и не совместимы друг с другом. Показывает изображение на экране следующие `TabHost` и `ActionBar` side-by-side: 

![Снимки экрана TabHost слева и панели действий справа](images/image01.png)

Эти несовместимые API существует из-за значительные изменения пользовательского интерфейса с момента 3.0 Android (API уровня 11). Одно из этих изменений пользовательского интерфейса был [действия строке шаблона проектирования](http://www.androidpatterns.com/uap_pattern/action-bar), шаблон предназначен для обеспечения быстрого доступа к функциональности навигации и ключ в приложении. `ActionBar` Впервые появился API для поддержки этого шаблона. 

`ActionBar` API проще и пожалуй предоставляет улучшить взаимодействие с пользователем. Он которое было перенесено обратно в ОС Android 2.2 и предпочтительнее для Xamarin.Android приложений. 

`TabHost` API совместимо во всех версиях Android, но требует больше усилий для использования и не согласуется с текущим [Android рекомендациям по пользовательскому Интерфейсу](http://developer.android.com/design/index.html). Разработчики не рекомендуется использовать этот API и следует под особенный новой панели действий с Xamarin.Android приложениями. 



## <a name="actionbarsherlock"></a>ActionBarSherlock

Прежде, чем API-Интерфейс панели действий были backported для ОС Android 2.2, разработчиков, нужна новая интерфейса API-интерфейса панели действий, но может использовать библиотеку стороннего [ActionBarSherlock](http://actionbarsherlock.com). ActionBarSherlock является расширением библиотеку поддержки Android предназначены для backport шаблон разработки панель действия для Android 2.x. В ОС Android 3.0 или более поздней версии, ActionBarSherlock будут автоматически использовать собственный `ActionBar` API, предоставляемые системой Android. Более старые версии Android будет использовать пользовательские реализации, имитирующую вида новой `ActionBar` API. [ActionBarSherlock компонент](https://www.nuget.org/packages/xamstore-XamarinActionBarSherlock/) позволяет легко добавить в приложение Xamarin.Android ActionBarSherlock. 



## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о TabHost](tab-host.md)
- [Пошаговое руководство TabHost](~/android/user-interface/layouts/tab-layout/creating-a-tabbed-ui.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet AppCompat v7 библиотеку поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека v7 совместимости приложений](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
