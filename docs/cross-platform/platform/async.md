---
title: Обзор поддержки асинхронного выполнения
description: В этом документе описывается программирование с использованием async и await концепции, реализованные в C# 5, чтобы было проще написать асинхронный код.
ms.prod: xamarin
ms.assetid: F87BF587-AB64-4C60-84B1-184CAE36ED65
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 22878695d93ae79bbbfe1b99961587ff0bf957be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782012"
---
# <a name="async-support-overview"></a>Обзор поддержки асинхронного выполнения

_C# 5 появились два ключевых слов для упрощения асинхронного программирования: async и await. Эти ключевые слова можно написать простой код, который использует библиотеку параллельных задач для выполнения длительных операций (например, доступ к сети) в другом потоке и легко получить доступ к результатам по завершении. Последние версии Xamarin.iOS и Xamarin.Android поддерживают async и await — в этом документе приводятся разъяснения и пример использования нового синтаксиса с помощью Xamarin._

Поддержка асинхронного выполнения Xamarin основанная на моно 3.0 и обновляет профиль API из процесса мобильной версии Silverlight на мобильной версии .NET 4.5.

## <a name="overview"></a>Обзор

В этом документе появился новый async и await ключевые слова, а затем последовательно описан простые примеры реализации асинхронных методов в Xamarin.iOS и Xamarin.Android.

Более полное обсуждение новые асинхронные возможности C# 5 (включая большое количество примеров и различных сценариев использования) см. в документации MSDN [асинхронное программирование с использованием ключевых слов Async и Await](http://msdn.microsoft.com/library/vstudio/hh191443.aspx).

