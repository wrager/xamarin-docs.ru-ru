---
title: Выбор Xamarin.Forms
description: Средство выбора Xamarin.Forms отображает короткий список элементов, из которых пользователь может выбрать элемент. В этой статье объясняется, как использовать класс выбора для выбора элемента из списка данных.
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/04/2018
ms.openlocfilehash: 82ae36a7be139e2a93d0e5c43c4bad355c49f217
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245043"
---
# <a name="xamarinforms-picker"></a>Выбор Xamarin.Forms

_Представление выбора является элементом управления для выбора элемента из списка данных._

Xamarin.Forms [ `Picker` ](xref:Xamarin.Forms.Picker) отображает короткий список элементов, из которых пользователь может выбрать элемент. `Picker` Определяет восемь свойства:

- [`Title`](xref:Xamarin.Forms.Picker.Title) Тип `string`, который по умолчанию `null`.
- [`ItemsSource`](xref:Xamarin.Forms.Picker.ItemsSource) Тип `IList`, исходного списка элементов для отображения, которая по умолчанию `null`.
- [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) Тип `int`, индекс выбранного элемента по умолчанию — -1.
- [`SelectedItem`](xref:Xamarin.Forms.Picker.SelectedItem) Тип `object`, выбранный элемент, значение по умолчанию `null`.
- [`TextColor`](xref:Xamarin.Forms.Picker.TextColor) Тип [ `Color` ](xref:Xamarin.Forms.Color), цвет, используемый для отображения текста, который по умолчанию использует [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/).
- [`FontAttributes`](xref:Xamarin.Forms.Picker.FontAttributes) Тип [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes), который по умолчанию [ `FontAtributes.None` ](xref:Xamarin.Forms.FontAttributes.None).
- [`FontFamily`](xref:Xamarin.Forms.Picker.FontFamily) Тип `string`, который по умолчанию `null`.
- [`FontSize`](xref:Xamarin.Forms.Picker.FontSize) Тип `double`, который по умолчанию -1,0.

Все восемь свойств, поддерживаемых [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) объектов, что означает, что они могут быть со стилем и свойства могут являться целевыми для привязки данных. [ `SelectedIndex` ](xref:Xamarin.Forms.Picker.SelectedIndex) И [ `SelectedItem` ](xref:Xamarin.Forms.Picker.SelectedItem) свойства имеют режима привязки по умолчанию [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay), что означает, что они могут быть целями привязок данных в приложении, которое использует [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) архитектуры. Сведения о настройке свойств шрифта см. в разделе [шрифты](~/xamarin-forms/user-interface/text/fonts.md).

Объект [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) не показывает все данные при первом отображении. Вместо этого значение его [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) свойство отображается как заполнитель для iOS и Android платформ:

[![](images/picker-initial.png "Начальный экран выбора")](images/picker-initial-large.png#lightbox "начальный экран выбора")

Когда [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) фокус прибыли, его данные отображается и пользователь может выбрать элемент:

[![](images/picker-selection.png "При выборе элемента выбора")](images/picker-selection-large.png#lightbox "при выборе элемента выбора")

[ `Picker` ](xref:Xamarin.Forms.Picker) Активируется [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) событие, когда пользователь выбирает элемент. После выбора, выбранный элемент отображается с `Picker`:

![](images/picker-after-selection.png "Средство выбора после выбора")

Существует два способа для заполнения [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) с данными:

- Установка [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) свойство для отображения данных. Это рекомендуемая методика, которая была введена в Xamarin.Forms 2.3.4. Дополнительные сведения см. в разделе [свойства ItemsSource элемент выбора](populating-itemssource.md).
- Добавление данных, отображаемых для [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) коллекции. Этот метод был исходного процесса заполнения [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) с данными. Дополнительные сведения см. в разделе [Добавление данных в коллекцию элементов в средстве выбора](populating-items.md).

## <a name="related-links"></a>Связанные ссылки

- [Средство выбора](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
