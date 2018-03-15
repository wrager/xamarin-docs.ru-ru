---
title: Android Wear
description: "Создание приложений для устройств Android переносном."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 5db4c735205753810466c26535ba9e2f525709a8
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="android-wear"></a>Android Wear

Android одежды — это версия Android, предназначенный для переносном устройств, таких как смарт-контрольных значений. Этот раздел содержит инструкции по установке и настройке средства, необходимые для разработки одежды, пошаговое руководство для создания вашего первого устройства Износ и список образцов, который можно использовать для создания собственного носили приложений.

##  <a name="getting-startedandroidwearget-startedindexmd"></a>[Начало работы](~/android/wear/get-started/index.md)

Вводит Android носят, описывается установка и настройка компьютера для разработки Износ и пошаговые инструкции для создания и выполнения первого приложения Android носят на эмуляторе или устройстве износ.

##  <a name="user-interfaceandroidwearuser-interfaceindexmd"></a>[Пользовательский интерфейс](~/android/wear/user-interface/index.md)

Описание Android износ зависящие от элементов управления и ссылки на примеры, демонстрирующие способы использования этих элементов управления.

##  <a name="platform-featuresandroidwearplatformindexmd"></a>[Функции платформы](~/android/wear/platform/index.md)

Документы в этом разделе рассматриваются функции, связанные с Android с. Здесь вы найдете раздел описывает, как создать WatchFace.

##  <a name="screen-sizesandroidwearscreen-sizesmd"></a>[Размеры экрана](~/android/wear/screen-sizes.md)

Предварительный просмотр и оптимизировать пользовательского интерфейса для размеров экрана.

##  <a name="deployment--testingandroidweardeploy-testindexmd"></a>[Тестирование и развертывание](~/android/wear/deploy-test/index.md)

Объясняется, как развернуть приложение Android носят на устройстве с Android или эмулятор Android, настроенных для одежды. Он также включает отладку советы и сведения о настройке подключения Bluetooth на компьютере разработчика устройства Android.



## <a name="samples"></a>Примеры

Можно найти несколько [образцы](https://developer.xamarin.com/samples/android/Android%20Wear/) с помощью Android носят (или перейти сразу к [github](https://github.com/xamarin/monodroid-samples/tree/master/wear)). 

|Пример|Описание:|Снимок экрана|
|--- |--- |--- |
|[SkeletonWear](https://developer.xamarin.com/samples/SkeletonWear/)|Простой пример основы переносном проектов, включая GridViewPager и интерактивные уведомления.|![Снимок экрана Skeletonwear](images/skeleton.png)|
|[WatchViewStub](https://developer.xamarin.com/samples/WatchViewStub/)|Простой демонстрационный WatchViewStub элемента управления, который определяет форму экрана и автоматически загружает правильный макет.  Увидеть, как работает WatchViewStub **Resources/layout/main_actvity.xml** макета.|![Снимок экрана WatchViewStub](images/watchview.png)|
|[RecipeAssistant](https://developer.xamarin.com/samples/RecipeAssistant/)|Демонстрация страниц износ уведомления, в виде действия инструкций. Уведомления создаются в RecipeService.cs.|![Снимок экрана RecipeAssistant](images/recipeassist.png)|
|[ElizaChat](https://developer.xamarin.com/samples/ElizaChat/)|Образец интересна взаимодействия с «личный помощник» вызывается Eliza, с помощью интерактивного уведомления износ Создание диалога, используя стандартные ответы.|![Снимок экрана ElizaChat](images/eliza.png)|
|[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/)|GridViewPager реализует шаблон «2D навигации», где пользователь предъявляет по вертикали и по горизонтали, для просмотра параметров и содержимое.|![Снимок экрана GridViewPager](images/gridviewpager.png)|
|[WatchFace](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)|WatchFace является пользовательской циферблате с стиле аналогом час, минуту и второй руки. В этом примере демонстрируется создание службы начертания контрольных значений, который отображает текущее время и окружения режиме дескрипторов и видимость событий изменения. Он включает широковещательных приемник, который прослушивает изменения часового пояса и автоматически обновляет время соответствующим образом.|![Снимок экрана WatchFace](images/gridviewpager.png)|


##  <a name="videos"></a>Видеоролики

Рекомендуем ознакомиться с видео, что ссылки, посвященные Xamarin.Android с износ поддерживают:

|Описание:|Снимок экрана|
|--- |--- |
|[Android L и гораздо больше](http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/) &ndash; Android Developer Preview L появился множество новых интерфейсов API для разработчиков, чтобы воспользоваться преимуществами, включая разработки материал, уведомления и новые анимации лишь некоторые из них.|![Снимок экрана видео презентации](images/video-android-l.png)|
|[C# находится в моем уши и в глаза: стекла Google и Android с](https://www.youtube.com/watch?v=80H8tXByZQc) &ndash; переносном вычислений и выглядит как что-нибудь из будущего (или эпизода инспектора мини-приложения), однако многие пользователи уже принятие будущих сегодня! Разработчикам C# знает об этом и уже есть средства и навыки использования возможностей переносном устройств (от Evolve 2014 г.).|![Снимок экрана видео презентации](images/video-eyes-ears.png)|
|[Новые возможности Xamarin.Android](https://www.youtube.com/watch?v=Gpqc2XZIQfU) &ndash; Android L, носят Android, Android ТВ, автоматически Android, материалы и ГРАФИКОЙ; что делает это среднее вам, как разработчику Xamarin? Evolve 2014.|![Снимок экрана видео презентации](Images/video-whats-new.png)|


<!--

March 18
http://blog.xamarin.com/android-wear/

August 14
http://blog.xamarin.com/android-l-developer-preview-android-wear-support/

August 27
http://blog.xamarin.com/tips-for-your-first-android-wear-app/

Watch Face
https://github.com/Redth/Xamarin.Wear.WatchFace
-->
