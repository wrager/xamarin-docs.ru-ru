---
title: Xamarin.Forms ячеек
description: Xamarin.Forms ячеек могут добавляться в представлениях ListView и TableViews. В этой статье перечислены ячеек, включенных в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 0947027b43eacd0bac269ebf7a779746e0d22866
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243360"
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms ячеек

_Xamarin.Forms ячеек могут добавляться в представлениях ListView и TableViews._

Объект *ячейки* — специализированные элемент, используемый для элементов в таблице и описывает, каким образом должна обрабатываться каждого элемента в списке. [ `Cell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Класс является производным от [ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), из которого [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) также является производным. Ячейки сам по себе не является визуальный элемент; Вместо это шаблон для создания визуального элемента.

`Cell` используется только с [ `ListView` ](views.md#listView) и [ `TableView` ](views.md#tableView) элементов управления. Чтобы узнать, как использовать и настраивать ячеек, обратитесь к [ `ListView` ](~/xamarin-forms/user-interface/listview/index.md) и [ `TableView` ](~/xamarin-forms/user-interface/tableview.md) документации.

## <a name="cells"></a>Ячейки

Xamarin.Forms поддерживает следующие типы ячейки:

<a name="textCell" />

### <a name="textcell"></a>TextCell

|     |     |
| --- | --- |
| Объект [ `TextCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) отображает один или два текстовых строк. Задать [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) свойство и, возможно, [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) свойство в эти текстовые строки.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [руководства](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![Пример TextCell](cells-images/TextCell.png "пример TextCell")](cells-images/TextCell-Large.png#lightbox "TextCell пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### <a name="imagecell"></a>ImageCell

|     |     |
| --- | --- |
| [ `ImageCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) Отображает те же сведения, как [ `TextCell` ](#textCell) , но включает точечного рисунка, который необходимо настроить [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) свойство.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [руководства](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![Пример ImageCell](cells-images/ImageCell.png "примере ImageCell")](cells-images/ImageCell-Large.png#lightbox "ImageCell пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### <a name="switchcell"></a>SwitchCell

|     |     |
| --- | --- |
| [ `SwitchCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) Содержит текст с [ `Text`"](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) свойство и и переключатель изначально настроено с двоичным [ `On` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) свойство. Обрабатывать [ `OnChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) событий получать уведомления при `On` изменения свойств.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [руководства](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![Пример SwitchCell](cells-images/SwitchCell.png "пример SwitchCell")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### <a name="entrycell"></a>EntryCell

|     |     |
| --- | --- |
| [ `EntryCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) Определяет [ `Label` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) свойства, которое определяет ячейку и одну строку для редактирования текста в [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) свойство. Обрабатывать [ `Completed` ](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) событие, чтобы получать уведомления, когда пользователь завершит ввод текста.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [руководства](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![Пример EntryCell](cells-images/EntryCell.png "пример EntryCell")](cells-images/EntryCell-Large.png#lightbox "EntryCell пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Образец Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
