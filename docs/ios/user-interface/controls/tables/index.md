---
title: "Работа с таблицами и ячеек"
description: "Отображение данных с помощью UITableView с Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/06/2016
ms.openlocfilehash: f6abcaa3a771954785df83e80c7e46dd200e6986
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-tables-and-cells"></a>Работа с таблицами и ячеек


В этом разделе представлены классы, используемые для создания и отображения таблиц, а затем приводятся примеры их использования в Xamarin.iOS. Мы рассмотрим, используя внешний вид по умолчанию для таблиц, Настройка макета, реализация редактирования и с помощью Xamarin iOS конструктор для визуального создания таблицы. Иногда отображение очевидно, список строк (например, приложение Music) и в других случаях, которые трудно распознавать управления таблицы (например, изменения в приложении «контакты» или диалога в приложении сообщения).

Для тех, кто занимается кросс платформенных приложений с помощью Xamarin.Android UITableView элемент управления похож на класс ListView в Android (и класс UITableViewSource аналогична классы адаптеров Android).

Эти статьи займет исчерпывающие сведения по работе с таблицами, включая:

-   **Таблицы частей** — введение и объясняющий визуальные элементы `UITableView` элемента управления. 
-   **Отображение данных в таблицах** , демонстрирующий, как создать и заполнить таблицу данными, используйте различные стили таблиц и ячеек и избежать проблем с памятью, повторное использование объектов ячейки. 
-   **Расширенное использование** — построение пользовательские ячейки и с помощью возможности редактирования UITableView класса. 
-   **Создание таблицы в визуально** — с помощью конструктора Xamarin для операций ввода-вывода для создания интерфейса на основе таблицы с раскадровкой. 


## <a name="contents"></a>Описание

 [Таблицы частей &amp; функциональные возможности](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Заполнение таблицы данных](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Настройка внешнего вида таблицы](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Редактирование](~/ios/user-interface/controls/tables/editing.md)
 
 [Строка действия](~/ios/user-interface/controls/tables/row-action.md)

 [Создание таблиц в раскадровку](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Высота строки автоматического изменения размера](~/ios/user-interface/controls/tables/autosizing-row-height.md)


## <a name="related-links"></a>Связанные ссылки

- [WorkingWithTables (пример)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables/)
- [Таблицы в раскадровки (пример)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Общие сведения о раскадровки](~/ios/user-interface/storyboards/index.md)
- [Раскадровка рецепт TableView](https://developer.xamarin.com/recipes/ios/general/storyboard/storyboard_a_tableview)
- [Общие сведения о MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пример TableEditing на Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Пример TableParts на Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Пример TableAndCellStyles на Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Ссылку на класс UITableView](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Ссылку на класс UITableViewCell](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
