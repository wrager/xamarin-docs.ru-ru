---
title: "Поддержка Objective c."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 047f7d7497a114bf4b7c94e50bdf09862b882794
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/17/2018
---
# <a name="objective-c-support"></a>Поддержка Objective c.

## <a name="specific-features"></a>Отдельные функции

Создание ObjC имеет несколько специальные функции, которые стоит обратить внимание.

### <a name="automatic-reference-counting"></a>Подсчет автоматических ссылок

Использование из автоматического подсчета ссылок (Окружности) — это **необходимые** для вызова созданных привязок. Проект, использующий библиотеку на основе embeddinator должна быть скомпилирована с `-fobjc-arc`.

### <a name="nsstring-support"></a>Поддержка NSString

Интерфейсы API, которые предоставляют `System.String` типы преобразуются в `NSString`. Это упрощает управление памятью задействования `char*`.

### <a name="protocols-support"></a>Поддержка протоколов

Управляемые интерфейсы, преобразуются в ObjC протоколы, где все члены являются `@required`.

### <a name="nsobject-protocol-support"></a>Поддержка протокола NSObject

По умолчанию предполагается хэширования по умолчанию и проверки на равенство .net и среды выполнения ObjC: нормально и взаимозаменяемыми, как они совместно используют очень похожую семантику.

Когда управляемый тип переопределяет `Equals(Object)` или `GetHashCode` это обычно означает поведения по умолчанию (.NET) не лучший. Можно предположить, что Objective-C поведения по умолчанию не либо.

