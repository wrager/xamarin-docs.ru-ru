---
title: Поддержка Objective c.
ms.prod: xamarin
ms.assetid: 3367A4A4-EC88-4B75-96D0-51B1FCBCE614
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 66ba31b1054559516fdbfbeb0421a82f4a0e9fa5
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="objective-c-support"></a>Поддержка Objective c.

## <a name="specific-features"></a>Отдельные функции

Создание Objective-C имеет некоторые специальные возможности, которые стоит обратить внимание.

### <a name="automatic-reference-counting"></a>Подсчет автоматических ссылок

Использование из автоматического подсчета ссылок (Окружности) — это **необходимые** для вызова созданных привязок. Проект, использующий библиотеку на основе embeddinator должна быть скомпилирована с `-fobjc-arc`.

### <a name="nsstring-support"></a>Поддержка NSString

Интерфейсы API, которые предоставляют `System.String` типы преобразуются в `NSString`. Это упрощает управление памятью при работе с `char*`.

### <a name="protocols-support"></a>Поддержка протоколов

Управляемые интерфейсы, преобразуются в протоколы Objective-C, где все члены являются `@required`.

### <a name="nsobject-protocol-support"></a>Поддержка протокола NSObject

По умолчанию по умолчанию хэширования и равенства .NET и среды выполнения C цель считаются взаимозаменяемыми, как они имеют похожую семантику.

Когда управляемый тип переопределяет `Equals(Object)` или `GetHashCode`, обычно это значит, что поведение по умолчанию (.NET) недостаточно; это означает, что поведение по умолчанию Objective-C, вероятнее всего недостаточно либо.

