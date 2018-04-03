---
title: ListView
description: Представления данных в привлекательных и интерактивных списков.
ms.topic: article
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: 494c69700ed0b12b4c9151b9a1b04ea091ebfa57
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="listview"></a>ListView

ListView — это представление для представления списков данных, особенно длинных списках, требующих прокрутки. В этом руководстве будет показано, как использовать ListView.

1. **[Источники данных](data-and-databinding.md)**  &ndash; заполнения ListView с данными или без привязки данных.
2. **[Внешний вид ячеек](customizing-cell-appearance.md)**  &ndash; настройки внешнего вида ячеек встроенные или создать собственные пользовательские ячейки.
3. **[Внешний вид списка](customizing-list-appearance.md)**  &ndash; настраивать внешний вид элемента управления ListView. Задавать верхние и нижние колонтитулы, включите группы и изменять высоту строк.
4. **[Интерактивные возможности](interactivity.md)**  &ndash; обрабатывать касания и выбранные, реализации запросу на обновление и добавление контекстных команд.
5. **[Производительность](performance.md)**  &ndash; избежать проблем с производительностью.

## <a name="use-cases"></a>Варианты использования
Убедитесь, что ListView находится справа управления вашим потребностям. ListView может использоваться в любой ситуации, где отображаются прокручиваемый списки данных. Представлениях ListView поддерживает контекст действия и привязки данных.

Не следует путать с ListView [TableView](~/xamarin-forms/user-interface/tableview.md). Элемент управления TableView является лучшим вариантом при наличии непривязанных список параметров или данных. Например параметры приложения iOS, которые главным образом предопределенный набор параметров, лучше подходит для использования TableView чем ListView.

Также Обратите внимание, что ListView лучше подходит для однородные данные &ndash; то есть все данные должны быть одного типа. Это так, как можно использовать только один тип ячейки для каждой строки в списке. TableViews может поддерживать несколько типов ячейки, так, чтобы они лучше, когда требуется использовать разные представления.


## <a name="components"></a>Компоненты
ListView имеет ряд компонентов, доступных для использования собственной функциональности каждой платформы. Каждый из этих компонентов описан ниже.

- **[Верхние и нижние колонтитулы](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; текста или представления для отображения в начале и в конце списка, отделить от данных в списке. Верхние и нижние колонтитулы можно привязать к источнику данных независимо от источника данных ListView.
- **[Группы](customizing-list-appearance.md#Grouping)**  &ndash; можно группировать данные в ListView для упрощения навигации. Группы обычно являются с привязкой к данным:

![](images/grouping-depth.png "ListView с сгруппированных данных")

- **[Ячейки](customizing-cell-appearance.md)**  &ndash; данные в ListView представлены в ячейках. Каждой ячейке соответствует строки данных. Встроенные ячейки для выбора, или можно определить собственные настраиваемые ячейки. Встроенные и пользовательские ячейки могут быть использовать или определен в XAML или коде.
  - **[Встроенные](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; построен в ячейках, особенно TextCell и ImageCell, не может быть отлично подходит для производительности, поскольку они соответствуют собственных элементов управления для каждой платформы.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; отображается строка текста, при необходимости текстом детализации. Текст сведений отображается в виде вторую строку меньший размер шрифта и Цвет диакритических знаков.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; отображает изображение с текстом. Отображается в виде TextCell с изображением в левой части экрана.
  - **[Пользовательские ячейки](customizing-cell-appearance.md#customcells)**  &ndash; ячеек настраиваемый удобны для представления сложных данных. Например пользовательское представление может использоваться для представления списка песен, включая альбома, а исполнителя:

![](images/image-cell-default.png "ListView с ImageCells")

Дополнительные сведения о настройке ячеек в ListView см. в разделе [Настройка внешнего вида ячеек в ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Функция
ListView поддерживает некоторые стили взаимодействия, включая:

- **[Потяните, чтобы обновить](interactivity.md#Pull_to_Refresh)**  &ndash; ListView поддерживает запросу на обновление для каждой платформы.
- **[Контекст действия](interactivity.md#Context_Actions)**  &ndash; ListView поддерживает принятия мер для отдельных элементов в списке. Например можно реализовать считывание к действию на iOS, или долго касание действия на Android и Windows Phone.
- **[Выбор](interactivity.md#selectiontaps)**  &ndash; может прослушивать выбора и deselections действовать при касании строки.

![](images/context-default.png "ListView с действиями контекста")

Дополнительные сведения о функциях интерактивные возможности элемента управления ListView см. в разделе [действия и взаимодействия с ListView](interactivity.md).


## <a name="related-links"></a>Связанные ссылки

- [Работа с ListView (пример)](https://developer.xamarin.com/samples/WorkingWithListview)
- [Двусторонней привязки (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [Встроенные в ячейках (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [Пользовательские ячейки (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [Группирование (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Пользовательское представление модуля подготовки отчетов (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [Интерактивные возможности ListView (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS книги](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android книги](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
