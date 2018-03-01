---
title: "Сводка Глава 20. Async и файл ввода-вывода"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: f7c81cfb77772af219fe28f081e7f8636e118fb1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="summary-of-chapter-20-async-and-file-io"></a>Сводка Глава 20. Async и файл ввода-вывода

 Графический пользовательский интерфейс должен отвечать на события пользовательского ввода последовательно. Это означает, что вся обработка событий пользовательского ввода должно находиться в одном потоке, который часто называют *основной поток* или *поток пользовательского интерфейса*.

Пользователи ожидают, что графические интерфейсы пользователя реагировать. Это означает, что программа должна обрабатывать быстро событий пользовательского ввода. Если это невозможно, то обработка должна делегированием для вторичных потоков выполнения.

Несколько примеров программ в этой книге использовали [ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) класса. В этом классе [ `BeginGetReponse` ](https://developer.xamarin.com/api/member/System.Net.WebRequest.BeginGetResponse/p/System.AsyncCallback/System.Object/) метод запускает рабочий поток, который вызывает функцию обратного вызова при завершении. Однако этой функции обратного вызова выполняется в рабочий поток, поэтому необходимо вызвать программу [ `Device.BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) метод для доступа к интерфейсу пользователя.

Более современный подход для асинхронной обработки доступен в .NET и C#. Это включает в себя [ `Task` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task/) и [ `Task<TResult>` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.Task%3CTResult%3E/) классов и других типов в [ `System.Threading` ](https://developer.xamarin.com/api/namespace/System.Threading/) и [ `System.Threading.Tasks` ](https://developer.xamarin.com/api/namespace/System.Threading.Tasks/) пространства имен, а также 5.0 C# `async` и `await` ключевые слова. Это рассматривается в этой главе.

## <a name="from-callbacks-to-await"></a>Из обратные вызовы для ожидания

`Page` Сам класс содержит три асинхронные методы для отображения полей предупреждений:

- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) Возвращает `Task` объекта
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/System.String/) Возвращает `Task<bool>` объекта
- [`DisplayActionSheet`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) Возвращает `Task<string>` объекта

`Task` Объекты указывают, что эти методы реализовать на основе задач асинхронную модель, известный как TAP. Эти `Task` быстро возвращаются объекты из метода. `Task<T>` Возвращают значения составляют «обещания» значение типа `TResult` будут доступны после завершения задачи. `Task` Возвращаемое значение указывает асинхронного действия, который будет завершено, однако с значение не возвращается.

Во всех этих случаях `Task` завершается, когда пользователь закрывает окно с предупреждением.  

### <a name="an-alert-with-callbacks"></a>Предупреждение при обработке обратного вызова

[ **AlertCallbacks** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertCallbacks) образце показано, как обрабатывать `Task<bool>` возвращают объекты и `Device.BeginInvokeOnMainThread` вызовов с помощью методов обратного вызова.

### <a name="an-alert-with-lambdas"></a>Предупреждение с лямбда-выражения

[ **AlertLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertLambdas) образце демонстрируется использование анонимного лямбда-функции для обработки `Task` и `Device.BeginInvokeOnMainThread` вызовов.  

### <a name="an-alert-with-await"></a>Оповещение с помощью await

Более простой подход включает `async` и `await` ключевые слова, представленные в C# 5. [ **AlertAwait** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/AlertAwait) демонстрирует их использования.

### <a name="an-alert-with-nothing"></a>Оповещение ничего не

Если асинхронный метод возвращает `Task` вместо `Task<TResult>`, программа не нужно использовать любой из этих методов, если его не нужно знать после завершения выполнения асинхронной задачи. [ **NothingAlert** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/NothingAlert) примере демонстрируется это.

### <a name="saving-program-settings-asynchronously"></a>Сохранение параметров программ асинхронно

[ **SaveProgramChanges** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/SaveProgramSettings) образце показано использование [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) метод `Application` для сохранения параметров программы, поскольку они изменяют без переопределение `OnSleep` метод.

### <a name="a-platform-independent-timer"></a>Таймер независимая от платформы

Можно использовать [ `Task.Delay` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Delay/p/System.Int32/) для создания таймера независимая от платформы. [ **TaskDelayClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TaskDelayClock) примере демонстрируется это.

## <a name="file-inputoutput"></a>Файл ввода вывода

