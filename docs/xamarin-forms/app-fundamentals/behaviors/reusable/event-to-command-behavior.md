---
title: Для повторного использования EventToCommandBehavior
description: Поведения можно использовать для связи команд с помощью элементов управления, которые не были предназначены для взаимодействия с командами. В этой статье показано, с помощью Xamarin.Forms поведения для вызова команды при запуске события.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: e89400c74c3d1afbf8954d0f88387c5967ebd534
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848237"
---
# <a name="reusable-eventtocommandbehavior"></a>Для повторного использования EventToCommandBehavior

_Поведения можно использовать для связи команд с помощью элементов управления, которые не были предназначены для взаимодействия с командами. В этой статье показано, с помощью Xamarin.Forms поведения для вызова команды при запуске события._

## <a name="overview"></a>Обзор

`EventToCommandBehavior` Класс является многократно используемых пользовательское поведение Xamarin.Forms, которое выполняет команду в ответ на *любой* событием. По умолчанию аргументы события для события будут передаваться в команду, а при необходимости может быть преобразован с [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) реализации.

Чтобы использовать поведение должно быть задано поведение следующие свойства:

- **EventName** — имя события поведение прослушивает.
- **Команда** – **ICommand** для выполнения. Поведение ожидает найти `ICommand` экземпляра [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) вложенного управления, который может быть унаследован от родительского элемента.

Также можно задать следующие свойства необязательным поведением.

- **CommandParameter** — `object` будут передаваться в команду.
- **Преобразователь** — [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) реализацию, которая будет изменить формат данных аргумента события, передаваемый между *источника* и *целевой*модулем привязки.

## <a name="creating-the-behavior"></a>Создание поведения

`EventToCommandBehavior` Класс является производным от `BehaviorBase<T>` класс, который в свою очередь является производным от [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) класса. Назначение `BehaviorBase<T>` класса является предоставление базового класса для любого Xamarin.Forms поведений, которые требуют [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) поведения, задаваемое для вложенного элемента управления. Это гарантирует, что поведение можно привязать к и выполнить `ICommand` заданные `Command` свойства, если используются в поведение.

`BehaviorBase<T>` Класс предоставляет overridable [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) метод, который задает [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) поведение и переопределяемым [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)метод, который выполняет очистку `BindingContext`. Кроме того, класс хранит ссылку на вложенный элемент управления в `AssociatedObject` свойство.

### <a name="implementing-bindable-properties"></a>Реализация свойства для привязки

`EventToCommandBehavior` Класс определяет четыре [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляров, выполняемых пользователем определенные команды при запуске события. В следующем примере кода показаны следующие свойства:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

При `EventToCommandBehavior` потребляются класс `Command` свойство должно быть данными, привязанными к `ICommand` должно выполняться в ответ на события Click, который определен в `EventName` свойство. Поведение ожидает найти `ICommand` на [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) вложенного элемента управления.

По умолчанию аргументы событий для события передаются в команду. Эти данные можно при необходимости преобразовать при передаче между *источника* и *целевой* модулем привязки, указывая [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) реализации как `Converter` значение свойства. Кроме того, параметр может быть передан команду, указав `CommandParameter` значение свойства.

### <a name="implementing-the-overrides"></a>Реализации переопределений

`EventToCommandBehavior` Класса переопределения [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) и [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) методы `BehaviorBase<T>` класса, как показано в следующем примере кода:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Метод выполняет программу установки с помощью вызова `RegisterEvent` метод, передавая значение `EventName` свойство в качестве параметра. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Метод выполняет очистку, вызвав `DeregisterEvent` метод, передавая значение `EventName` свойство в качестве параметра.

### <a name="implementing-the-behavior-functionality"></a>Реализация функциональных возможностей поведения

Поведение предназначено для выполнения команды, определяемые `Command` свойства в ответ на события Click, который определяется `EventName` свойство. В следующем примере кода показано поведение основные функциональные возможности:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }        

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent` Метод выполняется в ответ на `EventToCommandBehavior` , присоединяемый к элементу управления и он получает значение `EventName` свойство в качестве параметра. Метод пытается найти события, определенного в `EventName` свойство вложенного элемента управления. Условии, что событие может быть найден, `OnEvent` метод зарегистрирован методом обработчика для события.

`OnEvent` Метод выполняется в ответ на события Click, который определен в `EventName` свойство. При условии, что `Command` свойство ссылается на допустимый `ICommand`, метод пытается получить параметр для передачи `ICommand` следующим образом:

- Если `CommandParameter` свойство определяет параметр, они будут получены.
- В противном случае, если `Converter` свойство определяет [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) реализация преобразователя выполняется и преобразует данные аргумент события, передаваемый между *источника* и *целевой* модулем привязки.
- В противном случае аргументы события считаются параметра.

Данные, привязанные `ICommand` выполняется, передавая параметр в команду, при условии, что [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) возвращает `true`.

Несмотря на то, что не показано здесь, `EventToCommandBehavior` также включает `DeregisterEvent` метод, который выполняется путем [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) метод. `DeregisterEvent` Метод используется для поиска и удалить регистрацию события, определенного в `EventName` свойство, которое возникает утечка памяти потенциальных очистки.

## <a name="consuming-the-behavior"></a>Использование поведения

`EventToCommandBehavior` Класса можно подключить к [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) коллекцию элементов управления, как показано в следующем примере кода XAML:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Эквивалентный код на языке C# показан в следующем примере:

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command` Свойство поведения является данными, привязанными к `OutputAgeCommand` свойства связанного ViewModel при `Converter` свойству `SelectedItemConverter` экземпляр, который возвращает [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/)из [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) из [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

Во время выполнения поведение будет отвечать на действия с элементом управления. При выборе элемента в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) активизируется событие, в которой будет выполнен `OutputAgeCommand` в ViewModel. В свою очередь, при этом обновляются ViewModel `SelectedItemText` свойство, [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) привязывается к, как показано на следующем снимке экрана:

[![](event-to-command-behavior-images/screenshots-sml.png "Образец приложения с EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "образец приложения с EventToCommandBehavior")

Преимущество использования этого поведения для выполнения команды при запуске события, является, команды могут быть связаны с элементами управления, которые не были предназначены для взаимодействия с командами. Кроме того код обработки события пластина шаблон удаляется из файлов кода.

## <a name="summary"></a>Сводка

В этой статье показано, с помощью Xamarin.Forms поведения для вызова команды при запуске события. Поведения можно использовать для связи команд с помощью элементов управления, которые не были предназначены для взаимодействия с командами.


## <a name="related-links"></a>Связанные ссылки

- [Поведение EventToCommand (пример)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Поведение](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Поведение<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
