---
title: "Сводка по 25 главы. Создание страницы"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: bbe960357d9180df90a4423d6acfdf3f869d1b77
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-25-page-varieties"></a>Сводка по 25 главы. Создание страницы

Теперь вы знаете двух классов, производных от `Page`: `ContentPage` и `NavigationPage`. В этой главе приведено двумя другими числами.

- [`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Управляет две страницы, основной и сведений
- [`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Управляет несколько дочерних страниц, доступ через вкладок

Эти типы страницы обеспечивают более сложные параметры навигации, чем `NavagationPage` подробно [Глава 24. Страница навигации](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter24.md).

## <a name="master-and-detail"></a>Главный и сведений

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Определяет два свойства типа `Page`: [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) и [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/). Обычно каждое из этих свойств, чтобы задать `ContentPage`. `MasterDetailPage` Отображает и переключений между этим двум страницам.

Существует два основных способа переключения между этими двумя страницами:

- *Разделение* основные и подробные которых рядом друг с другом
- *popover* где подробно рассматриваются или частично рассматриваются master странице

Существует несколько вариантов *popover* подход (*слайд*, *перекрываются*, и *swap*), но они обычно платформы зависимые. Можно задать [ `MasterDetailBehavior` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.MasterBehavior/) свойство `MasterDetailPage` на член [ `MasterBehavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterBehavior/) перечисления:

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Default/)
- [`Split`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Split/)
- [`SplitOnLandscape`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnLandscape/)
- [`SplitOnPortrait`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.SplitOnPortrait/)
- [`Popover`](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterBehavior.Popover/)

Тем не менее это свойство имеет не влияет на телефонах. Телефоны всегда имеют popover поведение. Только планшетов и настольных компьютеров windows может иметь разбиения в работе.

### <a name="exploring-the-behaviors"></a>Изучение поведения

[ **MasterDetailBehaviors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailBehaviors) пример позволяет экспериментировать с поведением по умолчанию на разных устройствах. Программа содержит два отдельных `ContentPage` производные master и описание (с `Title` свойство, заданное для обоих) и еще один класс, производный от `MasterDetailPage` , объединяет их. Страница сведений заключается в `NavigationPage` так, как программа UWP не будет работать не будет.

Платформы Windows 8.1 и Windows Phone 8.1 требуется, растровое изображение было присвоено `Icon` свойство главной страницы.

### <a name="back-to-school"></a>Снова за учебу

[ **SchoolAndDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/SchoolAndDetail) образец принимает немного другой подход к построению отобразить студентов [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) библиотеки.

`Master` И `Detail` свойства определяются с помощью визуальных деревьев в [SchoolAndDetailPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml) файл, который является производным от `MasterDetailPage`. Такой подход позволяет привязки данных для задания между страницами главных и подчиненных.

Также устанавливает файл XAML [ `IsPresented` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsPresented/) свойство `MasterDetailPage` для `True`. В результате главной страницы для отображения при запуске; по умолчанию отображается страница сведений. [SchoolAndDetailPage.xaml.cs](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/SchoolAndDetail/SchoolAndDetail/SchoolAndDetail/SchoolAndDetailPage.xaml.cs) файл задает `IsPresented` для `false` при выборе элемента из `ListView` на главной странице. Затем открывается страница сведений:

[![Тройной экрана School детализации и](images/ch25fg09-small.png "страницу сведений из MasterDetailPage")](images/ch25fg09-large.png "страницу сведений из MasterDetailPage")

### <a name="your-own-user-interface"></a>Собственный пользовательский интерфейс

Несмотря на то, что Xamarin.Forms предоставляет пользовательский интерфейс для переключения между представлениями master и сведений, можно создать собственные. Для этого:

- Задать [ `IsGestureEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.IsGestureEnabled/) свойства `false` отключение проведение пальцем по экрану
- Переопределить [ `ShouldShowToolbarButton` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MasterDetailPage.ShouldShowToolbarButton()/) метода и возврата `false` для скрытия кнопки панели инструментов в Windows 8.1 и Windows Phone 8.1.

Затем необходимо предоставить средства для переключения между основной страницы и страницы сведений, таких, как показано в предыдущем [ **ColorsDetail** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/ColorsDetails) образца.

[ **MasterDetailTaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MasterDetailTaps) образце показано использование другой подход `TapGestureRecognizer` на страницах master и детализации.

## <a name="tabbedpage"></a>TabbedPage

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) — Это коллекция страниц, которые можно переключать с помощью вкладок. Он является производным от `MultiPage<Page>` и определяет не открытые свойства или методы свои собственные. [`MultiPage<T>`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiPage%3CT%3E/), однако определить свойство:

