---
title: Android Wear
description: "Создание приложений для устройств Android переносном."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE4A128-2D88-4500-9E48-20375EA99A49
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: ac83b74f39497333de7aa80079784adf61bf2e65
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
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

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <thead>
      <th>
          <strong>Пример</strong>
      </th>
      <th>
          <strong>Описание</strong>
      </th>
      <th>
          <strong>Screenshot</strong>
      </th>
  </thead>
  <tbody>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/SkeletonWear/">SkeletonWear</a>
      </td>
      <td valign="top">
Простой пример основы переносном проектов, включая GridViewPager и интерактивные уведомления.
      </td>
      <td>
          <img src="Images/skeleton.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/WatchViewStub/">WatchViewStub</a>
      </td>
      <td valign="top">
Простой демонстрационный WatchViewStub элемента управления, который определяет форму экрана и автоматически загружает правильный макет.
Увидеть, как работает WatchViewStub <b>Resources/layout/main_actvity.xml</b> макета.
      </td>
      <td>
          <img src="Images/watchview.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/RecipeAssistant/">RecipeAssistant</a>
      </td>
      <td valign="top">
Демонстрация страниц износ уведомления, в виде действия инструкций. Уведомления создаются в <b>RecipeService.cs</b>.
      </td>
      <td>
          <img src="Images/recipeassist.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/ElizaChat/">ElizaChat</a>
      </td>
      <td valign="top">
Образец интересна взаимодействия с «личный помощник» вызывается Eliza, с помощью интерактивного уведомления износ Создание диалога, используя стандартные ответы.
      </td>
      <td>
          <img src="Images/eliza.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/GridViewPager/">GridViewPager</a>
      </td>
      <td valign="top">
GridViewPager реализует шаблон «2D навигации», где пользователь предъявляет по вертикали и по горизонтали, для просмотра параметров и содержимое.
      </td>
      <td>
          <img src="Images/gridviewpager.png" class="tableimg">
      </td>
  </tr>
  <tr>
      <td valign="top">
          <a href="https://developer.xamarin.com/samples/monodroid/wear/WatchFace">WatchFace</a>
      </td>
      <td valign="top">
          <b>WatchFace</b> является пользовательской циферблате с стиле аналогом час, минуту и второй руки. В этом примере демонстрируется создание службы начертания контрольных значений, который отображает текущее время и окружения режиме дескрипторов и видимость событий изменения. Он включает широковещательных приемник, который прослушивает изменения часового пояса и автоматически обновляет время соответствующим образом.
      </td>
      <td>
          <img src="Images/watchface.png" class="tableimg">
      </td>
  </tr>
  </tbody>
</table>

##  <a name="videos"></a>Видеоролики

Рекомендуем ознакомиться с видео, что ссылки, посвященные Xamarin.Android с износ поддерживают.

<table align="center" border="0" cellpadding="1" cellspacing="1">
    <tr>
        <td>
        <a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/"><img src="Images/video-android-l.png" border="0" /></td>
        <td><a href="http://blog.xamarin.com/webinar-recording-android-l-and-so-much-more/">Android L и многое другое</a>
        <br />
Android Developer L Preview появилось множество новых интерфейсов API для разработчиков, чтобы воспользоваться преимуществами, включая разработки материал, уведомления и новые анимации лишь некоторые из них.</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=80H8tXByZQc"><img src="Images/video-eyes-ears.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=80H8tXByZQc">C# находится в моем уши и в глаза: стекла Google и с Android</a>
        <br />
Переносном вычислений и выглядит как что-нибудь из будущего (или эпизода инспектора мини-приложения), но многие пользователи уже принятие будущих сегодня! Разработчикам C# знает об этом и уже есть средства и навыки использования возможностей переносном устройств (от Evolve 2014 г.).</td>
    </tr>
    <tr>
        <td>
        <a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU"><img src="Images/video-whats-new.png" border="0" /></td>
        <td><a href="https://www.youtube.com/watch?v=Gpqc2XZIQfU">Новые возможности Xamarin.Android</a>
        <br />
        <i>Android L, Android одежды, Android ТВ, автоматически Android, материалов и ГРАФИКОЙ; Каково для вас как разработчика Xamarin? </i> из развивать 2014 г.</td>
    </tr>
</table>


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
