---
title: Рекомендации по ObjC Embeddinator 4000
ms.prod: xamarin
ms.assetid: 63C7F5D2-8933-4D4A-8348-E9CBDA45C472
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: ca5face9865c60fabe8359c2bf356d5d5555f517
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="embeddinator-4000-best-practices-for-objc"></a>Рекомендации по ObjC Embeddinator 4000

Это черновик, возможно, не синхронизован с функциями поддерживается в настоящее время с помощью средства. Мы надеемся, что в этом документе будет развиваться отдельно и в конечном итоге совпадают окончательного средство, т. е. будет предлагать рекомендации долгосрочные - не немедленного решения.

Большая часть этого документа справедливо и для других языков. Однако все поддерживаемые приведены в C# и цель-C.

## <a name="exposing-a-subset-of-the-managed-code"></a>Предоставление доступа к подмножеству управляемого кода

Созданный собственная библиотека или платформа содержит код Objective-C для каждой из управляемых интерфейсов API, предоставляемый вызова. Дополнительные API, можно выявить (сделать открытым) затем большего собственный _связующего_ станет библиотеки.

Возможно, следует создать другой, более мелкие сборку, чтобы предоставлять только необходимые API для собственного разработчика. Что фасадной также позволит вам больший контроль над видимость именования, проверка наличия ошибок... сформированного кода.

## <a name="exposing-a-chunkier-api"></a>Предоставление доступа к chunkier API

Нет цены для оплаты для перехода из машинного, управляемого (и назад). Таким образом, рекомендуется предоставлять _фрагментированных вместо «многословных»_ API-интерфейсы собственного разработчикам, например

**«Многословных»**

```csharp
public class Person {
  public string FirstName { get; set; }
  public string LastName { get; set; }
}
```

```objc
// this requires 3 calls / transitions to initialize the instance
Person *p = [[Person alloc] init];
p.firstName = @"Sebastien";
p.lastName = @"Pouliot";
```

**Фрагментированных**

```csharp
public class Person {
  public Person (string firstName, string lastName) {}
}
```

```objc
// a single call / transition will perform better
Person *p = [[Person alloc] initWithFirstName:@"Sebastien" lastName:@"Pouliot"];
```

Поскольку меньше количество переходов, производительность будет выше. Меньший объем кода, создаваемого, также необходимо, в результате получится также меньшего размера собственной библиотеки.

## <a name="naming"></a>Именование

Именования объектов является одним из двух сложных проблем в вычислительной технике других, кэш ошибки недействительности и off на 1. Надеемся, у внедрения .NET можно избежать, если вы из всех, за исключением имен.

### <a name="types"></a>Типы

Objective-C не поддерживает пространства имен. Как правило, ее типы с префиксом 2 (для Apple) или 3 (для сторонних производителей) символов префикса, таких как `UIView` UIKit его вид, обозначающий платформу.

Для типов .NET пропуск пространство имен не поддерживается как его использование может стать повторяется или путаницу, имена. В результате существующие типы .NET очень много, например

```csharp
namespace Xamarin.Xml.Configuration {
  public class Reader {}
}
```

будет использоваться как:

```objc
id reader = [[Xamarin_Xml_Configuration_Reader alloc] init];
```

Тем не менее можно повторно предоставить тип как:

```csharp
public class XAMXmlConfigReader : Xamarin.Xml.Configuration.Reader {}
```

делая его более понятного имени Objective-C для использования, например:

```objc
id reader = [[XAMXmlConfigReader alloc] init];
```

### <a name="methods"></a>Методы

Даже хорошо имена .NET может быть идеальным решением для Objective-C API.

Соглашения об именовании в Objective-C, отличаются от .NET (верблюжий вместо Pascal, в более подробных сведений).
Ознакомьтесь с [правила для Cocoa кодирования](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF).

Из Objective-C точки зрения разработчика, метод с `Get` префикс подразумевает вам не принадлежат экземпляра, т. е. [получить правило](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-SW1).

Это правило именования не имеет соответствия в мире сборщик Мусора .NET; метод .NET с `Create` префикс будет ведут себя одинаково в .NET. Однако для разработчиков, Objective-C, как правило, означают владельцем возвращаемого экземпляра, т. е. [создать правило](https://developer.apple.com/library/content/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html#//apple_ref/doc/uid/20001148-103029).

## <a name="exceptions"></a>Исключения

Это довольно commont в .NET, чтобы использовать исключения широко отправку отчетов об ошибках. Тем не менее они медленным и довольно идентичными в ObjC. По возможности их следует скрыть от разработчика Objective-C.

Например, .NET `Try` модель будет гораздо проще использовать из кода Objective-C:

```csharp
public int Parse (string number)
{
  return Int32.Parse (number);
}
```

Сравнение

```csharp
public bool TryParse (string number, out int value)
{
  return Int32.TryParse (number, out value);
}
```

### <a name="exceptions-inside-init"></a>Исключения в `init*`

В .NET конструктор должен либо завершиться успешно и вернуть (_надеюсь_) допустимый экземпляр или создает исключение.

Напротив, позволяет Objective-C `init*` для возврата `nil` при экземпляр не может быть создан. Такое поведение встречается распространенных, но не общие используется во многих платформ компании Apple. В некоторых других случаях `assert` может произойти (и завершить текущий процесс).

Генератор выполняют ту же `return nil` шаблона для сформированные `init*` методы. Если управляемое исключение возникает, то он будет напечатан (с помощью `NSLog`) и `nil` возвращается вызывающему объекту.

## <a name="operators"></a>Операторы

Objective-C не допускает операторы можно перегружать как C#, поэтому они преобразуются в значения выбора классов.

[«Понятное»](/dotnet/standard/design-guidelines/operator-overloads/) именованный метод создаются предпочтительнее, чем перегрузки операторов при найдено и может привести к более поздних API.

Классы, которые переопределяют операторы `==` и (или) `!=` должны переопределять также стандартный метод Equals (объект).
