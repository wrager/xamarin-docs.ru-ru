---
title: "Пошаговое руководство: Использование элемента JSON для создания пользовательского интерфейса"
description: "MonoTouch.Dialog (машинного перевода. D) включает поддержку динамическое создание пользовательского интерфейса через данных JSON. В этом учебнике мы рассмотрим способы использования JSONElement для создания пользовательского интерфейса из JSON, входящий в состав приложения, либо загрузить из URL-адрес удаленного."
ms.topic: article
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 63faa0c46abfb509a6834efa647f23ad0ed7f454
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough-using-a-json-element-to-create-a-user-interface"></a>Пошаговое руководство: Использование элемента JSON для создания пользовательского интерфейса

_MonoTouch.Dialog (машинного перевода. D) включает поддержку динамическое создание пользовательского интерфейса через данных JSON. В этом учебнике мы рассмотрим способы использования JSONElement для создания пользовательского интерфейса из JSON, входящий в состав приложения, либо загрузить из URL-адрес удаленного._


МАШИННОГО ПЕРЕВОДА. D поддерживает создание пользовательских интерфейсов, объявленных в JSON. При объявлении элементов с помощью JSON, машинного перевода. D будет автоматически создавать связанные элементы для вас. JSON можно загрузить из локального файла, проанализированный `JsonObject` экземпляра или даже удаленный URL-адрес.

МАШИННОГО ПЕРЕВОДА. D поддерживает полный спектр функций, доступных в API элементов при использовании JSON. Например на следующем снимке экрана приложение полностью объявляется с помощью JSON:

[ ![](json-element-walkthrough-images/01-load-from-file.png "Например, приложения на этом снимке экрана полностью объявляется с помощью JSON") ](json-element-walkthrough-images/01-load-from-file.png) [ ![ ] (json-element-walkthrough-images/02-load-from-file-details.png ", например приложения на этом снимке экрана полностью объявляется с помощью JSON")](json-element-walkthrough-images/02-load-from-file-details.png)

Давайте повторное использование примера в [пошагового руководства по API элементы](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) руководство добавить экран сведений задач с помощью JSON.

## <a name="json-walkthrough"></a>Пошаговое руководство JSON

Пример в этом пошаговом руководстве позволяет задачи должен быть создан. При выборе задачи на первом экране экран сведений представляются как показано.

 [ ![](json-element-walkthrough-images/03-task-list.png "При выборе задачи на первом экране экран сведений будет выглядеть, как показано")](json-element-walkthrough-images/03-task-list.png)

## <a name="creating-the-json"></a>Создание JSON

В этом примере мы загрузим JSON из файла в проект с именем `task.json`. МАШИННОГО ПЕРЕВОДА. D ожидает JSON, соответствует синтаксису, отражающую элементы API. Так же, как с помощью API элементы из кода, при использовании JSON, мы объявления разделов, и в этих блоках мы добавлять элементы. Чтобы объявить разделы и элементы в формате JSON, мы используем строки «разделами» и «элементы» соответственно как ключи. Для каждого элемента, тип элемента связанного задается с помощью `type` ключа. Каждый других элементов свойству с именем свойства, как ключ.

Например следующий JSON описывает разделы и элементы для сведения о задаче.

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Обратите внимание выше JSON включает идентификатор для каждого элемента. Любой элемент можно включать код, чтобы сослаться на него во время выполнения. Мы рассмотрим, как он используется в момент, когда мы показаны способы загрузки JSON в коде.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Загрузка JSON в коде

После определения JSON необходимо загрузить в машинного перевода. С помощью D `JsonElement` класса. При условии, что файл с JSON, созданным ранее была добавлена в проект с именем sample.json и получает действия построения содержимого, загрузка `JsonElement` так же просто, как и вызов следующую строку кода:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Поскольку мы добавляем по требованию каждый раз, что задача создана, можно изменять кнопки, нажатой из примера выше элементы API, как показано ниже:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>Доступ к элементам во время выполнения

Помните, что мы добавили идентификатора для обоих элементов при их объявлении в файле JSON. Свойство идентификатора можно использовать для доступа к каждому элементу во время выполнения можно изменить их свойства в коде. Например следующий код ссылается на запись и Дата элементы для задания значений из объекта задачи:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Загрузка JSON с URL-адреса

МАШИННОГО ПЕРЕВОДА. D также поддерживает динамическая загрузка JSON из внешнего URL-адреса, просто передав URL-адрес в конструктор класса `JsonElement`. МАШИННОГО ПЕРЕВОДА. D будет разверните иерархию, объявленные в JSON по требованию при навигации между экранами. Например рассмотрим JSON-файла, подобное показанному ниже, находящийся в корне локальный веб-сервер:

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Загрузить это с помощью `JsonElement` как в следующем коде:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Во время выполнения файл будет получаться и анализировать машинного перевода. D, когда пользователь переходит на второй вид, как показано на снимке экрана ниже:

 [ ![](json-element-walkthrough-images/04-json-web-example.png "Файл будет получаться и анализировать машинного перевода. D, когда пользователь переходит на второй вид")](json-element-walkthrough-images/04-json-web-example.png)

 <a name="Summary" />


## <a name="summary"></a>Сводка

В этой статье показано, как создать с помощью интерфейса с помощью машинного перевода. D из JSON. Оно было показано, как загрузить JSON, включенное в файл с приложением, а также URL-адрес удаленного. Также показано, как получить доступ к элементы, описанные в JSON, во время выполнения.


## <a name="related-links"></a>Связанные ссылки

- [MTDJsonDemo (sample)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Демонстрация - Icaza de Мигель создает экран входа iOS с MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Демонстрация - позволяет быстро создавать пользовательские интерфейсы iOS с MonoTouch.Dialog](http://youtu.be/j7OC5r8ZkYg)
- [Общие сведения о MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство элементы API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошагового руководства по API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Диалоговое окно MonoTouch на Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation приложения](https://github.com/migueldeicaza/TweetStation)
- [Ссылку на класс UITableViewController](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Ссылку на класс UINavigationController](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
