---
title: "Visual Basic.NET в Xamarin iOS и Android"
description: "XThis пошаговом руководстве показано, как создавать собственные приложения Xamarin.iOS и Xamarin.Android, использующие бизнес-логику на Visual Basic.NET."
ms.topic: article
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: dc107ee865ea93cdc12148a5498cf3d512f1dae9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET в Xamarin iOS и Android

[TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) образце приложения показано, как можно использовать код Visual Basic, компилируются в переносимой библиотеки классов с помощью Xamarin. Ниже приведены некоторые снимки полученный приложения, работающие на iOS, Android и Windows Phone.

 [ ![](native-apps-images/image5.png "iOS, Android и Windows телефоны под управлением приложения, созданного с помощью Visual Basic")](native-apps-images/image5.png)

IOS, Android и Windows Phone проекты в примере записываются на языке C#. Пользовательский интерфейс для каждого приложения создается с собственного технологий (раскадровки, Xml и Xaml соответственно), во время `TodoItem` управления обеспечивается переносимой библиотеки классов Visual Basic с помощью `IXmlStorage` реализации собственный проект.

## <a name="sample-walkthrough"></a>Практический руководство

В этом руководстве описывается, как Visual Basic реализовано в [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) Xamarin образец для iOS и Android.

> ⚠️ Просмотрите инструкции на [Visual Basic.NET PCLs](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/) перед продолжением работы с помощью этого руководства.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Переносимые библиотеки классов Visual Basic могут создаваться только в Visual Studio.
Образец библиотеки содержит основные приложения в четырех файлах Visual Basic:

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Так как доступ действия с файлами, значительно отличаются от платформы, переносимые библиотеки классов не имеют `System.IO` хранилище API-интерфейсы в любой профиль из файлов. Это означает, что если мы хотим взаимодействовать непосредственно с файловой системы в нашем переносимого кода, нам нужно обратный вызов нашей собственных проектов для каждой платформы.  Путем написания кода Visual Basic для простой интерфейс, который может быть реализован в C# для каждой платформы, мы имеют общий код Visual Basic, который по-прежнему имеет доступ к файловой системе.

В примере кода используется этот простейший интерфейс, который содержит только два метода: для чтения и записи сериализованного XML-файл.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android и Windows Phone реализации этого интерфейса будет показано далее в этом руководстве.

### <a name="todoitemvb"></a>TodoItem.vb

Этот класс содержит бизнес-объект для использования в приложении. Он будет определяться в Visual Basic и совместно с iOS, Android и Windows Phone проекты, которые написаны на C#.

Определение класса приведен ниже:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

В образце используется XML-сериализации и десериализации для загрузки и сохранения объектов TodoItem.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Класс диспетчера представляется «API» для создания переносимого кода. Он предоставляет основные операции CRUD для `TodoItem` класс, но нет реализации этих операций.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Конструктор принимает экземпляр IXmlStorage в качестве параметра. Это позволяет каждой платформы, чтобы предоставить собственную реализацию работы, обеспечивая переносимого кода, которые описаны другие функции, который может использоваться совместно.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Класс репозитория содержит логику для управления списком объектов TodoItem. Полный код показан ниже — логику существует главным образом для управления уникальное значение идентификатора через TodoItems, как они добавляются и удаляются из коллекции.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> ℹ️ **Примечание:** этот код является примером может служить механизм хранения данных очень простой.
> Он предоставляется для демонстрации того, как можно разрабатывать код переносимой библиотеки классов для интерфейса для доступа к функциям специфический для платформы (в данном случае загрузки и сохранения XML-файл).
> Он ее не должно быть альтернативой высококачественных базы данных.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android и проектов приложений Windows Phone

Этот раздел содержит реализации интерфейса IXmlStorage конкретную платформу и показано, как она используется в каждом приложении. Проекты приложений записываются на языке C#.

### <a name="ios-and-android-ixmlstorage"></a>iOS и Android IXmlStorage

Xamarin.iOS и Xamarin.Android предоставить полный `System.IO` функциональные возможности, поэтому можно легко загрузить и сохранить XML-файл, выполнив следующий класс:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

В приложении iOS `TodoItemManager` и `XmlStorageImplementation` создаются в **AppDelegate.cs** файла, как показано в этом фрагменте кода. Первые четыре строки выполняется построение только путь к файлу, где будут храниться данные; Последние две строки показано создание экземпляров классов.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

В приложении Android `TodoItemManager` и `XmlStorageImplementation` создаются в **Application.cs** файла, как показано в этом фрагменте кода. Первые три строки просто создаете путь к файлу, где будут храниться данные; Последние две строки показано создание экземпляров классов.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Остальная часть кода приложения в основном разработку пользовательского интерфейса и с помощью `TaskMgr` класса для загрузки и сохранения `TodoItem` классы.

### <a name="windows-phone-ixmlstorage"></a>IXmlStorage Windows Phone

Windows Phone не предоставляет полный доступ к файловой системе устройства, вместо предоставления IsolatedStorage API. `IXmlStorage` Реализацию для Windows Phone выглядит следующим образом:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

`TodoItemManager` И `XmlStorageImplementation` создаются в **App.xaml.cs** файла, как показано в этом фрагменте кода.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Остальная часть приложения Windows Phone состоит из Xaml и C# для создания пользовательского интерфейса и использовать `TodoMgr` класса для загрузки и сохранения `TodoItem` объектов.

# <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL в Visual Studio для Mac

