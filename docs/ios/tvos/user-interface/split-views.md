---
title: Работа с tvOS разбиение Просмотр контроллеров в Xamarin
description: В этом документе описывается работа с tvOS разделять представления в приложении, созданном с помощью Xamarin. Он предоставляет высокоуровневый обзор разбиение Просмотр контроллеров, как использовать их с помощью раскадровки, доступ к представлениям master и сведений и отображение и скрытие главного представления.
ms.prod: xamarin
ms.assetid: 21248CFB-5A94-4C19-B223-C72E0DC5F1D5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 2dd07cd8a4e92d6d39be50ba670441d965ed4d13
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789435"
---
# <a name="working-with-tvos-split-view-controllers-in-xamarin"></a>Работа с tvOS разбиение Просмотр контроллеров в Xamarin

Контроллер представление Split представляется и управляет Master и подробное представление контроллер side-by-side, на экране, в то же время. Просмотр контроллеров разбиение используются для представления содержимого постоянное и может иметь фокус в образец (меньше раздел слева) и связанные сведения в представлении «Подробности» (больше раздел справа).

[![](split-views-images/intro01.png "Разделенное представление образца")](split-views-images/intro01.png#lightbox)

<a name="About-Split-View-Controllers" />

## <a name="about-split-view-controllers"></a>О контроллерах разделение представления

Как уже говорилось выше, контроллер представление Split управляет Master и контроллер представление сведений, предоставляется side-by-side, образце принять меньшее представление с левой стороны экрана подробности большего размера в правой части. 

Кроме того, было скрыто или показано представление главный контроллер может при необходимости: 

[![](split-views-images/intro02.png "Контроллер представление Master скрыты")](split-views-images/intro02.png#lightbox)

Разбиение представления контроллеров часто позволяет предоставить список фильтруемых содержимое категории в представлении главного и отфильтрованные результаты в представлении «Подробности». Это обычно представляется в виде таблицы представления в левой части экрана и [представление коллекции](~/ios/tvos/user-interface/collection-views.md) справа.

При разработке пользовательского интерфейса, необходим контроллер представление Split, Apple предлагает с помощью главного и контроллеров представления сведений, не изменяйте (только изменения содержимого, не структуры). Если требуется для исходящего переключения Просмотр контроллеров, лучше использовать контроллер навигации как базовое представление-контроллер, которую нужно изменить (Master или сведений).

Apple имеет следующие рекомендации по работе с разделением Просмотр контроллеров.

- **Используйте разбиение доли** -по умолчанию View Controller разбиение использует трети экрана для контроллера представления главного и две трети контроллера подробное представление. При необходимости можно использовать 50/50 разбиения. Выберите правильный процент, чтобы сделать ваш содержимого отображаются на экране с балансировкой.
- **Сохранять выделение Main** — при содержимое на подробное представление могут изменить ответ на выбор пользователя в образец, содержимое главного представления должны быть исправлены. Кроме того четкое Отображение выбранного элемента в представлении данных Master.
- **Использовать отдельный заголовок единой** -как правило, имеет смысл использовать единый, выровненный по центру заголовка в представлении «Подробности» вместо заголовка в подробные сведения и образец.

<a name="Split-View-Controllers-and-Storyboards" />

## <a name="split-view-controllers-and-storyboards"></a>Разбиение Просмотр контроллеров и раскадровки

Для работы с разделением Просмотр контроллеров в приложении Xamarin.tvOS проще всего добавить их с помощью iOS конструктор пользовательского интерфейса приложения.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **Pad решения**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **разбиение Просмотр контроллеров** из **элементов** и поместите его в представлении: 

    [![](split-views-images/activity01.png "Контроллер разделение представления")](split-views-images/activity01.png#lightbox)
1. По умолчанию конструктор iOS для установки контроллера навигации и View Controller главного представления. Если это не соответствует требованиям приложения, просто удалите их.
1. При удалении по умолчанию образец перетащите нового контроллера представления на поверхность разработки: 

    [![](split-views-images/activity02.png "View Controller")](split-views-images/activity02.png#lightbox)
1. Удерживая нажатой и перетащите из контроллера представления разбиение новый контроллер главного представления. 
1. Выберите **Master** из **всплывающего меню**: 

    [![](split-views-images/activity03.png "Выберите образец из всплывающего меню")](split-views-images/activity03.png#lightbox)
1. Разработка содержимое главного и подробные представления. 

    [![](split-views-images/activity04.png "Пример макета")](split-views-images/activity04.png#lightbox)
1. Назначьте **имена** в **вкладку графического** из **Pad свойства** для работы с элементами управления пользовательского интерфейса в коде C#.
1. Сохранить изменения и вернуться в Visual Studio для Mac.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования.
1. Перетащите **разбиение Просмотр контроллеров** из **элементов** и поместите его в представлении: 

    [![](split-views-images/activity01-vs.png "Контроллер разделение представления")](split-views-images/activity01-vs.png#lightbox)
1. По умолчанию конструктор iOS добавит навигации контроллеры и представления Master View. Если это не соответствует требованиям приложения, просто удалите их.
1. При удалении по умолчанию образец перетащите нового контроллера представления на поверхность разработки: 

    [![](split-views-images/activity02-vs.png "View Controller")](split-views-images/activity02-vs.png#lightbox)
1. Удерживая нажатой и перетащите из контроллера представления разбиение новый контроллер главного представления. 
1. Выберите **Master** из **всплывающего меню**: 

    [![](split-views-images/activity03-vs.png "Выберите образец из всплывающего меню")](split-views-images/activity03-vs.png#lightbox)
1. Разработка содержимое главного и подробные представления. 

    [![](split-views-images/activity04.png "Макета содержимого")](split-views-images/activity04.png#lightbox)
1. Назначьте **имена** в **вкладку графического** из **свойства обозревателя** для работы с элементами управления пользовательского интерфейса в коде C#.
1. Сохраните изменения.
    
-----

Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md).

<a name="Working-with-Split-View-Controllers" />

## <a name="working-with-split-view-controllers"></a>Работа с разделением Просмотр контроллеров

Как уже говорилось выше, контроллер представление Split часто используется в ситуациях, где отображаются отфильтрованные содержимого для пользователя. Основные категории отображаются в левой части в представлении главного и отфильтрованные результаты справа в представлении «Подробности» в зависимости от выбора пользователя.

<a name="Accessing-Master-and-Detail" />

### <a name="accessing-master-and-detail"></a>Доступ к основной и сведений

Если требуется получить программный доступ к Master и просмотр сведений контроллеров, используйте `ViewControllers ` свойство View Controller разбиения. Пример:

```csharp
// Gain access to master and detail view controllers
var masterController = ViewControllers [0] as MasterViewController;
var detailController = ViewControllers [1] as DetailViewController;
```

Он представляется в виде массива, где первый элемент (0) в контроллере главного представления и второй элемент (1) заключается в степени детализации.

<a name="Accessing-Detail-from-Master" />

### <a name="accessing-detail-from-master"></a>Доступ к детализации из образца

Так как в представлении «Подробности» на основе выбора пользователя в базе данных Master обычно отображаются подробные сведения, вам потребуется способ доступа к подробную информацию из образца.

Для этого проще всего предоставить свойство в классе контроллера главного представления, например:

```csharp
public DetailViewController DetailController { get; set;}
```

В контроллере представление Split, переопределите `ViewDidLoad` метод идентичных два представления и друг с другом. Пример:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Gain access to master and detail view controllers
    var masterController = ViewControllers [0] as MasterViewController;
    var detailController = ViewControllers [1] as DetailViewController;

    // Wire-up views
    masterController.SplitViewController = this;
    masterController.DetailController = detailController;
    detailController.SplitViewController = this;
}
```

На контроллере представления сведений, образце можно использовать для представления новых данных, при необходимости можно предоставлять свойства и методы.

<a name="Showing-and-Hiding-Master" />

### <a name="showing-and-hiding-master"></a>Отображение и скрытие Master

При необходимости можно показать и скрыть контроллере главного представления, используя `PreferredDisplayMode` свойство View Controller разбиения. Пример:

```csharp
// Show hide split view
if (SplitViewController.DisplayMode == UISplitViewControllerDisplayMode.PrimaryHidden) {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.AllVisible;
} else {
    SplitViewController.PreferredDisplayMode = UISplitViewControllerDisplayMode.PrimaryHidden;
}
```

`UISplitViewControllerDisplayMode` Перечисление определяет представления Master View Controller как один из следующих:

- **Автоматическое** -tvOS контролирующую представление основных и подробных представлений.
- **PrimaryHidden** -это скрывает Master View Controller.
- **AllVisible** -отображение главного и контроллеров представления сведений рядом. Это нормально, представление по умолчанию.
- **PrimaryOverlay** -контроллера подробное представление расширяет в группе и связанная с Master.

Чтобы получить текущее состояние представления, используйте `DisplayMode` свойство View Controller разбиения.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован проектирование и работа с разделением Просмотр контроллеров внутри приложения Xamarin.tvOS.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
