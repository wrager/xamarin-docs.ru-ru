---
title: "Средство выбора"
description: "Представление выбора является элементом управления для выбора элемента из списка данных."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
ms.openlocfilehash: fc1aaffe4e31b596d57b5de30c87217ffba3772e
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="picker"></a>Средство выбора

_Представление выбора является элементом управления для выбора элемента из списка данных._

Объект [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) отображает короткий список элементов, из которых может выбрать пользователь. Тем не менее [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) не показывает все данные при первом отображении. Вместо этого значение его [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) свойство отображается как заполнитель для iOS и Android платформ:

[![](images/picker-initial.png "Начальный экран выбора")](images/picker-initial-large.png#lightbox "начальный экран выбора")

Когда [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) фокус прибыли, его данные отображается и пользователь может выбрать элемент:

[![](images/picker-selection.png "При выборе элемента выбора")](images/picker-selection-large.png#lightbox "при выборе элемента выбора")

После выбора, выбранный элемент отображается с [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Средство выбора после выбора")

Существует два способа для заполнения [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) с данными:

- Установка [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) свойство для отображения данных. Это рекомендуемая методика, которая была введена в Xamarin.Forms 2.3.4. Дополнительные сведения см. в разделе [свойства ItemsSource элемент выбора](populating-itemssource.md).
- Добавление данных, отображаемых для [ `Items` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) коллекции. Этот метод был исходного процесса заполнения [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) с данными. Дополнительные сведения см. в разделе [Добавление данных в коллекцию элементов в средстве выбора](populating-items.md).


## <a name="related-links"></a>Связанные ссылки

- [Средство выбора](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
