---
title: "Обзор"
description: "Сведения о принципах работы процесса привязки"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE288C5-8952-C5A9-E542-0BD847300EC6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: eb6d49433974a5e4e7bda69651508d5e9006a78e
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2018
---
# <a name="overview"></a>Обзор

_Сведения о принципах работы процесса привязки_

Привязка библиотеку Objective-C для использования с Xamarin принимает три этапа:

1. Написать на языке C# «Определения API» для описания как собственный API представляется в .NET и как они соответствуют основной целью-C. Это делается с помощью стандартных C# создает как `interface` и различные привязки **атрибуты** (см. в этом [простой пример](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_an_API)).

2. После написания «определения API» в C# компиляции может создавать сборку «привязка». Это можно сделать на [ **командной строки** ](#commandline) или с помощью [ **привязки проекта** ](#bindingproject) в Visual Studio для Mac или Visual Studio.

3. Эту сборку «привязка» добавляется в проект приложения Xamarin, чтобы вы могли использовать собственные функции с помощью API, определенный.
  В проектах приложений отделена привязки проекта.

**Примечание:** шаг 1 можно автоматизировать с помощью [ **Sharpie цель**](#objectivesharpie). Выполняет проверку API Objective-C и приводит к возникновению ошибки предложенный C# «Определения API». Можно настраивать файлы, созданные Sharpie цель и использовать их в проекте привязки (или в командной строке) для создания привязки сборки. Цели Sharpie не создает привязок сам по себе, он просто необязательную часть большего процесса.

Также можно прочитать дополнительные технические сведения о [принцип работы](#howitworks), который поможет выполнить запись привязок.

<a name="Command_Line_Bindings" /><a name="commandline" />

## <a name="command-line-bindings"></a>Привязки командной строки

Можно использовать `btouch-native` для Xamarin.iOS (или `bmac-native` при использовании Xamarin.Mac) для создания привязки напрямую. Он работает путем передачи определения API C#, созданных вручную (или с помощью Sharpie цель) в средство командной строки (`btouch-native` для операций ввода-вывода или `bmac-native` для Mac).


Ниже приведен общий синтаксис для вызова этих средств.

```csharp
# Use this for Xamarin.iOS:
bash$ /Developer/MonoTouch/usr/bin/btouch-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

```csharp
# Use this for Xamarin.Mac:
bash$ bmac-native -e cocos2d.cs -s:enums.cs -x:extensions.cs
```

Приведенная выше команда создаст файл `cocos2d.dll` в текущем каталоге, и он будет содержать полной привязкой библиотеки, который можно использовать в проекте. Это средство, которое использует Visual Studio для Mac для создания привязки, при использовании project привязки (описано [ниже](#bindingproject)).


<a name="bindingproject" />

## <a name="binding-project"></a>Привязка проекта

Проект привязки можно создать в Visual Studio для Mac или Visual Studio (Visual Studio поддерживает только привязки iOS) и упрощает для редактирования и сборки API определения привязки (а не с помощью командной строки).

После этого использовать [руководство по началу работы](~/cross-platform/macios/binding/objective-c-libraries.md#Getting_Started) чтобы узнать, как создавать и использовать привязки проекта для создания привязки.

<a name="objectivesharpie" />

## <a name="objective-sharpie"></a>Цели Sharpie

Цели Sharpie — еще один, отдельный командной строки, облегчающим начальных этапах Создание привязки. Сам по себе не создается привязка, а его автоматизирует первого шага создания определение API для собственной библиотеки целевой.

Чтение [Sharpie цель docs](~/cross-platform/macios/binding/objective-sharpie/index.md) чтобы узнать, как преобразовать собственные библиотеки, собственный платформ и CocoaPods в определений API, который может быть встроен в привязках.

<a name="howitworks" />

## <a name="how-binding-works"></a>Как работает привязка

Можно использовать [[регистрация]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) атрибут, [[экспортировать]](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) атрибут, и [вручную вызов селектор Objective-C](~/ios/internals/objective-c-selectors.md) друг с другом, чтобы вручную выполнить привязку new (ранее несвязанные) Objective-C типы.

Во-первых найти тип, для привязки. Обсуждения в целях (и простоты), мы привязать [NSEnumerator](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSEnumerator_Class/Reference/Reference.html) типа (который уже привязан в [Foundation.NSEnumerator](https://developer.xamarin.com/api/type/Foundation.NSEnumerator/); реализация ниже, например просто целей).

Во-вторых необходимо создать тип C#. Мы скорее всего, решите добавить его в пространстве имен. так как цель C не поддерживает пространства имен, нам нужно будет использовать `[Register]` атрибут, чтобы изменить имя типа, Xamarin.iOS будет зарегистрировать в среде выполнения Objective-C. Тип C# также должен наследоваться от [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/):

```csharp
namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        // see steps 3-5
    }
}
```

В-третьих, ознакомьтесь с документацией в Objective-C и создать [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) экземпляров для каждого выделения, вы хотите использовать. Разместите их в теле класса:

```csharp
static Selector selInit       = new Selector("init");
static Selector selAllObjects = new Selector("allObjects");
static Selector selNextObject = new Selector("nextObject");
```

В-четвертых тип, необходимо предоставить конструкторы. Вы *должен* цепочку ваш конструктор вызов конструктора базового класса. `[Export]` Атрибуты разрешать вызывать конструкторы с именем заданного селектора кода Objective-C:

```csharp
[Export("init")]
public NSEnumerator()
    : base(NSObjectFlag.Empty)
{
    Handle = Messaging.IntPtr_objc_msgSend(this.Handle, selInit.Handle);
}
```

```csharp
// This constructor must be present so that Xamarin.iOS
// can create instances of your type from Objective-C code.
public NSEnumerator(IntPtr handle)
    : base(handle)
{
}
```

В-пятых предоставляют методы для каждого селекторы, объявленной в шаге 3. Они будут использовать `objc_msgSend()` для вызова селектора на собственный объект. Обратите внимание на использование [Runtime.GetNSObject()](https://developer.xamarin.com/api/member/ObjCRuntime.Runtime.GetNSObject/(System.IntPtr)) для преобразования `IntPtr` в соответствующим образом типизированного `NSObject` (sub) типа. Если этот метод следует вызвать из кода Objective-C, член должен *должен* быть **виртуальный**.

```csharp
[Export("nextObject")]
public virtual NSObject NextObject()
{
    return Runtime.GetNSObject(
        Messaging.IntPtr_objc_msgSend(this.Handle, selNextObject.Handle));
}
```

```csharp
// Note that for properties, [Export] goes on the get/set method:
public virtual NSArray AllObjects {
    [Export("allObjects")]
    get {
        return (NSArray) Runtime.GetNSObject(
            Messaging.IntPtr_objc_msgSend(this.Handle, selAllObjects.Handle));
    }
}
```

Подведение итогов:

```csharp
using System;
using Foundation;
using ObjCRuntime;

namespace Example.Binding {
    [Register("NSEnumerator")]
    class NSEnumerator : NSObject
    {
        static Selector selInit       = new Selector("init");
        static Selector selAllObjects = new Selector("allObjects");
        static Selector selNextObject = new Selector("nextObject");

        [Export("init")]
        public NSEnumerator()
            : base(NSObjectFlag.Empty)
        {
            Handle = Messaging.IntPtr_objc_msgSend(this.Handle,
                selInit.Handle);
        }

        public NSEnumerator(IntPtr handle)
            : base(handle)
        {
        }

        [Export("nextObject")]
        public virtual NSObject NextObject()
        {
            return Runtime.GetNSObject(
                Messaging.IntPtr_objc_msgSend(this.Handle,
                    selNextObject.Handle));
        }

        // Note that for properties, [Export] goes on the get/set method:
        public virtual NSArray AllObjects {
            [Export("allObjects")]
            get {
                return (NSArray) Runtime.GetNSObject(
                    Messaging.IntPtr_objc_msgSend(this.Handle,
                        selAllObjects.Handle));
            }
        }
    }
}
```

