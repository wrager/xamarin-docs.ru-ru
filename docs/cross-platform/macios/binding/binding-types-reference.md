---
title: Привязки типов Справочник
description: Это руководство описывает различные атрибуты и основные понятия, необходимые для понимания при создании привязок C# библиотеки C цель.
ms.prod: xamarin
ms.assetid: C6618E9D-07FA-4C84-D014-10DAC989E48D
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: 3de85a1e3c84366a2059a8f7c479c20ce873508d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782174"
---
# <a name="binding-types-reference-guide"></a>Привязки типов Справочник

В этом документе описывается список атрибутов, которые можно использовать для создания заметок к API файлов контракт для привязки и создать код

Контракты Xamarin.iOS и Xamarin.Mac API на языке C# главным образом для определений интерфейса, которые определяют способ, код Objective-C будет отображена в C#. Процесс включает в себя сочетание объявления интерфейсов, а также некоторые основные определения которых может потребоваться контракта API-интерфейса. Введение типов привязки см. в разделе нашей дополнительное руководство по [библиотеки C цели привязки](~/cross-platform/macios/binding/objective-c-libraries.md).

## <a name="type-definitions"></a>Определения типов

Синтаксис:

```csharp
[BaseType (typeof (BTYPE))
interface MyType [: Protocol1, Protocol2] {
     IntPtr Constructor (string foo);
}
```