Visual Studio для Mac не поддерживает язык Visual Basic — не удается создать или компилировать проекты Visual Basic в Visual Studio для Mac.

Visual Studio для Mac в поддержку переносимой библиотеки классов означает, что он может ссылаться на сборки PCL, созданные в Visual Basic.

В этом разделе объясняется, как компилировать PCL сборку в Visual Studio, а затем убедитесь, что оно хранится в системе управления версиями и ссылаться на другие проекты.

## <a name="keeping-the-pcl-output-from-visual-studio"></a>Сохранение вывода PCL из Visual Studio

По умолчанию большинство систем контроля версий (включая TFS и Git) будет настроен для игнорирования **/bin/** каталог, что означает скомпилированную сборку PCL не будут сохранены. Это означает, что необходимо вручную скопировать его на всех компьютерах под управлением Visual Studio для Mac добавить ссылку на него.

Чтобы убедиться, что системы управления версиями можно сохранить выходные данные сборки PCL, можно создать скрипт после построения, который копирует его в корневой папке проекта. Этот шаг после сборки гарантирует легко добавить в систему управления версиями и совместно с другими проектами сборки.

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Щелкните правой кнопкой мыши проект и выберите **свойства > события построения** раздела.

2. Добавить _после построения_ скрипт, который копирует выходные данные библиотеки DLL из этого проекта в корневой каталог проекта (который находится за пределами **/bin/**). В зависимости от настройки управления версии библиотеки DLL теперь можно добавить в систему управления версиями.

  [ ![](native-apps-images/image6-vs-sml.png "События построения сценария после построения для копирования VB DLL")](native-apps-images/image6-vs.png)

### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Щелкните правой кнопкой мыши проект и выберите **свойства > компиляции** , затем должен быть выбран все конфигурации в поле объединение верхнего левого. Нажмите кнопку **события построения...**  кнопки в нижнем правом углу.

  [ ![](native-apps-images/image6.png "В разделе компиляции свойства проекта")](native-apps-images/image6.png)

1.  Добавьте скрипт после построения, который копирует выходные данные библиотеки DLL из этого проекта в корневой каталог проекта (который находится за пределами **/bin/** ). В зависимости от настройки управления версии библиотеки DLL теперь можно добавить в систему управления версиями.

  [ ![](native-apps-images/image7.png "Окна событий построения")](native-apps-images/image7.png)

### <a name="all-versions"></a>Все версии

Следующий раз при построении проекта, сборку переносимой библиотеки классов, которые будут скопированы в корневой каталог проекта и проверка в/commit/push изменения библиотеки DLL можно хранить (, чтобы его можно скачать на Mac с помощью Visual Studio для Mac).

  [ ![](native-apps-images/image8-sml.png "Расположение выходной сборки Visual Basic")](native-apps-images/image8.png)


Эта сборка затем добавляются в проекты Xamarin в Visual Studio для Mac, несмотря на то, что сам язык Visual Basic не поддерживается в Xamarin iOS или проектов Android.

## <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Создание ссылок на PCL в Visual Studio для Mac

Поскольку Xamarin Visual Basic не поддерживает его как показано на этом снимке экрана не удалось загрузить проект переносимой библиотеки Классов (и не приложения Windows Phone):

 [ ![](native-apps-images/image9.png "Visual Studio для Mac решения")](native-apps-images/image9.png)

Мы по-прежнему может включать DLL Visual Basic PCL сборки в проектах Xamarin.iOS и Xamarin.Android:

1.  Щелкните правой кнопкой мыши **ссылки** , а затем выберите **изменить ссылки...**

  [ ![](native-apps-images/image10.png "Меню "Правка ссылки" проекта")](native-apps-images/image10.png)

1.  Выберите **.Net сборки** вкладку и перейдите к выходу DLL в каталоге проекта Visual Basic. Несмотря на то, что Visual Studio для Mac не удается открыть проект, все файлы должны присутствовать из системы управления версиями. Нажмите кнопку **добавить** затем **ОК** для добавления этой сборки для iOS и Android приложений.

  [ ![](native-apps-images/image11-sml.png "Нажмите кнопку Добавить и нажмите кнопку ОК для добавления этой сборки для iOS и Android приложений")](native-apps-images/image11.png)

1.  IOS и Android приложений теперь можно включить логику приложения, предоставляемые переносимой библиотеки классов Visual Basic. На этом снимке экрана показано приложение iOS, и имеет код, который использует функциональные возможности из этой библиотеки Классов Visual Basic.

  [ ![](native-apps-images/image12-sml.png "Изменение ссылки Добавить окно сборки .NET")](native-apps-images/image12.png)


При внесении изменений в проект Visual Basic в Visual Studio не забудьте выполните построение проекта, хранилища результирующую сборку DLL в системе управления версиями, а затем переместить этой новой библиотеки DLL из системы управления версиями на компьютере Mac, чтобы Visual Studio для Mac создает содержится последняя функциональные возможности.


## <a name="summary"></a>Сводка

В этой статье продемонстрировал, как использовать код Visual Basic в приложениях Xamarin, с помощью Visual Studio и переносимой библиотеки классов. Несмотря на то что Xamarin непосредственно не поддерживает Visual Basic, компиляция Visual Basic в PCL позволяет кода, написанного на Visual Basic должны быть включены в iOS и Android.

## <a name="related-links"></a>Связанные ссылки

- [TaskyPortableVB (пример)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [Межплатформенная разработка в .NET Framework (Майкрософт)](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
