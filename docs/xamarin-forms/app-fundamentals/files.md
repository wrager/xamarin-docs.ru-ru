---
title: Обработка файлов в Xamarin.Forms
description: Обработка с помощью Xamarin.Forms файлов можно сделать с помощью внедренных ресурсов или создавали для собственного API-интерфейсы файловой системы.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 80fdedd6c5df15272e36e6ac9c1414a4f731123a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241937"
---
# <a name="file-handling-in-xamarinforms"></a>Обработка файлов в Xamarin.Forms

_Обработка с помощью Xamarin.Forms файлов можно сделать с помощью внедренных ресурсов или создавали для собственного API-интерфейсы файловой системы._

## <a name="overview"></a>Обзор

Код Xamarin.Forms выполняется на нескольких платформах, каждая из которых имеет свою собственную файловую систему. Это означает, что чтение и запись файлов большинство легко выполняется с помощью собственного API-интерфейсов файлового на каждой платформе. Кроме того внедренные ресурсы являются простым решением распределяйте файлы данных с помощью приложения.

В этом документе рассматриваются следующие общие обработки сценариев файлов:

-  [ **Файлы, внедренных в качестве ресурсов** ](#Loading_Files_Embedded_as_Resources) -файлов может доставляться в рамках приложения и загрузить с помощью API-интерфейса отражения.
-  [ **Сохранение и загрузка файлов** ](#Loading_and_Saving_Files) -пользователя для записи хранилище может быть реализована в собственном коде и затем получить доступ с помощью `DependencyService` .


Вызов компонентов сторонних **PCLStorage** также может использоваться для чтения и записи файлов для пользователя доступны хранилища из кода PCL.

Сведения об обработке файлов изображений [работа с образами](~/xamarin-forms/user-interface/images.md) страницы.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Загрузка файлов, внедренных в качестве ресурсов

Чтобы внедрить файл в **PCL** сборки, создание или добавление файла и убедитесь, что **действие при построении: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Настройка внедренных ресурсов действие построения](files-images/vs-embeddedresource-sml.png "EmbeddedResource BuildAction параметр")](files-images/vs-embeddedresource.png#lightbox "EmbeddedResource BuildAction параметр")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Текстовый файл, внедренных в PCL, Настройка действие построения внедренного ресурса](files-images/xs-embeddedresource-sml.png "EmbeddedResource BuildAction параметр")](files-images/xs-embeddedresource.png#lightbox "EmbeddedResource BuildAction параметр")

-----

`GetManifestResourceStream` используется для доступа к внедренного файла с помощью его **идентификатор ресурса**. По умолчанию, идентификатор ресурса — имя файла, в качестве префикса пространства имен по умолчанию для проекта, который он внедрен - в этом случае сборка является **WorkingWithFiles** и имя файла — **PCLTextResource.txt**, Поэтому идентификатор ресурса `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Переменной может затем использоваться для отображения текста или в противном случае используйте его в коде. Этот снимок экрана [пример приложения](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) показывает текста, отображаемого в `Label` элемента управления.

 [![Текстовый файл, внедренных в PCL](files-images/pcltext-sml.png "внедренные текстовый файл в PCL, отображаемый в приложении")](files-images/pcltext.png#lightbox "внедренные текстовый файл в PCL, отображаемый в приложении")

Загрузка и десериализации XML выполняется так же просто. В следующем коде показано файла XML, загрузки и десериализован из ресурса, а затем привязать к `ListView` для отображения. XML-файл содержит массив `Monkey` объектов (в примере кода определяется класс).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![XML-файл, внедренных в PCL, отображаемых в ListView](files-images/pclxml-sml.png "внедренный XML-файл в PCL, отображаемых в ListView")](files-images/pclxml.png#lightbox "внедренный XML-файл в PCL, отображаемых в ListView")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Внедрение в общих проектов

Общие проекты также могут содержать файлы как внедренные ресурсы, однако так как содержимое общий проект компилируются в ссылающейся проекты, префикс, используемый для внедренный ресурс файла, который можно изменить идентификаторы. Это означает, что идентификатор ресурса для каждого внедренного файла может быть разным для каждой платформы.

Существует два решения этой проблемы с общих проектов:

-  **Синхронизировать проекты** -изменение свойств проекта для каждой платформы, чтобы использовать **же** пространство имен по умолчанию и имя сборки. Затем это значение может быть «жестко задано» как префикс для внедренного ресурса идентификаторов в общий проект.
-  **директивы компилятора #if** -используйте директивы компилятора префикс идентификатора ресурсов и использовать это значение для динамического построения идентификатор необходимого ресурса.


Ниже приведен исходный код, иллюстрирующий второй вариант. Директивы компилятора, используемые для выбора префикс жестко закодировано ресурсов (который обычно является таким же, как пространство имен по умолчанию для ссылающегося проекта). `resourcePrefix` Переменная затем используется для создания идентификатора ресурса допустимым путем объединения его с именем файла внедренных ресурсов.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Организация ресурсов

Приведенных выше примерах предполагается, что файл внедрен в корне проект переносимой библиотеки Классов, в котором регистр идентификатор ресурса имеет форму **пространство_имен.имя_файла.расширение**, такие как `WorkingWithFiles.PCLTextResource.txt` и `WorkingWithFiles.iOS.SharedTextResource.txt`.

Можно организовать внедренные ресурсы в папках. При внедренный ресурс помещается в папку, имя папки становится частью Идентификаторы ресурсов (разделенных точками), так что становится формат идентификатора ресурса **Namespace.Folder.Filename.Extension**. Размещение файлов, используемый в пример приложения, в папку **MyFolder** сделает соответствующие идентификаторы ресурсов `WorkingWithFiles.MyFolder.PCLTextResource.txt` и `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Отладка внедренные ресурсы

Поскольку иногда бывает трудно понять, почему не загружаемых определенного ресурса, следующий код отладки можно добавить временно в приложение, чтобы гарантировать, что ресурсы настроены правильно. Будут выведены все известные ресурсы, встроенные в данной сборке для **ошибки** прокладки, чтобы помочь в отладке проблем загрузки ресурсов.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Сохранение и загрузка файлов

Поскольку Xamarin.Forms выполняется на нескольких платформах, каждый из которых свой собственный файловой системы не существует единого подхода для загрузки и сохранения файлов, созданных пользователем. Чтобы продемонстрировать, как сохранить и загрузить пример приложения включает экрана, который сохраняет и загружает пользовательский ввод - текстовые файлы по завершении экрана показан ниже:

 [![Сохранение и загрузка текст](files-images/saveandload-sml.png "сохранение и загрузка файлов в приложении")](files-images/saveandload.png#lightbox "сохранение и загрузка файлов в приложении")

Каждая платформа имеет структуру каталогов немного отличается и другую файловую систему возможности — например Xamarin.iOS и Xamarin.Android поддерживают большинство `System.IO` функции, но универсальной платформы Windows поддерживает только [ `Windows.Storage` ](/uwp/api/windows.storage/) API-интерфейсы.

Чтобы обойти эту проблему, в примере приложения определяет интерфейс, в Xamarin.Forms PCL для загрузки и сохранения файлов. Он предоставляет простой API для загрузки и сохранения текстовых файлов, которое будет храниться на устройстве.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

PCL кода затем использует [помощью DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) для получения ссылки на собственную реализацию интерфейса. Это позволяет переносимого кода делегировать Загрузка и сохранение файлов для класса, написанного на каждый из проектов конкретную платформу. В образце **Сохранить** и **нагрузки** записываются кнопки, как показано:

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Реализация затем необходимо добавить всех проектов платформы, прежде чем фактически файлы можно сохранять и загружать.

### <a name="ios-and-android"></a>iOS и Android

Реализация интерфейса для проектов Xamarin.iOS и Xamarin.Android могут совпадать. Ниже приведен исходный код, включая `[assembly: Dependency (typeof (SaveAndLoad))]` атрибут, необходимый для `DependencyService` для работы.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

UWP имеет другую файловую систему API — [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) — то есть используется для сохранения и загрузки файлов.
`ISaveAndLoad` Интерфейс может быть реализован, как показано ниже:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```

<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Сохранение и загрузка данных в общих проектов

Общие проекты поддерживают директивы компилятора его можно включить в один файл класса в общий проект кода под конкретную платформу (без использования `DependencyService`).
Один `SaveAndLoad` может быть записан класс, содержащий обе реализации выше, выборочно компилируются в ссылающейся проектов, с помощью директивы компилятора как `#if WINDOWS_PHONE`, `#if __IOS__`, и `#if __ANDROID__`.

## <a name="additional-information"></a>Дополнительные сведения

Проектов на основе PCL Xamarin.Forms можно также воспользоваться преимуществами [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([кода &amp; документации](https://pclstorage.codeplex.com/)) чтобы реализовать в виде кросс платформенных операций с файлами.


## <a name="summary"></a>Сводка

Этот документ на примере некоторых операций простой файл для загрузки внедренных ресурсов и сохранения и загрузки текста на устройстве. Разработчики могут реализовывать собственные API-интерфейсы собственного файла с помощью `DependencyService`, сделав его сложности, необходимую для обработки их требования к обработке файла.


## <a name="related-links"></a>Связанные ссылки

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Создание, запись и чтение файла (UWP)](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Примеры Xamarin.Forms](https://github.com/xamarin/xamarin-forms-samples)
- [Файлы книги](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
