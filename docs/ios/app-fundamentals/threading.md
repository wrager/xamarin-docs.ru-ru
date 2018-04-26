---
title: Потоки
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 63a213a62021923ac6dae8b080f3f8931621251d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="threading"></a>Потоки

Xamarin.iOS среда выполнения предоставляет разработчикам доступ к .NET threading API-интерфейсы, как явно, при работе с потоками (`System.Threading.Thread, System.Threading.ThreadPool`) и неявным образом при использовании шаблоны асинхронного делегата или методов BeginXXX, а также полный диапазон из API-интерфейсы, поддерживающие Библиотека параллельных задач.



Xamarin настоятельно рекомендует использовать [библиотека параллельных задач](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) для создания приложений по нескольким причинам:
-  Библиотека параллельных задач планировщик по умолчанию будут делегировать выполнение задачи в пуле потоков, который в свою очередь будет динамически увеличиваться количество потоков, как происходит процесс, избегая сценарии, где слишком много потоков в конечном итоге конкурировать за ресурсы процессора. 
-  Проще думать об операциях с точки зрения задач TPL. Можно легко управлять ими, запланировать их, сериализовать их выполнения или запуска многих параллельно с богатый набор интерфейсов API. 
-  Она является основой для программирования с C# async расширений языка. 


Медленно пула потоков будет увеличиваться число потоков, при необходимости в зависимости от числа ядер ЦП, доступных в системе, с нагрузкой системы и требования вашего приложения. Можно использовать этот пул потоков с помощью методов в `System.Threading.ThreadPool` или с помощью по умолчанию `System.Threading.Tasks.TaskScheduler` (частью *Parallel Frameworks*).

Обычно разработчики используют потоки, когда нужно создать отклика приложений и их не требуется блокировать основной пользовательский Интерфейс, запустить цикл.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Разработка приложений отвечать на запросы

Доступ к элементам пользовательского интерфейса должен быть предоставлен в том же потоке, на котором выполняется основной цикл для вашего приложения. Если вы хотите внести изменения в основной пользовательский Интерфейс из потока, должен очередь код с помощью [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), следующим образом:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Выше вызывает код внутри делегата в контексте основного потока, не вызывая любой гонки, которые потенциально может аварийно завершить работу приложения.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Потоки и сборка мусора

Во время выполнения среда выполнения C целью создания и освобождения объектов. Если объекты помечаются для «auto выпуск», среда выполнения Objective-C будет освобождать эти объекты к ее текущего потока `NSAutoReleasePool`. Создает Xamarin.iOS `NSAutoRelease` пула для каждого потока из `System.Threading.ThreadPool` и для основного потока. Это расширение охватывает все потоки, созданные с помощью стандартных TaskScheduler в System.Threading.Tasks.

При создании собственных потоков с помощью `System.Threading` вы должны предоставить вы являетесь владельцем `NSAutoRelease` пул для предотвращения утечки данных. Чтобы сделать это, просто перенести потока в следующем фрагменте кода:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Примечание: С момента Xamarin.iOS 5.2 не нужно предоставить свой собственный `NSAutoReleasePool` больше будет указан автоматически для вас.


## <a name="related-links"></a>Связанные ссылки

- [Работа с потоком пользовательского интерфейса](~/ios/user-interface/ios-ui/ui-thread.md)
