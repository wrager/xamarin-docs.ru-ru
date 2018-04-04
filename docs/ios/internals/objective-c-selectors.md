---
title: Селекторы Objective c.
ms.prod: xamarin
ms.assetid: A80904C4-6A89-389B-0487-057AFEB70989
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 60f107bda29b351c119f5702b0ca797d7d16b0b1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="objective-c-selectors"></a>Селекторы Objective c.

Создается на основе языка C целью *селекторы*. Селектора — это сообщение, которое может быть отправлено на объект или *класса*. [Xamarin.iOS](~/ios/internals/api-design/index.md) maps экземпляром селекторы для методов экземпляра и селекторы для статических методов класса.

В отличие от обычных функций C (и, как и функций-членов C++), невозможно вызвать непосредственно с помощью селектора [P/Invoke](http://www.mono-project.com/docs/advanced/pinvoke/).
(*Выделить*: Теоретически можно использовать P/Invoke для функций-членов C++ и виртуальным, но было нужно волноваться по поводу декорированием-компилятор, являющееся мире боль лучше учитывается.) Вместо этого селекторы отправляются в класс Objective-C или экземпляра с помощью [ `objc_msgSend` функция](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend).

Возможно, вам [данного руководства, полезно на обмен сообщениями Objective-C](http://developer.apple.com/iphone/library/documentation/cocoa/conceptual/ObjCRuntimeGuide/Articles/ocrtHowMessagingWorks.html) полезно.

<a name="Example" />

## <a name="example"></a>Пример

Предположим, что нужно для вызова [-[NSString sizeWithFont:forWidth:lineBreakMode:]](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:) селектора.
Объявление (из документации компании Apple) является:

```csharp
- (CGSize)sizeWithFont:(UIFont *)font forWidth:(CGFloat)width lineBreakMode:(UILineBreakMode)lineBreakMode
```

-  Возвращаемый тип — *CGSize* единой интерфейсов API.
-  *Шрифта* параметр [UIFont](https://developer.xamarin.com/api/type/UIKit.UIFont/) (и типа (неявно) производным от [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) ) и таким образом сопоставляется [System.IntPtr](https://developer.xamarin.com/api/type/System.IntPtr/) .
-  *Ширина* параметр *CGFloat* , сопоставляется с *nfloat*.
-  *LineBreakMode* параметр *UILineBreakMode* , уже были связаны в Xamarin.iOS как [UILineBreakMode перечисления](https://developer.xamarin.com/api/type/UIKit.UILineBreakMode/) .


Взаимодействие и хотим, чтобы объявление objc_msgSend, которое соответствует:

```csharp
CGSize objc_msgSend(IntPtr target, IntPtr selector,
    IntPtr font, nfloat width, UILineBreakMode mode);
```

Будет необходимо объявить его:

```csharp
[DllImport (Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend")]
static extern CGSize cgsize_objc_msgSend_IntPtr_float_int (
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

После объявления мы может вызывать после соответствующие параметры:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size = cgsize_objc_msgSend_IntPtr_float_int(
    target.Handle, selector.Handle,
    font == null ? IntPtr.Zero : font.Handle,
    width,
    mode);
```

Возвращаемое значение было структуру, которая была размер меньше, чем 8 байт (такие как более старую версию `SizeF` используется перед переключением в API-интерфейсы единой) приведенный выше код запустится в симуляторе, но зависших на устройстве. Таким образом, чтобы вызвать селектора, возвращающий значение меньше 8 бит в размере, поэтому мы *также* требуется объявлять `objc_msgSend_stret()` функции:

```csharp
[DllImport (MonoTouch.Constants.ObjectiveCLibrary, EntryPoint="objc_msgSend_stret")]
static extern void cgsize_objc_msgSend_stret_IntPtr_float_int (
    out CGSize retval,
    IntPtr target, IntPtr selector,
    IntPtr font,
    nfloat width,
    UILineBreakMode mode);
```

Затем станет вызов:

```csharp
NSString      target = ...
Selector    selector = new Selector ("sizeWithFont:forWidth:lineBreakMode:");
UIFont          font = ...
nfloat          width = ...
UILineBreakMode mode = ...

CGSize size;

if (Runtime.Arch == Arch.SIMULATOR)
    size = cgsize_objc_msgSend_IntPtr_float_int(
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero : font.Handle,
        width,
        mode);
else
    cgsize_objc_msgSend_stret_IntPtr_float_int(
        out size,
        target.Handle, selector.Handle,
        font == null ? IntPtr.Zero: font.Handle,
        width,
        mode);
```


<a name="Invoking_a_Selector" />

## <a name="invoking-a-selector"></a>Вызов селектора

Вызов селектора состоит из трех шагов.

1.  Получите целевой объект селектора.
1.  Получите имя селектора.
1.  Вызов objc_msgSend() с соответствующими аргументами.


<a name="Selector_Targets" />

### <a name="selector-targets"></a>Выбор целевых объектов

Селектор целевой объект — класс Objective-C или экземпляра объекта. Если целевой объект является экземпляром и полученные из связанного типа Xamarin.iOS, используйте [ObjCRuntime.INativeObject.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.INativeObject.Handle/) свойство.

Если целевой объект — это класс, используйте [ObjCRuntime.Class](https://developer.xamarin.com/api/type/ObjCRuntime.Class/) для получения ссылки на экземпляр класса, используйте [Class.Handle](https://developer.xamarin.com/api/property/ObjCRuntime.Class.Handle/) свойство.


<a name="Selector_Names" />

### <a name="selector-names"></a>Селектор имена

Селектор имена перечислены в документации компании Apple. Например [методы расширения UIKit NSString](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html) включают [sizeWithFont:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:) и [sizeWithFont:forWidth:lineBreakMode:](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/NSString_UIKit_Additions/Reference/Reference.html#//apple_ref/occ/instm/NSString/sizeWithFont:forWidth:lineBreakMode:). Внедренные и конечные двоеточия важны и являются частью имени селектора.

Получив имя селектора, можно создать [ObjCRuntime.Selector](https://developer.xamarin.com/api/type/ObjCRuntime.Selector/) экземпляра для него.


<a name="Calling_objc_msgSend()" />

### <a name="calling-objcmsgsend"></a>Вызов objc_msgSend()

 `objc_msgSend()` используется для отправки сообщения (для выбора) на объект. Это семейство функций использует по крайней мере два обязательных аргумента: целевой селектор (экземпляра или класса обработки), селектор сам и затем все аргументы, необходимые для конкретного селектора. Экземпляр и селектор аргументы должны быть `System.IntPtr`, и все остальные аргументы должны соответствовать типу ожидает селектор, например `nint` для `int`, или `System.IntPtr` для всех `NSObject`-производных типов. Используйте [NSObject.Handle](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) , чтобы получить `IntPtr` для экземпляра типа Objective-C.

К сожалению, имеется более одного `objc_msgSend()` функции.

Используйте [ `objc_msgSend_stret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) для селекторы, которые возвращают структуру.
Чтобы не усложнять «интерес», на устройствах ARM это включает в себя все возвращаемые типы, которые являются *не* перечисление или любых типов C builtin (char, short, int, long, с плавающей запятой). На x86 (имитатор) должна использоваться для всех структур, размер которых превышает размер 8 байт. (CGSize — используется в примере выше — именно 8 байт и поэтому не используют `objc_msgSend_stret()` в симуляторе.)

Используйте [ `objc_msgSend_fpret()` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_fpret) для селекторы, которые возвращают значение с плавающей запятой на x86 только. Эта функция не требуется использовать на устройствах ARM; Вместо этого используйте `objc_msgSend()`.

Главный [objc_msgSend()](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend) функция используется для всех других селекторов.

Если вы решили, что `objc_msgSend()` функции необходимо вызвать (и она может быть несколько, например для симулятора и устройства), можно использовать обычную [ `[DllImport]` ](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.DllImportAttribute/) метод для объявления функции для последующего вызова.

Набор готовых `objc_msgSend()` объявления можно найти в [ `ObjCRuntime.Messaging` ](https://developer.xamarin.com/api/type/ObjCRuntime.Messaging/).


<a name="ugly" />

## <a name="the-ugly"></a>Ужасно

Objective-C имеет три типа из `objc_msgSend` методов: один для обычных вызовов, один для вызовов, которые возвращают значения с плавающей запятой (x86) и один для вызовов функции, возвращающие структуры значения. Второй содержит суффикс `_stret` в `ObjCRuntime.Messaging`.

Если вы вызываете метод, который будет возвращать определенных структур (правил, описанных ниже), необходимо вызвать метод с помощью возвращаемого значения как первый параметр в качестве выходного значения, следующим образом:

```csharp
// The following returns a PointF structure:
PointF ret;
Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, this.Handle, selConvertPointFromWindow.Handle, point, window.Handle);
```

Все становится сложный здесь а потому, что правило должно использования _stret_ отличается на X86 и ARM, если требуется, чтобы привязки для работы в симуляторе и устройств, необходимо добавить код, который выглядит следующим образом:

```csharp
if (Runtime.Arch == Arch.DEVICE){
    PointF ret;

    Messaging.PointF_objc_msgSend_stret_PointF_IntPtr (out ret, myHandle, selector.Handle);

    return ret;
} else
    return Messaging.PointF_objc_msgSend_PointF_IntPtr (myHandle, selector.Handle);
```

### <a name="using-the-objcmsgsendstret-method"></a>С помощью objc\_msgSend\_stret-метод

Правила по использованию [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) похожи для **ARM**:

-  Любое значение, тип которого не перечисление или любых типов перечисления (int, byte, short, long, double, число с плавающей запятой).


Правила по использованию [ `objc_msgSend_stret` ](http://developer.apple.com/mac/library/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html#//apple_ref/c/func/objc_msgSend_stret) похожи для **X86**:

-  Любое значение типа, который не является перечислением или любого базового типа перечисления (int, byte, short, long, double, число с плавающей запятой) и собственного, размер которого больше, чем 8 байт.


### <a name="creating-your-own-signatures"></a>Создание собственных подписи.

Следующие [страницы](https://gist.github.com/rolfbjarne/981b778a99425a6e630c) можно использовать для создания собственных подписей при необходимости.



## <a name="related-links"></a>Связанные ссылки

- [Образец селекторы](https://developer.xamarin.com/samples/mac-ios/Objective-C/Selectors/)
