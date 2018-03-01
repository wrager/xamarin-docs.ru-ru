---
title: "Ограничения"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 43b099e8ddd6acc3e8cc4ce94580313a39a0c686
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="limitations"></a>Ограничения

Поскольку приложения на iPhone, с помощью Xamarin.iOS компилируются для статического кода, не позволяет использовать все функции, для которых требуется создание кода во время выполнения.

Существуют следующие ограничения Xamarin.iOS по сравнению с рабочего стола моно:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Поддержка ограниченный универсальных типов

В отличие от традиционных моно и .NET код на iPhone статически компилируется заранее вместо компиляции по требованию с помощью JIT-компилятора.

Моно [полный AOT](http://www.mono-project.com/AOT#Full_AOT) технологии имеет несколько ограничений, применительно к универсальным типам, они связаны, так как не все возможные универсального экземпляра может заранее определить во время компиляции. Это не проблему для регулярных .NET или Mono среды выполнения, поскольку во время выполнения с помощью непосредственно в компиляторе время всегда компиляции кода. Однако это создает проблему для статических компилятора как Xamarin.iOS.

Ниже перечислены некоторые общие проблемы, которые сталкиваются разработчики.

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Универсальный подклассов NSObjects ограничены.

Xamarin.iOS в настоящее время имеет ограниченную поддержку для создания универсального подклассы класса NSObject, таких как поддержка универсальных методов. Начиная с 7.2.1 с помощью универсального подклассов NSObjects невозможна, похожее на следующее:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Универсальный подклассов NSObjects возможны, существуют некоторые ограничения. Чтение [универсального подклассов NSObject](~/ios/internals/api-design/nsobject-generics.md) Дополнительные сведения



### <a name="pinvokes-in-generic-types"></a>P/Invokes в универсальных типах

Методы P/Invoke в универсальных классах не поддерживаются:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Property.SetInfo для типа Nullable не поддерживается.

С помощью отражения Property.SetInfo значение Nullable&lt;T&gt; в настоящее время не поддерживается.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Типы значений, что ключи словаря

Использовании типа значения в виде словаря&lt;TKey, TValue&gt; проблемный ключ, по умолчанию словарь конструктор пытается использовать EqualityComparer&lt;TKey&gt;. По умолчанию. EqualityComparer&lt;TKey&gt;. По умолчанию, в свою очередь, пытается использовать отражение для создания нового типа, который реализует компаратор IEqualityComparer&lt;TKey&gt; интерфейса.

Это работает для ссылочных типов (как отражение + создать новый тип шаг пропускается), но значение его тип сбоев и затемнению довольно быстро, как только вы пытаетесь использовать его на устройстве.

 **Инструкции по решению**: реализация вручную [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) интерфейс в новый тип, а также предоставить экземпляр этого типа для [словарь&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) конструктор.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Без создания динамического кода

Так как iPhone ядра не позволяет приложению динамически формировать код Mono на iPhone не поддерживает динамическое создание кода в любой форме. Сюда входит следующее.

-  System.Reflection.Emit недоступен.
-  Отсутствует поддержка System.Runtime.Remoting.
-  Отсутствует поддержка динамического создания типов (не Type.GetType («MyType "1»)), несмотря на то, что поиск существующих типов (Type.GetType («System.String»), отлично работает). 
-  Обратная обратные вызовы должны быть зарегистрированы с помощью среды выполнения во время компиляции.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

Отсутствие System.Reflection. **Порождение** означает, что будет работать без кода, зависящего от создания кода среды выполнения. Сюда входят, например:

-  Среда выполнения динамического языка.
-  Все языки, построена на базе среды DLR.
-  TransparentProxy удаленного взаимодействия либо все, что приведет к среде выполнения динамически создавать код. 


 **Важно:** не следует путать **Reflection.Emit** с **отражения**. Reflection.Emit — о создании кода динамически у этого кода JIT-компилируются и компилируются в машинный код. Из-за ограничений, накладываемых на iPhone (не JIT-компиляции) не поддерживается.

Но весь API отражения, включая Type.GetType («someClass»), список методов, список свойств, выборка атрибуты и значения работает нормально.

 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Обратная обратных вызовов

В стандартных моно можно передать экземпляры делегата C# в неуправляемый код за счет указатель на функцию. Среда выполнения обычно будет преобразовывать эти указатели на функции в небольших преобразователя, который позволяет неуправляемым кодом обратный вызов управляемого кода.

В моно этих мостах реализуются только время компилятора. Если с помощью компилятора упреждающего времени, необходимый для iPhone имеется два важных ограничения на этом этапе:

-  Необходимо пометить все методы обратного вызова с [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/MonoPInvokeCallbackAttribute/) 
-  Методы должны быть статическими методами, не поддерживается для экземпляра метода. 


 
 <a name="No_Remoting" />


## <a name="no-remoting"></a>Нет удаленного взаимодействия

Стек удаленного взаимодействия не доступен на Xamarin.iOS.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Среда выполнения отключены функции

Следующие функции были отключены на iOS Mono среды выполнения:

-  профилировщик
-  Reflection.Emit
-  Функциональные возможности Reflection.Emit.Save
-  Привязки модели COM
-  Подсистема JIT-компилятора
-  Средство проверки метаданных (поскольку нет JIT)


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>Ограничения API .NET

API .NET, представляемый является подмножеством полной инфраструктуры как не все элементы, доступные в iOS. В разделе часто задаваемые вопросы по [список сборок, поддерживаемых в настоящее время](~/cross-platform/internals/available-assemblies.md).



В частности API профиля, используемого Xamarin.iOS не включает System.Configuration, поэтому невозможно использовать внешний XML-файлы можно настроить поведение среды выполнения.
