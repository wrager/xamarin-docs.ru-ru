---
title: Привязка данных и кодирование ключ значение
description: В этой статье рассказывается, используя ключ значение кодирования и наблюдения для привязки данных к элементам пользовательского интерфейса в Xcode интерфейс построителя ключ значение.
ms.prod: xamarin
ms.assetid: 72594395-0737-4894-8819-3E1802864BE7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 48ee5d4e4a0a53de49fbba46d79424e03af6fe5c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding-and-key-value-coding"></a>Привязка данных и кодирование ключ значение

_В этой статье рассказывается, используя ключ значение кодирования и наблюдения для привязки данных к элементам пользовательского интерфейса в Xcode интерфейс построителя ключ значение._

## <a name="overview"></a>Обзор

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к же ключ значение в коде и методы привязки данных, работающий в *Objective-C* и *Xcode* does. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ для привязки данных с элементами пользовательского интерфейса, вместо написания кода.

С помощью ключа и значения кодирования и привязки данных методов в приложении Xamarin.Mac, позволяет значительно сократить объем кода, который необходимо написать и хранить для заполнения и работать с элементами пользовательского интерфейса. Имеется также преимущество дальнейшего разделения данных резервное копирование (_модели данных_) вашего спереди завершения пользовательского интерфейса (_Model-View-Controller_), что приведет к проще поддерживать более гибкие приложения конструктор.

