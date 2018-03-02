---
title: "Интерактивность ListView"
description: "Добавление интерактивности ListView путем реализации выбора, проведите для удаления и потяните, чтобы обновить."
ms.topic: article
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 74b275b77b6a70b3d074d68b14a97c73615753d2
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="listview-interactivity"></a>Интерактивность ListView

ListView поддерживает взаимодействия с данными, которые он представляет следующими средствами:

- [**Выбор & касания** ](#selectiontaps) &ndash; реагировать на нажатия и выбор/deselections элементов. Включить или отключить выделение строк (по умолчанию включено).
- [**Контекст действия** ](#Context_Actions) &ndash; предоставляют функциональные возможности каждого элемента, например, проведите для удаления.
- [**Потяните, чтобы обновить** ](#Pull_to_Refresh) &ndash; реализовать идиому запросу, чтобы обновить, который пользователи привыкли собственного из среды.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Выбор & касания
`ListView` поддерживает выбор одного элемента за раз. Выбор включен по умолчанию. Когда пользователь касается элемента, два события инициируются: `ItemTapped` и `ItemSelected`. Обратите внимание, что коснувшись тот же элемент дважды не будет срабатывать несколько `ItemSelected` события, но будет срабатывать несколько `ItemTapped` события. Также Обратите внимание, что `ItemSelected` будет вызываться, если элемент выбран.

Имейте в виду, что `ItemSelected` вызывается при элементов отменяется и при их выборе. Это означает, что необходимо для проверки null `SelectedItem` в ваш `ItemSelected` обработчик событий, прежде чем использовать его:

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>Отключение выделения

Если вы хотите отключить выделение, обрабатывать `ItemSelected` событий и набор `SelectedItem` свойство в значение null:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

Выбор включен:

![](interactivity-images/selection-default.png "ListView с поддерживает выбор")

Обратите внимание, что на Windows Phone, некоторые ячейки, включая `SwitchCell` не обновлять их визуальное состояние в ответ на выбор.

<a name="Context_Actions" />

## <a name="context-actions"></a>Контекст действия
Часто пользователи будет нужно предпринять действия на элемент в `ListView`. Например рассмотрим список адресов электронной почты в почтовом приложении. На iOS можно проведите, чтобы удалить сообщение и на Windows Phone можно долго press сообщения и затем удалите его.

![](interactivity-images/context-default.png "ListView с действиями контекста")

Контекст действия можно реализовать в C# и XAML. Далее вы найдете определенных направляющих для обоих форматов, но сначала давайте взглянем на некоторые ключевые особенности реализации для обоих.

Контекст действия создаются с помощью `MenuItem`s. Коснитесь для MenuItems событий с MenuItem, не ListView. Это отличается от обработки событий tap для ячеек, где ListView вызывает события, а не ячейки. Так как ListView, вызывающий событие, его обработчик событий получает сведения о ключе, текущих выбранных или касание элемента.

По умолчанию элемент меню не имеет возможности узнать, какая ячейка, она принадлежит. `CommandParameter` можно найти в `MenuItem` для хранения объектов, например объект, находящийся за ViewCell MenuItem. `CommandParameter` можно задать в XAML и C#.

### <a name="c"></a>C#  

Контекст действия могут быть реализованы в любом `Cell` подкласс (при условии, что она не используется в качестве заголовка) путем создания `MenuItem`s и добавляя их к `ContextActions` коллекции для ячейки. У вас есть следующие свойства можно настроить для контекста действия:

* **Текст** &ndash; строкой, которая отображается в элементе меню.
* **Щелчок** &ndash; события при щелчке данного элемента.
* **IsDestructive** &ndash; (необязательно) Если значение true, этот элемент отображается по-разному в iOS.

Несколько действий в контексте можно добавить в ячейку, однако должны иметь только один `IsDestructive` значение `true`. В следующем коде показано, как следует добавить действия контекста `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s могут также создаваться в коллекции XAML декларативно. Приведенный ниже код XAML демонстрирует пользовательских ячеек с два действия контекста реализации:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
               Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
             <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

В файле кода, обеспечения `Clicked` реализованы методы:

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Для обновления по запросу
Пользователи привыкли, перетащите вниз по списку данных обновится этого списка. `ListView` поддерживает этот out-of--box. Чтобы включить функцию запросу на обновление, задайте `IsPullToRefreshEnabled` значение true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Запрашивает запросу на обновление имени пользователя:

![](interactivity-images/refresh-start.png "Запросите ListView, чтобы обновить в процессе выполнения")

Запросу на обновление имени пользователя был выпущен запросу. Это то, что пользователь видит при обновлении списка: ![ ] (interactivity-images/refresh-in-progress.png "запросу ListView для завершения обновления")

Обратите внимание, что на момент Xamarin.Forms 1.4.3 запросу на обновление не поддерживается в Windows Phone 8.1. В Windows phone 8 запросу, чтобы обновить не возможность собственной платформы, поэтому реализация запросу, чтобы обновить обеспечивается Xamarin.Forms. Помните, что запросу на обновление не будет работать на Windows Phone, если все элементы в списке может поместиться на экране (другими словами, если прокрутка по вертикали не требуется).

ListView предоставляет несколько событий, которые позволяют реагировать на события запросу на обновление.

-  `RefreshCommand` Будет вызываться и `Refreshing` вызывается событие. `IsRefreshing` будет присвоено `true`.
-  Следует выполнять код требуется обновить содержимое списка, либо команду или события.
-  При обновлении завершения, вызовите `EndRefresh` или задать `IsRefreshing` для `false` сообщить представлении-списке завершения.

`CanExecute` Учитывается свойство, которое позволяет способ управления ли должна быть включена команда запросу, чтобы обновить.



## <a name="related-links"></a>Связанные ссылки

- [Интерактивные возможности ListView (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [заметки о выпуске 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [заметки о выпуске 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
