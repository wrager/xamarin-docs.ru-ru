---
title: "Пошаговое руководство. Создание приложения с помощью API элементов"
description: "В этой статье содержатся сведения о сведения, представленные в вводной статье MonoTouch диалогового окна. В нем представлено пошаговое руководство, которое показывает, как использовать MonoTouch.Dialog (машинного перевода. D) элементы API, чтобы быстро приступить к созданию приложения с помощью машинного перевода. Г."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 19e1ab4000e473aa773bf75015ff520a1f9a96d8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>Пошаговое руководство. Создание приложения с помощью API элементов

_В этой статье содержатся сведения о сведения, представленные в вводной статье MonoTouch диалогового окна. В нем представлено пошаговое руководство, которое показывает, как использовать MonoTouch.Dialog (машинного перевода. D) элементы API, чтобы быстро приступить к созданию приложения с помощью машинного перевода. Г._

В этом пошаговом руководстве мы будем использовать машинного перевода. D элементы API для создания стиля подробный приложения, откроется список задач. Когда пользователь выбирает <span class="ui"> + </span> кнопки на панели навигации будет добавлена новая строка в таблицу для задачи. При выборе строки откроется экран сведений, который позволяет обновлять описание задачи и срок, как показано ниже:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Выбрав строку, можно перейти на экран сведений, позволяющий обновлять описание задачи и срок")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>Пошаговое руководство элементы API

В [введение в диалоговом окне MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) статьи, мы получить основательные знания различных частей машинного перевода. Г. Чтобы собрать все вместе в приложение, воспользуемся элементы API.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>Настройка нескольких экранов приложения

Чтобы начать процесс создания экрана, создает MonoTouch.Dialog `DialogViewController`, а затем добавляет `RootElement`.

Чтобы создать приложение нескольких экранов с MonoTouch.Dialog, необходимо:

1.  Создание  `UINavigationController.`
1.  Создание  `DialogViewController.`
1.  Добавить `DialogViewController` как корень  `UINavigationController.` 
1.  Добавить `RootElement` для  `DialogViewController.`
1.  Добавить `Sections` и `Elements` для  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>С помощью UINavigationController

Создание стиль навигации приложения, необходимо ли создавать `UINavigationController`, а затем добавьте ее в качестве `RootViewController` в `FinishedLaunching` метод `AppDelegate`. Чтобы сделать `UINavigationController` работать с MonoTouch.Dialog, мы добавим `DialogViewController` для `UINavigationController` как показано ниже:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Приведенный выше код создает экземпляр `RootElement` и передает его в `DialogViewController`. `DialogViewController` Всегда имеет `RootElement` в верхней части иерархии. В этом примере `RootElement` создается со строкой «Список дел, «который служит в качестве заголовка контроллера навигации панели навигации. На этом этапе выполнение приложения должны выводиться на экран, показанный ниже:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Запуск приложения появится экран, показанный здесь")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Давайте посмотрим, как использовать MonoTouch.Dialog в иерархическую структуру `Sections` и `Elements` для добавления нескольких экранов.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>Создание экранов диалоговое окно

Объект `DialogViewController` — `UITableViewController` подкласс, в котором MonoTouch.Dialog использует порядок добавления экранов. MonoTouch.Dialog создает экраны, добавляя `RootElement` для `DialogViewController`, как было показано выше. `RootElement` Может иметь `Section` экземпляры, представляющие части таблицы.
Разделы состоят из элементов, другие разделы, или даже другими `RootElements`. Путем вложения `RootElements`, MonoTouch.Dialog автоматически создает приложение стиль навигации, как будет показано далее.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>С помощью DialogViewController

`DialogViewController`, Выполняется `UITableViewController` имеет подкласса, `UITableView` его представления. В этом примере мы хотим добавить элементы в таблицу при каждом <span class="ui"> + </span> касании кнопки. Поскольку `DialogViewController` был добавлен в `UINavigationController`, мы можем использовать `NavigationItem` `RightBarButton` свойство для добавления <span class="ui"> + </span> кнопки, как показано ниже:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Когда мы создали `RootElement` ранее, мы передать ему один `Section` , чтобы мы добавим элементы в виде <span class="ui"> + </span> кнопки, которые используются пользователем. Для выполнения в этом случае обработчик для кнопки, можно использовать следующий код:

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Этот код создает новый `Task` объект каждый раз при касании кнопки. Ниже приведен простой реализации `Task` класса:

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

 []()

Задача `Name` свойство используется для создания `RootElement`элемента заголовка вместе с переменную счетчика с именем `n` , увеличивается для каждой новой задачи. MonoTouch.Dialog включает элементы в строки, которые добавляются `TableView` при каждом `taskElement` добавляется.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Представления и управление экраны диалоговое окно

Мы использовали `RootElement` , чтобы MonoTouch.Dialog будет автоматически создавать экрана сведения о каждой задаче и перейти к нему, если выбрана строка.

Сам экран сведений задач состоит из двух частей; Каждый из них содержит один элемент. Первый элемент создается на основе `EntryElement` для обеспечения задачи от редактируемой строки `Description` свойство. При выборе элемента клавиатуру для текста при правке представляется, как показано ниже:

 [![](elements-api-walkthrough-images/03-create-task.png "При выборе элемента клавиатуру для текста при правке представляется, как показано")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Второго раздела посвящены `DateElement` позволяют управлять задачи `DueDate` свойство. Выберите в нем дату автоматически загружает элемент выбора даты, как показано:

 [![](elements-api-walkthrough-images/04-date-picker.png "Выберите в нем дату автоматически загружает элемент выбора даты как")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

В обоих `EntryElement` и `DateElement` вариантов (или для любого элемента ввода данных в MonoTouch.Dialog), любые изменения значения сохраняются автоматически. Мы продемонстрировать это, изменив дату и затем переходы назад и вперед между экрана корневой и различные сведения о задаче, где сохранены значения на экранах детализации.

 <a name="Summary" />


## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство, которое было показано, как использовать API-Интерфейс MonoTouch.Dialog элементов. Он описаны основные шаги для создания приложения на нескольких экранов с машинного перевода. D, включая способы использования `DialogViewController` и добавление элементов и разделы для создания экранов. Кроме того оно было показано, как использовать машинного перевода. D в сочетании с `UINavigationController`.


## <a name="related-links"></a>Связанные ссылки

- [MTDWalkthrough (пример)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Демонстрация - Icaza de Мигель создает экран входа iOS с MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Демонстрация - позволяет быстро создавать пользовательские интерфейсы iOS с MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Общие сведения о MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошагового руководства по API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Пошаговое руководство элемент JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Диалоговое окно MonoTouch на Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation приложения](https://github.com/migueldeicaza/TweetStation)
- [Ссылку на класс UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Ссылку на класс UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