В этом случае переопределяет генератор [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) метод и [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) свойство, определенное в [ `NSObject` протокола](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Это позволяет пользовательская реализация управляемого для использования из кода ObjC прозрачно.

### <a name="comparison"></a>Оператор

Управляемые типы, реализующие `IComparable` или это универсальная версия `IComparable<T>` сформирует ObjC понятное методы, которые возвращает `NSComparisonResult` и принять `nil` аргумент. Это делает сгенерированном API более удобен для разработчиков, ObjC, например

```csharp
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Категории

Управляемые расширения методов преобразуются в категории. Например следующие методы расширения в `Collection`:

```csharp
    public static class SomeExtensions {

        public static int CountNonNull (this Collection collection) { ... }

        public static int CountNull (this Collection collection) { ... }
    }
```

создать категорию Objective-C такого рода.

```csharp
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;
@end
```

При управлении один тип расширяет несколько типов, то создаются несколько категорий Objective-C.

### <a name="subscripting"></a>Индексация ;

Управляемые индексированного свойства преобразуются в объект индексация. Пример:

```csharp
    public bool this[int index] {
        get { return c[index]; }
        set { c[index] = value; }
    }
```

создать Objective-C аналогично:

```csharp
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

который можно использовать синтаксис подписи пропущена Objective-C:

```csharp
    if ([intCollection [0] isEqual:@42])
        intCollection[0] = @13;
```

В зависимости от типа индексатора индексация индексированного или с ключом будут создаваться при необходимости.

Это [статьи](http://nshipster.com/object-subscripting/) представляет собой отличный введение индексация.

## <a name="main-differences-with-net"></a>Основные различия в .NET Framework

### <a name="constructors-vs-initializers"></a>Конструкторы vs инициализаторы

В Objective-C можно вызвать любой из инициализатора прототипы какой-либо из родительских классов в цепочке наследования, если он отмечен как недоступен (NS_UNAVAILABLE).

В C# необходимо явно объявить элемент конструктора внутри класса, это означает, что конструкторы не наследуются.

В правом представления API для Objective-C, мы добавляем `NS_UNAVAILABLE` для любого инициализатор, не присутствующему в дочернем классе, от родительского класса.

API C#:

```csharp
public class Unique {
    public Unique () : this (1)
    {
    }

    public Unique (int id)
    {
    }
}

public class SuperUnique : Unique {
    public SuperUnique () : base (911)
    {
    }
}
```

Objective-C отображена API:

```objectivec
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Здесь мы видим, `initWithId:` был помечен как недоступный.

### <a name="operator"></a>Оператор

ObjC не поддерживает оператор перегрузка как C#, поэтому операторы преобразуются в значения выбора классов:

```csharp
    public static AllOperators operator + (AllOperators c1, AllOperators c2)
    {
        return new AllOperators (c1.Value + c2.Value);
    }
```

в

```csharp
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Однако некоторые языки .NET не поддерживают перегрузку операторов, поэтому обычно включают [«понятное»](https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx) с именем метода, кроме перегрузка оператора.

Если оператор версии и версии «понятное» найдены, будет создаваться только понятную версию, как они получат одно и то же имя цели c.

```csharp
    public static AllOperatorsWithFriendly operator + (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }

    public static AllOperatorsWithFriendly Add (AllOperatorsWithFriendly c1, AllOperatorsWithFriendly c2)
    {
        return new AllOperatorsWithFriendly (c1.Value + c2.Value);
    }
```

выглядит следующим образом:

```csharp
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Оператор равенства

В общие оператор == в C# обрабатывается как общие оператор как указано выше.

Тем не менее если оператор равенства «понятное» будет найдена, оба оператора == и оператор! = будет пропущен в поколении.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

Из [NSDate](https://developer.apple.com/reference/foundation/nsdate?language=objc) документации:

> Объекты NSDate инкапсулируют точки времени, независимым от конкретного calendrical системы или часового пояса. Дата объекты являются неизменяемыми, представляющее интервал времени инвариантный относительно дату абсолютная ссылка (00: 00:00 UTC 1 января 2001 года).

Из-за `NSDate` ссылаться даты всех преобразований между его и `DateTime` должно быть выполнено в формате UTC.

#### <a name="datetime-to-nsdate"></a>DateTime для NSDate

При преобразовании из `DateTime` для `NSDate` DateTime `Kind` учитывается свойство.

|Тип|Результаты                                                                                            |
|---|---|
|UTC|Преобразование выполняется с помощью указанных `DateTime` как.|
|Локальная|Результат вызова `ToUniversalTime()` в предоставленном `DateTime` объект используется для преобразования.|
|Не указан|Предоставленный `DateTime` предполагается, что объект является UTC, поэтому такое же поведение, как тип == Utc.|

Преобразование осуществляется с помощью следующей формулы:

> [!NOTE]
> **TimeInterval** = DateTimeObjectTicks - NSDateReferenceDateTicks[dt] / [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx)

После получения TimeInterval мы используем его NSDate [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) выделения, чтобы создать его.

#### <a name="nsdate-to-datetime"></a>NSDate к типу DateTime

Переход от NSDate к мы предполагаем, что мы получаем NSDate DateTime — экземпляр, который является Дата ссылки **00:00:00 UTC 1 января 2001 года** и использовать следующую формулу:

> [!NOTE]
> **DateTimeTicks** = NSDateTimeIntervalSinceReferenceDate * [TicksPerSecond](https://msdn.microsoft.com/en-us/library/system.timespan.tickspersecond(v=vs.110).aspx) + NSDateReferenceDateTicks[dt]

После вычисления мы **DateTimeTicks** мы используем следующие DateTime [конструктор](https://msdn.microsoft.com/en-us/library/w0d47c9c(v=vs.110).aspx) параметр его `kind` для `DateTimeKind.Utc`.

Существуют определенные рекомендации, которые необходимо обратить внимание, может быть NSDate `nil` , но DateTime структуры в .NET и по определению не может быть `null`. Если вы предоставите `nil` NSDate мы преобразует его в значении даты и времени по умолчанию, используемый для сопоставления `DateTime.MinValue`.

MinValue и MaxValue, также различаются, NSDate может поддерживать больше и нижнюю границы, чем даты и времени поэтому каждый раз, когда вы предоставляете значение выше или ниже мы будет задать значение даты и времени [MaxValue](https://msdn.microsoft.com/en-us/library/system.datetime.maxvalue(v=vs.110).aspx) или [MinValue](https://msdn.microsoft.com/en-us/library/system.datetime.minvalue(v=vs.110).aspx) соответственно.

**DT**: Дата ссылки NSDate **00:00:00 UTC 1 января 2001 года.** => `new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;`
