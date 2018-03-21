---
title: "Структура API"
description: "Перспектива в области конструктора Xamarin.iOS API"
ms.topic: article
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 8c336799a4d46359a78432837101dad43b572aea
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2018
---
# <a name="api-design"></a>Структура API

Помимо базовых библиотеках базовых классов, которые являются частью моно [Xamarin.iOS](http://www.xamarin.com/iOS) поставляется с привязок для различных iOS интерфейсы API, позволяющие разработчикам создавать приложения с машинным кодом iOS с моно.

В основе Xamarin.iOS отсутствует механизм взаимодействия, связывающим world со всем миром Objective-C, как, а также привязки для iOS API на основе C как CoreGraphics и [OpenGLES](#OpenGLES).

Низкоуровневые среда выполнения для взаимодействия с кодом C цель находится в [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). На основе этого, привязки для [Foundation](#MonoTouch.Foundation), CoreFoundation и [UIKit](#MonoTouch.UIKit) предоставляются.

## <a name="design-principles"></a>Принципы разработки

Ниже приведено несколько наши принципы проектирования для привязки Xamarin.iOS (они также применяться к Xamarin.Mac моно привязки для Objective-C в OS X):

- Следуйте указаниям по разработке Framework
- Позволяют разработчикам подкласс Objective-C-классы:

  - Являются производными от существующего класса
  - Вызвать конструктор базового класса в цепочке
  - Переопределение методов должно выполняться с системой переопределение в C#

- Подкласс должны работать с стандартных конструкций C#
- Не следует предоставлять разработчикам селекторы Objective-c.
- Предоставляет механизм для вызова произвольных библиотеки Objective-C
- Выполнять основные задачи Objective-C просто и жесткие возможные задачи Objective-c.
- Предоставляют свойства Objective-C в виде свойств C#
- Предоставьте строго типизированные API:
- Повышения безопасности типов
- Свести к минимуму ошибки времени выполнения
- Получить доступ к intellisense интегрированной среды разработки для типов возвращаемых значений
- Позволяет документации всплывающее окно интегрированной среды разработки
- Рекомендуем исследования API-интерфейсы в интегрированной среде разработки:
- Собственные типы C#:

    - Пример: вместо предоставления массива слабо типизированной следующим образом:
        ```
        NSArray *getViews
        ```
        Мы предоставляем их со строгими типами, следующим образом:
    
        ```
        NSView [] Views { get; set; }
        ```
    
    Это дает возможность автоматического завершения при просмотре API Visual Studio для Mac, а также позволяет всем `System.Array` операций на возвращаемое значение и позволяет возвращаемое значение для участия в LINQ

- [NSString становится строкой](~/ios/internals/api-design/nsstring.md)
- Включить параметры int и uint, должно было перечислений в C# перечисления и перечисления C# с атрибутами [Flags]
- Вместо NSArray зависящий от типа объектов предоставлять массивов в качестве строго типизированный массив.
- События и уведомления, предоставляют пользователям возможность выбора из:

    - Значение по умолчанию — строго типизированную версию
    - Слабо типизированной версии для обработки вариантов использования

- Модель делегата поддержки Objective-C:

    - Система событий в C#
    - Предоставлять делегаты C# (лямбда-выражения, анонимные методы и System.Delegate) для Objective-C API-интерфейсы как «блоки»

### <a name="assemblies"></a>Сборки

Xamarin.iOS включает несколько сборок, составляющих *Xamarin.iOS профиль*. [Сборки](~/cross-platform/internals/available-assemblies.md) страница содержит дополнительные сведения.

### <a name="major-namespaces"></a>Основные пространства имен 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) пространства имен позволяет разработчикам устранить миров между C# и цель-C.
Это новую привязку, разработанных специально для iOS, на основе опыта из Cocoa # и Gtk #.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) пространство имен предоставляет базовые типы данных предназначен для взаимодействия с платформой Foundation Objective-C, который является частью iOS и является основой для объектно-ориентированного программирования на языке C. цель

Xamarin.iOS отражает в C# иерархия классов с целью C. Например, базовый класс Objective-C [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) можно использовать в C# через [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Несмотря на то, что это пространство имен предоставляет привязки для базовых типов Foundation Objective-C, в некоторых случаях мы сопоставленной базовых типов для типов .NET. Пример:

- Вместо работы с [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) и [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), среда выполнения предоставляет их как C# [строка](https://developer.xamarin.com/api/type/System.String/)s и строго типизированные [массива](https://developer.xamarin.com/api/type/System.Array/)s на протяжении API-интерфейса.

- Чтобы позволить разработчикам привязать сторонние API Objective-C, в других операций ввода-вывода интерфейсов API или интерфейсы API, которые в настоящее время не привязаны, Xamarin.iOS различные вспомогательные API представленный здесь.

Дополнительные сведения о привязке API-интерфейсы в разделе [Xamarin.iOS привязки генератор](~/cross-platform/macios/binding/binding-types-reference.md) раздела.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) тип является основой для всех привязок Objective-C. Типы Xamarin.iOS зеркально отображать два класса типов из API-интерфейсы CocoaTouch iOS: C (как правило, на которое как типы CoreFoundation) и Objective-C типы (они являются производными от класса NSObject).

Для каждого типа, отражающую неуправляемого типа, можно получить через собственный объект [обработки](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) свойство.

Хотя моно предоставит сбора мусора для всех объектов, `Foundation.NSObject` реализует [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) интерфейса. Это означает, можно явно освобождения ресурсов любого заданного NSObject без ожидания для сборщика мусора в назначим. Это важно при использовании большого объема NSObjects, например, UIImages, который может содержать указатели на большие блоки данных.

Если требуется выполнять детерминированное завершение вашего типа, переопределите [NSObject.Dispose(bool) метод](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) параметр для удаления является «bool disposing», и если задать значение true, оно означает, так как вызывается метод Dispose пользователя явно вызываемые () Dispose для объекта. Если значение равно false, это означает, что метод Dispose (bool disposing) вызывается из метода завершения в потоке метода завершения. []()


##### <a name="categories"></a>Категории

Начиная с Xamarin.iOS 8.10 имеется возможность создавать категории Objective-C из C#.

Это делается с помощью `Category` атрибут, указывающий тип для расширения в качестве аргумента для атрибута. Следующий пример для экземпляра будут расширены NSString.

    [Category (typeof (NSString))]

Каждый метод категории использует стандартный механизм для экспорта в Objective-C с помощью методов `Export` атрибута:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Все методы управляемого расширения должны быть статическими, но можно создать методы экземпляра Objective-C, используя стандартный синтаксис для методов расширения в C#:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

и первый аргумент для метода расширения экземпляр, для которого был вызван метод.

Полный пример:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

Этот пример добавляет метод экземпляра собственного toUpper класс NSString, который может быть вызвана из цели-C.

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Один сценарий, где это полезно, добавив метод весь набор классов в базе кода, например, это сделает все `UIViewController` экземпляров отчетов, их можно поворачивать:

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```


##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute является пользовательским атрибутом, который сообщает mtouch — средство развертывания Xamarin.iOS — для сохранения типа или члена типа, во время фазы время обработки приложения для уменьшения его размера.

Все члены, которые не имеют статических ссылок из приложения, подлежат удалению. Таким образом этот атрибут используется для пометки членов, не были упомянуты статически, но, по-прежнему необходимы вашему приложению.

Например, если вы динамически создаете экземпляры типов, для них нужно сохранять в коде конструктор по умолчанию. Если используется XML-сериализация, нужно сохранять свойства типов.

Этот атрибут можно применить для любого члена типа или для типа в целом. Если вы хотите сохранить весь тип, можно использовать синтаксис [Сохранить (AllMembers = true)] на тип.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>UIKit

[UIKit](https://developer.xamarin.com/api/namespace/UIKit/) пространство имен содержит взаимно-однозначное сопоставление для всех компонентов пользовательского интерфейса, которые составляют CocoaTouch в виде классов C#. API был изменен для выполнения соглашения, используемые в языке C#.

Делегаты C# предоставляются для выполнения распространенных операций. В разделе [делегаты](#Delegates) Дополнительные сведения.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Для OpenGLES, распространять [изменения версии](https://developer.xamarin.com/api/namespace/OpenTK/) из [OpenTK](http://www.opentk.com/) API, объектно ориентированного привязку OpenGL, который был изменен для использования CoreGraphics типы и структуры данных, а также предоставление доступа только к функции, доступные в iOS.

OpenGLES 1.1 функциональные возможности доступны через тип ES11.GL, задокументированы [здесь](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) типа.

OpenGLES 2.0 функциональные возможности доступны через тип ES20.GL, задокументированы [здесь](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) типа.

OpenGLES 3.0 функциональные возможности доступны через тип ES30.GL, задокументированы [здесь](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) типа.


### <a name="binding-design"></a>Привязка разработки

Xamarin.iOS не просто привязки к базовой платформе Objective-C. Он расширяет систему типов .NET и диспетчеризации системы лучше blend C# и цель-C.

Так же, как P/Invoke — это эффективное средство для вызова собственных библиотек в Windows и Linux или IJW поддержки может использоваться как для COM-взаимодействия в Windows, Xamarin.iOS расширяет среда выполнения поддерживает C# объекты привязки к объектам Objective-C.

Обсуждение в следующих разделах, не является обязательным для пользователей, которые создают приложения, Xamarin.iOS, но могут помочь разработчикам понимать, как действия выполняются и поможет их при создании более сложных приложений.



#### <a name="types"></a>Типы

Где имело смысл, типов C#, предоставляются вместо низкоуровневые Foundation типов, чтобы среда C#.  Это означает, что [типа «string» C# использует API вместо NSString](~/ios/internals/api-design/nsstring.md) и использует вместо предоставления NSArray строго типизированных массивов в C#.

Как правило, в конструкторе Xamarin.iOS и Xamarin.Mac базового `NSArray` объекту не предоставляется. Вместо этого среда выполнения автоматически преобразует `NSArray`s в строго типизированные массивы некоторых `NSObject` класса. Таким образом Xamarin.iOS не предоставляет слабо типизированных методов, например GetViews для возврата NSArray:

```csharp
NSArray GetViews ();
```

Вместо этого привязка предоставляет строго типизированные возвращаемое значение следующим образом:

```csharp
UIView [] GetViews ();
```

Несколько методов, предоставляемых в `NSArray`, для тупиковых ситуаций, где вы можете использовать `NSArray` напрямую, но их использование не рекомендуется в API определения привязки.

Кроме того, в **классический API** вместо предоставления `CGRect`, `CGPoint` и `CGSize` из CoreGraphics API, мы заменили с `System.Drawing` реализации `RectangleF`, `PointF`и `SizeF` как они могут помочь разработчикам сохранить существующий OpenGL код, использующий OpenTK. При использовании новый 64-разрядный **единой API**, следует использовать CoreGraphics API.

<a name="Inheritance" />

#### <a name="inheritance"></a>Наследование

Структура Xamarin.iOS API позволяет разработчикам добавлять расширения собственные типы Objective-C таким же образом, что они бы расширить тип C#, используя ключевое слово «override» в производном классе, а также цепочки к базовой реализации с помощью ключевого слова C# «базовый».

Такой подход позволяет разработчикам не проводить с Objective-C селекторы в рамках процесса разработки, так как вся система Objective-C уже упакован в Xamarin.iOS библиотеки.


#### <a name="types-and-interface-builder"></a>Типы и интерфейс построителя

При создании классов .NET, которые являются экземплярами типов, созданный построителем интерфейса, необходимо указать конструктор, который принимает один `IntPtr` параметра.
Это необходимо выполнить привязку экземпляра управляемого объекта с неуправляемыми объектами.
Код состоит из одной строки, следующим образом:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Делегаты

Objective-C и C# имеют различные значения для делегата word на этих языках.

В мире Objective-C и в документации, вы найдете документации о CocoaTouch делегата обычно является экземпляром класса, который будет отвечать на несколько методов. Это очень похоже на C# интерфейс, с тем отличием, что методы не всегда являются обязательными.

Эти делегаты играют важную роль в UIKit и другие API CocoaTouch. Они используются для выполнения различных задач:

-  Для предоставления уведомления в код (аналогично доставки событий в C# или Gtk +).
-  Для реализации модели для элементов управления визуализации данных.
-  Накопитель поведения элемента управления.


Шаблон программирования был разработан для минимизации создание производных классов для изменения поведения элемента управления. Это решение работает аналогично дух другие наборы средств графического пользовательского интерфейса были выполнены за несколько лет: Gtk сигнал, WPF и Silverlight события, события Winforms, Qt гнезд и т. д. Во избежание необходимости сотни интерфейсы, по одному для каждого действия, или требующие разработчикам применять слишком много методов, которые не обязательно, Objective-C поддерживает необязательный метод определения. Это отличается от интерфейсов C#, которые требуют все методы должны быть реализованы.

В классах Objective-C, вы увидите, что классы, которые используют этот шаблон программирования предоставлять свойство, которое обычно называется `delegate`, которая необходима для реализации обязательные части интерфейса и нуль или более необязательных частей.

В Xamarin.iOS предлагается три взаимно исключают друг друга механизма для привязки к эти делегаты:

1.  [С помощью событий](#Via_Events).
2.  [Строго типизированные через `Delegate` свойство](#StrongDelegate)
3.  [Слабо типизированные через `WeakDelegate` свойство](#WeakDelegate)

Например, рассмотрим [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) класса. Это перенаправляется в [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) экземпляр, который назначен [делегировать](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) свойство.

<a name="Via_Events" />

##### <a name="via-events"></a>С помощью событий

Для многих типов Xamarin.iOS автоматически создаст соответствующий делегат, который будет пересылать `UIWebViewDelegate` звонков на события C#. Для `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) метода сопоставлен [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) событий.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) метода сопоставлен [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) событий.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) метода сопоставлен [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) событий.