Образец приложения создает простой асинхронного веб-запроса (не блокируя основной поток), а затем обновляет пользовательский Интерфейс с загруженный html и количество символов.

 [![](async-images/AsyncAwait_427x368.png "Образец приложения создает простой асинхронного веб-запроса не блокируя основной поток, а затем обновляет пользовательский Интерфейс с загруженный html и количество символов")](async-images/AsyncAwait.png#lightbox)

Поддержка асинхронного выполнения Xamarin основанная на моно 3.0 и обновляет профиль API из процесса мобильной версии Silverlight на мобильной версии .NET 4.5.

## <a name="requirements"></a>Требования

Возможности C# 5 требуют моно 3.0, который входит в состав Xamarin.iOS 6.4 и Xamarin.Android 4.8. Будет предложено обновить ваш Mono, Xamarin.iOS, Xamarin.Android и Xamarin.Mac его преимущества.

## <a name="using-async-amp-await"></a>Использование метода async &amp; await

 `async` и `await` , новые возможности языка C#, которые работают совместно с библиотеки параллельных задач, чтобы облегчить написание кода потоков для выполнения длительных задач без блокирования основного потока приложения.

## <a name="async"></a>async

### <a name="declaration"></a>Объявление

`async` Ключевое слово помещается в объявлении метода (или на лямбда-выражение или анонимный метод), чтобы указать, что она содержит код, который может выполняться асинхронно, т. е. не блокируют вызывающий поток.

Метод, помеченный атрибутом `async` должен содержать хотя бы один await выражения или оператора. Если не `await`s присутствуют в методе, он будет выполняться синхронно (же, как если бы не `async` модификатор). Это приведет также к Предупреждение компилятора (но не ошибка).

### <a name="return-types"></a>Типы возвращаемых значений

Асинхронный метод должен возвращать `Task`, `Task<TResult>` или `void`.

Укажите `Task` возвращаемый тип, если метод не возвращает любое другое значение.

Укажите `Task<TResult>` Если метод должен возвращать значение, где `TResult` тип возвращаемых (такие как `int`, например).

`void` Возвращаемый тип используется главным образом для обработчиков событий, которые требуют его. Код, который вызывает асинхронные методы, возвращающие void не может `await` на результат.

### <a name="parameters"></a>Параметры

Асинхронные методы не могут объявлять `ref` или `out` параметров.

## <a name="await"></a>await

Оператор await можно применения к задаче в метод, помеченный как async. Он вызывает метод, чтобы остановить выполнение в этой точке и дождаться завершения задачи.

С помощью await не блокирует вызывающий поток, а управление возвращается вызывающему объекту. Это означает, что вызывающий поток не заблокирован, так например пользователь потока интерфейса не будет блокироваться при ожидание задачи.

По завершении задачи метод возобновляет выполнение в той же точке в коде. Сюда входят возврат в область действия try блока try-catch-finally (при его наличии). await нельзя использовать в блоке catch или finally.

Дополнительные сведения о [await в библиотеке MSDN](http://msdn.microsoft.com/library/vstudio/hh156528.aspx).

## <a name="exception-handling"></a>Обработка исключений

Исключения, возникающие внутри асинхронного метода хранятся в задаче и вызывается, если задача является `await`ed. Эти исключения можно перехвачено и обработано внутри блока try-catch.

## <a name="cancellation"></a>Отмена

Асинхронные методы, принимающие длительное время, должны поддерживать отмену. Как правило отмены вызывается следующим образом:

- Объект `CancellationTokenSource` создается объект.
- `CancellationTokenSource.Token` Экземпляр передается отменяемую асинхронного метода.
- Запрошена отмена путем вызова `CancellationTokenSource.Cancel` метод.

Затем задача отменяет саму себя и подтверждает отмену.

Дополнительные сведения об отмене см. в разделе [Отмена асинхронной задачи](http://msdn.microsoft.com/library/vstudio/jj155761.aspx) на сайте MSDN.

## <a name="example"></a>Пример

Загрузить [пример решения Xamarin](https://developer.xamarin.com/samples/mobile/AsyncAwait/) (для iOS и Android) для просмотра рабочий пример `async` и `await` в мобильных приложениях. В этом разделе более подробно рассматривается в примере кода.

### <a name="writing-an-async-method"></a>Написание асинхронный метод

Следующий метод показывает, как код `async` метод с `await`ed задач:

```csharp
public async Task<int> DownloadHomepage()
{
    var httpClient = new HttpClient(); // Xamarin supports HttpClient!

    Task<string> contentsTask = httpClient.GetStringAsync("http://xamarin.com"); // async method!

    // await! control returns to the caller and the task continues to run on another thread
    string contents = await contentsTask;

    ResultEditText.Text += "DownloadHomepage method continues after async call. . . . .\n";

    // After contentTask completes, you can calculate the length of the string.
    int exampleInt = contents.Length;

    ResultEditText.Text += "Downloaded the html and found out the length.\n\n\n";

    ResultEditText.Text += contents; // just dump the entire HTML

    return exampleInt; // Task<TResult> returns an object of type TResult, in this case int
}
```

Обратите внимание на следующие моменты:

-  Объявление метода включает в себя `async` ключевое слово.
-  Возвращаемый тип — `Task<int>` , вызывающий код может получить доступ к `int` значение, которое вычисляется в этом методе.
-  Оператор return находится `return exampleInt;` которого является целое число со знаком — тот факт, что метод возвращает `Task<int>` является частью усовершенствования языка.


### <a name="calling-an-async-method-1"></a>Вызов асинхронного метода 1

Событие нажатия этой кнопки обработчик можно найти в приложении Android образец для вызова метода, описанные выше:

```csharp
GetButton.Click += async (sender, e) => {

    Task<int> sizeTask = DownloadHomepage();

    ResultTextView.Text = "loading...";
    ResultEditText.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await sizeTask;

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultTextView.Text = "Length: " + intResult ;
    // "returns" void, since it's an event handler
};
```

Примечания.

-  Этот анонимный делегат имеет префикс ключевого слова async.
-  Асинхронный метод DownloadHomepage Возвращает задачу, <int> , хранящегося в переменной sizeTask.
-  Код ожидание sizeTask переменной.  *Это* расположение, метод приостанавливается и управление возвращается в вызывающий код до завершения асинхронной задачи в своем собственном потоке.
-  Выполнение *не* приостановить при создании задачи в первой строке метода, несмотря на то что создаваемая задача. Ключевого слова await обозначает место, где выполнение приостановлено.
-  После завершения асинхронной задачи, intResult устанавливается и выполнение продолжается в исходном потоке, из строки await.


### <a name="calling-an-async-method-2"></a>Вызов асинхронного метода 2

В примере приложения iOS пример написан для демонстрации альтернативный подход немного по-разному. Вместо не использовать анонимного делегата в этом примере объявляется `async` обработчика событий, который назначен как обработчик регулярных событий:

```csharp
GetButton.TouchUpInside += HandleTouchUpInside;
```

Затем обработчик событий определяется, как показано ниже:

```csharp
async void HandleTouchUpInside (object sender, EventArgs e)
{
    ResultLabel.Text = "loading...";
    ResultTextView.Text = "loading...\n";

    // await! control returns to the caller
    var intResult = await DownloadHomepage();

    // when the Task<int> returns, the value is available and we can display on the UI
    ResultLabel.Text = "Length: " + intResult ;
}
```

Некоторые важные моменты:

-  Метод помечен как `async` , но возвращает `void` . Это обычно выполняется только для обработчиков событий (в противном случае будет возвращать `Task` или `Task<TResult>` ).
-  Код `await` s на `DownloadHomepage` метод непосредственно при присваивании переменной ( `intResult` ) в отличие от предыдущего примера, где мы использовали промежуточных `Task<int>` переменной для ссылки на задачу.  *Это* — это расположение, где управление возвращается вызывающему объекту до завершения асинхронного метода в другом потоке.
-  Если асинхронный метод завершения и возвращает, выполнение возобновляется с `await` означающее целочисленный результат возвращается и выводятся в графического пользовательского интерфейса.


## <a name="summary"></a>Сводка

Использование метода async и await значительно упрощает код, необходимый для создания длительные операции в фоновых потоках, не блокируя основной поток. Они также упрощают доступ к результатам, если задача была завершена.

В этом документе, давших обзор новых ключевых слов и примеры для Xamarin.iOS и Xamarin.Android.



## <a name="related-links"></a>Связанные ссылки

- [AsyncAwait (пример)](https://developer.xamarin.com/samples/mobile/AsyncAwait/)
- [Обратные вызовы от наших поколений, перейдите к инструкции](http://tirania.org/blog/archive/2013/Aug-15.html)
- [Данные (iOS) (пример)](https://developer.xamarin.com/samples/monotouch/Data/)
- [HttpClient (iOS) (пример)](https://developer.xamarin.com/samples/monotouch/HttpClient/)
- [MapKitSearch (iOS) (пример)](https://github.com/xamarin/monotouch-samples/tree/master/MapKitSearch)
- [Веб-семинар: C# Async в iOS и Android (видео)](http://xamarin.wistia.com/medias/k27mc627xz)
- [Асинхронное программирование с использованием Async и Await (MSDN)](http://msdn.microsoft.com/library/vstudio/hh191443.aspx)
- [Точная настройка асинхронного приложения (MSDN)](http://msdn.microsoft.com/library/vstudio/jj155761.aspx)
- [Await, а пользовательский Интерфейс и взаимоблокировки. Вот это да! (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2011/01/13/10115163.aspx)
- [Обработка задач по мере завершения (MSDN)](http://blogs.msdn.com/b/pfxteam/archive/2012/08/02/processing-tasks-as-they-complete.aspx)
- [Task-based Asynchronous Pattern (TAP)](http://msdn.microsoft.com/library/hh873175.aspx) (Асинхронный шаблон, основанный на задачах (TAP))
- [Асинхронности в C# 5 (Eric Липперта по) — о введении ключевые слова](http://blogs.msdn.com/b/ericlippert/archive/2010/11/11/whither-async.aspx)
