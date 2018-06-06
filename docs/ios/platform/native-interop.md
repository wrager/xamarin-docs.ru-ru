---
title: Создание ссылок на собственных библиотек в Xamarin.iOS
description: В этом документе описывается связывание собственные библиотеки C в приложении Xamarin.iOS. Он описывается создание универсальной собственным библиотекам и доступ к методам C из C#.
ms.prod: xamarin
ms.assetid: 1DA80280-E78A-EC4B-8673-C249C8425CF5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/28/2016
ms.openlocfilehash: bb27ba8b2d9c1b66448f22b7f80f17ba2e483544
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787731"
---
# <a name="referencing-native-libraries-in-xamarinios"></a>Создание ссылок на собственных библиотек в Xamarin.iOS

Xamarin.iOS поддерживает связь с собственные библиотеки C и Objective-C библиотеки. В этом документе рассматривается создание ссылок на собственные библиотеки C в проекте Xamarin.iOS. Сведения о выполнении то же самое для библиотеки Objective-C, см. Наши [типы привязки Objective-C](~/ios/platform/binding-objective-c/index.md) документа.

<a name="building_native" />

## <a name="building-universal-native-libraries-i386-armv7-and-arm64"></a>Создание универсальной собственные библиотеки (i386 ARMv7 и ARM64)

Часто желательно создания вашего собственного библиотек для каждой из поддерживаемых платформ для разработки приложений iOS (самих устройств i386 для симулятора и ARMv7/ARM64). Если у нас уже есть проект Xcode для библиотеки, это действительно просто сделать.

Чтобы создать версию i386 вашей собственной библиотеки, выполните следующую команду из терминала:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphonesimulator -arch i386 -configuration Release clean build
```

Это приведет к собственной статической библиотеки в разделе `MyProject.xcodeproj/build/Release-iphonesimulator/`. Скопируйте (или переместите) архивный файл библиотеки (libMyLibrary.a) где-то безопасным для последующего использования, предоставляя ему уникальное имя (например **libMyLibrary i386.a**), чтобы он не конфликтуют с версиями arm64 и armv7 той же библиотеки, который вы будете После построения.

Чтобы создать версию ARM64 вашей собственной библиотеки, выполните следующую команду:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch arm64 -configuration Release clean build
```

Это время, построенный собственной библиотеки будет находиться в `MyProject.xcodeproj/build/Release-iphoneos/`. Опять же, скопируйте (или переместите) этот файл в безопасном месте переименованием примерно в **libMyLibrary arm64.a** , чтобы он не конфликтуют.

Теперь сборки версии ARMv7 библиотеки:

```bash
/Developer/usr/bin/xcodebuild -project MyProject.xcodeproj -target MyLibrary -sdk iphoneos -arch armv7 -configuration Release clean build
```

Скопируйте (или переместите) полученный файл библиотеки в то же расположение перемещалась 2 версии библиотеки, переименованием примерно в **libMyLibrary armv7.a**.

Чтобы сделать универсальные двоичный, можно использовать `lipo` средство следующим образом:

```bash
lipo -create -output libMyLibrary.a libMyLibrary-i386.a libMyLibrary-arm64.a libMyLibrary-armv7.a
```

При этом создается `libMyLibrary.a` которого будет универсальной библиотека (fat), подходящим для использования для всех целей разработки iOS.


### <a name="missing-required-architecture-i386"></a>Отсутствует обязательный архитектура i386

