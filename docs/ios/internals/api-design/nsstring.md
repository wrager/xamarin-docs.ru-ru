---
title: NSString в Xamarin.iOS и Xamarin.Mac
description: В этом документе описывается Xamarin.iOS прозрачно преобразование NSString объекты в C# строковых объектов, если этого не происходит.
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: baf36700ab4d608296a9a67e234ce613da9ca077
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786094"
---
# <a name="nsstring-in-xamarinios-and-xamarinmac"></a>NSString в Xamarin.iOS и Xamarin.Mac

Вызывает конструктор Xamarin.iOS и Xamarin.Mac для используйте API для предоставления собственного строкового типа .NET `string`, для обработки в C# и других языков программирования .NET, а также для предоставления строки в качестве типа данных, предоставляемые API вместо `NSString` тип данных.

Это означает, что разработчики не следует хранить строки, которые предназначены для вызова Xamarin.iOS & Xamarin.Mac API (единый) в специальный тип (`Foundation.NSString`), можно продолжать использовать его моно `System.String` для всех операций и каждый раз, когда интерфейс API в Xamarin.iOS или Xamarin.Mac требуется строка, нашей привязкой API берет на себя сведения о маршалинге.

Например, свойство «text» Objective-C на `UILabel` типа `NSString`, объявляется следующим образом:

```objc
@property(nonatomic, copy) NSString *text
```

Это представляется в Xamarin.iOS как:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

В фоновом реализация этого свойства маршалирует строки в `NSString` и вызывает `objc_msgSend` метод таким же образом, который бы Objective-C.

Существует множество API Objective-C сторонних разработчиков, которые не используют `NSString`, но вместо этого использовать строка C (»*char*»). В таких случаях можно использовать тип данных string C#, но необходимо использовать [[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) атрибут для информирования генератор привязки, эта строка не должна быть маршалирован как `NSString`, но вместо этого, как строка C.

 <a name="Exceptions_to_the_Rule" />

## <a name="exceptions-to-the-rule"></a>Исключения для правила

В Xamarin.iOS и Xamarin.Mac мы внесли исключение для этого правила. Решение между когда мы предоставляем `string`s, и если мы предоставим, за исключением и предоставления `NSString`s, выполняются, если `NSString` метод может выполнять сравнение указателей вместо содержимого сравнения.

Это может произойти, когда открытый использует API-интерфейсы Objective-C `NSString` константы как токен, представляющий какое-либо действие, вместо сравнения фактическое содержимое строки.

В таких случаях `NSString` доступные интерфейсы API, и есть малого числа API, которые имеют это. Вы также заметите, что NSString свойства отображаются в некоторых классах. Те `NSString` свойства предоставляются для элементов, как уведомления. Те, свойства обычно выглядеть следующим образом:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```
Уведомления — это ключи, используемые для `NSNotification` класс при необходимости регистрации для конкретного события широковещательные средой выполнения.

Ключи обычно выглядеть примерно следующим образом:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Другое место где `NSString`s, предоставленные API — как маркеры, которые используются в качестве параметров для некоторых интерфейсов API в iOS или OS X, которые принимают `NSDictionary` объектов как параметры. Обычно содержится в словаре `NSString` ключей. Xamarin.iOS, по соглашению имена тех статических `NSString` свойства, добавив имя «Ключ».