Например Эта простая программа записывает время начала и окончания, при просмотре веб-сайт загрузки:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Свойства VIA

События полезны в тех случаях, когда может быть более одного подписчика на событие. Кроме того, события ограничены случаях там, где нет возвращаемого значения из кода.

Для случаев, когда код должен вернуть значение мы выбранные вместо этого для свойства. Это означает, что в определенный момент времени в объекте может быть установлен только один метод.

Например, можно использовать этот механизм для пропуска клавиатуры на экране на обработчик `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField` `ShouldReturn` Свойства в этом случае в качестве аргумента принимает делегат, который возвращает логическое значение и определяет ли TextField делать что-то с возвращаемого нажатие кнопки. В данном методе мы возвращаем *true* вызывающему объекту, но удалим клавиатуры на экране (это происходит, когда вызывает textfield `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Строго типизированные через свойство делегата

Если вы не хотите использовать события, можно предоставить свой собственный [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) подкласс и назначьте его [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) свойство. После назначения UIWebView.Delegate механизм диспетчеризации событий UIWebView больше не будет работать, и методы UIWebViewDelegate будет вызываться при возникновении соответствующего события.

Например этот простой тип записывает время, необходимое для загрузки веб-представление:

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Выше используется в коде следующим образом:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Это приведет к созданию UIWebViewer и даст его для отправки сообщений в экземпляре средство уведомления, класс, который мы создали для обработки сообщений.

Этот шаблон также используется для управления поведением для определенных элементов управления, например в случае UIWebView [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) свойство позволяет `UIWebView` экземпляр для элемента управления ли `UIWebView` загрузит страницы или нет.

Шаблон также используется для предоставления данных по запросу для нескольких элементов управления. Например [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) управления является мощным прорисовки таблиц — и вид и содержимое определяются экземпляр [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Слабо типизированные через свойство WeakDelegate

В дополнение к строго типизированное свойство также есть слабое типизированный делегат, который разработчик может привязать действия по-разному, при необходимости.
Строго типизированный Everywhere `Delegate` свойство также представлено в привязке Xamarin.iOS, соответствующий `WeakDelegate` также предоставляется свойство.

При использовании `WeakDelegate`, вы несете ответственность за правильно декорирования класса с помощью [Экспорт](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) атрибут, чтобы задать область выделения. Пример:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Обратите внимание, что когда `WeakDelegate` присваивалось свойству `Delegate` свойство не используется. Кроме того Если вы реализуете метод унаследованного базового класса, которую нужно [экспорта], необходимо его открытого метода.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>Сопоставление шаблон делегат Objective-C, C&#35;

При появлении Objective-C образцов, которые выглядят следующим образом:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Это указывает, что язык для создания и создания экземпляра класса «SomethingDelegate» и присвойте значение свойству делегат переменной foo. Этот механизм поддерживается Xamarin.iOS и C# синтаксис является:

```csharp
foo.Delegate = new SomethingDelegate ();
```

В Xamarin.iOS предоставлены строго типизированные классы, которые сопоставляются Objective-C классов делегатов. Их использование, можно будет создание подклассов и переопределение методов, определенных реализацией Xamarin.iOS. Дополнительные сведения о том, как они работают см. раздел «моделей» ниже.


##### <a name="mapping-delegates-to-c35"></a>Сопоставление делегаты в C&#35;

В целом UIKit использует Objective-C делегаты в двух формах.

Первая форма предоставляет интерфейс для модели компонентов. Например как механизм для предоставления данных по запросу для представления, такие как средства хранения данных для представления списка.  В таких случаях следует всегда создать экземпляр правильный класс и присвоить переменной.

В следующем примере представлены `UIPickerView` с реализацией для модели, которая использует строки:

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

Вторая форма является предоставление уведомления о событиях. В таких случаях несмотря на то, что мы по-прежнему предоставления доступа к API в форме, описанные выше, мы также предоставляем C# событий, которые должны быть проще использовать для быстрого операций и интеграции Анонимные делегаты и лямбда-выражения в C#.

Например, можно подписаться на `UIAccelerometer` событий:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Два варианта доступны, где они смысл, но программист, то необходимо выбрать одно из них. При создании собственного экземпляра строго типизированные респондент или делегат и назначьте его, события C# не будет работать. При использовании C# события методов в классе респондент или делегат никогда не вызываться.

Предыдущем примере, где используется `UIWebView` могут быть написаны с использованием лямбда-выражений C# 3.0 следующим образом:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Реагирование на события

Иногда в коде C цель обработчики событий для нескольких элементов управления и поставщиков данных для нескольких элементов управления будет размещаться в том же классе. Это можно сделать, так как классы для обработки сообщений, и при условии, что классы для обработки сообщений, возможно, для связывания объектов.

Как описано ранее Xamarin.iOS поддерживает оба C# на основе событий модели программирования и шаблоне делегат Objective-C, где можно создать новый класс, реализует делегата и переопределяет необходимыми методами.

Можно также для поддержки шаблона в Objective-C где ответчики для нескольких различных операций всех размещенных в одном экземпляре класса. Чтобы сделать это, однако, необходимо использовать низкоуровневые функции Xamarin.iOS привязки.

Например, если требуется, чтобы реагировать на оба класса `UITextFieldDelegate.textFieldShouldClear`: сообщение и `UIWebViewDelegate.webViewDidStartLoad`: в том же экземпляре класса необходимо использовать в объявлении атрибута [экспорта]:

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

Имена методов не важны; все из которых имеет значение являются строками, передаваемая атрибуту [экспорта].

При использовании такой стиль программирования, убедитесь, что параметры C# соответствует фактические типы, которые передают подсистемы среды выполнения.

<a name="Models" />

#### <a name="models"></a>Модели

Средства хранения UIKit, или ответчики, реализованных с помощью вспомогательных классов они обычно именуются в код Objective-C делегаты, и они реализуются как протоколы.

Протоколы Objective C представляют собой своего рода интерфейсов, но они поддерживают необязательные методы – то есть не все методы, необходимо для протокола для работы.

Существует два способа реализации модели. Можно реализовывать его вручную или использовать существующие определения строго типизированной.


При реализации класса, который не привязан, Xamarin.iOS, ручной механизм не требуется. Это очень просто:

-  Флаг класса для регистрации в среде выполнения
-  Примените атрибут [экспорта] с именем фактического селектора в каждый метод, который вы хотите переопределить
-  Создать экземпляр класса и передать его.

Например следующие реализации только одного из необязательных методов в определении протокола UIApplicationDelegate:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Имя селектора Objective-C («applicationDidFinishLaunching:») объявлена с атрибутом экспорта и класс, зарегистрированный с `[Register]` атрибута.

Xamarin.iOS предоставляет строго типизированные объявления, готовой к использованию, которых не требуется вручную привязки. Чтобы обеспечить поддержку этой модели программирования, среда выполнения Xamarin.iOS поддерживает атрибут [модель] в объявлении класса. Это позволяет сообщить среды выполнения, он должен не подключить все методы в классе, если методы являются явно реализованы.

Это означает, что в UIKit, классы, представляющие протокол с необязательные методы записываются следующим образом:

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Если необходимо реализовать модель, которая реализует только некоторые методы, необходимо выполнить всего для переопределения методов и не учитывать другие методы. Среда выполнения будет только подключения перезаписан методов, не исходного методы во всем мире Objective-C.

Является эквивалентом в предыдущем примере вручную:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Преимущества заключаются в том, что нет необходимости получать в файлы заголовков Objective-C, чтобы найти селектор, типы аргументов или сопоставление с C# и получение intellisense из Visual Studio для Mac, а также строгие типы


#### <a name="xib-outlets-and-c35"></a>XIB выходов и C&#35;

> [!IMPORTANT]
> В этом разделе объясняется интеграция IDE с торговцам при использовании XIB файлов. При использовании конструктора Xamarin для операций ввода-вывода, это все заменяется, введя имя в разделе **удостоверение > имя** в разделе "Свойства" IDE, как показано ниже:
>
> [![](images/designeroutlet.png "Введите имя элемента в конструкторе iOS")](images/designeroutlet.png#lightbox)
>
>Дополнительные сведения о конструкторе iOS см. в статье [введение в конструктор iOS](~/ios/user-interface/designer/introduction.md#how-it-works) документа.

Она представляет собой низкоуровневые Описание интеграции выходами с помощью C# и предоставляется для опытных пользователей Xamarin.iOS. Когда с помощью Visual Studio для Mac сопоставление выполняется автоматически в фоновом с помощью автоматически созданного кода во время полета автоматически.

При разработке пользовательского интерфейса с помощью построителя интерфейс будет только проектирование внешнего вида приложения, а затем устанавливает некоторые соединения по умолчанию. Если вы хотите программным путем извлечения сведений, изменения поведения элемента управления во время выполнения или изменения элемента управления во время выполнения, требуется привязать некоторые элементы управления в управляемом коде.

Это выполняется в несколько этапов:

1.  Добавить **объявление розетки** для вашей **владелец файла**.
1.  Элемент управления будет подключаться **владелец файла**.
1.  Храните пользовательского интерфейса, а также подключения в файле XIB или NIB.
1.  Загрузите файл NIB во время выполнения.
1.  Доступ к переменной розетке.


В документации Apple для создания интерфейсов с помощью интерфейса построителя рассматриваются шаги (1) по (3).

При использовании Xamarin.iOS, приложение должно создать класс, производный от UIViewController. Он реализован его следующим образом:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Затем для загрузки вашей ViewController из файла NIB, это можно сделать:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Пользовательский интерфейс будет загружен из NIB. Теперь для доступа к выходам, бывает необходимо сообщить среде выполнения, что требуется доступ к ним. Чтобы сделать это, `UIViewController` подкласс должен объявлять свойства и пометить их атрибутом [подключение]. Пример:

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

Реализация свойств является тот, который фактически выполняет выборку и сохраняет значение для фактического собственного типа.

Не нужно беспокоиться об этом при использовании Visual Studio для Mac и InterfaceBuilder. Visual Studio для Mac автоматически отражает все выходы, объявленные с помощью кода в разделяемый класс, который компилируется как часть проекта.

#### <a name="selectors"></a>Селекторы

Основное понятие программирования C цель — селекторов. Интерфейсы API, которые требуется передать селектора или ожидает, что ваш код для реагирования на селектора часто встречается.

Создание нового селекторы в C# является очень простым — просто создайте новый экземпляр `ObjCRuntime.Selector` и использовать в любом месте в API, который требуется для результат. Пример:

```csharp
var selector_add = new Selector ("add:plus:");
```

Для C# метод реагируют на вызов селектора, он должен наследовать из `NSObject` тип и метод C# должен быть снабжен атрибутом имени селектора с помощью `[Export]` атрибута. Пример:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Обратите внимание, что селектор имена **должен** должны совпадать, включая все промежуточные и конечные двоеточия («:»), если он существует.

#### <a name="nsobject-constructors"></a>Конструкторы NSObject

Большинство классов в Xamarin.iOS, который является производным от `NSObject` будет предоставлять конструкторам, специфичным для функциональность объекта, но они также будут предоставлены различные конструкторы, которые неочевидным.

Конструкторы используются следующим образом:

```csharp
public Foo (IntPtr handle)
```

Этот конструктор используется для создания экземпляра класса, когда требуется сопоставить класс неуправляемый класс среды выполнения. Это происходит при загрузке файла XIB или NIB.  На этом этапе выполнения C цель должна создать объект в неуправляемой среде, и этот конструктор будет вызываться для инициализации управляемой стороне.

Как правило необходимо выполнить достаточно вызвать конструктор базового класса с помощью маркера параметра и в тексте, выполните инициализацию, которая необходима.

```csharp
public Foo ()
```

Это конструктор по умолчанию для класса, и в Xamarin.iOS, предоставляемых классами, это между инициализирует класс Foundation.NSObject и все классы, а в конце связан это Objective-C `init` метод в классе.

```csharp
public Foo (NSObjectFlag x)
```

Этот конструктор используется для инициализации экземпляра, но запретить вызов метода «init» Objective-C в конце кода. Это обычно используется, если вы уже зарегистрировали для инициализации (при использовании `[Export]` на ваш конструктор) или когда еще вашей инициализации через другой среднее.

```csharp
public Foo (NSCoder coder)
```

Этот конструктор предназначен для случаев, где выполняется инициализация объекта из экземпляра NSCoding. Дополнительные сведения см. в разделе Apple [архивы и руководство по программированию сериализации.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Исключения

Структура Xamarin.iOS API не вызывают исключения Objective-C, как C# исключения. Структура обеспечивает, что нет сборки мусора отправляться во всем мире Objective-C, в первую очередь и создают все исключения, которые должны создаваться с самой привязке до когда-либо недопустимые данные передаются во всем мире Objective-C.

#### <a name="notifications"></a>Уведомления

В iOS и OS X разработчики могут подписаться на уведомлений, рассылаемых по базовой платформы. Это делается с помощью `NSNotificationCenter.DefaultCenter.AddObserver` метод. `AddObserver` Метод принимает два параметра, а одно уведомление, которое вы хотите подписаться; другой — метод, вызываемый при возникновении уведомления.

В Xamarin.iOS и Xamarin.Mac ключей для различных уведомлений размещаются в классе, которое запускает уведомления. Например, уведомления, вызванные `UIMenuController` размещенные как `static NSString` свойства в `UIMenuController` классов с именем «Уведомления».

### <a name="memory-management"></a>Управление памятью

Xamarin.iOS имеет сборщику мусора, берет на себя освобождения ресурсов для вас, когда они больше не используется. В дополнение к сборщику мусора все объекты, производные от `NSObject` реализовать `System.IDisposable` интерфейса.

#### <a name="nsobject-and-idisposable"></a>NSObject и IDisposable

Предоставление доступа к `IDisposable` интерфейс является удобным способом помощь разработчикам в освобождение объектов, которые могут инкапсулировать большие блоки памяти (например, `UIImage` может выглядеть просто указатель безобидно, но может указывать на изображение 2 мегабайта ) и другие важные и конечное ресурсы (например, декодирования видео буфер).

NSObject реализует интерфейс IDisposable, а также [шаблон .NET Dispose](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx). Это позволяет разработчикам Этот подкласс NSObject, чтобы переопределить поведение Dispose и освободить свои ресурсы по требованию. Например рассмотрим этот контроллер представление, хранящая вокруг множество изображений:

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

При удалении управляемого объекта, он не используется. По-прежнему возможно, ссылки на объекты, но объект недопустим для исчезнувшим на этом этапе. Некоторые интерфейсы API .NET обеспечения этого путем создания исключения ObjectDisposedException, если при попытке открыть любые методы на уничтоженном объекте — например:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

Даже если по-прежнему можно получить доступ к переменной «image», что это действительно недопустимую ссылку и больше не указывает на объект Objective-C, удерживаются изображения.

Но объекта на языке C# не означает объект обязательно будут уничтожены. Вам достаточно освободить ссылку C# пришлось объекта. Это возможно, что среда Cocoa может сохранил ссылку вокруг для внутреннего использования. Например, если свойство имеет значение UIImageView образ изображения и последующего удаления изображения, базовый UIImageView сделали собственную ссылку и будет хранить ссылку на этот объект до завершения его использования.

#### <a name="when-to-call-dispose"></a>Когда следует вызывать Dispose.

Если вам требуются моно отбрасывая объекта нужно вызывать Dispose. Вариант использования можно при моно ничего не знает, что ваш NSObject фактически содержит ссылку к важному ресурсу, такие как память или пул сведения. В таких случаях следует вызывать метод Dispose для немедленного высвобождения ссылка на память, не дожидаясь Mono для выполнения цикла сборки мусора.

Внутренне, когда моно создает [NSString ссылки из C# строки](~/ios/internals/api-design/nsstring.md), удаляет их немедленно, чтобы уменьшить объем работы, сборщику мусора. Чем меньше объектов решения для обрабатывающих, тем быстрее сборщик Мусора будет выполняться.

#### <a name="when-to-keep-references-to-objects"></a>Когда следует хранить ссылки на объекты

Одним из побочных эффектов, имеющий автоматическое управление памятью — сборка Мусора, которые будут избавиться от неиспользуемых объектов, при условии, что нет ссылок на них. Это иногда может иметь неожиданные побочные эффекты, например, если создает локальную переменную для хранения контроллер представление верхнего уровня или окна верхнего уровня и имеющих те удалить за архивную копию.

Если не сохранять ссылку по вашей статическим или переменные экземпляра к объектам, моно будет вызывать метод Dispose() на них и их освобождает ссылку на объект. Так, как это может быть только невыполненные ссылки, среда выполнения C цель приведет к удалению объекта для вас.

## <a name="related-links"></a>Связанные ссылки

- [Привязка полей](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