При получении `does not implement methodSignatureForSelector` или `does not implement doesNotRecognizeSelector` сообщений в выходных данных среды выполнения при попытке использовать библиотеки Objective-C, в симуляторе библиотеки iOS, скорее всего, не была скомпилирована для архитектуры i386 (см. [универсальной построение Собственные библиотеки](#building_native) выше).

Чтобы проверить архитектур, поддерживаемых данной библиотеки, используйте следующую команду в окне терминала:

```bash
lipo -info /full/path/to/libraryname.a
```

Где `/full/path/to/` — это полный путь к библиотеке, потребляемого и `libraryname.a` не гарантируется имя библиотеки.

Если имеется источник в библиотеку необходимо для компиляции и объединять его на архитектуру i386, если вы хотите тестировать приложение на симуляторе iOS.

### <a name="linking-your-library"></a>Связывание библиотеки

Должен быть статически компонуемые с приложением всех библиотек сторонних разработчиков, которые можно использовать. 

Если вы хотели статически связать библиотеку «libMyLibrary.a», полученный из Интернета или сборки с Xcode, необходимо выполнить ряд действий:

-  Перевести библиотеки в проект
-  Настройка Xamarin.iOS для связывания библиотеки
-  Доступ к методам в библиотеке.


Чтобы **перевести библиотеки в проект**, выберите проект в обозревателе решений и нажмите клавишу **параметр команды + +**. Перейдите к libMyLibrary.a и добавить его в проект. При появлении запроса сообщает о Visual Studio для Mac или Visual Studio скопировать его в проект. После его добавления найти libFoo.a в проекте, щелкните ее правой кнопкой и задать **действие при построении** для **нет**.

Для **Настройка Xamarin.iOS для связывания библиотеки**, параметры проекта для конечного исполняемого файла (не самой библиотеки, но последний программы), вам нужно добавить в **сборка iOS**дополнительный аргумент (это часть параметров проекта) «-gcc_flags» параметр следуют заключенные в кавычки строка, содержащая все дополнительные библиотеки, необходимые для программы, например:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -force_load ${ProjectDir}/libMyLibrary.a"
```

Приведенный выше пример свяжет **libMyLibrary.a**

Можно использовать `-gcc_flags` для указания любого набора аргументов командной строки для передачи для этого к ссылке на окончательный исполняемый файл компилятора GCC. Например эта команда также ссылается на CFNetwork framework:

```bash
-gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

Если вашей собственной библиотеки содержит код C++, необходимо также передать флаг - cxx в «Лишние аргументы», чтобы Xamarin.iOS знает, как использовать правильный компилятор. Для C++ прежние параметры выглядит следующим образом:

```bash
-cxx -gcc_flags "-L${ProjectDir} -lMylibrary -lSystemLibrary -framework CFNetwork -force_load ${ProjectDir}/libMyLibrary.a"
```

<a name="Accessing_C_Methods_from_C#" />

## <a name="accessing-c-methods-from-c35"></a>Доступ к методам C из C&#35;

Имеются следующие два вида собственные библиотеки в iOS.

-  Общие библиотеки, которые входят в состав операционной системы.

-  Статические библиотеки, которые поставляются вместе с приложением.


Для доступа к методам, которые определены в одном из них, используйте [функции P/Invoke моно](http://www.mono-project.com/docs/advanced/pinvoke/) же технологии, используемой в .NET, что равно:

-  Определить, какие нужно вызывать функции C
-  Определить его подпись
-  Определить библиотеку, в которой он находится в
-  Запись соответствующее объявление P/Invoke


При использовании P/Invoke, необходимо указать путь к библиотеке, связываемых с. Когда с помощью iOS общие библиотеки, вы можете либо жестко закодировать путь или можно использовать константы удобства, которые определены в нашей [константы класса](https://developer.xamarin.com/api/type/Constants/), эти константы должны охватывать библиотеки общих операций ввода-вывода.

Например, если нужно вызвать метод UIRectFrameUsingBlendMode из библиотеки UIKit компании Apple, который имеет следующую сигнатуру в C:

```csharp
void UIRectFrameUsingBlendMode (CGRect rect, CGBlendMode mode);
```

Объявление P/Invoke будет выглядеть следующим образом:

```csharp
[DllImport (Constants.UIKitLibrary,EntryPoint="UIRectFrameUsingBlendMode")]
public extern static void RectFrameUsingBlendMode (RectangleF rect, CGBlendMode blendMode);
```

Constants.UIKitLibrary является просто константой определяется как «/ System/Library/Frameworks/UIKit.framework/UIKit», EntryPoint позволяет при необходимости указать внешнее имя (UIRectFramUsingBlendMode) при предоставлении доступа к другим именем в C#, наименьшая RectFrameUsingBlendMode.

<a name="Accessing_C_Dylibs" />

### <a name="accessing-c-dylibs"></a>Доступ к C dylib-файлы

У вас есть необходимость использовать C Dylib приложения Xamarin.iOS, существует ли немного дополнительной установки, необходимые перед вызовом метода `DllImport` атрибут.

Например, если у нас есть `Animal.dylib` с `Animal_Version` метод, который мы назовем в приложении, необходимо сообщить Xamarin.iOS расположения библиотеки перед попыткой его использования.

Чтобы сделать это, измените `Main.CS` файла и сделать его выглядеть следующим образом:

```csharp
static void Main (string[] args)
{
    // Load Dylib
    MonoTouch.ObjCRuntime.Dlfcn.dlopen ("/full/path/to/Animal.dylib", 0);

    // Start application
    UIApplication.Main (args, null, "AppDelegate");
}
```

Где `/full/path/to/` — это полный путь к Dylib, которые используются. Этот код в месте, мы можно затем свяжите `Animal_Version` метод следующим образом:

```csharp
[DllImport("Animal.dylib", EntryPoint="Animal_Version")]
public static extern double AnimalLibraryVersion();
```

<a name="Static_Libraries" />

### <a name="static-libraries"></a>Статические библиотеки

Так как статические библиотеки можно использовать только в iOS, нет, не внешних общей библиотеки для компоновки, поэтому параметр path в атрибут DllImport должен использовать специальное имя `__Internal` (Обратите внимание двойного символа подчеркивания в начале имени) в отличие от имя пути.

Это заставляет DllImport для поиска символа метода, который вы ссылаетесь в текущей программе, вместо того загрузить ее из общей библиотеки.

