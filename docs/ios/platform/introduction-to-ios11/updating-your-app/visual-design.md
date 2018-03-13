---
title: "Обновления для визуального проектирования"
description: "Обзор новых возможностей iOS 11"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C300B94-0FAF-492E-A326-877419A1824B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: d66d8cd722aa9a7b6fe27db3f6128ee24309a1de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="visual-design-updates"></a>Обновления для визуального проектирования

_Обзор новых возможностей iOS 11_

В iOS 11 Apple появился новый визуальные изменения, включая обновления для панели навигации, панель поиска и представления таблицы. Кроме того внесены изменения для обеспечения дополнительной гибкости через поля и содержимого на весь экран. В этом руководстве описаны эти изменения.

## <a name="uikit"></a>UIKit

UIKit полосы соответствуют в iOS 11, чтобы сделать их более доступными для конечных пользователей.

Одно из таких изменений является новый дисплей HUD, которое появляется, когда пользователь долго нажатий на панели элементов. Чтобы включить эту возможность, установите `largeContentSizeImage` свойство `UIBarItem` и добавьте более крупного изображения через [каталога активов](~/ios/app-fundamentals/images-icons/displaying-an-image.md):

```csharp
barItem.LargeContentSizeImage = UIImage.FromBundle("AccessibleImage");
```

### <a name="navigation-bar"></a>Панель переходов
iOS 11 появились новые функции для упрощения заголовки панели навигации. Приложения можно отобразить это название большего размера, назначив `PrefersLargeTitles` значение true:

```csharp
NavigationController.NavigationBar.PrefersLargeTitles = true;
```

Делает параметр большего размера заголовков в приложении _все_ заголовки полосы навигации в приложении размер больше, как показано на следующем снимке экрана:

![Заголовок больших навигации](visual-design-images/image7.png)

Для управления при отображении крупным заголовком на панели навигации установите `LargeTitleDisplayMode` элемент навигации для `Always`, `Never`, или `Automatic`.

### <a name="search-controller"></a>Поиск контроллера

iOS 11 внес проще добавить контроллер поиска на панели навигации. После создания контроллера поиска добавить его на панель навигации с `SearchController` свойство:

```csharp
NavigationItem.SearchController = searchController;
```

[![Заголовок больших навигации с панель поиска](visual-design-images/image8-sml.png)](visual-design-images/image8-sml.png#lightbox)

В зависимости от функциональности приложения может или не может потребоваться скрыть, когда пользователь выполняет прокрутку списка панели поиска. Это можно изменить с помощью `HidesSearchBarWhenScrolling` свойство.

## <a name="margins"></a>Поля

Apple создал новое свойство — `directionalLayoutMargins` — который может использоваться для задания пространства между представлениями и представлений. Используйте `directionalLayoutMargins` с `leading` или `trailing` отступы. Независимо от того, система языка слева направо или справа налево интервалы в приложении задается соответствующим образом операций ввода-вывода.

В iOS 10 и перед все представления имеет размер минимальное поле, к которому будут выравниваться. iOS 11 появилась возможность для переопределения, с помощью `ViewRespectsSystemMinimumLayoutMargins`. Например присвоив этому свойству значение false позволяет настраивать вашей edge отступы в нуль:

```csharp
ViewRespectsSystemMinimumLayoutMargins = false;
View.LayoutMargins = UIEdgeInsets.Zero;
```
![Отображение поля рисунка с нуля отступа в ios 11](visual-design-images/image9.png)

<a name="fullscreen" />

## <a name="full-screen-content"></a>Содержимое на весь экран

iOS 7 [появился](~/ios/platform/introduction-to-ios7/ios7-ui.md#fullscreen) `topLayoutGuide` и `bottomLayoutGuide` служит для ограничения представления, чтобы они не скрыты UIKit строки и находятся в видимой области экрана. Они являются устаревшими в iOS 11 для _зарезервированная область_.

Зарезервированная область является новый способ обдумывания видимого пространства и способ добавления ограничений между представлением и super представления приложения. Например рассмотрим следующие изображения:

[![Зарезервированная область vs верхней и нижней направляющей макета](visual-design-images/image10-sml.png)](visual-design-images/image10.png#lightbox)

Ранее, если добавлено представление и хотели, чтобы быть видимым в области зеленый выше, можно будет указать для него для _нижней_ из `TopLayoutGuide` и _верхней_ из `BottomLayoutGuide`. В iOS 11 бы вместо этого для ограничения _верхней_ и _нижней_ безопасном области. См. пример ниже:

```csharp
var safeGuide = View.SafeAreaLayoutGuide;
imageView.TopAnchor.ConstraintEqualTo(safeGuide.TopAnchor).Active = true;
safeGuide.BottomAnchor.ConstraintEqualTo(imageView.BottomAnchor).Active = true;
```

## <a name="table-view"></a>Представление таблицы

UITableView имел ряд небольших, но важных изменений в iOS 11.

По умолчанию заголовки, нижние колонтитулы и ячейки теперь автоматически подгоняются по их содержимому. Чтобы отказаться от этого поведения автоматического изменения размера набора `EstimatedRowHeight`, `EstimatedSectionHeaderHeight`, или `EstimatedSectionFooterHeight` до нуля.

Однако в некоторых случаях (например, при добавлении UITableViewController в конструкторе iOS или при использовании существующих Storboards в интерфейс построителя) необходимо вручную включить самостоятельно изменение размера ячеек. Для этого убедитесь, что вы указали следующие свойства в представлении таблицы для ячеек, верхние и нижние колонтитулы, соответственно:

```csharp
// Cells
TableView.RowHeight = UITableView.AutomaticDimension;
TableView.EstimatedRowHeight = UITableView.AutomaticDimension;

// Header
TableView.SectionHeaderHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionHeaderHeight = 40f;

//Footer
TableView.SectionFooterHeight = UITableView.AutomaticDimension;
TableView.EstimatedSectionFooterHeight = 40f;

```

iOS 11 был расширен функциональные возможности действия строк. `UISwipeActionsConfiguration` была введена для определения набора действий, которые должны иметь место, когда пользователь предъявляет в любом направлении для строки в таблицу. Это поведение аналогично собственного Mail.app. Дополнительные сведения см. в разделе [строки действия](~/ios/user-interface/controls/tables/row-action.md) руководства.

Представления таблиц была добавлена поддержка перетаскивания в iOS 11. Дополнительные сведения см. в разделе [перетаскивание](~/ios/platform/introduction-to-ios11/drag-and-drop.md#uitableview) руководства.


## <a name="related-links"></a>Связанные ссылки

- [Новые возможности iOS 11 (Apple)](https://developer.apple.com/ios/)
- [Страница продукта обновленные App Store (Apple)](https://developer.apple.com/app-store/product-page/)
- [Обновление приложения для iOS 11 (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/204/)
