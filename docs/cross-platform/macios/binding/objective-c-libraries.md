---
title: "Библиотеки C цель привязки"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8A832A76-A770-1A7C-24BA-B3E6F57617A0
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: f0e8dabc47352213d18d079ee9f8abb3e557b868
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="binding-objective-c-libraries"></a>Библиотеки C цель привязки

При работе с Xamarin.iOS или Xamarin.Mac, возможны случаи, где требуется использовать библиотеку стороннего Objective-C. В такой ситуации можно использовать для создания привязки C# для собственных библиотек Objective-C проектов Xamarin привязки. В проекте используются те же средства, используемые для передачи iOS и Mac API-интерфейсы для C#.

Этот документ описывает привязку API Objective-C, при связывании просто C API-интерфейсы для этого следует использовать стандартный механизм .NET [framework P/Invoke](http://mono-project.com/Dllimport).
Подробные сведения о том, как статическая компоновка библиотеки доступны на [связывания собственные библиотеки](~/ios/platform/native-interop.md) страницы.

См. наш дополнительное [привязки Справочник типы](~/cross-platform/macios/binding/binding-types-reference.md).
Кроме того, если вы хотите узнать больше о том, что происходит за кулисами, проверьте нашей [Общие сведения о привязке](~/cross-platform/macios/binding/overview.md) страницы.

Привязки могут быть нацелены на iOS и Mac библиотеки.
Эта страница описывает работать на устройстве iOS, привязка, однако Mac привязок очень похожи.

**Образцы кода для iOS**

Можно использовать [iOS привязки образец](https://github.com/xamarin/monotouch-samples/tree/master/BindingSample) проект, чтобы поэкспериментировать с привязками.

<a name="Getting_Started" />

## <a name="getting-started"></a>Начало работы

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Самый простой способ создания привязки является создание проекта Xamarin.iOS привязки.
Это можно сделать из Visual Studio для Mac, выбрав тип проекта **iOS > библиотеки > Библиотека привязок**:

[![](objective-c-libraries-images/00-sml.png "Сделать из Visual Studio для Mac, выбора типа проекта, библиотеке привязки библиотеки iOS")](objective-c-libraries-images/00.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Самый простой способ создания привязки является создание проекта Xamarin.iOS привязки.
Путем выбора типа проекта, это можно сделать из Visual Studio в Windows **Visual C# > iOS > привязок библиотека (iOS)**:

[![](objective-c-libraries-images/00vs-sml.png "iOS привязки библиотеки iOS")](objective-c-libraries-images/00vs.png#lightbox)

> [!IMPORTANT]
> Примечание: Привязки проектов для **Xamarin.Mac** поддерживается только в Visual Studio для Mac.

-----

Созданный проект содержит небольшой шаблон, который можно редактировать, он содержит два файла: `ApiDefinition.cs` и `StructsAndEnums.cs`.

`ApiDefinition.cs` Является определение контракта API-интерфейса, это файл, который описывает, как базовый API Objective-C проецируются в C#. Синтаксис и содержимое этого файла являются основной раздел обсуждения этого документа и ее содержимое ограничены интерфейсы C# и объявление делегата в C#. `StructsAndEnums.cs` Файл является файлом, можно будет ввести любые определения, необходимые интерфейсы и делегаты. Сюда входят значения перечисления и структуры, которые может использовать код.

<a name="Binding_an_API" />

## <a name="binding-an-api"></a>Привязка API-Интерфейс

Чтобы сделать привязку комплексное, требуется понять определение API Objective-C и ознакомиться с рекомендации по разработке .NET Framework.

Чтобы привязать библиотеку будет обычно начинаются со файл определения API. Файл определения API является просто C# исходный файл, содержащий интерфейсы C#, которые аннотированы небольшое число атрибутов, которые помогают управлять привязки.  Этот файл является то, что определяет контракт между C# и Objective-C.

Например это тривиальные api файла для библиотеки:

```csharp
using Foundation;

namespace Cocos2D {
  [BaseType (typeof (NSObject))]
  interface Camera {
    [Static, Export ("getZEye")]
    nfloat ZEye { get; }

    [Export ("restore")]
    void Restore ();

    [Export ("locate")]
    void Locate ();

    [Export ("setEyeX:eyeY:eyeZ:")]
    void SetEyeXYZ (nfloat x, nfloat y, nfloat z);

    [Export ("setMode:")]
    void SetMode (CameraMode mode);
  }
}
```

Приведенном выше примере определяется класс с именем `Cocos2D.Camera` , производный от `NSObject` базовый тип (этот тип поступают из `Foundation.NSObject`) и который определяет статическое свойство (`ZEye`), два метода, которые принимают аргументы и метод, принимает три аргументы.

Подробное обсуждение формата файла API и атрибуты, которые можно использовать, описанные в [файл определения API](~/cross-platform/macios/binding/objective-c-libraries.md) разделе ниже.

Чтобы получить полный привязки, обычно будет работать с четырех компонентов:

-  Файл определения API (`ApiDefinition.cs` в шаблоне).
-  Необязательно: любой перечисления, типы, структуры, необходимые для файла определения API (`StructsAndEnums.cs` в шаблоне).
-  Необязательно: Дополнительные источники, разверните созданную привязку, или предоставляют дополнительные C# понятное API (все файлы C#, добавьте в проект).
-  Собственная библиотека выполняется привязка.

На этой диаграмме представлены связи между двумя файлами.

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png "На этой диаграмме показана связь между двумя файлами")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.33.07-pm.png#lightbox)

Файл определения API: будет содержать только определения пространства имен и интерфейс (с любого элемента, интерфейс может содержать) и не должна содержать классы, перечисления, делегаты и структуры. Файл определения API является просто контракт, который будет использоваться для создания API.

Любой дополнительный код, что требуется, например перечисления или вспомогательные классы должны размещаться в отдельном файле, в примере выше «CameraMode» — это значение перечисления, который не существует в файле CS и должны размещаться в отдельном файле, например `StructsAndEnums.cs` :

```csharp
public enum CameraMode {
    FlyOver, Back, Follow
}
```

`APIDefinition.cs` Файл используется в сочетании с `StructsAndEnum` класса и используются для создания привязки основной библиотеки. Можно использовать как полученная библиотека-является, но обычно требуется настроить полученная библиотека, чтобы добавить некоторые функции C# для пользователей. Некоторые примеры реализации `ToString()` метода, предоставляют C# индексаторов, добавлять неявные преобразования, некоторые собственные типы или предоставляет строго типизированную версии некоторые методы. Эти усовершенствования хранятся в дополнительных файлах C#. Просто добавить в проект файлов C#, и они будут включены в процесс построения.

В следующем примере показано реализации кода в ваш `Extra.cs` файла. Обратите внимание, что вы используете разделяемые классы как они расширяют разделяемые классы, которые создаются на основе сочетания `ApiDefinition.cs` и `StructsAndEnums.cs` основы привязки:

```csharp
public partial class Camera {
    // Provide a ToString method
    public override string ToString ()
    {
         return String.Format ("ZEye: {0}", ZEye);
    }
}
```

Построение библиотеки будет создавать собственный привязки.

Для завершения этой привязки, следует добавить собственной библиотеки в проект.  Это можно сделать путем добавления собственной библиотеки в проект, перетащив собственной библиотеки из поиска в проект в обозревателе решений, или, щелкнув проект правой кнопкой мыши и выбрав **добавить**  >  **Добавить файлы** установите собственной библиотеки.
Собственные библиотеки по соглашению начинаются со слова «lib» и заканчивается расширением «.a». При этом Visual Studio для Mac добавляется два файла: `.a` файл и автоматически заполняемый C# файл, содержащий сведения о собственной библиотеки содержит:

 [![](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png "Собственные библиотеки по соглашению начинаться со слова lib и заканчиваться .a расширения")](objective-c-libraries-images/screen-shot-2012-02-08-at-3.45.06-pm.png#lightbox)

Содержимое `libMagicChord.linkwith.cs` файл содержит сведения об использовании этой библиотеки и указывает, что интегрированная СРЕДА для упаковки этого двоичного файла в файле DLL:

```csharp
using System;
using ObjCRuntime;

[assembly: LinkWith ("libMagicChord.a", SmartLink = true, ForceLoad = true)]
```

Полные сведения о том, как использовать атрибут LinkWith описаны в нашем [привязки Справочник типы](~/cross-platform/macios/binding/binding-types-reference.md).

Теперь при построении проекта вы получите `MagicChords.dll` файл, содержащий привязку и собственной библиотеки. Этот проект можно распространять или использовать получившийся файл DLL другим разработчикам для своих собственных.

Иногда требуется несколько значений перечисления, определения делегата или другие типы. Не размещайте их в файл определения API, как это просто контракт

 <a name="The_API_definition_file" />

## <a name="the-api-definition-file"></a>Файл определения API

Файл определения API состоит из нескольких интерфейсов. Интерфейсы в определении API будет преобразовано в объявлении класса, и должен быть снабжен атрибутом [[BaseType]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут, чтобы задать базовый класс для класса.

Вы можете спросить, почему мы не использовали классы вместо интерфейсов для определения контракта. Мы выбрали интерфейсы, так как он разрешен нам создать контракт для метода без необходимости указания тела метода в файле определения API или необходимости указания текст, которым пришлось порождает исключение или возвращает осмысленное значение.

Но так как мы используем интерфейс как основу для создания класса нам было необходимо прибегать к декорирования различные части контракта с атрибутами для привязки.

<a name="Binding_Methods" />

### <a name="binding-methods"></a>Методы привязки

Для привязки метода является простой привязки, которые можно сделать. Просто объявите метод в интерфейсе с соглашения об именовании C# и оформлять метод с [[экспортировать]](~/cross-platform/macios/binding/binding-types-reference.md) атрибута. Атрибут [экспорта] является то, какие связи ваше имя C# с именем Objective-C во время выполнения Xamarin.iOS. Параметр экспорта атрибута является имя селектора Objective-C, некоторые примеры:

```csharp
// A method, that takes no arguments
[Export ("refresh")]
void Refresh ();

// A method that takes two arguments and return the result
[Export ("add:and:")]
nint Add (nint a, nint b);

// A method that takes a string
[Export ("draw:atColumn:andRow:")]
void Draw (string text, nint column, nint row);
```

Выше примерах показано, как связать методы экземпляра. Чтобы привязать статические методы, необходимо использовать `[Static]` атрибута следующим образом:

```csharp
// A static method, that takes no arguments
[Static, Export ("refresh")]
void Beep ();
```

Это необходимо, поскольку контракта является частью интерфейса и интерфейсов имеют отсутствует понятие объявления статические и экземпляра, поэтому необходимо еще раз обратится к атрибутам. Если вы хотите скрыть конкретного метода для привязки, вы можете декорировать метод с [[внутренний]](~/cross-platform/macios/binding/binding-types-reference.md) атрибута.

`btouch-native` Команда вынуждает проверяет наличие ссылочные параметры, не иметь значение null. Если вы хотите разрешить значения null для определенного параметра, используйте [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут параметра следующим образом:

```csharp
[Export ("setText:")]
string SetText ([NullAllowed] string text);
```

При экспорте ссылочным типом, с `[Export]` ключевое слово, можно также указать семантику распределения. Это необходимо для того, утечки нет данных.

<a name="Binding_Properties" />

### <a name="binding-properties"></a>Свойства привязки

Как и методы, свойства Objective-C связаны с помощью [[экспортировать]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут и карты напрямую к свойствам C#. Как и методы, свойства может быть снабжен атрибутом [[статических]](~/cross-platform/macios/binding/binding-types-reference.md) и [[внутренний]](~/cross-platform/macios/binding/binding-types-reference.md) атрибуты.

При использовании `[Export]` для свойства в разделе рассматриваются btouch собственный атрибут фактически привязывает два метода: метод считывания и метод задания. Указываемое для экспорта — имя **базовое имя** и метод задания значения вычисляется путем добавления слова «set», включение первую букву **базовым именем** в верхний регистр и принятия занять селектор аргумент. Это означает, что `[Export ("label")]` применяется свойство фактически привязывает «label» и «setLabel:» методы Objective-C.

Иногда свойства Objective-C не соответствуют шаблону, описанных выше, а имя — вручную перезаписан. В таких случаях можно управлять способом, что привязка создается с помощью `[Bind]` атрибут доступа get или SET, например:

```csharp
[Export ("menuVisible")]
bool MenuVisible { [Bind ("isMenuVisible")] get; set; }
```

Привязывается «isMenuVisible» и «setMenuVisible:». При необходимости может быть привязано с использованием следующего синтаксиса:

```csharp
[Category, BaseType(typeof(UIView))]
interface UIView_MyIn
{
  [Export ("name")]
  string Name();

  [Export("setName:")]
  void SetName(string name);
}
```

Где Get и set определены явно как в `name` и `setName` выше привязок.

В дополнение к поддержке статических свойств, с помощью `[Static]`, вы можете декорировать свойства статического потока с [[IsThreadStatic]](~/cross-platform/macios/binding/binding-types-reference.md), например:

```csharp
[Export ("currentRunLoop")][Static][IsThreadStatic]
NSRunLoop Current { get; }
```

Так же, как методы позволяют некоторые параметры с флагами [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md), можно применить [[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) к свойству, чтобы указать, что null является допустимым значением для свойства, например:

```csharp
[Export ("text"), NullAllowed]
string Text { get; set; }
```

[[NullAllowed]](~/cross-platform/macios/binding/binding-types-reference.md) непосредственно на метод задания значения также можно указать параметр:

```csharp
[Export ("text")]
string Text { get; [NullAllowed] set; }
```

#### <a name="caveats-of-binding-custom-controls"></a>Предупреждения привязки пользовательские элементы управления

При настройке привязки для пользовательского элемента управления, следует учитывать следующие факторы.

1. **Привязка свойств должны быть статическими** — при определении привязки свойств, `Static` следует использовать атрибут.
2. **Имена свойств должны точно совпадать** -имя, используемое для привязки свойства должно соответствовать имени свойства в пользовательском элементе управления точно.
3. **Типы свойств должны точно соответствовать** -тип переменной, используемой для привязки свойства должен совпадать тип свойства в пользовательском элементе управления.
4. **Точки останова и getter/setter** — разместить точки останова в методе считывания или методов задания свойства никогда не сработает.
5. **Наблюдать за обратные вызовы** -необходимо будет использовать наблюдения обратные вызовы для получения уведомлений об изменениях в значениях свойств пользовательских элементов управления.

Сбой, чтобы наблюдать за любой из перечисленных выше предупреждения может привести к автоматически сбой во время выполнения привязки.

<a name="MutablePattern" />

#### <a name="objective-c-mutable-pattern-and-properties"></a>Изменяемый шаблон Objective-C и свойства

Objective-C платформы применяют идиоматикой, где некоторые классы являются неизменяемыми изменяемый подклассом.   Например `NSString` неизменяемый версия пока `NSMutableString` имеет подкласса, в котором позволяет изменение.

В этих классах, присутствует неизменяемый базовый класс содержит свойства с метод получения, но нет метода задания свойства.   А изменяемый версии вызвать метод задания.   Поскольку это действительно невозможно с помощью C#, нам пришлось сопоставить эту идиому с идиоматикой, будет работать с помощью C#.

Способом, что сопоставляется C# является добавление метод считывания и метод задания базового класса, но Пометка задания с `[NotImplemented]` атрибута.

Затем на изменяемый подкласса, используйте `[Override]` атрибут в свойство, чтобы убедиться, что свойство фактически переопределяет поведение родительского элемента.

Пример

```csharp
[BaseType (typeof (NSObject))]
interface MyTree {
    string Name { get; [NotImplemented] set; }
}

[BaseType (typeof (MyTree))]
interface MyMutableTree {
    [Override]
    string Name { get; set; }
}
```

 <a name="Binding_Constructors" />

### <a name="binding-constructors"></a>Конструкторы привязки

**Btouch собственный** средство автоматически создаст четверки конструкторы в классе, к данному классу `Foo`, он создает:

-  `Foo ()`: конструктор по умолчанию (сопоставленное значению конструктор «init» Objective-C)
-  `Foo (NSCoder)`: конструктор, используемый во время десериализации NIB файлов (сопоставляется Objective-C «initWithCoder:» конструктор).
-  `Foo (IntPtr handle)`: конструктор для создания дескриптора на основе, он вызывается средой выполнения при среды выполнения должен предоставлять объект управляемого в неуправляемый объект.
-  `Foo (NSEmptyFlag)`: используется производными классами для предотвращения двойные инициализации.

Для конструкторов, которые указываются, они должны объявляться с помощью следующую сигнатуру в определении интерфейса: они должны возвращать `IntPtr` значение и имя метода должен быть конструктор. Например привязать `initWithFrame:` конструктор, это следует использовать:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);
```

 <a name="Binding_Protocols" />

### <a name="binding-protocols"></a>Привязки протоколов

Как описано в документе разработки API, см. в разделе [обсуждение модели и протоколы](~/ios/internals/api-design/index.md), Xamarin.iOS сопоставляет протоколы Objective-C в классы, с флагами [[модель]](~/cross-platform/macios/binding/binding-types-reference.md) атрибута. Обычно это используется при реализации классов делегат Objective-C.

Разница между обычный связанный класс и класс делегата: класс делегата может иметь один или несколько необязательных методов.

Для примера рассмотрим `UIKit` класса `UIAccelerometerDelegate`, это, как привязать в Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface UIAccelerometerDelegate {
        [Export ("accelerometer:didAccelerate:")]
        void DidAccelerate (UIAccelerometer accelerometer, UIAcceleration acceleration);
}
```

Так как это необязательный метод, в определении `UIAccelerometerDelegate` нет ничего делать. Но если протокол необходимый метод, следует добавить [[абстрактный]](~/cross-platform/macios/binding/binding-types-reference.md) к методу атрибут. Это заставит пользователя реализации для предоставления тела метода.

Как правило используются протоколы в классах, которые отвечают на сообщения. Обычно это делается в Objective-C путем назначения свойству «делегат» экземпляр объекта, который отвечает на методы в протоколе.

— Соглашение в Xamarin.iOS для поддержки обоих Objective-C слабо сочетании стиль там, где любой экземпляр `NSObject` могут назначаться делегату и также предоставляют строго типизированную версию. По этой причине мы обычно предоставляют со строгим типом свойства «Делегат» и «WeakDelegate», который слабо типизированным. Привязки обычно слабо типизированным версии с экспортом, и мы используем [[Wrap]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут для предоставления строго типизированную версию.

В следующем примере показано, как мы привязать `UIAccelerometer` класса:

```csharp
[BaseType (typeof (NSObject))]
interface UIAccelerometer {
        [Static] [Export ("sharedAccelerometer")]
        UIAccelerometer SharedAccelerometer { get; }

        [Export ("updateInterval")]
        double UpdateInterval { get; set; }

        [Wrap ("WeakDelegate")]
        UIAccelerometerDelegate Delegate { get; set; }

        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }
}
```

 <a name="iOS7ProtocolSupport" />

**Новые возможности MonoTouch 7.0**

Начиная с MonoTouch 7.0 новых и усовершенствованных функций привязки протокол был включен.  Эта новая поддержка делает проще в использовании идиом C цель перехода одного или нескольких протоколов в данного класса.

Для каждого определения протокола `MyProtocol` в Objective-C, теперь есть `IMyProtocol` интерфейс, который содержит все необходимые методы от протокола, а также класс расширений, который предоставляет дополнительные методы.  Выше, в сочетании с новой поддержке в Xamarin Studio, редактор позволяет разработчикам реализовать протокол методы без использования отдельных подклассов предыдущих классов абстрактной модели.

Все определения, содержащего `[Protocol]` атрибут на самом деле создаст три вспомогательных классов, которые значительно улучшают способ использования протоколов:

```csharp
// Full method implementation, contains all methods
    class MyProtocol : IMyProtocol {
        public void Say (string msg);
        public void Listen (string msg);
    }

    // Interface that contains only the required methods
    interface IMyProtocol: INativeObject, IDisposable {
        [Export ("say:")]
        void Say (string msg);
    }

    // Extension methods
    static class IMyProtocol_Extensions {
        public static void Optional (this IMyProtocol this, string msg);
        }
    }
```

**Реализацию класса** предоставляет полный абстрактный класс, можно переопределить отдельных методов и получить полный строгую типизацию.  Однако из-за C# не поддерживает множественное наследование, существуют сценарии, где может оказаться должен иметь другой базовый класс, но по-прежнему необходимо реализовать интерфейс, который, если

Созданный **определением интерфейса** поставляется в.  Он является интерфейсом, который содержит методы, необходимые от протокола.  Таким образом, разработчики, которые требуется реализовать протокол, просто реализовать интерфейс.  Среда выполнения автоматически регистрирует тип как переход протокола.

Обратите внимание, что интерфейс только перечислены требуемые методы предоставляют дополнительные методы.  Это означает классы, применяющие протокол получите полную проверку типа для необходимые методы, что потребуется прибегать к слабой типизацией (вручную с помощью экспорта атрибутов и соответствия сигнатуры) для методов необязательно протокола.

Чтобы сделать удобнее использовать API, который использует протоколы, средство привязки также создаст класс метод расширения, который предоставляет все необязательные методы.  Это означает, что до тех пор, пока, используют API, можно обрабатывать протоколы, как будто все методы.

Если вы хотите использовать определения протокола в API, необходимо написать каркас пустых интерфейсов в определении API.  Если вы хотите использовать MyProtocol в API-Интерфейс, потребуется это сделать:

```csharp
[BaseType (typeof (NSObject))]
[Model, Protocol]
interface MyProtocol {
    // Use [Abstract] when the method is defined in the @required section
    // of the protocol definition in Objective-C
    [Abstract]
    [Export ("say:")]
    void Say (string msg);

    [Export ("listen")]
    void Listen ();
}

interface IMyProtocol {}

[BaseType (typeof(NSObject))]
interface MyTool {
    [Export ("getProtocol")]
    IMyProtocol GetProtocol ();
}
```

Это необходимо, поскольку во время привязки `IMyProtocol` не будет существовать, т. е. Почему вы должны указать пустой интерфейс.

#### <a name="adopting-protocol-generated-interfaces"></a>Внедрение протокола созданные интерфейсы

Каждый раз, когда реализовывать один из интерфейсов, созданных для протоколов, следующим образом:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Реализации методов интерфейса автоматически получает экспортированы соответствующее имя, поэтому он эквивалентен следующему:

```csharp
class MyDelegate : NSObject, IUITableViewDelegate {
    [Export ("getRowHeight:")]
    nint IUITableViewDelegate.GetRowHeight (nint row) {
        return 1;
    }
}
```

Не важно, если интерфейс реализован, явно или неявно.

<a name="Binding_Class_Extensions" />

### <a name="binding-class-extensions"></a>Класс расширений привязки

<!--In Objective-C it is possible to extend classes with new methods,
similar in spirit to C#'s extension methods. When one of these methods
is present, you can use the `[Target]` attribute to flag the first
parameter of a method as being the receiver of the Objective-C
message.

For example, in Xamarin.iOS we bound the extension methods that are defined on
`NSString` when `UIKit` is imported as methods in the `UIView`, like this:

```csharp
[BaseType (typeof (UIResponder))]
interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

-->

В Objective-C имеется возможность расширения классов с помощью новых методов, сохранят дух методы расширения в C#. Если один из этих методов, можно использовать `BaseType` атрибут, чтобы пометить метод как получателя сообщения Objective-C.

Например, в Xamarin.iOS мы привязано методы расширения, которые определены в `NSString` при `UIKit` импортируются как методы в `NSStringDrawingExtensions`, следующим образом:

```csharp
[Category, BaseType (typeof (NSString))]
interface NSStringDrawingExtensions {
    [Export ("drawAtPoint:withFont:")]
    CGSize DrawString (CGPoint point, UIFont font);
}
```

 <a name="Binding_Objective-C_Argument_Lists" />

### <a name="binding-objective-c-argument-lists"></a>Списки аргументов C цель привязки

Objective-C поддерживает переменное число аргументов, можно использовать следующий способ описанным Зак Gris [этой записи блога](http://forums.monotouch.net/yaf_postst311_SOLVED-Binding-ObjectiveC-Argument-Lists.aspx).

Сообщение Objective-C выглядит следующим образом:

```csharp
- (void) appendWorkers:(XWorker *) firstWorker, ...
  NS_REQUIRES_NIL_TERMINATION ;
```

Для вызова этого метода в C# необходимо создать подпись следующим образом:

```csharp
[Export ("appendWorkers"), Internal]
void AppendWorkers (Worker firstWorker, IntPtr workersPtr)
```

Этот код объявляет метод как внутренние, скрытие выше API от пользователей, но предоставление доступа к библиотеке. Вы можете написать метод следующим образом:

```csharp
public void AppendWorkers(params Worker[] workers)
{
    if (workers == null)
         throw new ArgumentNullException ("workers");

    var pNativeArr = Marshal.AllocHGlobal(workers.Length * IntPtr.Size);
    for (int i = 1; i < workers.Length; ++i)
        Marshal.WriteIntPtr (pNativeArr, (i - 1) * IntPtr.Size, workers[i].Handle);

    // Null termination
    Marshal.WriteIntPtr (pNativeArr, (workers.Length - 1) * IntPtr.Size, IntPtr.Zero);

    // the signature for this method has gone from (IntPtr, IntPtr) to (Worker, IntPtr)
    WorkerManager.AppendWorkers(workers[0], pNativeArr);
    Marshal.FreeHGlobal(pNativeArr);
}
```

 <a name="Binding_Fields" />

### <a name="binding-fields"></a>Привязка полей

Иногда требуется доступ к открытых полей, которые были объявлены в библиотеке.

Обычно эти поля содержат значения строками или целыми числами, которые необходимо ссылаться. Обычно они используются как строка, представляющая определенных уведомлений и как ключи в словарях.

Чтобы привязать поле, добавить свойство в файл определения интерфейса и оформлять свойство с [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) атрибута. Этот атрибут принимает один параметр: имя C символа для уточняющего запроса. Пример:

```csharp
[Field ("NSSomeEventNotification")]
NSString NSSomeEventNotification { get; }
```

Чтобы обтекание различные поля в статическом классе, который является производным от `NSObject`, можно использовать `[Static]` атрибут на уровне класса, следующим образом:

```csharp
[Static]
interface LonelyClass {
    [Field ("NSSomeEventNotification")]
    NSString NSSomeEventNotification { get; }
}
```

Выше создаст `LonelyClass` которого не является производным от `NSObject` и будет содержать привязку к `NSSomeEventNotification` 
 `NSString` в виде `NSString`.

`[Field]` Атрибут может применяться для следующих типов данных:

-  `NSString` ссылки (только свойства только для чтения)
-  `NSArray` ссылки (только свойства только для чтения)
-  32-разрядных целых чисел (`System.Int32`)
-  64-разрядных целых чисел (`System.Int64`)
-  32-разрядные (`System.Single`)
-  64-разрядных значений с плавающей запятой (`System.Double`)
-  `System.Drawing.SizeF`
-  `CGSize`

В дополнение к имени собственного поля можно указать имя библиотеки, где находится поле, передав имя библиотеки:

```csharp
[Static]
interface LonelyClass {
    [Field ("SomeSharedLibrarySymbol", "SomeSharedLibrary")]
    NSString SomeSharedLibrarySymbol { get; }
}
```

Если статической компоновки нет библиотеки для привязки, поэтому вам нужно использовать `__Internal` имя:

```csharp
[Static]
interface LonelyClass {
    [Field ("MyFieldFromALibrary", "__Internal")]
    NSString MyFieldFromALibrary { get; }
}
```

<a name="Binding_Enums" />

### <a name="binding-enums"></a>Перечислимые типы привязки

Можно добавить `enum` непосредственно в привязке, файлы упрощает использовать их в пределах определения интерфейсов API - без использования различных исходных файла (которая должна быть скомпилирована в привязках и окончательной проекции).

Пример

```csharp
[Native] // needed for enums defined as NSInteger in ObjC
enum MyEnum {}

interface MyType {
    [Export ("initWithEnum:")]
    IntPtr Constructor (MyEnum value);
}
```

Также можно создать собственные перечислимые типы для замены `NSString` константы. В этом случае будет генератор **автоматически** создавать методы для преобразования значения перечисления и NSString константы.

Пример

```csharp
enum NSRunLoopMode {

    [DefaultEnumValue]
    [Field ("NSDefaultRunLoopMode")]
    Default,

    [Field ("NSRunLoopCommonModes")]
    Common,

    [Field (null)]
    Other = 1000
}

interface MyType {
    [Export ("performForMode:")]
    void Perform (NSString mode);

    [Wrap ("Perform (mode.GetConstant ())")]
    void Perform (NSRunLoopMode mode);
}
```

В приведенном выше примере можно было бы декорировать `void Perform (NSString mode);` с `[Internal]` атрибута. Это позволить **скрыть** API на основе константы из потребителей привязки.

Однако это будет ограничивать использование подклассов тип как лучше использует альтернативный API `[Wrap]` атрибута. Эти созданные методы, не `virtual`, т. е. Вы не будет иметь возможность переопределять ими, которые могут или не, рекомендуется использовать.

Альтернативой является пометить оригинал, `NSString`-на основе определения как `[Protected]`. Это позволит подклассы для работы, при необходимости, а wrap'ed версии по-прежнему будет работать и вызывать переопределенный метод.

### <a name="binding-nsvalue-nsnumber-and-nsstring-to-a-better-type"></a>Привязка к лучше типу NSValue, NSNumber и NSString

[[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут позволяет привязки `NSNumber`, `NSValue` и `NSString`(перечисления) в более точные типов C#. Атрибут может быть использован для создания лучшую, более точным, .NET API через API-Интерфейсы собственного.

Вы можете декорировать методов (на возвращаемое значение), параметры и свойства с [[BindAs]](~/cross-platform/macios/binding/binding-types-reference.md). Единственное ограничение состоит в том элементов **не должны** находиться внутри `[Protocol]` или `[Model]` интерфейса.

Пример:

```csharp
[return: BindAs (typeof (bool?))]
[Export ("shouldDrawAt:")]
NSNumber ShouldDraw ([BindAs (typeof (CGRect))] NSValue rect);
```

Отобразятся:

```csharp
[Export ("shouldDrawAt:")]
bool? ShouldDraw (CGRect rect) { ... }
```

Внутренне мы сделаем это `bool?`  <->  `NSNumber` и `CGRect`  <->  `NSValue` преобразования.

[[BindAs] ](~/cross-platform/macios/binding/binding-types-reference.md) также поддерживает массивы `NSNumber` `NSValue` и `NSString`(перечисления).

Пример:

```csharp
[BindAs (typeof (CAScroll []))]
[Export ("supportedScrollModes")]
NSString [] SupportedScrollModes { get; set; }
```

Отобразятся:

```csharp
[Export ("supportedScrollModes")]
CAScroll [] SupportedScrollModes { get; set; }
```

`CAScroll` — `NSString` резервного enum, получает право `NSString` значение и обработки преобразования типа.

См. в разделе [документации [BindAs]](~/cross-platform/macios/binding/binding-types-reference.md) чтобы увидеть поддерживаемые преобразования типов.

 <a name="Binding_Notifications" />

### <a name="binding-notifications"></a>Привязка уведомлений

Уведомления — это сообщения, которые учитываются в `NSNotificationCenter.DefaultCenter` и используются в качестве механизма для передачи сообщений из одной части приложения в другое. Разработчики подписаться на уведомления, обычно с помощью [NSNotificationCenter](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/) [AddObserver](https://developer.xamarin.com/api/type/Foundation.NSNotificationCenter/M/AddObserver/) метод. Когда приложение отправляет сообщение центр уведомлений, оно обычно содержит полезные данные, хранящиеся в [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) словаря. Этот словарь слабо типизированных и получения информации от его подвержен ошибкам, как пользователи обычно должны в документации, о ключах доступны в словаре и типы значений, которые могут храниться в словаре. Наличие ключей иногда используется как значение типа boolean.

Генератор привязки Xamarin.iOS обеспечивает поддержку для разработчиков для привязки уведомления. Для этого необходимо задать для [[уведомления]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут для свойства, которое также были помечены [[Field]](~/cross-platform/macios/binding/binding-types-reference.md) свойств (он может быть открытым или закрытым).

Этот атрибут может использоваться без аргументов для уведомлений, которые содержат полезные данные не найдены, или можно указать `System.Type` , ссылается на другого интерфейса, в определении API, обычно с именем, заканчивая «EventArgs». Генератор превратит интерфейса в класс, который наследуется от класса `EventArgs` и будет включать все свойства в списке. `[Export]` Атрибут должен использоваться в классе EventArgs для перечисления имя ключа, используемого для поиска в словарь Objective-C для выборки значения.

Пример:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassDidStartNotification")]
    NSString DidStartNotification { get; }
}
```

Приведенный выше код создаст вложенный класс `MyClass.Notifications` со следующими методами:

```csharp
public class MyClass {
   [..]
   public Notifications {
      public static NSObject ObserveDidStart (EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Пользователи кода можно затем легко подписаться на уведомления разнесены [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) с помощью следующего кода:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Значение, возвращаемое `ObserveDidStart` можно легко отказаться от получения уведомлений, следующим образом:

```csharp
token.Dispose ();
```

Или можно вызвать [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject/) и передать маркер. Если уведомления содержит параметры, необходимо указать вспомогательный `EventArgs` интерфейс следующим образом:

```csharp
interface MyClass {
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}

// The helper EventArgs declaration
interface MyScreenChangedEventArgs {
    [Export ("ScreenXKey")]
    nint ScreenX { get; set; }

    [Export ("ScreenYKey")]
    nint ScreenY { get; set; }

    [Export ("DidGoOffKey")]
    [ProbePresence]
    bool DidGoOff { get; }
}
```

Выше создаст `MyScreenChangedEventArgs` класса `ScreenX` и `ScreenY` свойства, которые получает данные из [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) словаря с помощью клавиш «ScreenXKey» и «ScreenYKey» соответственно и применять соответствующие преобразований. `[ProbePresence]` Атрибут используется для генератора для проверки, если этому параметру присвоено значение `UserInfo`, вместо того чтобы попытаться извлечь значение. Используется в случае, если наличие ключа значение (обычно для логических значений).

Это позволяет писать код следующим образом:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

 <a name="Binding_Categories" />

### <a name="binding-categories"></a>Привязка категории

Категории включают механизм Objective-C, используемый расширить набор методов и свойств, доступных в классе.   На практике, они используются для расширения функциональности базового класса (например `NSObject`) при связана конкретной платформы в (например `UIKit`), делая их методы доступны, но только если компонуется новой платформы.   В других случаях они используются для организации функций в классе по функциональным возможностям.   Они аналогичны дух методы расширения в C#. Это, как будет выглядеть категорию в цель C:

```csharp
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Приведенный выше пример Если найден в библиотеке распространится экземпляров `UIView` с помощью метода `makeBackgroundRed`.

Чтобы привязать их, можно использовать `[Category]` атрибут в определении интерфейса.  Когда атрибут с помощью категории, значение `[BaseType]` изменений неизменных атрибутов не могут использоваться для указания базового класса для расширения, должен иметь тип для расширения.

Показано как `UIView` расширений привязан и преобразуются в методы расширения в C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Выше создаст `MyUIViewExtension` класс, содержащий `MakeBackgroundRed` метода расширения.  Это означает, что теперь можно вызвать «MakeBackgroundRed» на любом `UIView` подкласс, обеспечивая те же функциональные возможности, получаемому в цель C. В других случаях категории используются не расширить класс системы, а для организации функции исключительно в целях оформления.  Пример:

```csharp
@interface SocialNetworking (Twitter)
- (void) postToTwitter:(Message *) message;
@end

@interface SocialNetworking (Facebook)
- (void) postToFacebook:(Message *) message andPicture: (UIImage*)
picture;
@end
```

Несмотря на то, что можно использовать `Category` атрибута также для этого стиля оформления, объявлений, можно также просто добавить их все в определении класса.  Эти же бы достичь.

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Twitter {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);
}

[Category]
[BaseType (typeof (SocialNetworking))]
interface Facebook {
    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

Просто короче в этих случаях для слияния категории:

```csharp
[BaseType (typeof (NSObject))]
interface SocialNetworking {
    [Export ("postToTwitter:")]
    void PostToTwitter (Message message);

    [Export ("postToFacebook:andPicture:")]
    void PostToFacebook (Message message, UIImage picture);
}
```

 <a name="Binding_Blocks" />

### <a name="binding-blocks"></a>Блоки привязки

Блоки, новую конструкцию, представленный Apple, чтобы перевести функциональный эквивалент C# анонимных методов цель-C. Например `NSSet` класс теперь предоставляет этот метод:

```csharp
- (void) enumerateObjectsUsingBlock:(void (^)(id obj, BOOL *stop) block
```

Описание выше объявляет метод с именем «*enumerateObjectsUsingBlock:*", который принимает один аргумент с именем *блок*. Этот блок аналогична анонимного метода C#, у нее есть поддержка записи в текущей среде (указатель «this», доступ к локальным переменным и параметрам). Приведенный выше метод в `NSSet` вызывает блок с двумя параметрами `NSObject` (часть «идентификатор obj») и указатель на логическое значение («BOOL * остановить») части.

Чтобы привязать такого рода API с btouch, необходимо сначала объявить сигнатура типа блока, как C# делегата и затем ссылаться на него из точки входа API следующим образом:

```csharp
// This declares the callback signature for the block:
delegate void NSSetEnumerator (NSObject obj, ref bool stop)

// Later, inside your definition, do this:
[Export ("enumerateObjectUsingBlock:")]
void Enumerate (NSSetEnumerator enum)
```

А теперь код может вызывать функции в C#:

```csharp
var myset = new NSMutableSet ();
myset.Add (new NSString ("Foo"));

s.Enumerate (delegate (NSObject obj, ref bool stop){
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

При желании можно также использовать лямбда-выражения:

```csharp
var myset = new NSMutableSet ();
mySet.Add (new NSString ("Foo"));

s.Enumerate ((obj, stop) => {
    Console.WriteLine ("The first object is: {0} and stop is: {1}", obj, stop);
});
```

 <a name="GeneratingAsync" />

### <a name="asynchronous-methods"></a>Асинхронные методы

Генератор привязки можно включить класс методы в понятного имени асинхронные методы (методы, возвращающие задачи или задачи&lt;T&gt;).

Можно использовать `[Async]` атрибут на методы, возвращающие void, последний аргумент является обратным вызовом.  При применении этого метода, генератор привязки создаст версия этого метода с суффиксом `Async`.  Если обратный вызов не имеет параметров, возвращаемое значение будет `Task`, если функция обратного вызова принимает параметр, результат будет `Task<T>`.  Если обратный вызов принимает несколько параметров, необходимо задать `ResultType` или `ResultTypeName` для указания желаемое имя созданного типа, который будет содержать все свойства.

Пример

```csharp
[Export ("loadfile:completed:")]
[Async]
void LoadFile (string file, Action<string> completed);
```

Приведенный выше код создаст обоих метод LoadFile, а также:

```csharp
[Export ("loadfile:completed:")]
Task<string> LoadFileAsync (string file);
```

<a name="Surfacing_Strong_Types" />

### <a name="surfacing-strong-types-for-weak-nsdictionary-parameters"></a>Распределение результатов строгие типы параметров слабого NSDictionary

Во многих местах в Objective-C API параметры передаются как слабая типизация `NSDictionary` API с определенные ключи и значения, но они являются подвержен ошибкам (можно передать недопустимый ключи и получить никаких предупреждений; можно передать недопустимые значения и получить без предупреждений) и утомительным для использования по мере необходимости несколько обращений к документации для поиска возможных имен ключей и значений.

Решение содержит строго типизированную версию, которая предоставляет строго типизированную версию API-интерфейса и в фоновом сопоставляет различные базовые ключи и значения.

Так например, если приняты API Objective-C `NSDictionary` и его описаны как принимающая «XyzVolumeKey», который принимает ключ `NSNumber` со значением тома от 0,0 1.0 и «XyzCaptionKey», который принимает строку, нужно предоставить пользователям работы с низким приоритетом API, который выглядит следующим образом:

```csharp
public class  XyzOptions {
    public nfloat? Volume { get; set; }
    public string Caption { get; set; }
}
```

`Volume` Свойство был определен как float, допускающие значение NULL, как соглашения для Objective-C не требует словари должны иметь значения, поэтому существуют сценарии, где может не иметь значение.

Чтобы сделать это, необходимо выполнить ряд действий:

* Создание строго типизированный класс, который наследуется от класса [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) и предоставляет различные методы get и set для каждого свойства.
* Объявления перегрузки для методов, принимающих `NSDictionary` новый строго типизированную версию вступили в силу.

Создание строго типизированного класса либо вручную или использовать генератор для выполнения работы для вас.  Во-первых, мы расскажем, как это сделать вручную, чтобы понять, что происходит и автоматической настройке.

Необходимо создать вспомогательный файл для этого, он не входит в API-Интерфейс контракта.  Это будет иметь записываемый создать класс XyzOptions:

```csharp
public class XyzOptions : DictionaryContainer {
# if !COREBUILD
    public XyzOptions () : base (new NSMutableDictionary ()) {}
    public XyzOptions (NSDictionary dictionary) : base (dictionary){}

    public nfloat? Volume {
       get { return GetFloatValue (XyzOptionsKeys.VolumeKey); }
       set { SetNumberValue (XyzOptionsKeys.VolumeKey, value); }
    }
    public string Caption {
       get { return GetStringValue (XyzOptionsKeys.CaptionKey); }
       set { SetStringValue (XyzOptionsKeys.CaptionKey, value); }
    }
# endif
}
```

Затем необходимо предоставить метод-оболочку, который предоставляет доступ к API высокого уровня, поверх API нижнего уровня.

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Если API не должны быть переписаны, можно безопасно скрыть API на основе NSDictionary с помощью [внутренний](~/cross-platform/macios/binding/binding-types-reference.md) атрибута.

Как видите, мы используем `[Wrap]` атрибута для отображения новой точки входа интерфейса API, и мы с помощью наших строго типизированных классов XyzOptions контактной.
Метод-оболочку также допустимо значение null должны быть переданы.

Теперь, единственное, что мы не рассказывать о — где `XyzOptionsKeys` значения, откуда.  Обычно можно сгруппировать ключи, предоставляет доступ к API в статическом классе как XyzOptionsKeys, следующим образом:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
```

Рассмотрим автоматическую поддержку для создания строго типизированной словари.  Это позволяет избежать достаточное количество стандартного и словаря можно определить непосредственно в API-Интерфейс контракта, то вместо использования внешнего файла.

Для создания строго типизированный словарь, вынуждает интерфейс в API и декорировать его [StrongDictionary](~/cross-platform/macios/binding/binding-types-reference.md) атрибута.  Это значение определяет генератор, оно должно создавать класс с тем же именем, как ваш интерфейс, который будет производным `DictionaryContainer` и предоставит строгого типизированные методы доступа для него.

`StrongDictionary` Атрибут принимает один параметр, имя статического класса, содержащий ключи словаря.  Затем каждое свойство интерфейса станет строго типизированного метода доступа.  По умолчанию код будет использовать имя свойства с суффиксом «Ключ» в статическом классе для создания метода доступа.

Это означает, что для создания строго типизированные больше не требуется внешний файл, а также вручную создавать методы get и set для каждого свойства и того, чтобы вручную найти ключи самостоятельно.

Это то, что все привязки выглядит следующим образом:

```csharp
[Static]
class XyzOptionKeys {
    [Field ("kXyzVolumeKey")]
    NSString VolumeKey { get; }

    [Field ("kXyzCaptionKey")]
    NSString CaptionKey { get; }
}
[StrongDictionary ("XyzOptionKeys")]
interface XyzOptions {
    nfloat Volume { get; set; }
    string Caption { get; set; }
}

[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

При необходимости для ссылки в вашей `XyzOption` члены другое поле (то есть не имя свойства с суффиксом `Key`), вы можете декорировать свойство с `Export` атрибут с именем, которое вы хотите использовать.

 <a name="Type_mappings" />

## <a name="type-mappings"></a>Сопоставления типов

В этом разделе рассматриваются как Objective-C типы сопоставляются с типов C#.

<a name="Simple_Types" />

### <a name="simple-types"></a>Простые типы

В следующей таблице показаны, как сопоставлять типы из Objective-C и CocoaTouch world во всем мире Xamarin.iOS:

<table border="1" cellpadding="1" cellspacing="1" width="80%">
      <caption> Сопоставления типов </caption>
      <tbody>
        <tr>
          <td>
Имя типа Objective-c. </td>
          <td>
Тип единого API Xamarin.iOS </td>
        </tr>
        <tr>
          <td>
BOOL-GLboolean </td>
          <td>
bool </td>
        </tr>
        <tr>
          <td>
NSInteger </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSUInteger </td>
          <td>
nuint </td>
        </tr>
        <tr>
          <td>
CFTimeInterval / NSTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
NSString (<a href="~/ios/internals/api-design/nsstring.md">дополнительные привязку NSString</a>) </td>
          <td>
string </td>
        </tr>
        <tr>
          <td>
char * </td>
          <td>
            <a href="~/cross-platform/macios/binding/binding-types-reference.md#plainstring"> [PlainString] </a> строки </td>
        </tr>
        <tr>
          <td>
CGRect </td>
          <td>
CGRect </td>
        </tr>
        <tr>
          <td>
CGPoint </td>
          <td>
CGPoint </td>
        </tr>
        <tr>
          <td>
CGSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
CGFloat GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Типы CoreFoundation (CF *) </td>
          <td>
CoreFoundation.CF* </td>
        </tr>
        <tr>
          <td>
GLint </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
GLfloat </td>
          <td>
nfloat </td>
        </tr>
        <tr>
          <td>
Типы Foundation (NS *) </td>
          <td>
Foundation.NS* </td>
        </tr>
        <tr>
          <td>
id </td>
          <td>
Foundation.NSObject </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSSize </td>
          <td>
CGSize </td>
        </tr>
        <tr>
          <td>
NSTextAlignment </td>
          <td>
UITextAlignment </td>
        </tr>
        <tr>
          <td>
SEL </td>
          <td>
ObjCRuntime.Selector </td>
        </tr>
        <tr>
          <td>
dispatch_queue_t </td>
          <td>
CoreFoundation.DispatchQueue </td>
        </tr>
        <tr>
          <td>
CFTimeInterval </td>
          <td>
double </td>
        </tr>
        <tr>
          <td>
CFIndex </td>
          <td>
nint </td>
        </tr>
        <tr>
          <td>
NSGlyph </td>
          <td>
nuint </td>
        </tr>
      </tbody>
    </table>

 <a name="Arrays" />

### <a name="arrays"></a>Массивы

Среда выполнения Xamarin.iOS автоматически отвечает за преобразование массивов в C# для `NSArrays` и выполнением преобразования, например метод мнимой Objective-C, возвращает `NSArray` из `UIViews`:

```csharp
// Get the peer views - untyped
- (NSArray *)getPeerViews ();

// Set the views for this container
- (void) setViews:(NSArray *) views
```

Привязанный следующим образом:

```csharp
[Export ("getPeerViews")]
UIView [] GetPeerViews ();

[Export ("setViews:")]
void SetViews (UIView [] views);
```

Идея заключается в том, чтобы использовать строго типизированный массив C#, как это позволит IDE, чтобы обеспечить правильную автозавершения с фактическим типом без перезагрузки пользователю угадать или искать в документации, чтобы определить фактический тип объектов, содержащихся в массиве.

В случаях, где может не обнаружить фактического наиболее производного типа, содержащихся в массиве, можно использовать `NSObject []` как возвращаемое значение.

 <a name="Selectors" />

### <a name="selectors"></a>Селекторы

Селекторы отображаются на Objective-C API как специальный тип «: Выбор папки». При привязке селектора, будет сопоставления типа `ObjCRuntime.Selector`.  Обычно селекторы, представлены в API с объектом, целевой объект и селектор для вызова в целевом объекте. По существу предоставляя эти соответствует делегату C#: то, что инкапсулирует метод, который вызывается как, так и для вызова метода в объект.

Вот как выглядит привязки:

```csharp
interface Button {
   [Export ("setTarget:selector:")]
   void SetTarget (NSObject target, Selector sel);
}
```

И вот как метод обычно будет использоваться в приложении:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        b.SetTarget (this, new Selector ("print"));
    }

    [Export ("print")]
    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

Чтобы сделать привязку лучше разработчикам C#, обычно будет укажите метод, который принимает `NSAction` параметра, который позволяет C# делегаты и лямбда-выражения для использования вместо `Target+Selector`. Для этого будет обычно скрыть метод «SetTarget» отмечая с атрибутом «Internal» и публиковать новый вспомогательный метод следующим образом:

```csharp
// API.cs
interface Button {
   [Export ("setTarget:selector:"), Internal]
   void SetTarget (NSObject target, Selector sel);
}

// Extensions.cs
public partial class Button {
     public void SetTarget (NSAction callback)
     {
         SetTarget (new NSActionDispatcher (callback), NSActionDispatcher.Selector);
     }
}
```

Поэтому теперь коде пользователя можно записать следующим образом:

```csharp
class DialogPrint : UIViewController {
    void HookPrintButton (Button b)
    {
        // First Style
        b.SetTarget (ThePrintMethod);

        // Lambda style
        b.SetTarget (() => {  /* print here */ });
    }

    void ThePrintMethod ()
    {
       // This does the printing
    }
}
```

 <a name="Strings" />

### <a name="strings"></a>Строки

При связывании метода, принимающего `NSString`, можно заменить, что C# строкового типа, и на возвращаемые типы и параметры.

Единственный случай, когда может потребоваться использовать `NSString` напрямую — когда строка используется в качестве маркера. Дополнительные сведения о строках и `NSString`, прочитайте [структура API на NSString](~/ios/internals/api-design/nsstring.md) документа.

В некоторых редких случаях API может предоставлять строку типа C (`char *`) вместо строки Objective-C (`NSString *`). В таких случаях можно снабдить параметр с [ `[PlainString]` ](~/cross-platform/macios/binding/binding-types-reference.md#plainstring) атрибута.

 <a name="outref_parameters" />

### <a name="outref-parameters"></a>out-параметры ref

Некоторые интерфейсы API возвращаемые значения параметров или передачи параметров по ссылке.

Обычно подпись выглядит следующим образом:

```csharp
- (void) someting:(int) foo withError:(NSError **) retError
- (void) someString:(NSObject **)byref
```

В первом примере распространенных случаях Objective-C, чтобы возвращать коды ошибок, указатель на `NSError` передается указатель, и возвращаемое значение.   Второй метод показано, как метод Objective-C может получить объект и изменить его содержимое.   Это было бы передачи по ссылку, а не чисто выходное значение.

Привязка будет выглядеть следующим образом:

```csharp
[Export ("something:withError:")]
void Something (nint foo, out NSError error);
[Export ("someString:")]
void SomeString (ref NSObject byref);
```

 <a name="Memory_management_attributes" />

### <a name="memory-management-attributes"></a>Атрибуты управления памяти

При использовании `[Export]` атрибут и вы передаете данные, которые будут храниться в вызывающий метод, можно указать аргумент семантику, передавая его как второй параметр, например:

```csharp
[Export ("method", ArgumentSemantic.Retain)]
```

Выше бы флаг значение как имеющий семантику «Сохранить». Доступные семантика.

-  Назначение:
-  Копировать:
-  Сохранить:

 <a name="Style_Guidelines" />

### <a name="style-guidelines"></a>Правила стилей

 <a name="Using_[Internal]" />

#### <a name="using-internal"></a>С помощью [внутренней]

Можно использовать [[внутренний]](~/cross-platform/macios/binding/binding-types-reference.md) атрибут, чтобы скрыть метод из открытого API-интерфейса. Вы можете сделать это в случаях, когда предоставленным API слишком низкого уровня, и вы хотите предоставить высокоуровневая реализация в отдельном файле на основе этого метода.

Это также можно использовать при можно столкнуться с ограничениями в генераторе привязки, например некоторых сложных сценариев может предоставлять типы, которые не связаны нужно привязать в свой собственный стиль и вы хотите самостоятельно перенос этих типов, в свой собственный стиль.

 <a name="Event_Handlers_and_Callbacks" />

## <a name="event-handlers-and-callbacks"></a>Обработчики событий и обратных вызовов

Классы Objective-C обычно рассылка уведомлений или запроса данных путем отправки сообщения на класс делегата (делегат Objective-C).

Эта модель полностью поддерживаются и отображаются Xamarin.iOS иногда может быть сложно. Xamarin.iOS предоставляет шаблон событий C# и в системе обратного вызова метода в классе, который может использоваться в таких ситуациях. Это позволяет для выполнения следующего кода:

```csharp
button.Clicked += delegate {
    Console.WriteLine ("I was clicked");
};
```

Генератор привязки способен уменьшение объема ввода требуется сопоставить шаблон Objective-C в шаблоне C#.

Начиная с версии 1.4 Xamarin.iOS будет можно также дать генератор для создания привязок для конкретного делегатов Objective-C и предоставить делегат, как C# событий и свойств для типа узла.

В этом процессе участвуют два класса, хост-класса, который является тот, который в настоящее время выпускает события и отправляет их в `Delegate` или `WeakDelegate` и класс фактическое делегата.

Учитывая следующие настройки:

```csharp
[BaseType (typeof (NSObject))]
interface MyClass {
    [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
    NSObject WeakDelegate { get; set; }

    [Wrap ("WeakDelegate")][NullAllowed]
    MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
    [Export ("loaded:bytes:")]
    void Loaded (MyClass sender, int bytes);
}
```

Чтобы поместить класс в оболочку необходимо:

-  В классе узла, добавьте в ваш `[BaseType]` объявления типа, который выступает в качестве своего делегата и C# имя, которое вы предоставили. В приведенном выше примере это «typeof (MyClassDelegate)» и «WeakDelegate» соответственно.
-  В классе делегата в каждый метод, имеющий более двух параметров необходимо указать тип, который вы хотите использовать для автоматически создаваемого класса EventArgs.

Генератор привязки не ограничивается только одно событие назначения упаковки, возможно, некоторые классы Objective-C для выдачи сообщений в несколько делегатов, их необходимо будет предоставить массивы для поддержки программы установки. Большинство настроек не потребуется, но генератор готов для поддержки этих вариантов.

Результирующий код будет:

```csharp
[BaseType (typeof (NSObject),
    Delegates=new string [] {"WeakDelegate"},
    Events=new Type [] { typeof (MyClassDelegate) })]
interface MyClass {
        [Export ("delegate", ArgumentSemantic.Assign)][NullAllowed]
        NSObject WeakDelegate { get; set; }

        [Wrap ("WeakDelegate")][NullAllowed]
        MyClassDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
interface MyClassDelegate {
        [Export ("loaded:bytes:"), EventArgs ("MyClassLoaded")]
        void Loaded (MyClass sender, int bytes);
}
```

`EventArgs` Используется для указания имени `EventArgs` класса. Следует использовать по одному на подпись (в этом примере `EventArgs` будет содержать свойство nint типа «С»).

Приведенные выше определения генератор создает следующее событие в созданный MyClass:

```csharp
public MyClassLoadedEventArgs : EventArgs {
        public MyClassLoadedEventArgs (nint bytes);
        public nint Bytes { get; set; }
}

public event EventHandler<MyClassLoadedEventArgs> Loaded {
        add; remove;
}
```

Поэтому теперь можно использовать следующий код:

```csharp
MyClass c = new MyClass ();
c.Loaded += delegate (sender, args){
        Console.WriteLine ("Loaded event with {0} bytes", args.Bytes);
};
```

Обратные вызовы используются так же, как вызовов событий, различие состоит в том вместо несколько потенциальных подписчиков (например, несколько методов можно было использовать в событие «Щелчок» или «Загрузка завершена» событие) обратные вызовы могут иметь только одного подписчика.

Процесс идентичен, единственное различие заключается в том, что вместо предоставления имя класса EventArgs, который будет создан, EventArgs фактически используется для именования результирующее имя делегата C#.

Если метод в классе делегат возвращает значение, генератор привязки сопоставит это в метод делегата в родительском классе вместо события. В таких случаях необходимо указать значение по умолчанию, которое должно возвращаться с помощью метода, если пользователь не подключать к делегату. Это делается с помощью `[DefaultValue]` или `[DefaultValueFromArgument]` атрибуты.

DefaultValue будет указывать значение, возвращаемое, пока `[DefaultValueFromArgument]` используется для указания, какой входной аргумент будет возвращено.

 <a name="Enumerations_and_Base_Types" />

## <a name="enumerations-and-base-types"></a>Перечисления и базовые типы

Можно также ссылаться на перечисления или базовые типы, которые напрямую не поддерживаются в системе определения интерфейса btouch. Для этого поместите перечисления и основных типов в отдельный файл и включите этот как часть какого-либо дополнительные файлы, передаваемые в btouch.

 <a name="Linking_the_Dependencies" />

## <a name="linking-the-dependencies"></a>Связывание зависимостей

Если выполнить привязку API, которые не являются частью приложения, необходимо убедитесь в том, что исполняемый файл связана от этих библиотек.

Необходимо Xamarin.iOS о том, как связать библиотеками, это можно сделать либо путем изменения конфигурации сборки для вызова команды mtouch с некоторые дополнительные сборки аргументы, которые указывают способ связи с новой библиотеки, с помощью «-gcc_flags» за по строку в кавычках, содержит все дополнительные библиотеки, необходимые для программы, следующим образом:

```csharp
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load -lSystemLibrary -framework CFNetwork -ObjC"
```

Приведенный выше пример свяжет `libMyLibrary.a`, `libSystemLibrary.dylib` и `CFNetwork` framework библиотеки в конечный исполняемый файл.

Или можно воспользоваться преимуществами уровня сборки `LinkWithAttribute`, который можно внедрить в файлах контракта (например, `AssemblyInfo.cs`). При использовании `LinkWithAttribute`, потребуется вашей собственной библиотеки, доступные во время внесения привязки, как внедрить собственной библиотеки вместе с приложением. Пример:

```csharp
// Specify only the library name as a constructor argument and specify everything else with properties:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget = LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]

// Or you can specify library name *and* link target as constructor arguments:
[assembly: LinkWith ("libMyLibrary.a", LinkTarget.ArmV6 | LinkTarget.ArmV7 | LinkTarget.Simulator, ForceLoad = true, IsCxx = true)]
```

Вы можете спросить, почему вы должны команда «force_load» и обусловлено тем, что - ObjC флаг, несмотря на то, что он компилирует код в, не сохраняет метаданные, необходимые для поддержки категории (устранение компоновщика и компилятора Невызываемый код извлекает его), которые необходимо во время выполнения для Xamarin.iOS.

 <a name="Assisted_References" />

## <a name="assisted-references"></a>Ссылки на службы

Некоторые временные объекты, например действия таблицы и поля предупреждения довольно сложно для отслеживания для разработчиков и генератор привязки могут помочь немного ниже.

Например, если имеется класс, который показал сообщения и затем созданный событием «Готово», а традиционным способом обработки это бы:

```csharp
class Demo {
    MessageBox box;

    void ShowError (string msg)
    {
        box = new MessageBox (msg);
        box.Done += { box = null; ... };
    }
}
```

В вышеприведенном сценарии разработчик должен хранить ссылку на объект себе и либо утечка или активно снимите ссылку для поля в свой собственный.  Хотя код привязки, генератор поддерживает отслеживания ссылки для вас и очистить его при вызове используется специальный метод, приведенный выше код станет:

```csharp
class Demo {
    void ShowError (string msg)
    {
        var box = new MessageBox (msg);
        box.Done += { ... };
    }
}
```

Обратите внимание на то, как он больше не требуется хранить переменную в экземпляре, что оно работает с локальной переменной и не является необходимо, чтобы удалить ссылку, когда объект выходит из строя.

Для использования этой возможности, класс должен иметь значение свойства события `[BaseType]` объявление, а также `KeepUntilRef` переменной присвоено имя метода, который вызывается, когда объект завершил свою работу, следующим образом:

```csharp
[BaseType (typeof (NSObject), KeepUntilRef="Dismiss"), Delegates=new string [] { "WeakDelegate" }, Events=new Type [] { typeof (SomeDelegate) }) ]
class Demo {
    [Export ("show")]
    void Show (string message);
}
```

 <a name="Inheriting_Protocols" />

## <a name="inheriting-protocols"></a>Наследование протоколы

Начиная с Xamarin.iOS версия 3.2, корпорация Майкрософт поддерживает наследование от протоколов, которые помечены атрибутом `[Model]` свойство. Это полезно в определенных шаблонов API, таких как `MapKit` где `MKOverlay` протокола, наследует от `MKAnnotation` протокола и применяет различные классы наследуют от `NSObject`.

Исторически требуется копирование протокол в любой реализации, но в таких случаях теперь можно есть `MKShape` класс наследует от `MKOverlay` протокола и он создает все необходимые методы автоматически.

## <a name="related-links"></a>Связанные ссылки

- [Пример привязки](https://developer.xamarin.com/samples/BindingSample/)