В большинстве случаев .NET [ `System.IO` ](https://developer.xamarin.com/api/namespace/System.IO/) пространство имен было источник поддержки файлового ввода-вывода. Несмотря на то, что некоторые методы в это пространство имен поддерживает асинхронные операции, большинство — нет. Пространство имен также поддерживает несколько обычных вызовов методов, выполняющих операции ввода-вывода файлов.

### <a name="good-news-and-bad-news"></a>Хорошие новости и плохие новости

Всех платформах, поддерживаемых в локальное хранилище Xamarin.Forms поддержки приложения & #x 2014; хранилище, закрытых для приложения.

Библиотеки Xamarin.iOS и Xamarin.Android включают в себя версию .NET, Xamarin, явно не настроенных для этих двух платформ. К ним относятся классы из `System.IO` , можно использовать для выполнения файлового ввода-вывода с локальным хранилищем приложений в этих двух платформ.

Тем не менее при поиске этих `System.IO` классов в Xamarin.Forms PCL, вы не найдете их. Проблема в том, полностью пересмотрен Microsoft файлового ввода-вывода для API среды выполнения Windows. Программы, предназначенные для Windows 8.1, Windows Phone 8.1 и универсальная платформа Windows не используют `System.IO` для файлового ввода-вывода.

Это означает, что вам требуется использовать [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) (сначала подробно [ **главе 9. Вызовы API платформой** ](chapter09.md) для реализации файлового ввода-вывода.

### <a name="a-first-shot-at-cross-platform-file-io"></a>Первый снимок при кросс платформенных файлового ввода-вывода

[ **TextFileTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileTryout) образец определяет [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/TextFileTryout/TextFileTryout/TextFileTryout/IFileHelper.cs) интерфейс для файлового ввода-вывода и реализации этого интерфейса на всех платформах. Тем не менее реализации среды выполнения Windows не работают с методы в этом интерфейсе, так как методы ввода-вывода файлов среды выполнения Windows являются асинхронными.

### <a name="accommodating-windows-runtime-file-io"></a>При размещении среды выполнения Windows файлового ввода-вывода

Программы, работающие в среде выполнения Windows используются классы из [ `Windows.Storage` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.aspx) и [ `Windows.Storage.Streams` ](https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.aspx) пространства имен для файлового ввода-вывода, включая приложения локального хранилища. Поскольку корпорация Майкрософт определила, что любая операция, в которых требуется более чем 50 мс должен быть асинхронным, чтобы избежать блокировки потока пользовательского интерфейса, эти методы ввода-вывода файла большей части являются асинхронными.

Кода, демонстрирующий этот новый подход будет в библиотеке, чтобы он может использоваться другими приложениями.

## <a name="platform-specific-libraries"></a>Библиотеки для конкретных платформ

Полезно для хранения многократно используемого кода в библиотеках. Это гораздо более надежной, когда разные части повторно используемый код для совершенно разных операционных систем.

[ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) решение демонстрирует один подход. Это решение содержит семь различных проектов:

- [**Xamarin.FormsBook.Platform**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform), обычный PCL Xamarin.Forms
- [**Xamarin.FormsBook.Platform.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS), библиотека классов iOS
- [**Xamarin.FormsBook.Platform.Android**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android), библиотека классов Android
- [**Xamarin.FormsBook.Platform.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.UWP), библиотеку классов для универсальных приложений Windows
- [**Xamarin.FormsBook.Platform.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Windows), переносимую библиотеку Классов для Windows 8.1.
- [**Xamarin.FormsBook.Platform.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinPhone), переносимую библиотеку Классов для Windows Phone 8.1
- [**Xamarin.FormsBook.Platform.WinRT**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT), общего проекта для кода, который является общим для всех платформ Windows

Все проекты отдельных платформы (за исключением элемента **Xamarin.FormsBook.Platform.WinRT**) имеют ссылки на **Xamarin.FormsBook.Platform**. Три проекта Windows имеется ссылка на **Xamarin.FormsBook.Platform.WinRT**.

Все проекты содержат статический `Toolkit.Init` метод, чтобы обеспечить загрузку библиотеки, если он является непосредственно не ссылается проект в решения xamarin.Forms создается приложение.

**Xamarin.FormsBook.Platform** проект содержит новый [ `IFileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/IFileHelper.cs) интерфейса. Все методы теперь имеют имена с `Async` суффиксы и возврат `Task` объектов.

**Xamarin.FormsBook.Platform.WinRT** проект содержит [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) класса среды выполнения Windows.

**Xamarin.FormsBook.Platform.iOS** проект содержит [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/FileHelper.cs) класс для операций ввода-вывода. Эти методы теперь должен быть асинхронным. Некоторые методы использовать асинхронные версии методов, определенных в `StreamWriter` и `StreamReader`: [ `WriteAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamWriter.WriteAsync/p/System.String/) и [ `ReadToEndAsync` ](https://developer.xamarin.com/api/member/System.IO.StreamReader.ReadToEndAsync()/). Другие преобразования результат, чтобы `Task` с помощью [ `FromResult` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.FromResult%7BTResult%7D/p/TResult/) метод.

**Xamarin.FormsBook.Platform.Android** проект содержит аналогичное [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/FileHelper.cs) класса для Android.

**Xamarin.FormsBook.Platform** проект также содержит [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/FileHelper.cs) класс, который облегчает использование `DependencyService` объекта.

Чтобы использовать эти библиотеки, решение приложений должен включать все проекты **Xamarin.FormsBook.Platform** решение, а каждый из проектов приложения должен иметь ссылку на соответствующей библиотеке в  **Xamarin.FormsBook.Platform**.

[ **TextFileAsync** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/TextFileAsync) решения демонстрируется использование **Xamarin.FormsBook.Platform** библиотеки. Каждый из проектов должен вызывать `Toolkit.Init`. Приложение использует асинхронный файловый ввод-вывод функции.

### <a name="keeping-it-in-the-background"></a>Сохранение его в фоновом режиме

Методы в библиотеках, которые выполняют вызовы несколько асинхронных методов & #x 2014; например `WriteFileAsync` и `ReadFileASync` методы в среде выполнения Windows [ `FileHelper` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/FileHelper.cs) класс & #x 2014; можно сделать несколько более эффективно, используя [ `ConfigureAwait` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task%3CTResult%3E.ConfigureAwait/p/System.Boolean/) метода Избегайте переключение обратно в поток пользовательского интерфейса.

### <a name="dont-block-the-ui-thread"></a>Не блокировали поток пользовательского интерфейса.

Иногда бывает привлекательным Избегайте использования `ContinueWith` или `await` с помощью [ `Result` ](https://developer.xamarin.com/api/property/System.Threading.Tasks.Task%3CTResult%3E.Result/) свойства для методов. Следует избегать для может блокировать поток пользовательского интерфейса или даже зависание приложения.

## <a name="your-own-awaitable-methods"></a>Свои собственные методы типа awaitable

Можно выполнять определенный код асинхронно путем передачи его в один из [ `Task.Run` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.Run/p/System.Action/) методы. Можно вызвать `Task.Run` в асинхронный метод, который обрабатывает некоторые издержки.

Различные `Task.Run` шаблонов, описаны ниже.

### <a name="the-basic-mandelbrot-set"></a>Базовый набор "Мандельброт"

Чтобы нарисовать "Мандельброт", установка в режиме реального времени, [ **Xamarin.Forms.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) библиотека имеет [ `Complex` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/Complex.cs) структуры, похожий на тот, в `System.Numerics` пространство имен.

[ **MandelbrotSet** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotSet) образце `CalculateMandeblotAsync` метод в файле кода, вычисляет базовый набор "Мандельброт" черно-белые и использует [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)говоря точечного рисунка.

### <a name="marking-progress"></a>Пометка хода выполнения

Уведомления о ходе выполнения из асинхронного метода можно создать экземпляр [ `Progress<T>` ](https://developer.xamarin.com/api/type/System.Progress%3CT%3E/) класса и определить пользовательский асинхронный метод, чтобы аргумент типа [ `IProgress<T>` ](https://developer.xamarin.com/api/type/System.IProgress%3CT%3E/). Это показано в [ **MandelbrotProgress** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotProgress) образца.

### <a name="cancelling-the-job"></a>Отмена заданий

Также можно написать асинхронный метод поддерживала отмену. Начать с классом [ `CancellationTokenSource` ](https://developer.xamarin.com/api/type/System.Threading.CancellationTokenSource/). [ `Token` ](https://developer.xamarin.com/api/property/System.Threading.CancellationTokenSource.Token/) Свойство является значением типа [ `CancellationToken` ](https://developer.xamarin.com/api/type/System.Threading.CancellationToken/). Аргумент передается в функцию асинхронной. Программа вызывает [ `Cancel` ](https://developer.xamarin.com/api/member/System.Threading.CancellationTokenSource.Cancel()/) метод `CancellationTokenSource` (обычно в ответ на действия пользователя) для отмены асинхронной функции.

Асинхронный метод может периодически проверять [ `IsCancellationRequested` ](https://developer.xamarin.com/api/property/System.Threading.CancellationToken.IsCancellationRequested/) свойство `CancellationToken` и завершать работу, если свойство является `true`, или просто вызвать [ `ThrowIfCancellationRequested` ](https://developer.xamarin.com/api/member/System.Threading.CancellationToken.ThrowIfCancellationRequested()/) метод в противном случае метод заканчивается [ `OperationCancelledException` ](https://developer.xamarin.com/api/type/System.OperationCanceledException/).

[ **MandelbrotCancellation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotCancellation) образце демонстрируется использование вызовов функции.

### <a name="an-mvvm-mandelbrot"></a>MVVM "Мандельброт"

[ **MandelbrotXF** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/MandelbrotXF) образец содержит расширенный пользовательский интерфейс, и он основан на основном [ `MandelbrotModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotModel.cs) и [ `MandelbrotViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter20/MandelbrotXF/MandelbrotXF/MandelbrotXF/MandelbrotViewModel.cs)классы:

[![Тройной экрана "Мандельброт" X F](images/ch20fg13-small.png ""Мандельброт" MVVM")](images/ch20fg13-large.png "MVVM "Мандельброт"")

## <a name="back-to-the-web"></a>Обратно в веб-сайте

[ `WebRequest` ](https://developer.xamarin.com/api/type/System.Net.WebRequest/) Класс, используемый в некоторых образцах использует проверенное асинхронный протокол, называется модели асинхронного программирования или APM. Такой класс можно преобразовать в современных TAP протокола, с помощью одного из `FromAsync` методы в [ `TaskFactory` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskFactory%3CTResult%3E/) класса. [ **ApmToTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20/ApmToTap) примере демонстрируется это.



## <a name="related-links"></a>Связанные ссылки

- [Полный текст Глава 20 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch20-Apr2016.pdf)
- [Образцы Глава 20](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter20)
- [Работа с файлами](~/xamarin-forms/app-fundamentals/files.md)