- [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.Children/) свойство типа `IList<T>`

Заполните эту `Children` коллекции с объектами страницы.

Другой подход позволяет определить `TabbedPage` как дочерние элементы `ListView` с помощью этих двух свойств, которые автоматически создают страницы с вкладками:

- [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemsSource/) типа `IEnumerable`
- [`ItemTemplate`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.ItemTemplate/) типа `DataTemplate`

Однако такой подход не работает на iOS, если коллекция содержит несколько элементов.

`MultiPage<T>` определяет две дополнительные свойства, которые позволяют хранить список страниц просматривается в настоящее время:

- [`CurrentPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.CurrentPage/) Тип `T`, ссылающийся на странице
- [`SelectedItem`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%3CT%3E.SelectedItem/) Тип `Object`, ссылающийся на объект в `ItemsSource` коллекции

`MultiPage<T>` также определяет два события:

- [`PagesChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.PagesChanged/) Когда `ItemsSource` изменения коллекции
- [`CurrentPageChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.MultiPage%3CT%3E.CurrentPageChanged/) При изменении просматриваемой страницы

### <a name="discrete-tab-pages"></a>Дискретные вкладок

[ **DiscreteTabbedColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/DiscreteTabbedColors) Образец состоит из трех страницы с вкладками, отображение цветов тремя разными способами. Каждая вкладка предназначена `ContentPage` производный класс, а затем `TabbedPage` производный класс [DiscreteTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColors/DiscreteTabbedColorsPage.xaml) объединяет три страницы.

Для каждой страницы, который отображается в `TabbedPage`, `Title` свойство необходимо указать текст на вкладке и Apple Store требует также использования значка поэтому `Icon` задано для iOS:

[![Тройной экрана отдельных цветов с вкладками](images/ch25fg13-small.png "TabbedPage")](images/ch25fg13-large.png "TabbedPage")

[ **StudentNotes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/StudentNotes) образец имеет домашней страницы, в которой перечислены все студентов. При касании студент, это приведет к открытию `TabbedPage` производный класс, [ `StudentNotesDataPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/StudentNotes/StudentNotes/StudentNotes/StudentNotesDataPage.xaml), который включает в себя три `ContentPage` объекты его визуального дерева, один из которых позволяет ввести несколько заметок для этого студента.

### <a name="using-an-itemtemplate"></a>С помощью ItemTemplate

[ **MultiTabbedColor** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25/MultiTabbedColors) примере используются [ `NamedColor` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NamedColor.cs) класса в [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотеки. [MultiTabbedColorsPage.xaml](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter25/MultiTabbedColors/MultiTabbedColors/MultiTabbedColors/MultiTabbedColorsPage.xaml) файл задает `DataTemplate` свойство `TabbedPage` в визуальном дереве, начиная с `ContentPage` , содержащая привязки к свойствам `NamedColor` (включая привязку к `Title` свойство).

Однако это создает проблему в iOS. Только некоторые элементы могут быть отображены, и нет хороший способ дать им значков.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст главе 25 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch25-Apr2016.pdf)
- [Образцы главе 25](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter25)
- [Главные и подчиненные страницы](~/xamarin-forms/app-fundamentals/navigation/master-detail-page.md)
- [Страница с вкладками](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
