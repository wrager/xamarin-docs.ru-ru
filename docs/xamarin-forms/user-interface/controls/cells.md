---
title: "Xamarin.Forms ячеек"
description: "Xamarin.Forms ячеек могут добавляться в представлениях ListView и TableViews."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms ячеек

_Xamarin.Forms ячеек могут добавляться в представлениях ListView и TableViews._

<style>.tableimg {Максимальная ширина: нет! важные;}</style>

## <a name="cells"></a>Ячейки

Ячейка является специализированный элемент, используемый для элементов в таблице и описывает, как должно отображаться каждый элемент в списке. Ячейка является производным от [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), из которого VisualElement также является производным. Однако ячейка не визуальный элемент, только описывает шаблон для создания визуального элемента. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Предоставляет базовый класс и возможностей для всех ячеек Xamarin.Forms. Ячейки являются элементами, предназначенных для добавления [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) или [ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) элементов управления.

Чтобы узнать, как использовать и настраивать ячеек, обратитесь к [ListView](~/xamarin-forms/user-interface/listview/index.md) и [TableView](~/xamarin-forms/user-interface/tableview.md) документации.

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> с меткой "и" поле ввода одну строку текста.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="Пример EntryCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> с меткой и двухфазным переключателем.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="Пример SwitchCell" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> с основным и дополнительным текстом.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="Пример TextCell" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
Объект <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">текст ячейки</a> , также содержит изображение.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="Пример ImageCell" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Коллекция Xamarin.Forms (пример)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