Каждый интерфейс в определении контракта, который имеет [ `[BaseType]` ](#BaseTypeAttribute) атрибут, объявляющий базовый тип для создаваемого объекта. В приведенном выше объявлении `MyType` будет создан класс тип C#, привязывается к типу Objective-C именем `MyType`.

Если после имени типа, необходимо указать все типы (в примере выше `Protocol1` и `Protocol2`) с помощью синтаксиса наследования интерфейса содержимое этих интерфейсов, будет встроенным как если бы они были частью контракта для `MyType`.
Способ, Xamarin.iOS поверхности, что тип использует протокол, встроив все методы и свойства, которые были объявлены в протоколе в самом типе.

Показано как объявление Objective-C `UITextField` должны быть определены в Xamarin.iOS контракт:

```objc
@interface UITextField : UIControl <UITextInput> {

}
```

Будет записан следующим образом как контракт API C#:

```csharp
[BaseType (typeof (UIControl))]
interface UITextField : UITextInput {
}
```

Многие другие аспекты создания кода можно управлять путем применения других атрибутов к интерфейсу, а также настройка [ `[BaseType]` ](#BaseTypeAttribute) атрибута.


### <a name="generating-events"></a>Создание событий

Одной из особенностей разработки Xamarin.iOS и Xamarin.Mac API — что мы сопоставление классы делегатов Objective-C, как C# события и обратные вызовы. Пользователи могут выбирать в каждого экземпляра следует ли применять шаблон программирования Objective-C, назначив свойствами, например `Delegate` экземпляром класса, который реализует различные методы, которые среда выполнения C цель вызовет или по Выбор C#-стиля, событиях и свойствах.

Сообщите нам см. один пример использования модели Objective-C.

```csharp
bool MakeDecision ()
{
    return true;
}

void Setup ()
{
     var scrollView = new UIScrollView (myRect);
     scrollView.Delegate = new MyScrollViewDelegate ();
     ...
}

class MyScrollViewDelegate : UIScrollViewDelegate {
    public override void Scrolled (UIScrollView scrollView)
    {
        Console.WriteLine ("Scrolled");
    }

    public override bool ShouldScrollToTop (UIScrollView scrollView)
    {
        return MakeDecision ();
    }
}
```

В приведенном выше примере, вы увидите, что мы решили перезаписать два метода, одно уведомление, что прокрутки событие имело место и второй, обратный вызов, который должен возвращать логическое значение, оно указывает `scrollView` ли его следует перейти к первые или нет.

Модель C# позволяет пользователю библиотеки для прослушивания уведомлений с использованием синтаксиса C# событий или синтаксис свойства подключить обратные вызовы, которые должны возвращать значения.

Вот как выглядит код C# для одной функции с помощью лямбда-выражения.

```csharp
void Setup ()
{
    var scrollview = new UIScrollView (myRect);
    // Event connection, use += and multiple events can be connected
    scrollView.Scrolled += (sender, eventArgs) { Console.WriteLine ("Scrolled"); }

    // Property connection, use = only a single callback can be used
    scrollView.ShouldScrollToTop = (sv) => MakeDecision ();
}
```

Поскольку события не возвращают значения (они имеют тип возвращаемого значения void) можно подключить несколько копий. `ShouldScrollToTop` Не является событием вместо это свойство с типом `UIScrollViewCondition` которого имеет следующую сигнатуру:

```csharp
public delegate bool UIScrollViewCondition (UIScrollView scrollView);
```

Он возвращает `bool` значение, при этом синтаксис лямбда-выражения позволяет нам достаточно вернуть значение из `MakeDecision` функции.

Генератор привязки поддерживает создание события и свойства, связывающие класса, например `UIScrollView` с его `UIScrollViewDelegate` (также вызвать эти класс модели), это делается путем создания заметок к [ `[BaseType]` ](#BaseTypeAttribute) определение с `Events` и `Delegates` параметров (как описано ниже). Помимо добавления заметок к [ `[BaseType]` ](#BaseTypeAttribute) с этими параметрами это необходимо для информирования генератор несколько дополнительных компонентов.

Для событий, которые принимают несколько параметров (в Objective-C соглашением является первый параметр в класс делегата экземпляр объекта-отправителя) необходимо указать имя, которое требуется для создаваемого `EventArgs` класса. Это делается с [ `[EventArgs]` ](#EventArgsAttribute) атрибут в объявлении метода в классе модели. Пример:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Указанное выше объявление создает `UIImagePickerImagePickedEventArgs` класс, производный от `EventArgs` и пакеты оба параметра `UIImage` и `NSDictionary`. Генератор выдает это:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Затем он предоставляет следующие параметры в `UIImagePickerController` класса:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```

Модель методов, возвращающих значение привязываются по-разному. Те, требуется как имя для созданного C# делегата (сигнатура для метода), а также значения по умолчанию, для возврата в случае, если пользователь не предоставляет реализацию себе. Например `ShouldScrollToTop` определение — это:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIScrollViewDelegate {
    [Export ("scrollViewShouldScrollToTop:"), DelegateName ("UIScrollViewCondition"), DefaultValue ("true")]
    bool ShouldScrollToTop (UIScrollView scrollView);
}
```

Выше создаст `UIScrollViewCondition` с сигнатурой делегата, было показано выше, и если он не предоставляет реализацию, возвращаемым значением будет true.

В дополнение к [ `[DefaultValue]` ](#DefaultValueAttribute) атрибут, можно также использовать [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute) атрибут, который направляет генератора возвращает значение указанного параметра в вызове или [ `[NoDefaultValue]` ](#NoDefaultValueAttribute) параметр, который указывает, что генератор, нет значения по умолчанию.

<a name="BaseTypeAttribute" />

### <a name="basetypeattribute"></a>Атрибут BaseTypeAttribute

Синтаксис:

```csharp
public class BaseTypeAttribute : Attribute {
        public BaseTypeAttribute (Type t);

        // Properties
        public Type BaseType { get; set; }
        public string Name { get; set; }
        public Type [] Events { get; set; }
        public string [] Delegates { get; set; }
        public string KeepRefUntil { get; set; }
}
```

#### <a name="basetypename"></a>BaseType.Name

Использовать `Name` свойства, имя, которое будет привязан этот тип в мире Objective-C. Обычно используется для задания имени, которые соответствуют рекомендации по разработке .NET Framework, однако которой сопоставляется с именем в Objective-C, соглашению, не тип C#.

Пример, в приведенном ниже примере мы сопоставим Objective-C `NSURLConnection` тип `NSUrlConnection`, как рекомендации по разработке .NET Framework используйте «Url» вместо «URL»:

```csharp
[BaseType (typeof (NSObject), Name="NSURLConnection")]
interface NSUrlConnection {
}
```

Указанное имя используется в качестве значения для создаваемого `[Register]` атрибут в привязке. Если `Name` не указан, используется короткое имя типа в качестве значения для `[Register]` атрибута в созданные выходные данные.

#### <a name="basetypeevents-and-basetypedelegates"></a>BaseType.Events и BaseType.Delegates

Эти свойства используются для формирования C#-стиля в созданных классов событий. Они используются для связи с классом делегата Objective-C данного класса. Возникает часто, где класс использует класс делегата для отправки уведомлений и событий. Например `BarcodeScanner` бы дополнения `BardodeScannerDelegate` класса. `BarcodeScanner` Класс обычно есть `Delegate` свойство, которое можно назначить экземпляр `BarcodeScannerDelegate` в то время как это работает, может потребоваться предоставить для пользователей C#-как интерфейс в стиле событий, а также в таких случаях можно использовать `Events`и `Delegates` свойства [ `[BaseType]` ](#BaseTypeAttribute) атрибута.

Эти свойства всегда устанавливаются вместе и должны иметь одинаковое количество элементов и обеспечивать синхронизацию. `Delegates` Массив содержит одну строку для каждого слабо типизированной делегат, который вы хотите перенести, и `Events` массив содержит один тип для каждого типа, который требуется связать с ним.

```csharp
[BaseType (typeof (NSObject),
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIAccelerometerDelegate)})]
public interface UIAccelerometer {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIAccelerometerDelegate {
}
```


#### <a name="basetypekeeprefuntil"></a>BaseType.KeepRefUntil

Если этот атрибут применяется, когда создаются новые экземпляры этого класса, экземпляр этого объекта будут храниться вокруг до метод, заданный параметром `KeepRefUntil` был вызван. Это полезно для повышения удобства использования API-функций, если не хотите, чтобы ваши пользователи, чтобы сохранить ссылку на объект вокруг для использования в коде. Значение этого свойства является имя метода в `Delegate` класса, поэтому его же следует использовать в сочетании с `Events` и `Delegates` также свойства.

В следующем примере показано, как это имя используется `UIActionSheet` в Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject), KeepRefUntil="Dismissed")]
[BaseType (typeof (UIView),
           KeepRefUntil="Dismissed",
           Delegates=new string [] { "WeakDelegate" },
           Events=new Type [] {typeof(UIActionSheetDelegate)})]
public interface UIActionSheet {
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UIActionSheetDelegate {
    [Export ("actionSheet:didDismissWithButtonIndex:"), EventArgs ("UIButton")]
    void Dismissed (UIActionSheet actionSheet, nint buttonIndex);
}
```


### <a name="disabledefaultctorattribute"></a>DisableDefaultCtorAttribute

Если этот атрибут применяется к определению интерфейса он сделает невозможным генератор формирующего конструктор по умолчанию.

Этот атрибут используется в том случае, когда требуется инициализировать с помощью одного из других конструкторов в классе объекта.


### <a name="privatedefaultctorattribute"></a>PrivateDefaultCtorAttribute

Если этот атрибут применяется к определению интерфейса отметит конструктор по умолчанию как закрытый. Это означает, что можно по-прежнему создать экземпляр объекта этого класса внутренним образом из файла расширения, но он просто не будут доступны для пользователей вашего класса.

<a name="CategoryAttribute" />

### <a name="categoryattribute"></a>CategoryAttribute

Этот атрибут используется в определении типа для привязки категории Objective-C и предоставлять их как методы расширения C# для отражения способов Objective-C предоставляет функциональные возможности.

Категории включают механизм Objective-C, используемый расширить набор методов и свойств, доступных в классе.   На практике, они используются для расширения функциональности базового класса (например `NSObject`) при связана конкретной платформы в (например `UIKit`), делая их методы доступны, но только если компонуется новой платформы.   В других случаях они используются для организации функций в классе по функциональным возможностям.   Они аналогичны дух методы расширения в C#.

Это, как будет выглядеть категорию в цель C:

```objc
@interface UIView (MyUIViewExtension)
-(void) makeBackgroundRed;
@end
```

Приведенный выше пример находится на библиотеку, которая распространится экземпляров `UIView` с помощью метода `makeBackgroundRed`.

Чтобы привязать их, можно использовать [ `[Category]` ](#CategoryAttribute) атрибут в определении интерфейса.   При использовании [ `[Category]` ](#CategoryAttribute) атрибута значение [ `[BaseType]` ](#BaseTypeAttribute) изменений неизменных атрибутов не могут использоваться для указания базового класса, чтобы расширить, чтобы тип для расширения.

Показано как `UIView` расширений привязан и преобразуются в методы расширения в C#:

```csharp
[BaseType (typeof (UIView))]
[Category]
interface MyUIViewExtension {
    [Export ("makeBackgroundRed")]
    void MakeBackgroundRed ();
}
```

Выше создаст `MyUIViewExtension` класс, содержащий `MakeBackgroundRed` метода расширения.   Это означает, что теперь можно вызвать `MakeBackgroundRed` на любом `UIView` подкласс, обеспечивая те же функциональные возможности, получаемому в цель C.

В некоторых случаях вы найдете **статических** элементы внутри категории, например в следующем примере:

```objc
@interface FooObject (MyFooObjectExtension)
+ (BOOL)boolMethod:(NSRange *)range;
@end
```

Это приведет к **неправильный** определение интерфейса категории C#:

```csharp
[Category]
[BaseType (typeof (FooObject))]
interface FooObject_Extensions {

    // Incorrect Interface definition
    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Это неверно так как использование `BoolMethod` расширения требуется экземпляр `FooObject` , но вы привязываете ObjC **статических** расширения, это является побочным эффектом потому, что о реализации методов расширения в C#.

В следующем примере кода сложный — единственный способ использовать приведенные выше определения:

```csharp
(null as FooObject).BoolMethod (range);
```

Во избежание этого рекомендуется встроенного `BoolMethod` определения внутри `FooObject` само определение интерфейса, это позволит вам для вызова этого модуля, как он предназначен `FooObject.BoolMethod (range)`.

```csharp
[BaseType (typeof (NSObject))]
interface FooObject {

    [Static]
    [Export ("boolMethod:")]
    bool BoolMethod (NSRange range);
}
```

Мы будет выдано предупреждение (BI1117), каждый раз, когда мы находим [ `[Static]` ](#StaticAttribute) члена внутри [ `[Category]` ](#CategoryAttribute) определения. Если Вы действительно хотите иметь [ `[Static]` ](#StaticAttribute) элементы внутри вашей [ `[Category]` ](#CategoryAttribute) предупреждение можно отключить с помощью определения `[Category (allowStaticMembers: true)]` или посредством декорирования иличлен[ `[Category]` ](#CategoryAttribute) определение с помощью интерфейса [ `[Internal]` ](#InternalAttribute).

<a name="StaticAttribute_Class" />

### <a name="staticattribute"></a>StaticAttribute

При применении к классу этого атрибута будет просто создавать статического класса, который является производным от `NSObject`, поэтому [ `[BaseType]` ](#BaseTypeAttribute) атрибут игнорируется. Статические классы используются для размещения C открытые переменные, которые требуется предоставить.

Пример:

```csharp
[Static]
interface CBAdvertisement {
    [Field ("CBAdvertisementDataServiceUUIDsKey")]
    NSString DataServiceUUIDsKey { get; }
```

Создаст класс C# с помощью следующих API:

```csharp
public partial class CBAdvertisement  {
    public static NSString DataServiceUUIDsKey { get; }
}
```

## <a name="protocolmodel-definitions"></a>Модель, протокола и определения

Модели обычно используются реализацией протокола.
Они отличаются тем, что среда выполнения будет регистрировать только с целью C методы, которые действительно были перезаписаны.
В противном случае метод не будет зарегистрирована.

Обычно это означает, что при подкласс класса, помеченный с `ModelAttribute`, не следует вызывать базовый метод.   Вызов этого метода возникает исключение, должны реализовать поведение всего на подкласс для любых методов, которые можно переопределить.

<a name="AbstractAttribute" />

### <a name="abstractattribute"></a>AbstractAttribute

По умолчанию члены, которые являются частью протокола не являются обязательными. Это позволяет пользователям создавать подкласс `Model` объект просто производный от класса в C#, и переопределив только методы, их. Иногда требуется Objective-C контракту, что пользователь предоставляет реализацию этого метода (с флагами те `@required` директив в Objective-C). В таких случаях следует пометить эти методы с `[Abstract]` атрибута.

`[Abstract]` Атрибут может применяться к методам и свойствам и приводит к флаг созданный элемент как абстрактный и класс для абстрактного класса генератора.

Следующие берется из Xamarin.iOS:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface UITableViewDataSource {
    [Export ("tableView:numberOfRowsInSection:")]
    [Abstract]
    nint RowsInSection (UITableView tableView, nint section);
}
```

<a name="DefaultValueAttribute" />

### <a name="defaultvalueattribute"></a>DefaultValueAttribute

Указывает значение по умолчанию, возвращаемое методом, модели, если пользователь не предоставляет метод для данный конкретный метод в объект модели

Синтаксис:

```csharp
public class DefaultValueAttribute : Attribute {
        public DefaultValueAttribute (object o);
        public object Default { get; set; }
}
```

Например, в следующем классе мнимой делегата для `Camera` класса, мы предоставляем `ShouldUploadToServer` которого будет предоставляться как свойство на `Camera` класса. Если пользователь `Camera` класс явно не задано значение для лямбда-выражения, которая может реагировать на true или false, возвращаемое значение по умолчанию в этом случае будет иметь значение false, значение, которое мы указали в `DefaultValue` атрибута:

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("camera:shouldPromptForAction:"), DefaultValue (false)]
    bool ShouldUploadToServer (Camera camera, CameraAction action);
}
```

Если пользователь задает обработчик мнимой класса, это значение будет проигнорировано:

```csharp
var camera = new Camera ();
camera.ShouldUploadToServer = (camera, action) => return SomeDecision ();
```

См. также: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [ `[DefaultValueFromArgument]` ](#DefaultValueFromArgumentAttribute).

<a name="DefaultValueFromArgumentAttribute" />

### <a name="defaultvaluefromargumentattribute"></a>DefaultValueFromArgumentAttribute

Синтаксис:

```csharp
public class DefaultValueFromArgumentAttribute : Attribute {
    public DefaultValueFromArgumentAttribute (string argument);
    public string Argument { get; }
}
```

Этот атрибут, если предоставлен для метода, который возвращает значение на класс модели даст генератора, чтобы вернуть значение указанного параметра, если пользователь не предоставил свой собственный метод или лямбда-выражения.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, nfloat progress);
}
```

В приведенном выше случае, если пользователь `NSAnimation` выбрано использование любого из свойств C# события или класса и не задал `NSAnimation.ComputeAnimationCurve` метода или лямбда-выражение, возвращаемое значение будет иметь значение, переданное в параметре хода выполнения.

См. также: [ `[NoDefaultValue]` ](#NoDefaultValueAttribute), [`[DefaultValue]`](#DefaultValueAttribute)

### <a name="ignoredindelegateattribute"></a>IgnoredInDelegateAttribute

Иногда целесообразно не предоставляется доступ к событию или делегатом свойство класс модели в хост-класса, при добавлении этого атрибута даст генератора, чтобы избежать создания любого метода, назначив его.

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);

    [Export ("imagePickerController:didFinishPickingImage:"), IgnoredInDelegate)] // No event generated for this method
    void FinishedPickingImage (UIImagePickerController picker, UIImage image);
}
```

### <a name="delegatenameattribute"></a>DelegateNameAttribute

Этот атрибут используется в модели методов, возвращающих значения, задаваемые имя сигнатуре делегата для использования.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateName ("NSAnimationProgress"), DefaultValueFromArgumentAttribute ("progress")]
    float ComputeAnimationCurve (NSAnimation animation, float progress);
}
```

Вместе с определением выше генератор выдаст следующие объявления public:

```csharp
public delegate float NSAnimationProgress (MonoMac.AppKit.NSAnimation animation, float progress);
```

### <a name="delegateapinameattribute"></a>DelegateApiNameAttribute

Этот атрибут используется позволяет генератор для изменения имени свойства в классе узла. Иногда полезно при имя метода класса FooDelegate имеет смысл для класса делегата, но будет выглядеть странно в классе ведущего как свойство.

