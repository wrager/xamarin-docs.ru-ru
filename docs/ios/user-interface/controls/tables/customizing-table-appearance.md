---
title: Настройка внешнего вида таблицы
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a447c59e7384ce7da168efdd018bc23c2abb25c2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-tables-appearance"></a>Настройка внешнего вида таблицы

Чтобы изменить внешний вид таблицы проще всего использовать стиль другую ячейку. Можно изменить стиль ячейки, который используется при создании каждой ячейки в `UITableViewSource` `GetCell` метод.

## <a name="cell-styles"></a>Стили ячеек

Существует четыре встроенных стилей:

-  **По умолчанию** — поддерживает `UIImageView`.
-  **Подзаголовок** — поддерживает `UIImageView` и подзаголовок.
-  **Значение1** — справа выровненных подзаголовок поддерживает `UIImageView`.
-  **Value2** — заголовок — по правому краю и подзаголовок по левому краю (но не изображения).


Эти снимки экрана показывают, как выглядит каждого стиля:

 [![](customizing-table-appearance-images/image7.png "Эти снимки экрана Показать, как выглядит каждого стиля")](customizing-table-appearance-images/image7.png#lightbox)

Образец **CellDefaultTable** содержит код для создания этих экранов. Стиль ячейки задается в `UITableViewCell` конструктор следующим образом:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Поддерживаемые свойства](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/) ячейки стиль можно затем задать:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Стандартные

Ячейки можно имеют следующие стандартные, добавляемый в правой части представления:

-   **Флажок** — может использоваться для указания множественным выбором в таблице.
-   **DetailButton** — отвечать на touch независимо от остальной части ячейки, что позволяет выполнять другую функцию для касаясь ячейки самого (таких как Открытие контекстного меню или новое окно, которое не является частью `UINavigationController` стека).
-   **DisclosureIndicator** — обычно используется, чтобы указать, что касается ячейки будет открыть другое представление.
-   **DetailDisclosureButton** — сочетание `DetailButton` и `DisclosureIndicator`.


Вот, как они выглядят.

 [![](customizing-table-appearance-images/image8.png "Стандартные образца")](customizing-table-appearance-images/image8.png#lightbox)

Для отображения одного из этих компонентов, можно задать `Accessory` свойства `GetCell` метод:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

Когда `DetailButton` или `DetailDisclosureButton` отображается, необходимо также переопределить `AccessoryButtonTapped` для выполнения некоторых операций, когда он обрабатывается.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

Образец **CellAccessoryTable** показан пример использования стандартные.

## <a name="cell-separators"></a>Ячейки в качестве разделителей

Разделители ячейки, ячейки таблицы, используемый для разделения таблицы. Свойства задаются в таблице.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

Можно также добавить эффект размытия или естественность разделителя:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

Разделитель также может иметь отступ:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>Создание макетов пользовательских ячеек

Изменение стиля таблицы необходимо предоставить пользовательские ячейки для отображения. Пользовательские ячейки могут иметь разные макеты цветов и управления.

В примере CellCustomTable реализуется `UITableViewCell` подкласс, в котором определяется пользовательский макет `UILabel`s и `UIImage` различные шрифты и цвета. Полученный ячеек будет выглядеть следующим образом:

 [![](customizing-table-appearance-images/image9.png "Макеты пользовательских ячеек")](customizing-table-appearance-images/image9.png#lightbox)

Пользовательский класс ячейки с состоит только из трех методов:

-   **Конструктор** — создает элементы управления пользовательского интерфейса и устанавливает свойства пользовательский стиль (например) начертание шрифта, размера и цвета).
-   **UpdateCell** — это метод `UITableView.GetCell` для использования при задании свойства ячейки.
-   **LayoutSubviews** — задать расположение элементов управления пользовательского интерфейса. В примере каждая ячейка имеет тот же макет, но более сложные ячейки (особенно в системах с различных размеров) может потребоваться позиций другой макет в зависимости от отображаемого содержимого.


Полный образец кода в **CellCustomTable > CustomVegeCell.cs** ниже:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell` Метод `UITableViewSource` должно быть изменено для создания пользовательских ячеек:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>Связанные ссылки

- [WorkingWithTables (пример)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
