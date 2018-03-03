---
title: "Xamarin.Forms страниц"
description: "Xamarin.Forms страницы представляют экраны кросс платформенные мобильные приложения."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 35822dbbb7d5694e7f1f0a3f35f10df404206af9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms страниц

_Xamarin.Forms страницы представляют экраны кросс платформенные мобильные приложения._

<style>.tableimg {Максимальная ширина: нет! важные;}</style>

## <a name="pages"></a>Число страниц

[ `Page` ](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) Класс является визуальный элемент, который занимает большинство или все части экрана и содержит один из дочерних элементов. Объект `Xamarin.Forms.Page` представляет представление-контроллер в iOS или на странице в Windows Phone. В Android требует каждой страницы на экран как действия, но страницы Xamarin.Forms *не* действий.

 [ ![](pages-images/pages-sml.png "Типы страниц Xamarin.Forms")](pages-images/pages.png "типы Xamarin.Forms страницы")

<br clear="all" />

Поддерживает Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
  <thead>
    <th>
      <strong>Тип</strong>
    </th>
    <th>
      <strong>Описание</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a> отображается одна <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">представление</a>, часто контейнере, такие как <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> или <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="Пример ContentPage" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">страницы</a> , управляющий двух областей данных.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="Пример MasterDetailPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">страницы</a> , управляющий навигацию и взаимодействие с пользователем стека из других страниц.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="Пример NavigationPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">страницы</a> , позволяет осуществлять навигацию между дочерних страниц с использованием вкладок.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="Пример TabbedPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">страницы</a> отображения содержимого на весь экран с помощью шаблона элемента управления и базовый класс для <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="Пример TemplatedPage" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">страницы</a> позволяя жесты проведите между вложенные страницы, такие как коллекции.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="Пример CarouselPage" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Коллекция Xamarin.Forms (пример)](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Документация по API-интерфейса Xamarin.Forms](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