[![Пример выполняемого приложения](databinding-images/intro01.png "пример выполняемого приложения")](databinding-images/intro01-large.png#lightbox)

В этой статье мы обсудим основы работы с кодирования ключ значение и привязка данных в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` атрибуты используется для подключения классов C# для Objective-C-объекты и пользовательского интерфейса элементов.

<a name="What_is_Key-Value_Coding" />

## <a name="what-is-key-value-coding"></a>Что такое кодирование ключ значение

Ключ значение кодирования (KVC) — это механизм для доступа к свойствам объекта неявно, с помощью ключей (специальный формат строки) для определения свойства вместо обращения к ним через переменные экземпляра или методы доступа (`get/set`). Путем реализации методов доступа кодирования совместимые ключ значение в приложении Xamarin.Mac, можно получить доступ к другим функциям macOS (прежнее название-OS X), например наблюдения ключ значение (KVO), привязка данных, основных данных, Cocoa привязок и применимость сценариев.

С помощью ключа и значения кодирования и привязки данных методов в приложении Xamarin.Mac, позволяет значительно сократить объем кода, который необходимо написать и хранить для заполнения и работать с элементами пользовательского интерфейса. Имеется также преимущество дальнейшего разделения данных резервное копирование (_модели данных_) вашего спереди завершения пользовательского интерфейса (_Model-View-Controller_), что приведет к проще поддерживать более гибкие приложения конструктор. 

Например рассмотрим следующее определение класса объекта KVC совместимым.

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Во-первых, `[Register("PersonModel")]` атрибут класс регистрируется и делает его уязвимым для цели-C. Затем необходимо наследовать от класса `NSObject` (или подкласс, который наследует от `NSObject`), при этом добавляется несколько базовый метод, который позволяет классу быть совместимым KVC. Далее, `[Export("Name")]` атрибут предоставляет `Name` свойство и определяет значение ключа, которое будет использоваться позже для доступа к свойству через методы KVC и KVO. 

Наконец, чтобы иметь возможность быть ключ-значение, наблюдаемое примет значение свойства, метод доступа необходимо заключить изменения его значения в `WillChangeValue` и `DidChangeValue` вызовы методов (указание один и тот же ключ как `Export` атрибута).  Пример:

```csharp
set {
    WillChangeValue ("Name");
    _name = value;
    DidChangeValue ("Name");
}
```

Этот шаг является _очень_ важные для привязки данных в Xcode интерфейс построителя, (как мы увидим далее в этой статье).

Дополнительные сведения см. в разделе Apple [ключ-значение кодирования руководство по программированию](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html).

### <a name="keys-and-key-paths"></a>Ключи и путей

Объект _ключ_ является строка, определяющая указанное свойство объекта. Как правило ключ соответствует имени метода доступа в значении ключа, кодирование совместимые объекта. Ключи необходимо использовать кодировку ASCII, обычно начинаются со строчной буквы и не может содержать пробелы. Таким образом, заданному в приведенном выше примере `Name` бы значение ключа `Name` свойство `PersonModel` класса. Ключ и имя свойства, предоставляемые ими нет должны совпадать, однако в большинстве случаев они являются.

Объект _путь к ключу_ строка точка разделены ключи, используемые для указания свойств объекта для обхода иерархии. Свойство первого ключа в последовательности относительно получателя, а каждый ключ последующих вычисляется относительно значение предыдущего свойства. Таким же образом использовать точечную нотацию для обхода объекта и его свойства в классе C#.

Например, если вы развернули `PersonModel` класса и добавления `Child` свойство:

```csharp
using System;
using Foundation;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        private string _name = "";
        private PersonModel _child = new PersonModel();
        
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }
        
        [Export("Child")]
        public PersonModel Child {
            get { return _child; }
            set {
                WillChangeValue ("Child");
                _child = value;
                DidChangeValue ("Child");
            }
        }
        
        public PersonModel ()
        {
        }
    }
}
```

Путь к ключу имени ребенка будет `self.Child.Name` или просто `Child.Name` (с учетом как использовалось значение ключа).

### <a name="getting-values-using-key-value-coding"></a>Получение значений, с помощью кодирования ключ значение

`ValueForKey` Метод возвращает значение для указанного ключа (как `NSString`) по отношению к экземпляру класса KVC получения запроса. Например если `Person` является экземпляром класса `PersonModel` класс, определенный выше:

```csharp
// Read value 
var name = Person.ValueForKey (new NSString("Name"));
```

Вернет значение `Name` свойства для этого экземпляра `PersonModel`. 

### <a name="setting-values-using-key-value-coding"></a>Установка значений с помощью кодирования ключ значение

Аналогичным образом `SetValueForKey` задать значение для указанного ключа (как `NSString`) по отношению к экземпляру класса KVC получения запроса. И опять же, используя экземпляр `PersonModel` класса, как показано ниже:

```csharp
// Write value
Person.SetValueForKey(new NSString("Jane Doe"), new NSString("Name"));
```

Изменить значение `Name` свойства `Jane Doe`.

<a name="Observing_Value_Changes" />

### <a name="observing-value-changes"></a>Рассмотрение изменения значения

С помощью ключа и значения, наблюдение за (KVO), можно прикрепить наблюдателя к определенного ключа совместимого класса KVC и получать уведомления каждый раз, когда изменяются для этого ключа значение (методами KVC или прямой доступ к конкретного свойства в коде C#). Пример:

```csharp
// Watch for the name value changing
Person.AddObserver ("Name", NSKeyValueObservingOptions.New, (sender) => {
    // Inform caller of selection change
    Console.WriteLine("New Name: {0}", Person.Name)
});
```

Теперь в любое время `Name` свойство `Person` экземпляр `PersonModel` класс изменяется, новое значение записывается в консоль. 

Дополнительные сведения см. в разделе Apple [введение в ключ-значение рассмотрение руководство по программированию](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i).

## <a name="data-binding"></a>привязка данных,

В следующих разделах будет показано, как использовать ключ значение в коде и рассмотрение совместимого класса ключ значение для привязки данных к элементам пользовательского интерфейса в Xcode интерфейс, вместо построителя чтение и запись значений с использованием кода C#. Таким образом разделения вашей _модели данных_ из представлений, используемых для их отображения, сделать его Xamarin.Mac более гибкими и проще было обслуживать. Вы также значительно сократить объем кода, который должен быть написан.

<a name="Defining_your_Data_Model" />

### <a name="defining-your-data-model"></a>Определение модели данных

Прежде чем выполнить привязку данных элемента пользовательского интерфейса в построителе интерфейс, должен иметь совместимый класс KVC/KVO, заданных в приложении Xamarin.Mac в качестве _модели данных_ для привязки. Модель данных предоставляет все данные, которое будет отображаться в пользовательском интерфейсе и получает данные, когда пользователь вносит в пользовательском Интерфейсе во время выполнения приложения каких-либо изменений.

Например если бы вы создавали приложения, управляемые группы сотрудников, можно использовать следующие классы для определения модели данных:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDatabinding
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        #endregion

        #region Computed Properties
        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion
    }
}
```

