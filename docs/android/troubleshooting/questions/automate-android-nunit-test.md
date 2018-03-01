---
title: "Как автоматизировать Android NUnit тестового проекта?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5f98fc351c879be55475808b5ab412449dadc7d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Как автоматизировать Android NUnit тестового проекта?

> [!NOTE]
> **Примечание:** в настоящем руководстве приводятся инструкции по настройке проекта тестирования Android NUnit не Xamarin.UITest проекта. Можно найти руководства Xamarin.UITest [здесь](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

При создании Android проект модульного теста [Visual Studio для Mac] или приложение модульного тестирования (Android) [Visual Studio], по умолчанию она не запустится автоматически тестов.
Для автоматизации модульного теста android: для выполнения тестов NUnit на целевом устройстве, мы используем `Android.App.Instrumentation` подкласс, в котором можно создавать и выполнять с помощью `adb shell am instrument` команды.

Во-первых, мы создаем **TestInstrumentation.cs** файл, который создает подкласс `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (объявлено в `Xamarin.Android.NUnitLite.dll`). `TestInstrumentation(IntPtr, JniHandleOwnership)` Конструктор _должен_ предоставляться и виртуальный `AddTests()` метод должен быть переопределен.
`AddTests()` элементы управления, фактически выполняются тесты. Этот файл является главным образом шаблона.

Далее, `.csproj` необходимо изменить, чтобы добавить `TestInstrumentation.cs`.

При необходимости `.csproj` может быть изменен для добавления `RunTests` цели MSBuild, которая позволит вызов модульные тесты как:

```shell
msbuild /t:RunTests Project.csproj
```

С помощью новый целевой объект не является обязательным. Вместо этого может использоваться соответствующей adb команды:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Замените `@PACKAGE\_NAME@` в соответствии с потребностями; это значение присутствует в **AndroidManifest.xml** `/manifest/@package` атрибута.

*Важное примечание*: С [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) выпуска пакета имена по умолчанию для Android с помощью вызываемых оболочек будет основываться на команду MD5SUM экспортируемого типа имени сборки. Это позволяет полностью уточненное имя, совпадающее должен предоставляться из двух разных сборках и не возникает ошибка упаковки. Поэтому убедитесь, что используется \`имя\` свойство \`инструментария\` значений для создания имени для чтения ACW или класс.

_Необходимо использовать имя ACW в `adb` команда_. Переименование и рефакторинг классов C# таким образом потребуется изменение `RunTests` команду, чтобы использовать правильное имя ACW.

Дополнения к CSPROJ-файл:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

```cs 
using System;
using System.Reflection;
 
using Android.App;
using Android.Content;
using Android.Runtime;
 
using Xamarin.Android.NUnitLite;
 
namespace App.Tests {
 
    [Instrumentation(Name="app.tests.TestInstrumentation")]
    public class TestInstrumentation : TestSuiteInstrumentation {
 
        public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
        {
        }
 
        protected override void AddTests ()
        {
            AddTest (Assembly.GetExecutingAssembly ());
        }
    }
}
```

