---
title: Универсальный подклассов NSObject в Xamarin.iOS
description: В этом документе описывается создание Создание универсального подклассов NSObject. Он проверяет, что можно и не может быть выполнена, рассматриваются статических регистратора и смотрит на производительность.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9caad9d4990225a0468be8ee4987eaa9fea0c118
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786487"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Универсальный подклассов NSObject в Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Использование универсальных шаблонов с NSObjects

Начиная с Xamarin.iOS 7.2.1 можно использовать универсальные типы в подклассах `NSObject` (например [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)).

Теперь можно создать универсальных классов, похожее на следующее:

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Поскольку объекты Этот подкласс `NSObject` зарегистрированы с помощью среды выполнения C цель существуют некоторые ограничения относительно возможности универсального подклассы `NSObject` типов.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Рекомендации для универсального подклассов NSObject

В этом документе описаны ограничения в ограниченную поддержку для универсальных подклассы `NSObjects` предварен Xamarin.iOS 7.2.1.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Аргументы универсального типа в сигнатурах членов

Все аргументы универсального типа в сигнатуре члена, доступны для Objective-C должны иметь `NSObject` ограничение.

**Хороший**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: параметр универсального типа является `NSObject`, поэтому подпись селектор для `myMethod:` безопасно доступные для Objective-C (всегда будет иметь `NSObject` или его подклассу).

**Неверный**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: не удается создать подпись Objective-C для экспортируемых элементов, которые могут вызывать код Objective-C, так как подпись будет отличаться в зависимости от точного типа универсального типа `T`.

**Хороший**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Причина**: можно неограниченное аргументов универсального типа, до тех пор, пока они не принимают участия подписи экспортированного элемента.

**Хороший**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Причина**: `T` экспорта параметров в Objective-C `MyMethod` должен поддерживать `NSObject`, без ограничений типа `U` не является частью сигнатуры.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Создание экземпляров универсальных типов из Objective c.

Создание экземпляров универсальных типов для Objective-C не допускается. Обычно это происходит при использовании управляемого типа в xib.

Рассмотрим следующее определение класса, который предоставляет конструктор, принимающий `IntPtr` (Xamarin.iOS способ создания объекта C# из собственного экземпляра Objective-C):
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Хотя выше конструкция нормально после того, во время выполнения, то будет создано исключение во Objective-C пытается создать его экземпляр.

Это происходит потому, что Objective-C имеет отсутствует понятие универсальных типов, и он не может задавать точное универсального типа для создания.

Эта проблема может быть решена путем создания специализированным подклассом универсального типа.   Пример:
    
```
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Теперь нет неоднозначности, класс `GenericUIView` может использоваться в xibs.

## <a name="no-support-for-generic-methods"></a>Универсальные методы не поддерживаются

### <a name="generic-methods-are-not-allowed"></a>Универсальные методы не допускаются.

Следующий код компилироваться не будет.

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Причина**: это не допускается, так как Xamarin.iOS не знает, какой тип следует использовать для аргумента типа `T` при вызове метода с целью C.

Альтернативой является создание специализированных метод и экспорт, вместо этого:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Не допускается экспортированного статические члены

Можно не предоставлять статические члены для Objective-C, если она размещается внутри универсального подкласс `NSObject`.

Пример неподдерживаемым сценарием.

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Причина:** так же, как универсальные методы Xamarin.iOS среды выполнения необходимо определить, какой тип для аргумента универсального типа T.

Для экземпляра членов, сам экземпляр используется (поскольку никогда не будет экземпляр универсального<T>, всегда будет иметь универсальный<SomeSpecificClass>), однако для статических элементов эта информация отсутствует.

Обратите внимание, что это применимо, даже если рассматриваемый элемент не используется аргумент типа T каким-либо образом.

Вместо него следует использовать в этом случае является создание специализированных подклассов.

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIVIewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

### <a name="requires-new-static-registrar"></a>Требуется новый статический регистратора

Поддержка универсальных типов требуется новый [системой регистрации](~/ios/internals/registrar.md).

При попытке использовать старый системой устаревших регистрации будет показывать предупреждения при обнаружении универсальные типы (в дополнение к не создавать правильный код, что приводит к неопределенному поведению).
    
## <a name="performance"></a>Производительность

Статические регистратора не удается разрешить экспортированного элемента, в универсальном типе во время сборки, как обычно, он должен выполняться поиск во время выполнения. Это означает, что вызов метода из Objective-C немного медленнее, чем вызов членов из классов, не являющимися универсальными.

