---
title: "Макеты Xamarin.Forms"
description: "Xamarin.Forms макеты используются для создания элементов управления пользовательского интерфейса в логической структуры."
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Макеты Xamarin.Forms

_Xamarin.Forms макеты используются для создания элементов управления пользовательского интерфейса в логической структуры._

<style>.tableimg {Максимальная ширина: нет! важные;}</style>

## <a name="layouts"></a>Макеты

[`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Класса в Xamarin.Forms является подтипом специализированные представления, который служит контейнером для других схемах или представления. Как правило, он содержит логику для настроить положение и размеры дочерних элементов в приложениях Xamarin.Forms.

 [ ![](layouts-images/layouts-sml.png "Типы макета Xamarin.Forms")](layouts-images/layouts.png "типы макета Xamarin.Forms")

<br clear="all" />

Поддерживает Xamarin.Forms:

<table align="center" border="1" cellpadding="1" cellspacing="1">
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
  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
Диспетчер макет шаблона представления. Используется в <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a> для пометки, где отображаются данные, отображаемые.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="Пример ContentPresenter" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
Элемент с одиночное содержимое. ContentView имеет очень мало использование. Он предназначен для использования в качестве базового класса для определяемой пользователем составных представлений.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="Пример ContentView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Кадра</a>
    </td>
    <td valign="top">
Элемент, содержащий один из дочерних элементов, с некоторыми параметрами обрамления. Рамка иметь значение по умолчанию <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="Пример создания" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
Требуется возможность прокрутки, если содержимое элемента.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="Пример ScrollView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
Элемент, который отображает содержимое с помощью шаблона элемента управления и базовый класс для <a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="Пример TemplatedView" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
Размещает дочерние элементы в абсолютные запрошенной позиции. Пользователь, которому назначен привязки и границы определяет положение и размер элемента управления.
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="Пример AbsoluteLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a>
    </td>
    <td valign="top">
Макет, содержащий сгруппированные по строкам и столбцам представления.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="Пример сетки" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">макета</a> , использующий <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">ограничение</a>s макет его дочерних элементов.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="Пример RelativeLayout" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">макета</a> , размещает дочерние элементы в одну строку, которая может быть ориентированную горизонтально или вертикально. Этот макет будет настройка дочерних границы автоматически во время цикла разметки. Границы, пользователь будет перезаписан и поэтому не следует задавать для дочернего элемента пользователем.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="Пример StackLayout" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Коллекция Xamarin.Forms (пример)](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