Большинство функций этого класса охваченных [возможности кодирования ключ значение](#What_is_Key-Value_Coding) выше. Тем не менее, давайте взглянем на несколько конкретных элементов и некоторыми исключениями, которые были внесены разрешить этот класс в качестве модели данных для **массива контроллеров устройств** и **дерева контроллеров** (который мы будем использовать позднее к данным привязать **представления в виде дерева**, **представления структуры** и **представления коллекций**).

Во-первых, так как сотрудник может быть менеджером, мы использовали `NSArray` (в частности `NSMutableArray` , значения могут быть изменены) позволяет сотрудникам, которые они управляются для присоединения к ним:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
```

Обратите внимание на две вещи:

1. Мы использовали `NSMutableArray` вместо стандартной C# массива или коллекции, так как это требование для привязки данных к элементам управления AppKit **представления таблиц**, **представления структуры** и **коллекции** .
2. Мы предоставляемые приведения его в массиве сотрудников `NSArray` для имени, формат данных привязки и изменить его C# `People`, на адрес, который ожидает привязки данных, `personModelArray` в форме **{class_name} массива** (Обратите внимание, что первый символ были внесены нижнем регистре).

Далее необходимо добавить некоторые специально имя открытые методы для поддержки **массива контроллеров устройств** и **дерева контроллеров**:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Они позволяют контроллеры для запроса и изменения данных, которые они отображаются. Как и предоставляемым `NSArray` выше, они имеют особых соглашение об именовании (который отличается от обычной C# соглашения об именовании):

- `addObject:` — Добавляет объект в массив.
- `insertObject:in{class_name}ArrayAtIndex:` -Где `{class_name}` имя класса. Этот метод вставляет объект в массив по указанному индексу.
- `removeObjectFrom{class_name}ArrayAtIndex:` -Где `{class_name}` имя класса. Этот метод удаляет объект в массиве, с заданным индексом.
- `set{class_name}Array:` -Где `{class_name}` имя класса. Этот метод позволяет заменить существующие разноситься на новый.

Внутри этих методов, мы оболочку изменения в массив в `WillChangeValue` и `DidChangeValue` сообщений на соответствие KVO.

Наконец, так как `Icon` свойство использует значение `isManager` изменения свойств, `isManager` свойство не может быть отражено в `Icon` для элементов пользовательского интерфейса привязки данных (в ходе KVO):

```csharp
[Export("Icon")]
public NSImage Icon {
    get {
        if (isManager) {
            return NSImage.ImageNamed ("group.png");
        } else {
            return NSImage.ImageNamed ("user.png");
        }
    }
}
``` 

Чтобы исправить это, мы используем следующий код:

```csharp
[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");
    }
}
```

Обратите внимание, что помимо свой собственный ключ `isManager` доступа также отправляет `WillChangeValue` и `DidChangeValue` сообщений для `Icon` ключа, он будет видеть изменения также.

Мы будем использовать `PersonModel` модели данных во всей оставшейся части этой статьи.

<a name="Simple_Data_Binding" />

### <a name="simple-data-binding"></a>Простая привязка данных

С нашей модели данных, определенной давайте взглянем на простой пример привязки данных в построителе интерфейса в Xcode. Например, добавим формы для нашего приложения Xamarin.Mac, который может использоваться для изменения `PersonModel` , мы определили выше. Мы добавим несколько текстовых полей и флажок для отображения и изменения свойств нашей модели.

Во-первых давайте добавим новый **View Controller** для наших **Main.storyboard** файла в интерфейс построителя и назовите его класс `SimpleViewController`: 

[![Добавление нового представления контроллера](databinding-images/simple01.png "добавлении нового контроллера представления")](databinding-images/simple01-large.png#lightbox)

Затем вернитесь в Visual Studio для Mac, изменить **SimpleViewController.cs** -файл (который был автоматически добавлен в данном проекте) и предоставить экземпляр `PersonModel` мы будем нашу форму для привязки данных. Добавьте следующий код:

```csharp
private PersonModel _person = new PersonModel();
...

