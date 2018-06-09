---
title: Xamarin.Forms страниц
description: Xamarin.Forms страницы представляют экраны кросс платформенных мобильных приложений. В этой статье перечислены страницы, которые включены в Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 9C8C710F-E312-420B-9324-A7A20CEDB7EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: f4b8d895c6c9c7f47b8af8e56ad60a5a453e2d56
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243162"
---
# <a name="xamarinforms-pages"></a>Xamarin.Forms страниц

_Xamarin.Forms страницы представляют экраны кросс платформенных мобильных приложений._

Все типы страниц, которые описаны ниже, являются производными от Xamarin.Forms [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) класса. Эти визуальные элементы занимают все или большинство экрана. Объект `Page` представляет `ViewController` в iOS и `Page` в универсальной платформы Windows. На Android, каждая страница занимает экран, подобный `Activity`, но страницы Xamarin.Forms *не* `Activity` объектов.

[ ![](pages-images/pages-sml.png "Типы страниц Xamarin.Forms")](pages-images/pages.png#lightbox "типы Xamarin.Forms страницы")

## <a name="pages"></a>Pages

Xamarin.Forms поддерживает следующие типы страниц:

<a name="contentPage" />

### <a name="contentpage"></a>ContentPage

|     |     |
| --- | --- |
| [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) является простейшим и наиболее распространенным типом страницы. Задать [ `Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentPage.Content/) свойства должен быть единственным [ `View` ](views.md) объекта, который является наиболее часто [ `Layout` ](layouts.md) например [ `StackLayout` ](layouts.md#stackLayout), [ `Grid` ](layouts.md#grid), или [ `ScrollView` ](layouts.md#scrollView).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) | [![Пример ContentPage](pages-images/ContentPage.png "пример ContentPage")](pages-images/ContentPage-Large.png#lightbox "ContentPage пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ContentPageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ContentPageDemoPage.xaml) |
|     |     |

### <a name="masterdetailpage"></a>MasterDetailPage

|     |     |
| --- | --- |
| Объект [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) управляет двумя областей данных. Задать [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) свойство страницу, обычно отображаются в списке или меню. Задать [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) свойство на странице отображается выбранный элемент из главной страницы. [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) Регулирует ли страница основной или подробности.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) / [руководство](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md) / [образца](https://developer.xamarin.com/samples/xamarin-forms/Navigation/MasterDetailPage/) | [![Пример MasterDetailPage](pages-images/MasterDetailPage.png "пример MasterDetailPage")](pages-images/MasterDetailPage-Large.png#lightbox "MasterDetailPage пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MasterDetailPageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml) с [кода](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MasterDetailPageDemoPage.xaml.cs) |
|     |     |

### <a name="navigationpage"></a>NavigationPage

|     |     |
| --- | --- |
| [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Управляет навигацию между другие страницы, использующие архитектуру на основе стека. При использовании навигацию по страницам в приложении, домашней страницы экземпляра должны быть переданы в конструктор `NavigationPage` объекта.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) / [руководство](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) / [пример 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/), [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/), и [3](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)  | [![Пример NavigationPage](pages-images/NavigationPage.png "пример NavigationPage")](pages-images/NavigationPage-Large.png#lightbox "NavigationPage пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/NavigationPageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml) с [код = за](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/NavigationPageDemoPage.xaml.cs) |
|     |     |

### <a name="tabbedpage"></a>TabbedPage

|     |     |
| --- | --- |
| [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) является производным от абстрактного [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) класса и позволяет навигацию между дочерней страницы с помощью вкладок. Задать [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) коллекция страниц или набора [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) коллекция объектов данных и [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) свойства [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) описывающее как визуально представить каждого объекта.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) / [руководство](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) / [пример 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/) и [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage) | [![Пример TabbedPage](pages-images/TabbedPage.png "пример TabbedPage")](pages-images/TabbedPage-Large.png#lightbox "TabbedPage пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TabbedPageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TabbedPageDemoPage.xaml) |
|     |     |

### <a name="carouselpage"></a>CarouselPage

|     |     |
| --- | --- |
| [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) является производным от абстрактного [ `MultiPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/) класса и позволяет навигацию между дочерней страницы посредством считывания пальцем. Задать [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) свойство в коллекцию [ `ContentPage` ](#contentPage) объектов или набор [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) коллекция объектов данных и [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) свойства [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) описывающее как визуально представить каждого объекта.<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) / [руководство](~/xamarin-forms/app-fundamentals/navigation/carousel-page.md) / [пример 1](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/) и [2](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/) | [![Пример CarouselPage](pages-images/CarouselPage.png "пример CarouselPage")](pages-images/CarouselPage-Large.png#lightbox "CarouselPage пример")<br />[Код C# для этой страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CarouselPageDemoPage.cs) / [XAML-страницы](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CarouselPageDemoPage.xaml) |
|     |     |

### <a name="templatedpage"></a>TemplatedPage

|     |     |
| --- | --- |
| [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) Отображает содержимое полноэкранном режиме с помощью шаблона элемента управления и является базовым классом для [ `ContentPage` ](#contentPage).<br /><br />[Документация по API](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/) / [руководства](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md) | [![Пример TemplatedPage](pages-images/TemplatedPage.png "пример TemplatedPage")](pages-images/TemplatedPage.png "TemplatedPage пример") |
|     |     |

## <a name="related-links"></a>Связанные ссылки

- [Введение в Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Образец Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Примеры Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Документация по API-интерфейса Xamarin.Forms](https://developer.xamarin.com/api/root/Xamarin.Forms/)
