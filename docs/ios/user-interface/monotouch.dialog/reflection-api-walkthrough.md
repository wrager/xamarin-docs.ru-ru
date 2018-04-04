---
title: 'Пошаговое руководство: Создание приложения с помощью API отражения'
description: Помимо элементы API, MonoTouch.Dialog (машинного перевода. D) также включает API отражения на основе атрибутов. API отражения делает создание экранов с машинного перевода. D так же легко, как декорирования классов с атрибутами. В этой статье подробно рассматриваются для создания приложения с помощью API-интерфейса отражения.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e56eaeccb2e09d9f1ad84245bf41e2a4bf1b56f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>Пошаговое руководство: Создание приложения с помощью API отражения

_Помимо элементы API, MonoTouch.Dialog (машинного перевода. D) также включает API отражения на основе атрибутов. API отражения делает создание экранов с машинного перевода. D так же легко, как декорирования классов с атрибутами. В этой статье подробно рассматриваются для создания приложения с помощью API-интерфейса отражения._


Машинного перевода. API отражения D позволяет классам быть снабжен атрибутом атрибуты этого машинного перевода. D использует для создания экранов, автоматически. API-интерфейса отражения предоставляет привязку между этими классами и сведения, отображаемые на экране. Несмотря на то, что этот API не обеспечивают точное управление, выполняющий элементы API, он уменьшает сложность, автоматически построении элемент иерархию, основанную на классе оформления.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Приступая к работе с API-интерфейса отражения

С помощью API отражения прост:

1.  Создание класса, декорированных машинного перевода. Атрибуты D.
1.  Создание `BindingContext` экземпляра, передавая ему экземпляр класса выше. 
1.  Создание `DialogViewController` , передавая ему `BindingContext’s` `RootElement` . 


Давайте рассмотрим пример, чтобы показать, как использовать API отражения. В этом примере мы создадим экрана ввода простых данных, как показано ниже:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "В этом примере мы создадим экрана ввода данных в простой как показано ниже")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>Создание класса с помощью машинного перевода. Атрибуты D

Первое, что необходимо использовать API отражения — это класс, атрибуты. Эти атрибуты будут использоваться машинного перевода. D внутренним образом, чтобы создать объекты из элементов API. Например рассмотрим следующее определение класса:

```csharp
public class Expense
{
        [Section("Expense Entry")]

        [Entry("Enter expense name")]
        public string Name;
        
        [Section("Expense Details")]
  
        [Caption("Description")]
        [Entry]
        public string Details;
        
        [Checkbox]
        public bool IsApproved = true;
}
```

`SectionAttribute` Приведет к разделам `UITableView` создается с строковый аргумент, используемый для заполнения заголовок раздела. После объявления раздела каждого поля, следующий за ним будут включены в этот раздел пока объявлен другой раздел.
Тип элемента пользовательского интерфейса, созданные для поля будут зависеть от типа поля и машинного перевода. Атрибут D декорирования его.

Например `Name` поле является `string` и его снабжен `EntryAttribute`. Это приводит к строки, добавляемой в таблицу с полем ввода текста и с указанным заголовком. Аналогичным образом `IsApproved` поле является `bool` с `CheckboxAttribute`, полученный в строке таблицы с флажком в правой части ячейки таблицы. МАШИННОГО ПЕРЕВОДА. D использует имя поля автоматически добавит пробел, для создания заголовка таким образом, так как он не указан в атрибуте.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Добавление контекста BindingContext

Для использования `Expense` класса, необходимо создать `BindingContext`. Объект `BindingContext` является классом, который будет привязать данные из класса с атрибутом, чтобы создать иерархию элементов. Чтобы сделать это, мы просто создать его экземпляр и передать конструктору экземпляра класса с атрибутом.

Например, чтобы добавить пользовательский Интерфейс, который мы объявлено с помощью атрибута в `Expense` класса, поместите приведенный ниже код в `FinishedLaunching` метод `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Все что нужно сделать для создания пользовательского интерфейса является добавление `BindingContext` для `DialogViewController` и задайте его как `RootViewController` окна, как показано ниже:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

Запуск приложения теперь результатов на экране, показанном выше отображаемых.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Добавление UINavigationController

Однако обратите внимание что заголовок «Создание задачи», которые мы переданы `BindingContext` не отображается. Это вызвано `DialogViewController` — не является частью `UINavigatonController`. Изменим код, чтобы добавить `UINavigationController` как окно `RootViewController,` и добавьте `DialogViewController` корнем `UINavigationController` как показано ниже:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Теперь при запуске приложения, это название появится в `UINavigationController’s` панель навигации как на снимке экрана ниже показано:

 [![](reflection-api-walkthrough-images/02-create-task.png "Теперь при запуске приложения, это название появится в панели навигации UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Включив `UINavigationController`, мы теперь могут воспользоваться преимуществами других функций машинного перевода. D, для которого необходим навигации. Например, можно добавить перечисление `Expense` класса для определения категории расходов и машинного перевода. D автоматически создаст экран выбора. Чтобы продемонстрировать, измените `Expense` класса для включения `ExpenseCategory` поле следующим образом:

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

Запуск приложения теперь приводит к новой строки в таблицу для категории, как показано:

 [![](reflection-api-walkthrough-images/03-set-details.png "Запуск приложения теперь приводит к новой строки в таблицу для категории, как показано")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

При выборе строки приводит приложение перемещение на новый экран с помощью строки, соответствующие перечисления, как показано ниже:

 [![](reflection-api-walkthrough-images/04-set-category.png "При выборе строки приводит к приложения перемещение на новый экран с помощью строки, соответствующие перечисления")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство по API отражения. Мы показали, как добавлять атрибуты к классу, для управления, отображаемых. Мы также рассматриваются способы использования `BindingContext` привязывать данные из класса в иерархии элементов, который создается, а также способы использования машинного перевода. D с `UINavigationController`.


## <a name="related-links"></a>Связанные ссылки

- [MTDReflectionWalkthrough (пример)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Демонстрация - Icaza de Мигель создает экран входа iOS с MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Демонстрация - позволяет быстро создавать пользовательские интерфейсы iOS с MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Общие сведения о диалоговом окне MonoTouch](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство элементы API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство элемент JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно MonoTouch на Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation приложения](https://github.com/migueldeicaza/TweetStation)
- [Ссылку на класс UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Ссылку на класс UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