[Export("Person")]
public PersonModel Person {
    get {return _person; }
    set {
        WillChangeValue ("Person");
        _person = value;
        DidChangeValue ("Person");
    }
}
```

Далее при загрузке представления, давайте создадим экземпляр нашей `PersonModel` и заполнить его следующим кодом:

```csharp
public override void ViewDidLoad ()
{
    base.AwakeFromNib ();

    // Set a default person
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    Person = Craig;

}
```

Теперь нам нужно создать нашу форму, дважды щелкните **Main.storyboard** файл, чтобы открыть его для редактирования в построителе интерфейса. Макет формы выглядит примерно следующим образом:

[![Редактирование раскадровки в Xcode](databinding-images/simple02.png "редактирование раскадровки в Xcode")](databinding-images/simple02-large.png#lightbox)

Для привязки данных формы `PersonModel` , мы через `Person` ключ, выполните следующие действия:

1. Выберите **имя сотрудника** текстовое поле и переключиться на **инспектора привязки**.
2. Проверьте **привязки к** и выберите **простое представление-контроллер** из раскрывающегося списка. Далее введите `self.Person.Name` для **путь к ключу**: 

    [![Введите путь к разделу](databinding-images/simple03.png "ввода пути к ключу")](databinding-images/simple03-large.png#lightbox)
3. Выберите **род занятий** текстовое поле и проверьте **привязки к** и выберите **простое представление-контроллер** из раскрывающегося списка. Далее введите `self.Person.Occupation` для **путь к ключу**:  

    [![Введите путь к разделу](databinding-images/simple04.png "ввода пути к ключу")](databinding-images/simple04-large.png#lightbox)
4. Выберите **сотрудника является менеджером** флажок и проверьте **привязки к** и выберите **простое представление-контроллер** из раскрывающегося списка. Далее введите `self.Person.isManager` для **путь к ключу**:  

    [![Введите путь к разделу](databinding-images/simple05.png "ввода пути к ключу")](databinding-images/simple05-large.png#lightbox)
5. Выберите **число сотрудников управляемых** текстовое поле и проверьте **привязки к** и выберите **простое представление-контроллер** из раскрывающегося списка. Далее введите `self.Person.NumberOfEmployees` для **путь к ключу**:  

    [![Введите путь к разделу](databinding-images/simple06.png "ввода пути к ключу")](databinding-images/simple06-large.png#lightbox)
6. Если сотрудник не является диспетчером, нам нужно скрыть номер из сотрудников управляемых метку и текстовое поле.
7. Выберите **число сотрудников управляемых** метки, разверните **Hidden** turndown и проверьте **привязки к** и выберите **простое представление-контроллер** из раскрывающегося списка. Далее введите `self.Person.isManager` для **путь к ключу**:  

    [![Введите путь к разделу](databinding-images/simple07.png "ввода пути к ключу")](databinding-images/simple07-large.png#lightbox)
8. Выберите `NSNegateBoolean` из **Transformer значение** раскрывающегося списка:  

    ![При выборе преобразования ключа NSNegateBoolean](databinding-images/simple08.png "при выборе преобразования ключа NSNegateBoolean")
9. Это значение определяет привязку данных, метки будут скрыты, если значение `isManager` свойство `false`.
10. Повторите шаги 7 и 8 для **число сотрудников управляемых** текстового поля.
11. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

При запуске приложения, значения из `Person` свойства будут заполнены автоматически нашу форму:

[![Отображение в форме автоматически заполняемая](databinding-images/simple09.png "отображение в форме заполняется автоматически")](databinding-images/simple09-large.png#lightbox)

Любые изменения, внесенные в форму пользователей будут записаны в `Person` свойство в представление-контроллер. Например, сняв **сотрудника является менеджером** обновления `Person` экземпляр нашей `PersonModel` и **число сотрудников управляемых** метку и текстовое поле скрыты автоматически (с помощью Привязка данных):

[![Скрытие количество работников для от менеджеров](databinding-images/simple10.png "скрытие количество работников для менеджеров")](databinding-images/simple10-large.png#lightbox)

<a name="Table_View_Data_Binding" />

### <a name="table-view-data-binding"></a>Привязка данных представление таблицы

Теперь, когда у нас есть основные принципы привязки данных, в сторону, давайте взглянем на более сложные задачи привязки данных с помощью _контроллера массива_ и привязка данных к представлению таблицы. Дополнительные сведения о работе с представлениями таблицы см. в разделе нашей [представления таблиц](~/mac/user-interface/table-view.md) документации.

Во-первых давайте добавим новый **View Controller** для наших **Main.storyboard** файла в интерфейс построителя и назовите его класс `TableViewController`:

[![Добавление нового представления контроллера](databinding-images/table01.png "добавлении нового контроллера представления")](databinding-images/table01-large.png#lightbox)

Теперь изменим **TableViewController.cs** -файл (который был автоматически добавлен в данном проекте) и предоставить массив (`NSArray`) из `PersonModel` классов, мы будем нашу форму для привязки данных. Добавьте следующий код:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Так же, как мы делали на `PersonModel` класса выше в [определения модели данных](#Defining_your_Data_Model) раздела, мы предоставляется четыре специально именованную открытых методов, чтобы контроллер массива и чтение и запись данных из наш набор `PersonModels`.

Затем при загрузке представления нужно заполнить нашего массива следующим кодом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Теперь нам нужно создать наши таблицы представления, дважды щелкните **Main.storyboard** файл, чтобы открыть его для редактирования в построителе интерфейса. Макет таблицы выглядит примерно следующим образом:

[![Определение нового представления таблицы](databinding-images/table02.png "с макетом новое представление таблицы")](databinding-images/table02-large.png#lightbox)

Необходимо добавить **контроллера массива** для предоставления связанных данных в нашей таблице, выполните следующие действия:

1. Перетащите **массива контроллера** из **инспектора библиотеки** на **редактора интерфейса**:  

    ![При выборе контроллера массива из библиотеки](databinding-images/table03.png "при выборе контроллера массива из библиотеки")
2. Выберите **контроллера массива** в **иерархии интерфейса** и переключитесь в **инспектора атрибут**:  

    [![При выборе инспектора атрибуты](databinding-images/table04.png "при выборе инспектора атрибуты")](databinding-images/table04-large.png#lightbox)
3. Введите `PersonModel` для **имя класса**, нажмите кнопку **, а также** и добавьте три ключа. Назовите их `Name`, `Occupation` и `isManager`:  

    ![Добавление необходимых путей ключа](databinding-images/table05.png "добавление необходимых путей ключа")
4. Это заставляет контроллера массива, что он управляет массив, и свойства, которые должны предоставлять доступ (через ключи).
5. Переключитесь в **инспектора привязок** и в разделе **содержимого массива** выберите **привязки к** и **контроллер представление таблицы**. Введите **модели пути к ключу** из `self.personModelArray`:  

    ![Введите путь к разделу](databinding-images/table06.png "ввода пути к ключу")
6. Этот способ привязывает контроллера массива в массив `PersonModels` , мы публикуются в нашем представление-контроллер.

Теперь нам нужно привязать контроллер массива наши таблицы представления, выполните следующее:

1. Выберите представление таблицы и **привязки инспектора**:  

    [![При выборе привязки инспектора](databinding-images/table07.png "при выборе инспектора привязки")](databinding-images/table07-large.png#lightbox)
2. В разделе **содержимое таблицы** turndown, выберите **привязки к** и **контроллера массива**. Введите `arrangedObjects` для **ключ контроллера** поля:  

    ![Определение ключа контроллера](databinding-images/table08.png "определение ключа контроллера")
3. Выберите **ячейки таблицы представления** под **сотрудника** столбца. В **инспектора привязок** под **значение** turndown, выберите **привязки к** и **представление ячейки таблицы**. Введите `objectValue.Name` для **модели пути к ключу**:  

    [![Задание пути к ключу модели](databinding-images/table09.png "задание пути к ключу модели")](databinding-images/table09-large.png#lightbox)
4. `objectValue` — Текущая `PersonModel` в массиве, который управляется контроллером массива.
5. Выберите **ячейки таблицы представления** под **род занятий** столбца. В **инспектора привязок** под **значение** turndown, выберите **привязки к** и **представление ячейки таблицы**. Введите `objectValue.Occupation` для **модели пути к ключу**:  

    [![Задание пути к ключу модели](databinding-images/table10.png "задание пути к ключу модели")](databinding-images/table10-large.png#lightbox)
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Если мы запустим приложение, таблица будет заполняться нашей массив `PersonModels`:

[![Запуск приложения](databinding-images/table11.png "работающим с приложением")](databinding-images/table11-large.png#lightbox)

<a name="Outline_View_Data_Binding" />

### <a name="outline-view-data-binding"></a>Привязка данных для представления структуры

Привязка данных к структура очень похожа на привязку к представлению таблицы. Ключевое различие состоит в том, что мы будем использовать **дерева контроллера** вместо **контроллера массива** для предоставления связанных данных в режим структуры. Дополнительные сведения о работе с представления структуры см. в разделе нашей [представления структуры](~/mac/user-interface/outline-view.md) документации.

Во-первых давайте добавим новый **View Controller** для наших **Main.storyboard** файла в интерфейс построителя и назовите его класс `OutlineViewController`: 

[![Добавление нового представления контроллера](databinding-images/outline01.png "добавлении нового контроллера представления")](databinding-images/outline01-large.png#lightbox)

Теперь изменим **OutlineViewController.cs** -файл (который был автоматически добавлен в данном проекте) и предоставить массив (`NSArray`) из `PersonModel` классов, мы будем нашу форму для привязки данных. Добавьте следующий код:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Так же, как мы делали на `PersonModel` класса выше в [определения модели данных](#Defining_your_Data_Model) раздела, мы предоставляется четыре специально именованную открытых методов, чтобы данные контроллера дерева и чтение и запись в наш набор `PersonModels`.

Затем при загрузке представления нужно заполнить нашего массива следующим кодом:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    var Craig = new PersonModel ("Craig Dunn", "Documentation Manager");
    Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (Craig);

    var Larry = new PersonModel ("Larry O'Brien", "API Documentation Manager");
    Larry.AddPerson (new PersonModel ("Mike Norman", "API Documenter"));
    AddPerson (Larry);

}
```

