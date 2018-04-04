---
title: Шаблоны данных
description: DataTemplate используется для определения внешнего вида данных в элементе управления, поддерживаемых и обычно выполняет привязку к данным для отображения.
ms.prod: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
ms.openlocfilehash: 14de42acd1bde00df146a9fe5d772366735ed295
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="data-templates"></a>Шаблоны данных

_DataTemplate используется для определения внешнего вида данных в элементе управления, поддерживаемых и обычно выполняет привязку к данным для отображения._

## <a name="introductionintroductionmd"></a>[Введение](introduction.md)

Шаблоны Xamarin.Forms данных предоставляют возможность определения представления данных в элементе управления, поддерживаемых. В этой статье содержатся вводные данные шаблонов, проверки, поэтому они необходимы.

## <a name="creating-a-datatemplatecreatingmd"></a>[Создание DataTemplate](creating.md)

Шаблоны данных можно создать встроенным образом, в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), или из пользовательского типа и соответствующий тип ячейки Xamarin.Forms. Встроенного шаблона следует использовать, если нет необходимости повторно использовать шаблон данных в другом месте. Кроме того шаблон данных могут использоваться повторно путем определения его как пользовательский тип, или как ресурс на уровне страницы или уровня приложения уровня управления.

## <a name="creating-a-datatemplateselectorselectormd"></a>[Создание DataTemplateSelector](selector.md)

Объект [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) можно использовать для выбора [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) во время выполнения на основе значения свойства с привязкой к данным. Это позволяет нескольким `DataTemplate` экземпляры, чтобы применить тот же тип объекта, чтобы настроить внешний вид определенных объектов. В этой статье показано, как создавать и использовать `DataTemplateSelector`.


## <a name="related-links"></a>Связанные ссылки

- [Шаблоны данных (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/datatemplates/)