В таких случаях переопределяет генератор [ `isEqual:` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418795-isequal?language=objc) метод и [ `hash` ](https://developer.apple.com/reference/objectivec/1418956-nsobject/1418859-hash?language=objc) свойство, определенное в [ `NSObject` протокола](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc). Это позволяет пользовательская реализация управляемого для использования из кода Objective-C прозрачно.

### <a name="exceptions-support"></a>Поддержка исключений

Передача `--nativeexception` как аргумент `objcgen` преобразует управляемые исключения в Objective-C исключения, которые может быть перехвачено и обработано. 

### <a name="comparison"></a>Оператор

Управляемые типы, реализующие `IComparable` (или его универсальная версия `IComparable<T>`) создает Objective-C понятное методы, которые возвращают `NSComparisonResult` и принять `nil` аргумент. Это делает более удобен для разработчиков Objective-C сгенерированном API. Пример:

```objc
- (NSComparisonResult)compare:(XAMComparableType * _Nullable)other;
```

### <a name="categories"></a>Категории

Управляемые расширения методов преобразуются в категории. Например, следующие методы расширения в `Collection`:

```csharp
public static class SomeExtensions {
    public static int CountNonNull (this Collection collection) { ... }
    public static int CountNull (this Collection collection) { ... }
}
```

создать категорию Objective-C такого рода.

```objc
@interface Collection (SomeExtensions)

- (int)countNonNull;
- (int)countNull;

@end
```

Если один управляемый тип расширяет некоторые типы, создаются несколько категорий Objective-C.

### <a name="subscripting"></a>Индексация ;

Управляемые индексированного свойства преобразуются в объект индексация. Пример:

```csharp
public bool this[int index] {
    get { return c[index]; }
    set { c[index] = value; }
}
```

создать Objective-C аналогично:

```objc
- (id)objectAtIndexedSubscript:(int)idx;
- (void)setObject:(id)obj atIndexedSubscript:(int)idx;
```

который можно использовать синтаксис подписи пропущена Objective-C:

```objc
if ([intCollection [0] isEqual:@42])
    intCollection[0] = @13;
```

В зависимости от типа индексатора индексация индексированного или с ключом будут создаваться при необходимости.

Это [статьи](http://nshipster.com/object-subscripting/) представляет собой отличный введение индексация.

## <a name="main-differences-with-net"></a>Основные различия в .NET Framework

### <a name="constructors-vs-initializers"></a>Инициализаторы конструкторов vs

В Objective-C, можно вызывать любые инициализатора прототипы какой-либо из родительских классов в цепочке наследования, если он помечен как недоступный (`NS_UNAVAILABLE`).

В C# необходимо явно объявить конструктор члена внутри класса, это означает, что конструкторы не наследуются.

В правом представления API для Objective-C `NS_UNAVAILABLE` добавляется любой инициализатор, не присутствующему в дочернем классе, от родительского класса.

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

```objc
@interface SuperUnique : Unique

- (instancetype)initWithId:(int)id NS_UNAVAILABLE;
- (instancetype)init;

@end
```

Здесь `initWithId:` был помечен как недоступный.

### <a name="operator"></a>Оператор

Objective-C не поддерживает оператор перегрузка как C#, поэтому операторы преобразуются в значения выбора классов:

```csharp
public static AllOperators operator + (AllOperators c1, AllOperators c2)
{
    return new AllOperators (c1.Value + c2.Value);
}
```

в

```objc
+ (instancetype)add:(Overloads_AllOperators *)anObjectC1 c2:(Overloads_AllOperators *)anObjectC2;
```

Однако некоторые языки .NET не поддерживают перегрузку операторов, поэтому обычно включают [«понятное»](https://docs.microsoft.com/dotnet/standard/design-guidelines/operator-overloads) с именем метода, кроме перегрузка оператора.

Если оператор версии и версии «понятное» найдены, будет создаваться только понятную версию, как они получат одно и то же имя Objective-C.

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

```objc
+ (instancetype)add:(Overloads_AllOperatorsWithFriendly *)anObjectC1 c2:(Overloads_AllOperatorsWithFriendly *)anObjectC2;
```

### <a name="equality-operator"></a>Оператор равенства

В операторе Общие `==` в C# обрабатывается как общая оператора, как указано выше.

Тем не менее если оператор равенства «понятное» будет найдена, и оператором `==` и оператор `!=` будет пропущен в поколении.

### <a name="datetime-vs-nsdate"></a>DateTime vs NSDate

Из [ `NSDate` ](https://developer.apple.com/reference/foundation/nsdate?language=objc) документации:

> `NSDate` объекты инкапсулируют точки времени, независимым от конкретного calendrical системы или часового пояса. Дата объекты являются неизменяемыми, представляющее интервал времени инвариантный относительно дату абсолютная ссылка (00: 00:00 UTC 1 января 2001 года).

Из-за `NSDate` ссылаться даты всех преобразований между его и `DateTime` должно быть выполнено в формате UTC.

#### <a name="datetime-to-nsdate"></a>DateTime для NSDate

При преобразовании из `DateTime` для `NSDate`, `Kind` свойство `DateTime` принимать во внимание:

|Тип|Результаты|
|---|---|
|`Utc`|Преобразование выполняется с помощью указанных `DateTime` как.|
|`Local`|Результат вызова `ToUniversalTime()` в предоставленном `DateTime` объект используется для преобразования.|
|`Unspecified`|Предоставленный `DateTime` предполагается, что объект является UTC, поэтому такое же поведение при `Kind` — `Utc`.|

Преобразование использует следующую формулу:

```
TimeInterval = DateTimeObjectTicks - NSDateReferenceDateTicks / TicksPerSecond
```

В следующей формуле: 

- `NSDateReferenceDateTicks` вычисляется на основе `NSDate` ссылаться Дата 00:00:00 UTC 1 января 2001 года: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) определенные в [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

Для создания `NSDate` объекта, `TimeInterval` используется с `NSDate` [dateWithTimeIntervalSinceReferenceDate:](https://developer.apple.com/reference/foundation/nsdate/1591577-datewithtimeintervalsincereferen?language=objc) селектора.

#### <a name="nsdate-to-datetime"></a>NSDate к типу DateTime

Преобразование из `NSDate` для `DateTime` использует следующую формулу:

```
DateTimeTicks = NSDateTimeIntervalSinceReferenceDate * TicksPerSecond + NSDateReferenceDateTicks
```

В следующей формуле: 

- `NSDateReferenceDateTicks` вычисляется на основе `NSDate` ссылаться Дата 00:00:00 UTC 1 января 2001 года: 
    ```csharp
    new DateTime (year:2001, month:1, day:1, hour:0, minute:0, second:0, kind:DateTimeKind.Utc).Ticks;
    ```
- [`TicksPerSecond`](https://docs.microsoft.com/dotnet/api/system.timespan.tickspersecond) определенные в [`TimeSpan`](https://docs.microsoft.com/dotnet/api/system.timespan)

После расчета `DateTimeTicks`, `DateTime` [конструктор](https://docs.microsoft.com/dotnet/api/system.datetime.-ctor?#System_DateTime__ctor_System_Int64_System_DateTimeKind_) вызывается, установка его `kind` для `DateTimeKind.Utc`.

> [!NOTE]
> `NSDate` может быть `nil`, но `DateTime` является структурой в .NET, которая по определению не может быть `null`. Если вы предоставите `nil` `NSDate`, он будет преобразован в значение по умолчанию `DateTime` значение, которое сопоставляется `DateTime.MinValue`.

`NSDate` поддерживает до более поздней версии и меньше минимального значения `DateTime`. При преобразовании из `NSDate` для `DateTime`, эти значения выше и ниже заменяются `DateTime` [MaxValue](https://docs.microsoft.com/dotnet/api/system.datetime.maxvalue) или [MinValue](https://docs.microsoft.com/dotnet/api/system.datetime.minvalue)соответственно.