Теперь нам нужно создать представление нашей структуры, дважды щелкните **Main.storyboard** файл, чтобы открыть его для редактирования в построителе интерфейса. Макет таблицы выглядит примерно следующим образом:

[![Создание представления структуры](databinding-images/outline02.png "Создание представления структуры")](databinding-images/outline02-large.png#lightbox)

Необходимо добавить **дерева контроллера** для предоставления привязанных данных наших структуры, выполните следующие действия:

1. Перетащите **дерева контроллера** из **инспектора библиотеки** на **редактора интерфейса**:  

    ![При выборе контроллера дерева из библиотеки](databinding-images/outline03.png "выборе контроллера дерева из библиотеки")
2. Выберите **дерева контроллера** в **иерархии интерфейса** и переключитесь в **инспектора атрибут**:  

    [![При выборе атрибута инспектора](databinding-images/outline04.png "при выборе инспектора атрибута")](databinding-images/outline04-large.png#lightbox)
3. Введите `PersonModel` для **имя класса**, нажмите кнопку **, а также** и добавьте три ключа. Назовите их `Name`, `Occupation` и `isManager`:  

    ![Добавление необходимых путей ключа](databinding-images/outline05.png "добавление необходимых путей ключа")
4. Это значение определяет контроллера дерева то, что он управляет массив, и свойства, которые должны предоставлять доступ (через ключи).
5. В разделе **дерева контроллера** введите `personModelArray` для **дочерние элементы**, введите `NumberOfEmployees` под **число** и введите `isEmployee` под  **Конечный**:  

    ![Задание путей ключа дерева контроллера](databinding-images/outline05.png "задание путей ключа дерева контроллера")
6. Это значение определяет контроллера дерева where для поиска всех дочерних узлов, сколько узлов дочерних и если текущий узел имеет дочерние узлы.
7. Переключитесь в **инспектора привязок** и в разделе **содержимого массива** выберите **привязки к** и **владелец файла**. Введите **модели пути к ключу** из `self.personModelArray`:  

    ![Изменение пути к ключу](databinding-images/outline06.png "редактирования реестра")
8. Этот способ привязывает контроллера дерево с элементами массива `PersonModels` , мы публикуются в нашем представление-контроллер.

Теперь нам нужно привязать нашей представление структуры дерева контроллер, выполните следующее:

1. Выберите представление структуры и в **привязки инспектора** выберите:  

    [![При выборе привязки инспектора](databinding-images/outline07.png "при выборе инспектора привязки")](databinding-images/outline07-large.png#lightbox)
2. В разделе **просмотреть содержимое структуры** turndown, выберите **привязки к** и **дерева контроллера**. Введите `arrangedObjects` для **ключ контроллера** поля:  

    ![Задание ключа контроллера](databinding-images/outline08.png "задание ключа контроллера")
3. Выберите **ячейки таблицы представления** под **сотрудника** столбца. В **инспектора привязок** под **значение** turndown, выберите **привязки к** и **представление ячейки таблицы**. Введите `objectValue.Name` для **модели пути к ключу**:  

    [![Введите путь к разделу модели](databinding-images/outline09.png "ввода пути к ключу модели")](databinding-images/outline09-large.png#lightbox)
4. `objectValue` — Текущая `PersonModel` в массиве, который управляется контроллером дерева.
5. Выберите **ячейки таблицы представления** под **род занятий** столбца. В **инспектора привязок** под **значение** turndown, выберите **привязки к** и **представление ячейки таблицы**. Введите `objectValue.Occupation` для **модели пути к ключу**:  

    [![Введите путь к разделу модели](databinding-images/outline10.png "ввода пути к ключу модели")](databinding-images/outline10-large.png#lightbox)
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Если мы запустим приложение, контур будет заполняться нашей массив `PersonModels`:

[![Запуск приложения](databinding-images/outline11.png "работающим с приложением")](databinding-images/outline11-large.png#lightbox)

### <a name="collection-view-data-binding"></a>Привязка данных представления коллекции

Привязка данных с помощью представления коллекции очень сходно привязке с помощью таблицы представления, так как контроллера массива используется для предоставления данных для коллекции. Поскольку представление коллекции, не имеет формат стиль отображения, больше работы требуется для предоставления отзывов взаимодействие с пользователем и отслеживания выбора пользователя.

> [!IMPORTANT]
> Из-за проблемы в Xcode 7 и macOS 10.11 (и более поздней версии) представления коллекций не могут использоваться в файлах раскадровки (.storyboard). В результате необходимо продолжать использовать файлы .xib для определения представления коллекции для Xamarin.Mac приложений. См. в нашем [представления коллекций](~/mac/user-interface/collection-view.md) Дополнительные сведения см.

<!--KKM 012/16/2015 - Once Apple fixes the issue with Xcode and Collection Views in Storyboards, we can uncomment this section.

First, let's add a new **View Controller** to our **Main.storyboard** file in Interface Builder and name its class `CollectionViewController`: 

![](databinding-images/collection01.png)

Next, let's edit the **CollectionViewController.cs** file (that was automatically added to our project) and expose an array (`NSArray`) of `PersonModel` classes that we will be data binding our form to. Add the following code:

```csharp
private NSMutableArray _people = new NSMutableArray();
...

[Export("personModelArray")]
public NSArray People {
    get { return _people; }
}
...

[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    _people.Add (person);
    DidChangeValue ("personModelArray");
}

[Export("insertObject:inPersonModelArrayAtIndex:")]
public void InsertPerson(PersonModel person, nint index) {
    WillChangeValue ("personModelArray");
    _people.Insert (person, index);
    DidChangeValue ("personModelArray");
}

[Export("removeObjectFromPersonModelArrayAtIndex:")]
public void RemovePerson(nint index) {
    WillChangeValue ("personModelArray");
    _people.RemoveObject (index);
    DidChangeValue ("personModelArray");
}

[Export("setPersonModelArray:")]
public void SetPeople(NSMutableArray array) {
    WillChangeValue ("personModelArray");
    _people = array;
    DidChangeValue ("personModelArray");
}
```

Just like we did on the `PersonModel` class above in the [Defining your Data Model](#Defining_your_Data_Model) section, we've exposed four specially named public methods so that the Array Controller and read and write data from our collection of `PersonModels`.

Next when the View is loaded, we need to populate our array with this code:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Build list of employees
    AddPerson (new PersonModel ("Craig Dunn", "Documentation Manager", true));
    AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
    AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
    AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
    AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
    AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
    AddPerson (new PersonModel ("Larry O'Brien", "API Documentation Manager", true));
    AddPerson (new PersonModel ("Mike Norman", "API Documenter"));

}
```

Now we need to create our Collection View, double-click the **Main.storyboard** file to open it for editing in Interface Builder. Layout the Collection View to look something like the following:

![](databinding-images/collection02.png)

When you add a Collection View to a User Interface design, two extra elements are also added:

1.  **Collection View Item** -  That manages a single instance of an item in the collection.
2.  **View** - A custom view that provides the visual size and appearance of each item in the collection. This view is tied to and managed by the **Collection View Item**.

Select the view and make it look like the following using an Image View and two Text Fields:

![](databinding-images/collection03.png)

One thing to note here, a `NSBox` was added behind everything in the view with the following attributes:

![](databinding-images/collection04.png)

We'll be using this box to provide feedback to the user when an item is selected in the Collection View.

We need to add an **Array Controller** to provide bound data to our collection, do the following:

1. Drag an **Array Controller** from the **Library Inspector** onto the **Interface Editor**: <br/>![](databinding-images/table03.png)
2. Select **Array Controller** in the **Interface Hierarchy** and switch to the **Attribute Inspector**: <br/>![](databinding-images/collection05.png)
3. Enter `PersonModel` for the **Class Name**, click the **Plus** button and add four Keys. Name them `Icon`, `Name`, `Occupation` and `isManager`: <br/>![](databinding-images/table05.png)
4. This tells the Array Controller what it is managing an array of, and which properties it should expose (via Keys).
5. Switch to the **Bindings Inspector** and under **Content Array** select **Bind to** and **File's Owner**. Enter a **Model Key Path** of `self.personModelArray`: <br/>![](databinding-images/table06.png)
6. This ties the Array Controller to the array of `PersonModels` that we exposed on our View Controller.

Now we need to bind our Collection View to the Array Controller, do the following:

1. Select the Collection View and the **Binding Inspector**: <br/>![](databinding-images/collection06.png)
2. Under the **Contents** turndown, select **Bind to** and **Array Controller**. Enter `arrangedObjects` for the **Controller Key** field: <br/>![](databinding-images/collection07.png)
3. Under the **Selection Indexes** turndown, select **Bind to** and **Array Controller**. Enter `selectionIndexes` for the **Controller Key** field: <br/>![](databinding-images/collection08.png)
4. Select the **Image View**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Person** (the name of our Collection View Item). Enter `representedObject.Icon` for the **Model Key Path**: <br/>![](databinding-images/collection09.png)
5. `representedObject` is the current `PersonModel` in the array being managed by the Array Controller.
6. Select the first **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Name` for the **Model Key Path**: <br/>![](databinding-images/collection10.png)
7. Select the second **Label**. In the **Bindings Inspector** under the **Value** turndown, select **Bind to** and **Collection View Item**. Enter `representedObject.Occupation` for the **Model Key Path**: <br/>![](databinding-images/collection11.png)
8. Select the `NSBox`. In the **Bindings Inspector** under the **Hidden** turndown, select **Bind to** and **Collection View Item**. Enter `selected` for the **Model Key Path** and `NSNegateBoolean` for the **Value Transformer**: <br/>![](databinding-images/collection12.png)
9. Save your changes and return to Visual Studio for Mac to sync with Xcode.

If we run the application, the table will be populated with our array of `PersonModels`:

![](databinding-images/collection13.png)

For more information on working with Collection Views, please see our [Collection Views](~/mac/user-interface/collection-view.md) documentation.-->

## <a name="debugging-native-crashes"></a>Отладка машинного сбоев

Допустить ошибку в привязок данных может привести к _собственного приводит к сбою_ в неуправляемом коде и привести к неработоспособности с приложения Xamarin.Mac `SIGABRT` ошибка:

[![Пример диалогового окна собственного сбоя](databinding-images/debug01.png "примере аварийного завершения собственного диалогового окна")](databinding-images/debug01-large.png#lightbox)

Обычно существует четыре основных причин для собственного сбоев во время привязки данных.

1. Модель данных не наследует от `NSObject` или подкласс `NSObject`.
2. Не было предоставлять свойство Objective-C помощью `[Export("key-name")]` атрибута.
3. Не удалось wrap внесение изменений в метод доступа значение `WillChangeValue` и `DidChangeValue` вызовы методов (указание один и тот же ключ как `Export` атрибута).
4. Имеет неправильный или вводом неправильного ключа **привязки инспектора** в построителе интерфейса.

### <a name="decoding-a-crash"></a>Декодирование сбоя

Давайте вызывает сбой собственного в нашей привязкой данных, показываются способы поиска и устранить ее. В построителе интерфейс изменим нашей привязкой первая метка в примере представления коллекции из `Name` для `Title`:

[![Ключ привязки редактирования](databinding-images/debug02.png "редактирования раздела привязки")](databinding-images/debug02-large.png#lightbox)

Давайте сохранить изменение, вернитесь в Visual Studio для Mac для синхронизации с Xcode и запуска приложения. При отображении представления коллекции будут моментально аварийное завершение приложения с `SIGABRT` ошибки (как показано в **выходных данных приложения** в Visual Studio для Mac) с момента `PersonModel` предоставляет свойства с ключом `Title`:

[![Ошибки привязки](databinding-images/debug03.png "примером ошибки привязки")](databinding-images/debug03-large.png#lightbox)

Если мы найдите ошибку в самом верху **выходных данных приложения** можно увидеть ключ к решению проблемы:

[![Поиск проблемы в журнале ошибок](databinding-images/debug04.png "поиск проблемы в журнале ошибок")](databinding-images/debug04-large.png#lightbox)

Эта строка сообщают, что ключ `Title` не существует, по которому выполняется привязка. Мы можем изменить привязку, снова `Name` в построителе интерфейс сохранения, синхронизации, перестроение и выполнения, приложение будет работать должным образом без проблем.

## <a name="summary"></a>Сводка

Подробное описание работы с привязкой данных и ключ значение кода в приложении Xamarin.Mac вступила в этой статье. Во-первых хотя рассматривались предоставления класса C# для Objective-C, используя ключ значение кодирования (KVC) и ключа и значения, наблюдение за (KVO). Затем демонстрировалось совместимого класса KVO и его привязать данные к элементам пользовательского интерфейса в построителе интерфейса в Xcode. Наконец, показали сложных данных с помощью привязки **массива контроллеров устройств** и **дерева контроллеров**.


## <a name="related-links"></a>Связанные ссылки

- [Раскадровка MacDatabinding (пример)](https://developer.xamarin.com/samples/mac/MacDatabinding-Storyboard/)
- [MacDatabinding XIBs (пример)](https://developer.xamarin.com/samples/mac/MacDatabinding-XIBs/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Стандартные элементы управления](~/mac/user-interface/standard-controls.md)
- [Представления таблиц](~/mac/user-interface/table-view.md)
- [Представления структуры](~/mac/user-interface/outline-view.md)
- [Представления коллекций](~/mac/user-interface/collection-view.md)
- [Ключ значение кодирования, руководство по программированию](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Введение в руководство по программированию на рассмотрение ключ значение](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html)
- [Введение в программирование разделы привязок Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Общие сведения о Cocoa ссылки привязки](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS рекомендациям по интерфейсам](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
