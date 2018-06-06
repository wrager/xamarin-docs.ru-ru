---
title: Перетаскивание в Xamarin.iOS
description: В этом документе описывается реализовать перетащите и поместите в Xamarin.iOS приложениях, использующих API-интерфейсы, представленные в iOS 11. В частности, в нем описывается включение путем перетаскивания в UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: 7c41f96dae88047e64ec1e74838e3efab55958cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786968"
---
# <a name="drag-and-drop-in-xamarinios"></a>Перетаскивание в Xamarin.iOS

_Реализация операции перетаскивания для iOS 11_

Перетащите включает iOS 11 поддержки для копирования данных между приложениями на устройствах iPad перетаскивания. Пользователи могут выбрать и перетащите все типы содержимого из приложений находится рядом или перетащив значке приложения, которое запускает приложение для открытия и разрешить передачу данных для удаления:

![Перетаскивание пример из пользовательского приложения в приложение заметок](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Перетаскивание доступен только в одном приложении на iPhone.

Рассмотреть возможность поддержки перетаскивания операций перетаскивания в любом создания или изменения содержимого:

- Текстовые элементы управления поддерживают операции перетаскивания для всех приложений, созданных для iOS 11, не требуется никаких дополнительных действий.
- Таблицы и представления коллекции включен усовершенствований в iOS 11, упростить добавление перетащите и поместите поведение.
- Любое другое представление, можно сделать для поддержки перетаскивания и drop дополнительной настройки.

При добавлении операции перетаскивания поддерживать в приложения, можно предоставить различные уровни качества содержимого; Например можно предоставить форматированный текст и текстовая версия данных, чтобы принимающее приложение можно выбрать, который подходит лучше всего в цель перетаскивания. Можно также настраивать визуализации перетаскивания, а также включить перетаскивание нескольких элементов за один раз.

## <a name="drag-and-drop-with-text-controls"></a>Перетаскивание с текстовых элементов управления

`UITextView` и `UITextField` автоматически поддерживают выделенного текста out, перетаскивание текста содержимого в.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Перетаскивание с UITableView

`UITableView` предусмотрена встроенная обработка перетаскивание взаимодействий со строками таблицы, требуется лишь несколько методов, чтобы включить поведение по умолчанию.

Состоит из двух интерфейсов:

- `IUITableViewDragDelegate` — Сведения о пакетах при инициации перетаскивания в табличном представлении.
- `IUITableViewDropDelegate` — Обрабатывает сведения, когда выполняется удаление попытка и завершения.

В [пример DragAndDropTableView](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) эти два интерфейса реализуются на `UITableViewController` класса вместе с делегатом и источник данных. Они присвоены `ViewDidLoad` метод:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Далее приведено минимальный требуемый код для этих интерфейсов.

### <a name="table-view-drag-delegate"></a>Делегат перетащите представление таблицы

Это единственный метод _необходимые_ для поддержки перетаскивания строки из таблицы представления `GetItemsForBeginningDragSession`. Если пользователь начинает перетаскивать строки, этот метод будет вызываться.

Реализация показан ниже. Извлекает данные, связанные с перетаскиваемого строки, кодирует его и настраивает `NSItemProvider` определяет, как приложения будет обрабатывать «drop» часть операции (например, является ли они могут обрабатывать тип данных `PlainText`, в примере):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Существует множество методов необязательно перетащите делегата, который можно использовать для настройки поведения перетаскивания, например, несколько представлений данных, которые могут быть предприняты преимуществами целевого приложения (такие как форматированный текст как контейнер в виде обычного текста или вектор и Битовая карта версии рисунок). Можно также предоставить пользовательские данные представления для использования при перетаскивании в пределах одного приложения.

### <a name="table-view-drop-delegate"></a>Делегат Drop представление таблицы

Эти методы в делегате drop вызывается при операции перетаскивания через представление таблицы, или выполнении над ним. Требуемые методы определяют, разрешено ли данные для удаления и действия, предпринимаемые при завершения:

- `CanHandleDropSession` — Во время перетаскивания в ходе выполнения и потенциально, вставляемыми в приложение, этот метод определяет, разрешено ли перетаскиваемые данные для удаления.
- `DropSessionDidUpdate` — Перетаскивание во время выполнения, этот метод вызывается, чтобы определить, какое действие предназначено. Сведения из таблицы представления, перетаскиваемый над, перетащите сеанса и путь возможно индекс можно использовать для определения поведения и представляются пользователю визуальную обратную связь.
- `PerformDrop` — Когда пользователь завершает раскрывающегося (снимая пальцев), этот метод извлекает перетаскиваемые данные и изменяет представление таблицы для добавления данных в новую строку (или строк).

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Указывает, может ли представление таблицы принимать перетаскиваемых данных. В этом фрагменте кода `CanLoadObjects` используется для подтверждения, что эта таблица представление может принимать строковые данные.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` Метод вызывается многократно, операции перетаскивания во время выполнения, для предоставления пользователю визуальные подсказки.

В следующем коде `HasActiveDrag` используется для определения ли операция была создана в виде таблицы. В этом случае перемещаемого разрешены только одной строки.
Если перетаскивание из другого источника, операция копирования будет указано:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Операция drop может быть одним из `Cancel`, `Move`, или `Copy`.

Цель перетаскивания можно вставить новую строку, или добавьте/Добавление данных в существующую строку.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` Метод вызывается, когда пользователь завершает операцию и изменяет представление и данные источника таблицы в соответствии с перетаскиваемых данных.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Можно добавить дополнительный код для асинхронной загрузки больших объемов данных объектов.

### <a name="testing-drag-and-drop"></a>Тестирование операции перетаскивания

Необходимо использовать для тестирования iPad [пример](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Откройте образец наряду с другого приложения (например, см. примечания) и перетащите строк и текст между ними:

![Снимок экрана: выполняется операция перетаскивания](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Связанные ссылки

- [Перетаскивание рекомендации пользовательского интерфейса (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Перетаскивание View, образец таблицы](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Перетаскивание коллекции просмотр результата](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Общие сведения о перетаскивания и Drop (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Перетаскивание с коллекцией и представления таблицы (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/223/)
