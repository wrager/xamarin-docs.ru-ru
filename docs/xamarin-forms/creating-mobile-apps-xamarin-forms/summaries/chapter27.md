---
title: "Сводка Глава 27. Пользовательские модули подготовки отчетов"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 6d7c2b17e9596b7d2dd26aaf77cf13f7f8086cd5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Сводка Глава 27. Пользовательские модули подготовки отчетов

Элемент Xamarin.Forms, такие как `Button` визуализируется с платформой кнопки, содержащийся в классе с именем `ButtonRenderer`.  Вот [версию iOS `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android версии `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)и [версию среды выполнения Windows `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

В этой главе рассматривается, как можно создавать собственные модули подготовки отчетов для создания пользовательских представлений, сопоставленные с объектами конкретную платформу.

## <a name="the-complete-class-hierarchy"></a>Иерархия полного класса

Существует семь сборок, содержащих код конкретной платформы Xamarin.Forms.
Источник можно просмотреть на GitHub, используя следующие ссылки:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (очень мало)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (больше трех далее)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) пример отображает иерархию классов для сборок, которые являются допустимыми для выполнения платформы.

Обратите внимание на важные класс с именем `ViewRenderer`. Это класс, наследуют от при создании визуализации конкретную платформу. Так как он привязывается к системе представление целевой платформы, он существует в трех версиях:

IOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) имеет универсальные аргументы:

- `TView` ограничен [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` ограничен [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) имеет универсальные аргументы:

- `TView` ограничен [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` ограничен [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Среда выполнения Windows [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) по-разному с именем универсальных аргументов:

- `TElement` ограничен [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` ограничен [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

При написании модуля подготовки отчетов, будет производным от класса `View`и последующей записи нескольких `ViewRenderer` классов, по одному для каждой поддерживаемой платформы. Каждая реализация платформой будет ссылаться на собственный класс, производный от типа, укажите в качестве `TNativeView` или `TNativeElement` параметра.

## <a name="hello-custom-renderers"></a>Привет, пользовательские модули подготовки отчетов.

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) программа ссылается на пользовательское представление с именем `HelloView` в его [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) класса.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Класс включен в **HelloRenderers** проекта, а просто является производным от `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Класса в **HelloRenderers.iOS** проекта является производным от `ViewRenderer<HelloView, UILabel>`. В `OnElementChanged` override, он создает машинным кодом iOS `UILabel` и вызывает метод `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Класса в **HelloRenderers.Droid** проекта является производным от `ViewRenderer<HelloView, TextView>`. В `OnElementChanged` override, он создает Android `TextView` и вызывает метод `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Класса в **HelloRenderers.UWP** и другие проекты Windows является производным от `ViewRenderer<HelloView, TextBlock>`. В `OnElementChanged` override, создается Windows `TextBlock` и вызывает метод `SetNativeControl`.

Все `ViewRenderer` содержать производные `ExportRenderer` атрибутов на уровне сборки, которая связывает `HelloView` класса с определенной `HelloViewRenderer` класса. Это обнаружение модулей подготовки отчетов в проектах отдельных платформы Xamarin.Forms:

[![Снимок экрана тройной Hello представления](images/ch27fg02-small.png "настраиваемых модулей подготовки")](images/ch27fg02-large.png#lightbox "настраиваемых модулей подготовки")

## <a name="renderers-and-properties"></a>Модули подготовки отчетов и свойства

Рисование эллипса, который реализует следующий набор модулей подготовки отчетов и находится в различных проектах [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) решения.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Класс находится в **Xamarin.FormsBook.Platform** платформы. Класс аналогичен `BoxView` и определяет одно свойство: `Color` типа `Color`.

Модули подготовки отчетов могут передавать значения свойств, заданные на `View` собственному объекту путем переопределения `OnElementPropertyChanged` метод в модуле подготовки отчетов. В этом методе (и в большую часть модуля подготовки отчетов) доступны два свойства:

- `Element`, элемент Xamarin.Forms
- `Control`, объект собственного представления или мини-приложения или элемента управления

Типы эти свойства определяются универсальных параметров `ViewRenderer`. В этом примере `Element` относится к типу `EllipseView`.

`OnElementPropertyChanged` Переопределение таким образом можно переносить `Color` значение `Element` собственному `Control` объекта, возможно, с каким-либо преобразования. Ниже перечислены три модулей подготовки отчетов

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), которая использует [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) класс эллипса.
- Android — [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), которая использует [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) класс эллипса.
- Среда выполнения Windows: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), который можно использовать с приложениями Windows [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) класса.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) класс выводит некоторые из них `EllipseView` объектов:

[![Тройной экрана эллипс демонстрации](images/ch27fg03-small.png "EllipseView настраиваемых модулей подготовки")](images/ch27fg03-large.png#lightbox "EllipseView настраиваемых модулей подготовки")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) клики `EllipseView` off стороны экрана.

## <a name="renderers-and-events"></a>Модули подготовки отчетов и событий

Можно также для модулей подготовки отчетов для создания событий косвенно. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Класса похоже на обычный Xamarin.Forms `Slider` , но можно указать несколько отдельных действий между `Minimum` и `Maximum` значения.

Ниже перечислены три модулей подготовки отчетов

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Среда выполнения Windows: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Модули подготовки отчетов, обнаруживать изменения собственного элемента управления, а затем вызвать `SetValueFromRenderer`, ссылки, которые привязываемых свойство, определенное в `StepSlider`, изменения в результате `StepSlider` срабатывание `ValueChanged` событий.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) образец демонстрирует этот новый ползунок.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 27 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Глава 27 образцы](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