Также это действительно полезно (и необходимые) при наличии двух или дополнительные перегрузка методов, имеет смысл сохранить их с именем, как в классе FooDelegate, но вы хотите предоставлять к ним доступ в классе ведущего с заданным именем, лучше.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
public interface NSAnimationDelegate {
    [Export ("animation:valueForProgress:"), DelegateApiName ("ComputeAnimationCurve"), DelegateName ("Func<NSAnimation, float, float>"), DefaultValueFromArgument ("progress")]
    float GetValueForProgress (NSAnimation animation, float progress);
}
```

Вместе с определением выше генератор выдаст следующие глобальное объявление в классе ведущего:

```csharp
public Func<NSAnimation, float, float> ComputeAnimationCurve { get; set; }
```

<a name="EventArgsAttribute" />

### <a name="eventargsattribute"></a>EventArgsAttribute

Для событий, которые принимают несколько параметров (в Objective-C соглашением является первый параметр в класс делегата экземпляр объекта-отправителя) необходимо указать имя для создаваемого класса EventArgs может иметь. Это делается с `[EventArgs]` атрибут в объявлении метода в вашей `Model` класса.

Пример:

```csharp
[BaseType (typeof (UINavigationControllerDelegate))]
[Model][Protocol]
public interface UIImagePickerControllerDelegate {
    [Export ("imagePickerController:didFinishPickingImage:editingInfo:"), EventArgs ("UIImagePickerImagePicked")]
    void FinishedPickingImage (UIImagePickerController picker, UIImage image, NSDictionary editingInfo);
}
```

Указанное выше объявление создает `UIImagePickerImagePickedEventArgs` класс, который наследуется от EventArgs и пакеты оба параметра `UIImage` и `NSDictionary`. Генератор выдает это:

```csharp
public partial class UIImagePickerImagePickedEventArgs : EventArgs {
    public UIImagePickerImagePickedEventArgs (UIImage image, NSDictionary editingInfo);
    public UIImage Image { get; set; }
    public NSDictionary EditingInfo { get; set; }
}
```

Затем он предоставляет следующие параметры в `UIImagePickerController` класса:

```csharp
public event EventHandler<UIImagePickerImagePickedEventArgs> FinishedPickingImage { add; remove; }
```


### <a name="eventnameattribute"></a>EventNameAttribute

Этот атрибут используется позволяет генератора, чтобы изменить имя события или свойства в классе. Иногда полезно при имя метода класса модели имеет смысл в класс модели, но будет выглядеть странно в исходного класса как событие или свойство.

Например `UIWebView` использует следующий бит из `UIWebViewDelegate`:

```csharp
[Export ("webViewDidFinishLoad:"), EventArgs ("UIWebView"), EventName ("LoadFinished")]
void LoadingFinished (UIWebView webView);
```

Предоставляет выше `LoadingFinished` в качестве метода в `UIWebViewDelegate`, но `LoadFinished` как событие до отключают в `UIWebView`:

```csharp
var webView = new UIWebView (...);
webView.LoadFinished += delegate { Console.WriteLine ("done!"); }
```

<a name="ModelAttribute" />

### <a name="modelattribute"></a>ModelAttribute

При применении `[Model]` атрибут в определение типа в контракте API, среда выполнения вызывает специальный код, который только обнаружатся вызовы методов в классе, если пользователь перезаписывает метод в классе. Этот атрибут обычно применяется ко всем интерфейсам API, которые являются оболочкой для Objective-C класс делегата.

<a name="NoDefaultValueAttribute" />

### <a name="nodefaultvalueattribute"></a>NoDefaultValueAttribute

Указывает, что метод модели не предоставляет возвращаемое значение по умолчанию.

Это осуществляется с помощью среды выполнения C цель отвечать на запросы `false` на запрос Objective-C времени выполнения для определения, если указанный селектор реализована в этом классе.

```csharp
[BaseType (typeof (NSObject))]
[Model][Protocol]
interface CameraDelegate {
    [Export ("shouldDisplayPopup"), NoDefaultValue]
    bool ShouldUploadToServer ();
}
```

См. также: [ `[DefaultValue]` ](#DefaultValueAttribute), [`[DefaultValueFromArgument]`](#DefaultValueFromArgumentAttribute)  

<a name="ProtocolAttribute" />

## <a name="protocols"></a>Протоколы

Концепция протокола Objective-C не существует действительно в C#. Протоколы похожи на интерфейсы C#, но они отличаются тем, что не все методы и свойства, объявленные в протокол должен быть реализован классом, принимает его. Вместо этого некоторые методы и свойства являются необязательными.

Некоторые протоколы обычно используются как классы модели, их должна связываться с помощью [ `[Model]` ](#ModelAttribute) атрибута.

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
```

Начиная с Xamarin.iOS 7.0 новых и усовершенствованных функций привязки протокол был включен.  Все определения, содержащего `[Protocol]` атрибут на самом деле создаст три вспомогательных классов, которые значительно улучшают способ использования протоколов:

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

**Реализацию класса** предоставляет полный абстрактный класс, можно переопределить отдельных методов и получить полный строгую типизацию. Однако из-за C# не поддерживает множественное наследование, существуют сценарии, где может потребоваться другой базовый класс, но по-прежнему необходимо реализовать интерфейс.

Это место, куда созданный **определением интерфейса** поставляется в.  Он является интерфейсом, который содержит методы, необходимые от протокола.  Таким образом, разработчики, которые требуется реализовать протокол, просто реализовать интерфейс.  Среда выполнения автоматически регистрирует тип как переход протокола.

Обратите внимание, что интерфейс только перечислены требуемые методы предоставляют дополнительные методы.   Это означает классы, применяющие протокол получите полную проверку типа для необходимые методы, что потребуется прибегать к слабой типизацией (вручную с помощью экспорта атрибутов и соответствия сигнатуры) для методов необязательно протокола.

Чтобы сделать удобнее использовать API, который использует протоколы, средство привязки также создаст класс метод расширения, который предоставляет все необязательные методы.   Это означает, что до тех пор, пока, используют API, можно обрабатывать протоколы, как будто все методы.

Если вы хотите использовать определения протокола в API, необходимо написать каркас пустых интерфейсов в определении API.   Если вы хотите использовать MyProtocol в API-Интерфейс, потребуется это сделать:

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

### <a name="adopting-protocol-generated-interfaces"></a>Внедрение протокола созданные интерфейсы

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

### <a name="protocol-inlining"></a>Встраивание протокола

