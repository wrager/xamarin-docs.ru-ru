---
title: Вступление
description: Xamarin.Forms шаблонов элементов управления предоставляют возможность легко темы и re темы страницы приложения во время выполнения. В этой статье содержатся общие сведения о шаблонах элементов управления.
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 744419cbc457ffb6dab6b46d690151c08ca35d42
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="introduction"></a>Вступление

_Xamarin.Forms шаблонов элементов управления предоставляют возможность легко темы и re темы страницы приложения во время выполнения. В этой статье содержатся общие сведения о шаблонах элементов управления._

Элементы управления имеют различные свойства, такие как `BackgroundColor` и `TextColor`, который можно определить внешний вид элемента управления. Эти свойства можно задать с помощью [стили](~/xamarin-forms/user-interface/styles/index.md), который может быть изменен во время выполнения для реализации основных тем. Тем не менее стили не поддерживать четкое разделение внешний вид страницы и его содержимого и изменения, которые можно сделать, задав такие параметры ограничены.

Шаблоны элементов управления предоставляют четкое разделение внешний вид страницы и его содержимым, таким образом, что позволяет создавать страницы, которые легко может быть включен в тему. Например приложение может содержать шаблоны управление на уровне приложения, которые предоставляют темной темой и светлой темами. Каждый [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) в приложении может быть включен в тему, применив один из шаблонов элементов управления без изменения содержимого, отображаемого в каждой странице. Кроме того темы, предоставляемые шаблонов элементов управления не только для изменения свойств элементов управления. Также можно изменить элементы управления, используемые для реализации темы.

## <a name="creating-a-controltemplate"></a>Создание шаблона ControlTemplate

Объект [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) определяет внешний вид страницы или представления и содержит корневой макет и на макет элементов управления, реализующих шаблон. Как правило `ControlTemplate` задействует [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) для пометки, где будет отображаться содержимое, отображаемое на странице или представления. Страница или представление, которое использует `ControlTemplate` этого необходимо определить содержимое, отображаемое по `ContentPresenter`. На следующей схеме показана `ControlTemplate` для страницы, которая содержит ряд элементов управления, включая `ContentPresenter` помеченные синим прямоугольником:

![](introduction-images/control-template.png "Шаблон элемента управления для страницы")

Объект [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) могут применяться к следующим типам, задав их `ControlTemplate` свойства:

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

Когда [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) и назначается для этих типов, любые существующие внешний вид заменяется внешний вид, определенные в `ControlTemplate`. Кроме того, а также настройка внешнего вида с помощью `ControlTemplate` свойства, шаблоны также можно применить с помощью стилей для дальнейшего управления разверните возможность темы.

> [!NOTE]
>  *Что такое `TemplatedPage` и `TemplatedView` типов?* `TemplatedPage` является базовым классом для `ContentPage`, а — самый простой тип страницы, предоставляемые Xamarin.Forms. В отличие от `ContentPage`, `TemplatedPage` не имеет `Content` свойства. Таким образом, содержимое не может быть добавлено непосредственно `TemplatedPage` экземпляра. Вместо этого содержимое добавляется, установив шаблон элемента управления для `TemplatedPage` экземпляра. Аналогичным образом `TemplatedView` является базовым классом для `ContentView`. В отличие от `ContentView`, `TemplatedView` не имеет `Content` свойства. Таким образом, содержимое не может быть добавлено непосредственно `TemplatedView` экземпляра. Вместо этого содержимое добавляется, установив шаблон элемента управления для `TemplatedView` экземпляра.

Шаблоны элементов управления можно создать в XAML и C#:

- Шаблоны элементов управления, созданный в XAML, определяются в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) , которые назначены [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) коллекции страницы или чаще для [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) сбора приложения.
- Шаблоны элементов управления, созданных в C# обычно определяются в классе страницы или в классе, который может осуществляться глобально.

Выбор места для определения [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляра влияние, где он может использоваться:

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляры, определенные на уровне страниц может применяться только к странице.
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляры, определенного на уровне приложения могут применяться на страницы в приложении.

Шаблоны элементов управления более низкого уровня в иерархии представления имеют приоритет над указанных выше вверх. Например [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) с именем `DarkTheme` , определенный на уровне страницы будет иметь приоритет над одноименного шаблона, определенного на уровне приложения. Таким образом шаблон элемента управления, который определяет тему для применения к каждой страницы в приложении должен быть определен на уровне приложения.


## <a name="related-links"></a>Связанные ссылки

- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