При связывании существующих типов Objective-C, объявленные как внедрение протокола требуется встроенный протокол напрямую. Чтобы сделать это, просто объявите протокол как интерфейс без каких-либо [ `[BaseType]` ](#BaseTypeAttribute) список протокол в списке базовых интерфейсов для интерфейса и атрибута.

Пример

```csharp
interface SpeakProtocol {
    [Export ("say:")]
    void Say (string msg);
}

[BaseType (typeof (NSObject))]
interface Robot : SpeakProtocol {
    [Export ("awake")]
    bool Awake { get; set; }
}
```


## <a name="member-definitions"></a>Определения элементов

Атрибуты в этом разделе применяются к отдельным членам типа: свойства и объявления метода.


### <a name="alignattribute"></a>AlignAttribute

Можно задать значение выравнивания для возврата типов свойств. Некоторые свойства принимают указатели на адреса, которые должны быть выровнены в определенные границы (в Xamarin.iOS, это происходит, например, с некоторыми `GLKBaseEffect` свойства, которые должны быть 16-байтовое выравнивание). Это свойство используется для оформления метод считывания и использовать значение выравнивания. Обычно используется с `OpenTK.Vector4` и `OpenTK.Matrix4` типов при интеграции с Objective-C API-интерфейсы.

Пример

```csharp
public interface GLKBaseEffect {
    [Export ("constantColor")]
    Vector4 ConstantColor { [Align (16)] get; set;  }
}
```


### <a name="appearanceattribute"></a>AppearanceAttribute

`[Appearance]` Атрибут ограничена iOS 5, где была введена внешний вид диспетчера.

`[Appearance]` Атрибут может применяться для любого метода или свойства, которые участвуют в `UIAppearance` framework. Когда этот атрибут применяется к методу или свойству в классе, он будет направлять генератор привязки для создания строго типизированного внешний вид класса, используемого для стиля все экземпляры этого класса или экземпляров, которые удовлетворяют определенным критериям.

Пример

```csharp
public interface UIToolbar {
    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);
}
```

Это приведет к возникновению следующий код в UIToolbar:

```csharp
public partial class UIToolbar {
    public partial class UIToolbarAppearance : UIView.UIViewAppearance {
        public virtual void SetBackgroundImage (UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);
        public virtual UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics)
    }
    public static new UIToolbarAppearance Appearance { get; }
    public static new UIToolbarAppearance AppearanceWhenContainedIn (params Type [] containers);
}
```

### <a name="autoreleaseattribute-xamarinios-54"></a>AutoReleaseAttribute (Xamarin.iOS 5.4)

Используйте `[AutoReleaseAttribute]` на методы и свойства, чтобы заключить в нее вызов метода в метод `NSAutoReleasePool`.

В Objective-C существует несколько методов, которые возвращают значения, которые добавляются по умолчанию `NSAutoReleasePool`. По умолчанию эти перейдет к потоку `NSAutoReleasePool`, но поскольку Xamarin.iOS также хранит ссылки на объекты до тех пор, пока существует управляемый объект, не может потребоваться сохранить ссылку на дополнительные в `NSAutoReleasePool` которого только получить слив до вашего потока Возвращает управление следующий поток, или вернуться к основной цикл.

Этот атрибут применяется, например на большой свойства (например `UIImage.FromFile`), возвращает объекты, которые были добавлены в значение по умолчанию `NSAutoReleasePool`. Без этого атрибута изображений будет сохранено при условии, что поток не обнаружил управления основного цикла. Uf потока был каким-то загрузчик фона, всегда активен и ожидании, образы никогда не снимается.

### <a name="forcedtypeattribute"></a>ForcedTypeAttribute

`[ForcedTypeAttribute]` Используется для принудительного создания управляемого типа, даже если неуправляемый возвращаемый объект не соответствует типу, описанные в определение привязки.

Это полезно, когда тип, описанный в заголовке не соответствует возвращаемый тип метода машинного кода, например выполнять следующее определение Objective-C из `NSURLSession`:

`- (NSURLSessionDownloadTask *)downloadTaskWithRequest:(NSURLRequest *)request`

Явно указано, что будут возвращены `NSURLSessionDownloadTask` экземпляр, но еще его **возвращает** `NSURLSessionTask`, который является суперклассом и поэтому не могут быть преобразованы в `NSURLSessionDownloadTask`. Так как мы находимся в контексте типобезопасный `InvalidCastException` будет происходить.

В соответствии с описанием заголовок и избежать `InvalidCastException`, `[ForcedTypeAttribute]` используется.

```csharp
[BaseType (typeof (NSObject), Name="NSURLSession")]
interface NSUrlSession {

    [Export ("downloadTaskWithRequest:")]
    [return: ForcedType]
    NSUrlSessionDownloadTask CreateDownloadTask (NSUrlRequest request);
}
```

`[ForcedTypeAttribute]` Также принимает значение типа boolean с именем `Owns` , `false` по умолчанию `[ForcedType (owns: true)]`. Владеет параметр используется для отслеживания [политику владения](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html) для **основу** объектов.

`[ForcedTypeAttribute]` Допустим только для параметров, свойства и возвращаемое значение.

<a name="BindAsAttribute" />

### <a name="bindasattribute"></a>BindAsAttribute

`[BindAsAttribute]` Разрешает привязку `NSNumber`, `NSValue` и `NSString`(перечисления) в более точные типов C#. Атрибут может быть использован для создания лучшую, более точным, .NET API через API-Интерфейсы собственного.

Вы можете декорировать методов (на возвращаемое значение), параметры и свойства с `BindAs`. Единственное ограничение состоит в том элементов **не должны** находиться внутри `[Protocol]` или [ `[Model]` ](#ModelAttribute) интерфейса.

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

Текущие типы поддерживаемых инкапсуляции являются:

* `NSValue`
* `NSNumber`
* `NSString`

#### <a name="nsvalue"></a>NSValue

Поддерживает следующие типы данных C# для инкапсуляции из/в `NSValue`:

* CGAffineTransform
* NSRange
* CGVector
* SCNMatrix4
* CLLocationCoordinate2D
* SCNVector3
* SCNVector4
* CGPoint / PointF
* CGRect / RectangleF
* CGSize / SizeF
* UIEdgeInsets
* UIOffset
* MKCoordinateSpan
* CMTimeRange
* CMTime
* CMTimeMapping
* CATransform3D

#### <a name="nsnumber"></a>NSNumber

Поддерживает следующие типы данных C# для инкапсуляции из/в `NSNumber`:

* bool
* byte
* double
* float
* short
* int
* long
* sbyte
* ushort
* uint
* ulong
* nfloat
* nint
* nuint
* перечислениям;

#### <a name="nsstring"></a>NSString

[`[BindAs]`](#BindAsAttribute) работает в conjuntion с [перечислений, поддерживаемый константа NSString](#enum-attributes) чтобы вы могли создавать улучшенный интерфейс API .NET, например:

```csharp
[BindAs (typeof (CAScroll))]
[Export ("supportedScrollMode")]
NSString SupportedScrollMode { get; set; }
```

Отобразятся:

```csharp
[Export ("supportedScrollMode")]
CAScroll SupportedScrollMode { get; set; }
```

Мы будет обрабатывать `enum`  <->  `NSString` преобразование только в том случае, если тип указанного перечисления [ `[BindAs]` ](#BindAsAttribute) — [поддерживаемый константа NSString](#enum-attributes).

#### <a name="arrays"></a>Массивы

[`[BindAs]`](#BindAsAttribute) также поддерживает массивы любого из поддерживаемых типов, может иметь следующее определение API в качестве примера:

```csharp
[return: BindAs (typeof (CAScroll []))]
[Export ("getScrollModesAt:")]
NSString [] GetScrollModes ([BindAs (typeof (CGRect []))] NSValue [] rects);
```

Отобразятся:

```csharp
[Export ("getScrollModesAt:")]
CAScroll? [] GetScrollModes (CGRect [] rects) { ... }
```

`rects` Параметр будет включен в `NSArray` , содержащий `NSValue` для каждого `CGRect` взамен вы получите массив `CAScroll?` которого был создан с помощью значения, возвращаемого `NSArray` содержащий `NSStrings`.

<a name="BindAttribute" />

### <a name="bindattribute"></a>BindAttribute

`[Bind]` Атрибут имеет два варианта использования одного при применении к методу или объявление свойства и еще один при применении к отдельным доступа get или SET в свойстве.

При использовании метода или свойства, эффект `[Bind]` атрибута является создание метода, который вызывает указанный селектор. Но не помечено полученный созданный метод [ `[Export]` ](#ExportAttribute) атрибута, это означает, что он не может участвовать в переопределении метода. Обычно используется в сочетании с `[Target]` атрибут для реализации методов расширения Objective-C.

Пример:

```csharp
public interface UIView {
    [Bind ("drawAtPoint:withFont:")]
    SizeF DrawString ([Target] string str, CGPoint point, UIFont font);
}
```

При использовании в метод получения или задания, `[Bind]` атрибут используется для изменения значения по умолчанию, чтобы вывести генератором кода при создании имен селектора Objective-C Get и set для свойства. По умолчанию, если флаг свойство с именем `fooBar`, влекущих за собой генератор `fooBar` экспорта для метода получения значения и `setFooBar:` для метода. В некоторых случаях Objective-C не соответствуют этому соглашению, обычно они изменения имени метода считывания следует `isFooBar`.
Этот атрибут используется для информирования генератор данного объекта.

Пример:

```csharp
// Default behavior
[Export ("active")]
bool Active { get; set; }

// Custom naming with the Bind attribute
[Export ("visible")]
bool Visible { [Bind ("isVisible")] get; set; }
```

<a name="AsyncAttribute" />

### <a name="asyncattribute"></a>AsyncAttribute

Только на Xamarin.iOS 6.3 и более поздних.

Этот атрибут может применяться к методам, которые принимают обработчик завершения, как их последнего аргумента.

Можно использовать `[Async]` атрибут, для которого последний аргумент является обратным вызовом методов.  При применении этого метода, генератор привязки создаст версия этого метода с суффиксом `Async`.  Если обратный вызов не имеет параметров, возвращаемое значение будет `Task`, если функция обратного вызова принимает параметр, результат будет `Task<T>`.

```csharp
[Export ("upload:complete:")]
[Async]
void LoadFile (string file, NSAction complete)
```

Ниже приведет к возникновению этого асинхронного метода.

```csharp
Task LoadFileAsync (string file);
```

Если обратный вызов принимает несколько параметров, необходимо задать `ResultType` или `ResultTypeName` для указания желаемое имя созданного типа, который будет содержать все свойства.

```csharp
delegate void OnComplete (string [] files, nint byteCount);

[Export ("upload:complete:")]
[Async (ResultTypeName="FileLoading")]
void LoadFiles (string file, OnComplete complete)
```

Следующие создаст этот асинхронный метод, где `FileLoading` содержит свойства для доступа к `files` и `byteCount`:

```csharp
Task<FileLoading> LoadFile (string file);
```

Если последний параметр обратного вызова является `NSError`, созданный `Async` метод будет проверять, если значение не равно null, и если это так, созданный асинхронный метод установит исключение задачи.

```csharp
[Export ("upload:onComplete:")]
[Async]
void Upload (string file, Action<string,NSError> onComplete);
```

Выше приводит к возникновению следующих асинхронного метода:

```csharp
Task<string> UploadAsync (string file);
```

И в случае ошибки, результирующая задача будет иметь значение исключение `NSErrorException` -оболочки итоговый `NSError`.

#### <a name="asyncattributeresulttype"></a>AsyncAttribute.ResultType

Это свойство используется для указания значения для возвращения `Task` объекта.   Этот параметр принимает существующий тип и поэтому должна определяться в одном из определений api core.

#### <a name="asyncattributeresulttypename"></a>AsyncAttribute.ResultTypeName

Это свойство используется для указания значения для возвращения `Task` объекта.   Этот параметр имеет имя нужного типа, генератор возникает ряд свойств, один для каждого параметра, принимающий обратного вызова.

#### <a name="asyncattributemethodname"></a>AsyncAttribute.MethodName

Это свойство можно используйте для настройки имя созданного асинхронных методов.   Значение по умолчанию — имя метода и добавьте текст «Async», это можно использовать, чтобы изменить это значение по умолчанию.

### <a name="disablezerocopyattribute"></a>DisableZeroCopyAttribute

Этот атрибут применяется к строковые параметры или свойства строк и генератора кода, не используйте строку нулевой копирования маршалинга для этого параметра указывает, что вместо создания нового экземпляра NSString из строки C#.
Этот атрибут требуется для строк, только если вы указываете использовать маршалинг строки нулевой копирования с помощью генератора `--zero-copy` параметр командной строки или атрибут уровня сборки `ZeroCopyStringsAttribute`.

Это происходит в случаях, где свойство объявлено в Objective-C, чтобы быть `retain` или `assign` вместо `copy` свойство. Они обычно происходят в библиотеках сторонних разработчиков, которые неправильно «оптимизированные» разработчиками. Как правило `retain` или `assign` `NSString` с момента неверно определены свойства, `NSMutableString` или классы, производные от пользователя из `NSString` может привести к изменению содержимого строк независимо от кода библиотеки, слегка критические приложение. Обычно это происходит из-за преждевременное оптимизации.

Ниже приведен два свойства в цель C:

```csharp
@property(nonatomic,retain) NSString *name;
@property(nonatomic,assign) NSString *name2;
```


### <a name="disposeattribute"></a>DisposeAttribute

При применении `[DisposeAttribute]` к классу, предоставляют фрагмента кода, который будет добавлен в `Dispose()` реализации метода класса.

С момента `Dispose` метод автоматически создаваемых `bmac-native` и `btouch-native` средства, необходимо использовать `[Dispose]` атрибут для вставки некоторый код в созданный `Dispose` реализации метода.

Пример:

```csharp
[BaseType (typeof (NSObject))]
[Dispose ("if (OpenConnections > 0) CloseAllConnections ();")]
interface DatabaseConnection {
}
```

<a name="ExportAttribute" />

### <a name="exportattribute"></a>ExportAttribute

`[Export]` Атрибута можно пометить метод или свойство, должен быть предоставлен для среды выполнения C цель. Этот атрибут совместно используется средство привязки и фактического Xamarin.iOS и Xamarin.Mac среды выполнения. Для методов, этот параметр передается verbatim в сформированный код для свойства, метод считывания и метод задания экспорта создаются на основе в базовом объявлении (см. в разделе [ `[BindAttribute]` ](#BindAttribute) сведения об изменении поведение привязки инструмента).

Синтаксис:

```csharp
public enum ArgumentSemantic {
    None, Assign, Copy, Retain.
}

[AttributeUsage (AttributeTargets.Method | AttributeTargets.Constructor | AttributeTargets.Property)]
public class ExportAttribute : Attribute {
    public ExportAttribute();
    public ExportAttribute (string selector);
    public ExportAttribute (string selector, ArgumentSemantic semantic);
    public string Selector { get; set; }
    public ArgumentSemantic ArgumentSemantic { get; set; }
}
```

[Селектор](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Selector.html) представляет имя базовому методу Objective-C или свойства, к которому осуществляется привязка.

#### <a name="exportattributeargumentsemantic"></a>ExportAttribute.ArgumentSemantic

<a name="FieldAttribute" />

### <a name="fieldattribute"></a>FieldAttribute

Этот атрибут используется для предоставления глобальной переменной C как поле, которое загружается по запросу и открыт для кода C#. Обычно это необходимо для получения значения констант, определенных в C или Objective-C, может быть либо маркеры, используемые в некоторых API-интерфейсов и, значения которых не видны и должен использоваться в качестве-— с помощью пользовательского кода.

Синтаксис:

```csharp
public class FieldAttribute : Attribute {
    public FieldAttribute (string symbolName);
    public FieldAttribute (string symbolName, string libraryName);
    public string SymbolName { get; set; }
    public string LibraryName { get; set; }
}
```

`symbolName` — Символ C для компоновки. По умолчанию это будут загружены из библиотеки, имя которого выводится из пространства имен, где определен тип. Если это не библиотеки, где выполняется поиск символа, следует передать `libraryName` параметра. При создании связи статической библиотеки, используйте `__Internal` как `libraryName` параметр.

Создаваемые свойства всегда являются статическими.

Свойства, отмеченные атрибутом поля могут быть следующих типов:

* `NSString`
* `NSArray`
* `nint` / `int` / `long`
* `nuint` / `uint` / `ulong`
* `nfloat` / `float`
* `double`
* `CGSize`
* `System.IntPtr`
* перечислениям;

Задания не поддерживаются для [перечислений, поддерживаемый константы NSString](#enum-attributes), но их можно вручную привязать его при необходимости.

Пример

```csharp
[Static]
interface CameraEffects {
     [Field ("kCameraEffectsZoomFactorKey", "CameraLibrary")]
     NSString ZoomFactorKey { get; }
}
```

<a name="InternalAttribute" />

### <a name="internalattribute"></a>InternalAttribute

`[Internal]` Атрибут может применяться к методам и свойствам и действует отметка созданный код с `internal` ключевого слова C# доступности код только для кода в созданную сборку. Обычно используется для скрытия API, которые являются слишком низкого уровня или предоставить неоптимальных открытые API, который нужно повысить по или по API, которые не поддерживаются в генератор и требуются некоторые кодировки вручную.

При создании привязки, обычно следует скрыть метод или свойство, с помощью этого атрибута и укажите другое имя для метода или свойства и затем в файле дополнительную поддержки C# необходимо добавить строго типизированную оболочку, которая предоставляет Базовая функциональность.

Пример:

```csharp
[Internal]
[Export ("setValue:forKey:")]
void _SetValueForKey (NSObject value, NSObject key);

[Internal]
[Export ("getValueForKey:")]
NSObject _GetValueForKey (NSObject key);
```

Затем в файле вспомогательные может иметь некоторый код следующим образом:

```csharp
public NSObject this [NSObject idx] {
    get {
        return _GetValueForKey (idx);
    }
    set {
        _SetValueForKey (value, idx);
    }
}
```

<a name="IsThreadStaticAttribute" />

### <a name="isthreadstaticattribute"></a>IsThreadStaticAttribute

Этот атрибут flags резервное поле для свойства, для которого создается заметка с .NET `[ThreadStatic]` атрибута. Это полезно, если поле является статической переменной потока.

### <a name="marshalnativeexceptions-xamarinios-606"></a>MarshalNativeExceptions (Xamarin.iOS 6.0.6)

Этот атрибут будет делать исключения метод поддержки native (Objective-C).
Вместо вызова метода `objc_msgSend` напрямую, вызов будет проходить через пользовательские trampoline, который перехватывает исключения ObjectiveC и маршалирует их в управляемые исключения.

В настоящее время только несколько `objc_msgSend` подписи поддерживаются (можно будет определить, если подпись не поддерживается при связывания собственного приложения, которая использует привязку завершается отсутствует monotouch_*_objc_msgSend* символа), но более может быть добавить в запрос.


### <a name="newattribute"></a>NewAttribute

Этот атрибут применяется к методам и свойствам иметь генератор создания `new` ключевого слова в начале объявления.

Он используется для устранения предупреждений компилятора, если один метод или свойство имя введено в подкласс, в котором уже существует в базовом классе.

<a name="NotificationAttribute" />

### <a name="notificationattribute"></a>NotificationAttribute

Этот атрибут можно применять к полям, чтобы иметь создают Генератор строго типизированных вспомогательный класс уведомления.

Этот атрибут может использоваться без аргументов для уведомлений, которые содержат полезные данные не найдены, или можно указать `System.Type` , ссылается на другого интерфейса, в определении API, обычно с именем, заканчивая «EventArgs». Генератор превратит интерфейса в класс, который наследуется от класса `EventArgs` и будет включать все свойства в списке. [ `[Export]` ](#ExportAttribute) Атрибут должен использоваться в `EventArgs` класса в списке имя ключа, используемого для поиска в словарь Objective-C для выборки значения.

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
      public static NSObject ObserveDidStart (NSObject objectToObserve, EventHandler<NSNotificationEventArgs> handler)
   }
}
```

Пользователи кода можно затем легко подписаться на уведомления разнесены [NSDefaultCenter](https://developer.xamarin.com/api/property/Foundation.NSNotificationCenter.DefaultCenter/) с помощью следующего кода:

```csharp
var token = MyClass.Notifications.ObserverDidStart ((notification) => {
    Console.WriteLine ("Observed the 'DidStart' event!");
});
```

Или для указанного объекта, который следует контролировать. Если передать `null` для `objectToObserve` этот метод будет вести себя так же, как его однорангового узла.

```csharp
var token = MyClass.Notifications.ObserverDidStart (objectToObserve, (notification) => {
    Console.WriteLine ("Observed the 'DidStart' event on objectToObserve!");
});
```

Значение, возвращаемое `ObserveDidStart` можно легко отказаться от получения уведомлений, следующим образом:

```csharp
token.Dispose ();
```

Или можно вызвать [NSNotification.DefaultCenter.RemoveObserver](https://developer.xamarin.com/api/member/Foundation.NSNotificationCenter.RemoveObserver/p/Foundation.NSObject//) и передать маркер. Если уведомления содержит параметры, необходимо указать вспомогательный `EventArgs` интерфейс следующим образом:

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

Выше создаст `MyScreenChangedEventArgs` класса `ScreenX` и `ScreenY` свойства, которые получает данные из [NSNotification.UserInfo](https://developer.xamarin.com/api/property/Foundation.NSNotification.UserInfo/) словаря с помощью клавиш `ScreenXKey` и `ScreenYKey` соответственно и применять соответствующие преобразований. `[ProbePresence]` Атрибут используется для генератора для проверки, если этому параметру присвоено значение `UserInfo`, вместо того чтобы попытаться извлечь значение. Используется в случае, если наличие ключа значение (обычно для логических значений).

Это позволяет писать код следующим образом:

```csharp
var token = MyClass.NotificationsObserveScreenChanged ((notification) => {
    Console.WriteLine ("The new screen dimensions are {0},{1}", notification.ScreenX, notification.ScreenY);
});
```

В некоторых случаях нет константу, не связанные со значением, передаваемым в словаре.  Apple иногда использует константы открытых символов и иногда строковых констант.  По умолчанию [ `[Export]` ](#ExportAttribute) атрибута в ваш предоставленный `EventArgs` класс будет использовать указанное имя в качестве открытых символов поиск во время выполнения.  Если это не так, а вместо этого он должен выполняться как строковая константа, а затем передать `ArgumentSemantic.Assign` значение для атрибута экспорта.

**Новые возможности Xamarin.iOS 8.4**

В некоторых случаях уведомления начнется жизни без аргументов, поэтому использование [ `[Notification]` ](#NotificationAttribute) без аргументов является приемлемым.  Однако в некоторых случаях будут вводиться параметры уведомления.  Для поддержки этого сценария, атрибут может применяться несколько раз.

Если вы разрабатываете привязки и вы хотите избежать нарушения существующий пользовательский код, будет включать существующего уведомлений из:

```csharp
interface MyClass {
    [Notification]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

В версию, в которой перечислены атрибут уведомления дважды, следующим образом:

```csharp
interface MyClass {
    [Notification]
    [Notification (typeof (MyScreenChangedEventArgs)]
    [Field ("MyClassScreenChangedNotification")]
    NSString ScreenChangedNotification { get; }
}
```

<a name="NullAllowedAttribute" />

### <a name="nullallowedattribute"></a>NullAllowedAttribute

Когда это применяется к свойству отмечает свойство как допускающий значение `null` для назначения его. Это допустимо только для ссылочных типов.

Если это применяется к параметра в подписи метода, указывает, что указанный параметр может иметь значение null, и что проверка должна быть выполнена для передачи `null` значения.

Если этот атрибут не имеет ссылочный тип, средство привязки создаст проверяет наличие значения, присваиваемого перед его передачей Objective-C и создаст убедитесь, что вызовет `ArgumentNullException` Если присвоенное значение `null`.

Пример:

```csharp
// In properties

[NullAllowed]
UIImage IconFile { get; set; }

// In methods
void SetImage ([NullAllowed] UIImage image, State forState);
```

<a name="OverrideAttribute" />

### <a name="overrideattribute"></a>OverrideAttribute

Этот атрибут используется для указания привязки генератора, привязки для этого конкретного метода должны быть отмечены с `override` ключевое слово.

### <a name="presnippetattribute"></a>PreSnippetAttribute

Этот атрибут можно использовать для вставки некоторый код, вставляемый после проверки входных параметров, но перед код вызывает метод в цель-C.

Пример

```csharp
[Export ("demo")]
[PreSnippet ("var old = ViewController;")]
void Demo ();
```

### <a name="prologuesnippetattribute"></a>PrologueSnippetAttribute

Этот атрибут можно использовать для вставки некоторый код, вставляемый перед любой из параметров, проверяются в созданный метод.

Пример

```csharp
[Export ("demo")]
[Prologue ("Trace.Entry ();")]
void Demo ();
```

### <a name="postgetattribute"></a>PostGetAttribute

Указывает, что генератор привязки для вызова указанного свойства из этого класса для выборки значения из него.

Это свойство обычно используется для обновления кэша, указывающий ссылаться на объекты, обеспечивающие ссылка на граф объекта. Обычно оно отображается в коде, который имеет такие операции, как добавить или удалить. Этот метод используется, чтобы после добавления или удаления обновлялись внутренний кэш, чтобы убедиться, что элементы оставлены управляемые ссылки на объекты, которые действительно используются. Это возможно, поскольку средство привязки создает резервное поле для всех объектов по ссылке в данной привязки.

Пример

```csharp
[BaseType (typeof (NSObject))]
[Since (4,0)]
public interface NSOperation {
    [Export ("addDependency:")][PostGet ("Dependencies")]
    void AddDependency (NSOperation op);

    [Export ("removeDependency:")][PostGet ("Dependencies")]
    void RemoveDependency (NSOperation op);

    [Export ("dependencies")]
    NSOperation [] Dependencies { get; }
}
```

В этом случае `Dependencies` свойство будет вызываться после добавления или удаления зависимостей из `NSOperation` объекта, чтобы получился граф, который представляет фактический загружены объекты, предотвращение утечек памяти как, так и повреждение памяти.

### <a name="postsnippetattribute"></a>PostSnippetAttribute

Этот атрибут можно использовать для вставки некоторых исходный код C# для вставки после как код вызывает базовый метод Objective-C

Пример

```csharp
[Export ("demo")]
[PostSnippet ("if (old != null) old.DemoComplete ();")]
void Demo ();
```

### <a name="proxyattribute"></a>ProxyAttribute

Этот атрибут применяется к возвращаемым значениям, чтобы пометить их как прокси-объекты. Некоторые объекты возвращаемого прокси Objective-C API-интерфейсы, которые не могут отличаться от пользователя привязки. Чтобы пометить объект как действует этот атрибут `DirectBinding` объекта. Для сценария в Xamarin.Mac, вы увидите [обсуждения на эту ошибку](https://bugzilla.novell.com/show_bug.cgi?id=670844).

### <a name="retainlistattribute"></a>RetainListAttribute

Указывает, что генератор управляемого ссылку на параметр Сохранить или удалить внутреннюю ссылку на параметр. Это позволяет хранить ссылки на объекты.

Синтаксис:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Если значение `doAdd` имеет значение true, то добавляется параметр `__mt_{0}_var List<NSObject>;`. Где `{0}` заменяется данного `listName`. В разделяемом классе дополнительный API-интерфейса, необходимо объявить этот резервное поле.

Пример [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) и [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)

### <a name="releaseattribute-xamarinios-60"></a>ReleaseAttribute (Xamarin.iOS 6.0)

Это может применяться для возврата типов, чтобы показать, что генератор должен вызывать `Release` для объекта перед его возвратом. Это требуется, только если метод не является объектом сохранено (в отличие от объекта autoreleased является наиболее распространенным сценарием)

Пример

```csharp
[Export ("getAndRetainObject")]
[return: Release ()]
NSObject GetAndRetainObject ();
```

Кроме того, чтобы среда выполнения Xamarin.iOS известно, что его необходимо сохранить объект при возвращении для Objective-C с такой функции этот атрибут отражается в созданный код.


### <a name="sealedattribute"></a>SealedAttribute

Указывает, что генератор для флага созданный метод как запечатанный. Если этот атрибут не указан, по умолчанию используется для создания виртуального метода (виртуальный метод, абстрактного метода или переопределения в зависимости от того, как используются другие атрибуты).

<a name="StaticAttribute" />

### <a name="staticattribute"></a>StaticAttribute

Когда `[Static]` атрибут применяется к методу или свойству, это приводит к возникновению ошибки статический метод или свойство. Если этот атрибут не указан, генератор создает экземпляр метода или свойства.


### <a name="transientattribute"></a>TransientAttribute

Используйте этот атрибут для свойства флагов, значения которого являются временными, то есть, временно созданных операций ввода-вывода, но не долгоживущих объектов. Когда этот атрибут применяется к свойству, генератор не создает резервное поле для этого свойства, это означает, что управляемый класс не сохраняет ссылку на объект.

<a name="WrapAttribute" />

### <a name="wrapattribute"></a>WrapAttribute

В конструкторе привязки Xamarin.iOS/Xamarin.Mac `[Wrap]` атрибут используется для создания оболочек объект слабо типизированной с строго типизированный объект. Это вступает в действие главным образом с объектами делегат Objective-C, которые обычно объявлять как оно принадлежит к типу `id` или `NSObject`. Является правилом, используемым Xamarin.iOS и Xamarin.Mac для предоставления этих делегатов или источников данных, как оно принадлежит к типу `NSObject` и именуются соглашение «Weak» плюс имя раскрытия. `id delegate` Свойство из Objective-C будет представляться как `NSObject WeakDelegate { get; set; }` свойства в файле контракта API.

Но обычно значение, присвоенное этот делегат имеет строгий тип, поэтому мы контактной строгий тип, а затем применить `[Wrap]` атрибут, это означает, что пользователи могут пожелать использования слабых типов, если требуются некоторые fine управления или при необходимости прибегать к tric низкого уровня KS, или же их можно использовать строго типизированным свойством большую часть своей работы.

Пример

```csharp
[BaseType (typeof (NSObject))]
interface Demo {
     [Export ("delegate"), NullAllowed]
     NSObject WeakDelegate { get; set; }

     [Wrap ("WeakDelegate")]
     DemoDelegate Delegate { get; set; }
}

[BaseType (typeof (NSObject))]
[Model][Protocol]
interface DemoDelegate {
    [Export ("doDemo")]
    void DoDemo ();
}
```

Вот как пользователь будет использовать версию слабо типизированной делегата.

```csharp
// The weak case, user has to roll his own
class SomeObject : NSObject {
    [Export ("doDemo")]
    void CallbackForDoDemo () {}

}

var demo = new Demo ();
demo.WeakDelegate = new SomeObject ();
```

И это является как пользователь будет использовать строго типизированную версию, обратите внимание, что пользователь использует преимущества системы типов C# и использует ключевое слово override объявить его назначение и что он не требуется вручную оформлять метод с `[Export]`, так как мы сделали, работают в привязке для пользователя.

```csharp
// This is the strong case,
class MyDelegate : DemoDelegate {
   override void Demo DoDemo () {}
}

var strongDemo = new Demo ();
demo.Delegate = new MyDelegate ();
```

Другое использование оператора `[Wrap]` атрибут предназначен для поддержки версии строго типизированных методов.  Пример:

```csharp
[BaseType (typeof (NSObject))]
interface XyzPanel {
    [Export ("playback:withOptions:")]
    void Playback (string fileName, [NullAllowed] NSDictionary options);

    [Wrap ("Playback (fileName, options == null ? null : options.Dictionary")]
    void Playback (string fileName, XyzOptions options);
}
```

Элементы, формируемые службами `[Wrap]` не `virtual` по умолчанию, если вам требуется `virtual` можно задать для члена `true` необязательный `isVirtual` параметра.

```csharp
[BaseType (typeof (NSObject))]
interface FooExplorer {
    [Export ("fooWithContentsOfURL:")]
    void FromUrl (NSUrl url);

    [Wrap ("FromUrl (NSUrl.FromString (url))", isVirtual: true)]
    void FromUrl (string url);
}
```

## <a name="parameter-attributes"></a>Атрибуты параметра

В этом разделе описаны атрибуты, которые можно применить к параметров в определении метода и `[NullAttribute]` , относящееся к свойству в целом.

<a name="BlockCallback" />

### <a name="blockcallback"></a>BlockCallback

Этот атрибут применяется к типам параметров в объявлениях делегатов в C# для уведомления связыватель, что рассматриваемый параметр соответствует блока Objective-C, соглашение о вызовах и должны быть таким образом.

Обычно используется для обратных вызовов, которые определены следующим образом в цель C:

```objc
typedef returnType (^SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

См. также: [CCallback](#CCallback).

<a name="CCallback" />

### <a name="ccallback"></a>CCallback

Этот атрибут применяется к типам параметров в объявлениях делегатов в C# для уведомления связыватель, что рассматриваемый параметр соответствует C ABI функция указатель соглашение о вызовах и должны быть таким образом.

Обычно используется для обратных вызовов, которые определены следующим образом в цель C:

```objc
typedef returnType (*SomeTypeDefinition) (int parameter1, NSString *parameter2);
```

См. также: [BlockCallback](#BlockCallback).

### <a name="params"></a>Params

Можно использовать `[Params]` атрибута для определения метода, чтобы внедрить «params» в определении генератор последнего параметра массива.   Это позволяет легко разрешить для необязательных параметров привязку.

Например следующее определение:

```csharp
[Export ("loadFiles:")]
void LoadFiles ([Params]NSUrl [] files);
```

Позволяет записать следующий код:

```csharp
foo.LoadFiles (new NSUrl (url));
foo.LoadFiles (new NSUrl (url1), new NSUrl (url2), new NSUrl (url3));
```

Это имеет дополнительное преимущество, что он не требует от пользователей для создания массива исключительно для передачи элементов.

<a name="plainstring" />

### <a name="plainstring"></a>PlainString

Можно использовать `[PlainString]` атрибута перед строковых параметров, чтобы дать указание генератора привязки передать строку как строка C, вместо того чтобы передавать параметр как `NSString`.

Большинство интерфейсов API Objective-C используют `NSString` параметры, но они предоставляют несколько API-интерфейсы `char *` API-Интерфейс для передачи строк, а не `NSString` вариантов.
Используйте `[PlainString]` в таких случаях.

Например, следующие объявления Objective-C:

```csharp
- (void) setText: (NSString *) theText;
- (void) logMessage: (char *) message;
```

Должен быть привязан к следующим образом:

```csharp
[Export ("setText:")]
void SetText (string theText);

[Export ("logMessage:")]
void LogMessage ([PlainString] string theText);
```

### <a name="retainattribute"></a>RetainAttribute

Указывает, что генератора, чтобы сохранить ссылку на указанный параметр. Генератор предоставит резервного хранилища для этого поля, или можно указать имя ( `WrapName`) для хранения в значения. Это полезно для хранения ссылки на управляемый объект, который передается как параметр Objective-C, и если известно, что Objective-C будет хранить только эта копия объекта. Например, API-Интерфейс, как `SetDisplay (SomeObject)` будет использовать этот атрибут, вполне вероятно, что SetDisplay только удалось отобразить один объект за раз. Если необходимо для отслеживания нескольких объектов (например, для API стека по принципу) будет использовать `[RetainList]` атрибута.

Синтаксис:

```csharp
public class RetainAttribute {
    public RetainAttribute ();
    public RetainAttribute (string wrapName);
    public string WrapName { get; }
}
```


### <a name="retainlistattribute"></a>RetainListAttribute

Указывает, что генератор управляемого ссылку на параметр Сохранить или удалить внутреннюю ссылку на параметр. Это позволяет хранить ссылки на объекты.

Синтаксис:

```csharp
public class RetainListAttribute: Attribute {
     public RetainListAttribute (bool doAdd, string listName);
}
```

Если значение `doAdd` имеет значение true, то добавляется параметр `__mt_{0}_var List<NSObject>`. Где `{0}` заменяется данного `listName`. В разделяемом классе дополнительный API-интерфейса, необходимо объявить этот резервное поле.

Пример [foundation.cs](https://github.com/mono/maccore/blob/master/src/foundation.cs) и [NSNotificationCenter.cs](https://github.com/mono/maccore/blob/master/src/Foundation/NSNotificationCenter.cs)


### <a name="transientattribute"></a>TransientAttribute

Этот атрибут применяется к параметрам и используется только во время перехода с Objective-C на C#.  Во время этих переходов различных Objective-C `NSObject` параметры упаковываются в представление управляемого объекта.

Среда выполнения будет принимать ссылку на собственный объект и сохранять ссылку до последней управляемые ссылки на объект удаляется и сборщик Мусора имеет возможность запуска.

В некоторых случаях важно для среды выполнения C# не сохранить ссылку на собственный объект.  Это возможно, если базовый собственный код присоединил особое поведение в жизненном цикле параметра.  Например: деструктор для параметра выполнения некоторых операций очистки или удаления некоторых ценным ресурсом.

Этот атрибут информирует среду выполнения, который нужен объект удаляется, если это возможно при возврате к Objective-C перезаписан метод.

Простое правило: Если среда выполнения была возможность создания нового управляемого представления из собственного объекта, то в конце функции будут удалены данные о количестве сохранить собственный объект и свойство дескриптор управляемого объекта будет очищено.   Это означает, что если ссылку на управляемый объект, такая ссылка будет бесполезен (вызов методов в нем будет вызвано исключение).

Если переданный объект не был создан, или если уже был представление необработанных управляемого объекта, принудительного удаления не производится. 


## <a name="property-attributes"></a>Атрибуты свойства

<a name="NotImplementedAttribute" />

### <a name="notimplementedattribute"></a>NotImplementedAttribute

Этот атрибут используется для поддержки идиоматикой Objective-C, где свойство с получения впервые появился в базовом классе и изменяемый подкласс вводит setter.

C# не поддерживает эту модель, поэтому базовый класс должен иметь метод задания и считывания и можно использовать подкласс [OverrideAttribute](#OverrideAttribute).

Этот атрибут используется только в методах задания свойств и используется для поддержки изменяемый идиому в цель-C.

Пример

```csharp
[BaseType (typeof (NSObject))]
interface MyString {
    [Export ("initWithValue:")]
    IntPtr Constructor (string value);

    [Export ("value")]
    string Value {
        get;

    [NotImplemented ("Not available on MyString, use MyMutableString to set")]
        set;
    }
}

[BaseType (typeof (MyString))]
interface MyMutableString {
    [Export ("value")]
    [Override]
    string Value { get; set; }
}
```

<a name="enum-attributes" />

## <a name="enum-attributes"></a>Атрибуты перечислений

Сопоставление `NSString` константы, значения перечисления является простой способ создания улучшенный интерфейс API .NET. Он:

* позволяет автозавершения удобнее, отображая **только** правильные значения для API-интерфейса;
* Добавляет тип безопасности, нельзя использовать другое `NSString` константы в неправильный контекст; и
* позволяет скрыть некоторые константы, делая автозавершения Показать более короткому списку API без потери функциональности.

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
```

По определению выше привязки создает генератор `enum` сам и будет также создан `*Extensions` статического типа, который включает методы двух способов преобразования между значениями перечисления и `NSString` константы. Это означает, что константы остается доступной для разработчиков, даже если они не являются частью API-интерфейса.

Примеры

```csharp
// using the NSString constant in a different API / framework / 3rd party code
CallApiRequiringAnNSString (NSRunLoopMode.Default.GetConstant ());
```

```csharp
// converting the constants from a different API / framework / 3rd party code
var constant = CallApiReturningAnNSString ();
// back into an enum value
CallApiWithEnum (NSRunLoopModeExtensions.GetValue (constant));
```

### <a name="defaultenumvalueattribute"></a>DefaultEnumValueAttribute

Вы можете декорировать **один** значение перечисления с помощью этого атрибута. Это может быть константой, возвращается, если значение перечисления не известен.

Из предыдущего примера:

```csharp
var x = (NSRunLoopMode) 99;
Call (x.GetConstant ()); // NSDefaultRunLoopMode will be used
```

Если значение enum не помечено то `NotSupportedException` будет создано.

### <a name="errordomainattribute"></a>ErrorDomainAttribute

Коды ошибок привязаны как значения перечисления. Ошибка домена для них обычно есть и не всегда просто найти, какая из них применяется (или если он еще существует).

Этот атрибут можно использовать для присоединения к ним перечисления ошибка домена.

Пример

```csharp
    [Native]
    [ErrorDomain ("AVKitErrorDomain")]
    public enum AVKitError : nint {
        None = 0,
        Unknown = -1000,
        PictureInPictureStartFailed = -1001
    }
```

Затем можно вызвать метод расширения `GetDomain` для получения константы любая ошибка домена.

### <a name="fieldattribute"></a>FieldAttribute

Это то же самое `[Field]` атрибут, используемый для константы внутри типа. Он также может использоваться внутри перечисления для сопоставления значения с определенной константой.

Объект `null` значение может использоваться для указания нужное значение перечисления должен быть возвращен `null` `NSString` указана константа.

Из предыдущего примера:

```csharp
var constant = NSRunLoopMode.NewInWatchOS3; // will be null in watchOS 2.x
Call (NSRunLoopModeExtensions.GetValue (constant)); // will return 1000
```

Если не `null` будет присутствовать то `ArgumentNullException` будет создано.

## <a name="global-attributes"></a>Глобальные атрибуты

Глобальные атрибуты либо применяются с использованием `[assembly:]` модификатор атрибута как [ `[LinkWithAttribute]` ](#LinkWithAttribute) или может быть использована везде, где, таких как [ `[Lion]` ](#SinceAndLionAttributes) и [ `[Since]` ](#SinceAndLionAttributes) атрибуты.

<a name="LinkWithAttribute" />

### <a name="linkwithattribute"></a>LinkWithAttribute

Это атрибут уровня сборки, который позволяет разработчикам задать флаги связывания, требуется повторно использовать связанные библиотеки не требует, чтобы потребитель библиотеки, чтобы вручную настроить gcc_flags и mtouch дополнительные аргументы, переданные в библиотеку.

Синтаксис:

```csharp
// In properties
[Flags]
public enum LinkTarget {
    Simulator    = 1,
    ArmV6    = 2,
    ArmV7    = 4,
    Thumb    = 8,
}

[AttributeUsage(AttributeTargets.Assembly, AllowMultiple=true)]
public class LinkWithAttribute : Attribute {
    public LinkWithAttribute ();
    public LinkWithAttribute (string libraryName);
    public LinkWithAttribute (string libraryName, LinkTarget target);
    public LinkWithAttribute (string libraryName, LinkTarget target, string linkerFlags);
    public bool ForceLoad { get; set; }
    public string Frameworks { get; set; }
    public bool IsCxx { get; set;  }
    public string LibraryName { get; }
    public string LinkerFlags { get; set; }
    public LinkTarget LinkTarget { get; set; }
    public bool NeedsGccExceptionHandling { get; set; }
    public bool SmartLink { get; set; }
    public string WeakFrameworks { get; set; }
}
```

Этот атрибут применяется на уровне сборки, например, это [CorePlot привязки](https://github.com/mono/monotouch-bindings/tree/master/CorePlot) использовать:

```csharp
[assembly: LinkWith ("libCorePlot-CocoaTouch.a", LinkTarget.ArmV7 | LinkTarget.ArmV7s | LinkTarget.Simulator, Frameworks = "CoreGraphics QuartzCore", ForceLoad = true)]
```

При использовании `[LinkWith]` атрибут, указанный `libraryName` внедряется в результирующей сборке, позволяя пользователям поставлен одну библиотеку DLL, содержащий неуправляемых зависимостей как, так и флаги командной строки, необходимые для правильной работы библиотеку Xamarin.iOS.

Существует также возможность предоставляет `libraryName`в этом случае `LinkWith` атрибут может использоваться только задание флагов дополнительных компоновщика:

``` csharp
[assembly: LinkWith (LinkerFlags = "-lsqlite3")]
```

#### <a name="linkwithattribute-constructors"></a>Конструкторы LinkWithAttribute

Эти конструкторы позволяют укажите библиотеку для связи с и внедрять в результирующей сборке, поддерживаемые целевые объекты, которые поддерживает библиотеку и любые дополнительные библиотеки флаги, которые необходимы для связывания с библиотекой.

Обратите внимание, что `LinkTarget` аргумент неявно определяется Xamarin.iOS и не требуется задать.

Примеры

```csharp
// Specify additional linker:
[assembly: LinkWith (LinkerFlags = "-sqlite3")]

// Specify library name for the constructor:
[assembly: LinkWith ("libDemo.a");

// Specify library name, and link target for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator);

// Specify only the library name, link target and linker flags for the constructor:
[assembly: LinkWith ("libDemo.a", LinkTarget.Thumb | LinkTarget.Simulator, SmartLink = true, ForceLoad = true, IsCxx = true);
```

#### <a name="linkwithattributeforceload"></a>LinkWithAttribute.ForceLoad

`ForceLoad` Чтобы определить, используется свойство ли `-force_load` ссылку флаг используется для связывания собственной библиотеки. Сейчас это всегда будет значение true.

#### <a name="linkwithattributeframeworks"></a>LinkWithAttribute.Frameworks

Если выполняется привязка библиотеки имеет жестких требований ни одной из платформ (отличного от `Foundation` и `UIKit`), необходимо задать `Frameworks` свойство строку, содержащую перечень платформ платформы, необходимые. Например, если выполняется привязка библиотеки, требует `CoreGraphics` и `CoreText`, необходимо установить `Frameworks` свойства `"CoreGraphics CoreText"`.

#### <a name="linkwithattributeiscxx"></a>LinkWithAttribute.IsCxx

Если установлено значение true, если необходимо скомпилировать с помощью компилятора C++ вместо значения по умолчанию, который компилятор C полученный исполняемый файл. Используйте только для библиотеки, в которой выполняется привязка была на языке C++.

#### <a name="linkwithattributelibraryname"></a>LinkWithAttribute.LibraryName

Имя неуправляемую библиотеку для формирования пакета. Это файл с расширением «.a» и может содержать объектный код для нескольких платформ (для, например, ARM и x86 для имитатора).

Проверка более ранних версиях Xamarin.iOS `LinkTarget` свойства, чтобы определить платформу библиотеки поддерживается, но это определяется теперь автоматически и `LinkTarget` свойство игнорируется.

#### <a name="linkwithattributelinkerflags"></a>LinkWithAttribute.LinkerFlags

`LinkerFlags` Строка предоставляет способ авторы привязок для указания необходимости компоновщика дополнительные флаги при связывании собственной библиотеки в приложение.

Например, если собственной библиотеки требует libxml2 и zlib, можно задать `LinkerFlags` строка `"-lxml2 -lz"`.

#### <a name="linkwithattributelinktarget"></a>LinkWithAttribute.LinkTarget

Проверка более ранних версиях Xamarin.iOS `LinkTarget` свойства, чтобы определить платформу библиотеки поддерживается, но это определяется теперь автоматически и `LinkTarget` свойство игнорируется.

#### <a name="linkwithattributeneedsgccexceptionhandling"></a>LinkWithAttribute.NeedsGccExceptionHandling

Если установлено значение true, если библиотеки, в которой выполняется связывание требуется библиотека GCC обработка исключений (gcc_eh)

#### <a name="linkwithattributesmartlink"></a>LinkWithAttribute.SmartLink

`SmartLink` Свойство должно быть присвоено значение true, чтобы определить, Xamarin.iOS ли `ForceLoad` либо не требуется.

#### <a name="linkwithattributeweakframeworks"></a>LinkWithAttribute.WeakFrameworks

`WeakFrameworks` Работает так же, как свойство `Frameworks` свойства, за исключением того, что во время компоновки `-weak_framework` описатель передается gcc для каждого из перечисленных платформ.

`WeakFrameworks` делает возможным библиотек и приложений на слабо связи с платформой платформ, чтобы их при необходимости их можно использовать, если они доступны, но не выполняют жесткие зависимости от их, это полезно, если библиотека предназначена для добавления дополнительных возможностей в более новой версии iOS. Дополнительные сведения о слабое связывание документации компании Apple на [слабое связывание](http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WeakLinking.html).

Хорошими кандидатами для слабого связывания будет `Frameworks` учетные записи, такие как `CoreBluetooth`, `CoreImage`, `GLKit`, `NewsstandKit` и `Twitter` так, как они доступны только в iOS 5.

<a name="SinceAndLionAttributes" />

### <a name="sinceattribute-ios-and-lionattribute-macos"></a>SinceAttribute (iOS) и LionAttribute (macOS)

Вы используете `[Since]` атрибут флаг API-интерфейсы как имеющий проникновения в определенный момент времени. Атрибут должен использоваться только для флага типы и методы, которые могли стать причиной проблемы среды выполнения, если базовый класс, метод или свойство не будет доступна.

Синтаксис:

```csharp
public SinceAttribute : Attribute {
     public SinceAttribute (byte major, byte minor);
     public byte Major, Minor;
}
```

Он обычно не применяться к перечислений, ограничения или новые структуры как те, не приведет к ошибке времени выполнения, если они выполняются на устройстве со старой версией операционной системы.

Пример, при применении к типу.

```csharp
// Type introduced with iOS 4.2
[Since (4,2)]
[BaseType (typeof (UIPrintFormatter))]
interface UIViewPrintFormatter {
    [Export ("view")]
    UIView View { get; }
}
```

Пример, при применении к новому члену.

```csharp
[BaseType (typeof (UIViewController))]
public interface UITableViewController {
    [Export ("tableView", ArgumentSemantic.Retain)]
    UITableView TableView { get; set; }

    [Since (3,2)]
    [Export ("clearsSelectionOnViewWillAppear")]
    bool ClearsSelectionOnViewWillAppear { get; set; }
```

`[Lion]` Таким же образом, но для типов, представленных в Lion применяется атрибут. Причина использования `[Lion]` и более конкретные номер версии, который используется в iOS — iOS, пересматривается очень часто, хотя основные выпуски OS X происходит редко, легче запомнить, их с кодовым названием чем их номера версии операционной системы

### <a name="adviceattribute"></a>AdviceAttribute

Этот атрибут используется для предоставления разработчикам подсказкой другие интерфейсы API, который может быть более удобным для него.   Например при указании строго типизированную версию API-интерфейса можно позволяет этого атрибута для атрибута слабо типизированной указать разработчику улучшенный интерфейс API.

Сведения из этого атрибута отображается в документации и средств, которые могут разрабатываться для предоставления пользователю предложения по улучшению

### <a name="zerocopystringsattribute"></a>ZeroCopyStringsAttribute

Только в Xamarin.iOS 5.4 и более поздних.

Этот атрибут указывает, что генератор, привязки для этой определенной библиотеки (если применяется с `[assembly:]`) или типа следует использовать маршалинг строки быстрого копирования на ноль. Этот атрибут является эквивалентно передаче параметр командной строки `--zero-copy` генератора.

При использовании копирования ноль для строк, эффективно генератор использует ту же строку на C# в качестве строки, потребляемой Objective-C без дополнительных создания нового `NSString` объекта и как избежать копирования данных из строки C# Objective-C-строку. С помощью копирования ноль строк только недостаток заключается в том, что необходимо убедиться, что любое строковое свойство, можно переносить, происходит помечен как `retain` или `copy` имеет `[DisableZeroCopy]` набором атрибутов. Это нужно, так как дескриптор для копирования нулевой строки размещаются в стеке и является недопустимым после возврата функции.

Пример

```csharp
[ZeroCopyStrings]
[BaseType (typeof (NSObject))]
interface MyBinding {
    [Export ("name")]
    string Name { get; set; }

    [Export ("domain"), NullAllowed]
    string Domain { get; set; }

    [DisablZeroCopy]
    [Export ("someRetainedNSString")]
    string RetainedProperty { get; set; }
}

```

Можно также применить атрибут на уровне сборки и будут применяться ко всем типам сборки:

```csharp
[assembly:ZeroCopyStrings]
```

## <a name="strongly-typed-dictionaries"></a>Строго типизированные словарей

С помощью Xamarin.iOS 8.0 мы представили поддержки для упрощения создания строго типизированных классов, которые упаковывают `NSDictionaries`.

Хотя всегда можно было использовать [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) вручную интерфейс API, тип данных стало гораздо проще сделать это.  Дополнительные сведения см. в разделе [распределение результатов строгих типов](~/cross-platform/macios/binding/objective-c-libraries.md#Surfacing_Strong_Types).

<a name="StrongDictionary" />

### <a name="strongdictionary"></a>StrongDictionary

Когда этот атрибут применяется к интерфейсу, генератор создает класс с тем же именем, как интерфейс, который является производным от [DictionaryContainer](https://developer.xamarin.com/api/type/Foundation.DictionaryContainer/) и включает каждого свойства, определенные в интерфейсе в строго типизированный Get и set для словаря.

Это автоматически создает класс, который может быть создан из существующего `NSDictionary` или который был создан новый.

Этот атрибут принимает один параметр, имя класса, содержащий ключи, используемые для доступа к элементам в словаре.   По умолчанию каждое свойство в интерфейсе, атрибут будет Уточняющий запрос элемента в указанном типе имени с суффиксом «Ключ».

Пример:

```csharp
[StrongDictionary ("MyOptionKeys")]
interface MyOption {
    string Name { get; set; }
    nint    Age  { get; set; }
}

[Static]
interface MyOptionKeys {
    // In Objective-C this is "NSString *MYOptionNameKey;"
    [Field ("MYOptionNameKey")]
    NSString NameKey { get; }

    // In Objective-C this is "NSString *MYOptionAgeKey;"
    [Field ("MYOptionAgeKey")]
    NSString AgeKey { get; }
}

```

В приведенном выше примере `MyOption` класс сформирует строковое свойство для `Name` , использующий `MyOptionKeys.NameKey` как ключ в словаре для получения строки.   И будет использовать `MyOptionKeys.AgeKey` как ключ в словаре для получения `NSNumber` , содержащее значения типа int.

Если вы хотите использовать другой ключ, можно использовать атрибут export для свойства, например:

```csharp
[StrongDictionary ("MyColoringKeys")]
interface MyColoringOptions {
    [Export ("TheName")]  // Override the default which would be NameKey
    string Name { get; set; }

    [Export ("TheAge")] // Override the default which would be AgeKey
    nint    Age  { get; set; }
}

[Static]
interface MyColoringKeys {
    // In Objective-C this is "NSString *MYColoringNameKey"
    [Field ("MYColoringNameKey")]
    NSString TheName { get; }

    // In Objective-C this is "NSString *MYColoringAgeKey"
    [Field ("MYColoringAgeKey")]
    NSString TheAge { get; }
}
```

#### <a name="strong-dictionary-types"></a>Надежный словарей.

Следующие типы данных поддерживаются в `StrongDictionary` определение:

|Тип интерфейса в C#|`NSDictionary` Тип хранилища|
|---|---|
|`bool`|`Boolean` хранимые в `NSNumber`|
|Значения перечисления|целое число хранимых в `NSNumber`|
|`int`|32-разрядное целое число со знаком, хранимых в `NSNumber`|
|`uint`|32-разрядное целое число без знака, хранящиеся в `NSNumber`|
|`nint`|`NSInteger` хранимые в `NSNumber`|
|`nuint`|`NSUInteger` хранимые в `NSNumber`|
|`long`|64-разрядное целое число, хранимых в `NSNumber`|
|`float`|32-разрядное целое число со знаком, хранятся в виде `NSNumber`|
|`double`|64-разрядное целое число, хранятся в виде `NSNumber`|
|`NSObject` и подклассов|`NSObject`|
|`NSDictionary`|`NSDictionary`|
|`string`|`NSString`|
|`NSString`|`NSString`|
|C# `Array` из `NSObject`|`NSArray`|
|C# `Array` перечислений|`NSArray` содержащий `NSNumber` значения|
