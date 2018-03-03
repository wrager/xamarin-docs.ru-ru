---
title: "Добавление универсального платформы (UWP) приложения Windows"
description: "В этой статье объясняется, как добавить проекта приложения UWP решение Xamarin.Forms, который был создан в Visual Studio для Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: a3746165db83b5b8adba9adb862f9ea09f7069b9
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-universal-windows-platform-uwp-app"></a>Добавление универсального платформы (UWP) приложения Windows

_В этой статье объясняется, как добавить проекта приложения UWP решение Xamarin.Forms, который был создан в Visual Studio для Mac._

Должен быть запущен **2017 г. Visual Studio** на **Windows 10** для построения приложений UWP. Дополнительные сведения об универсальной платформы Windows см. в разделе [введение в универсальной платформы Windows](/windows/uwp/get-started/universal-application-platform-guide/).

UWP в Xamarin.Forms 2.1 и более поздней версии и Xamarin.Forms.Maps поддерживается в Xamarin.Forms 2.2 и более поздних версий.

Проверьте <a href="#troubleshooting">Устранение неполадок</a> раздел полезные советы.

Выполните следующие действия, чтобы добавить приложение UWP, которая будет выполняться на телефонах, планшетных ПК и рабочим столам Windows 10.

 1 . Щелкните правой кнопкой мыши решение и выберите пункт **Добавить > Новый проект...**  и добавьте **пустое приложение (универсальные приложения Windows)** проекта:

  ![](universal-images/add-wu.png "Добавить диалоговое окно нового проекта")

 2 . В **нового проекта универсальной платформы Windows** диалоговое окно, выберите минимальное и целевой версии Windows 10, в которой будет выполняться приложение:

  ![](universal-images/target-version.png "Универсальные приложения для Windows платформа диалоговое окно нового проекта")

 3 . Правой кнопкой мыши проект UWP и выберите **управление пакетами NuGet...**  и добавьте **Xamarin.Forms** пакета. Убедитесь, что другие проекты в решении, также будут обновлены до той же версии пакета Xamarin.Forms.

 4 . Убедитесь, что будет строиться проект UWP **сборки > Configuration Manager** окна (это возможно, не произойти по умолчанию). Деления **построения** и **развернуть** поля для универсальных проектов:

  [ ![](universal-images/configuration-sml.png "Окно диспетчера конфигурации")](universal-images/configuration.png "окно диспетчера конфигурации")

 5 . Щелкните правой кнопкой мыши проект и выберите пункт **Добавить > справочник** и создать ссылку на проект приложения Xamarin.Forms (PCL, .NET Standard или общий проект).

  ![](universal-images/addref-sml.png "Диалоговое окно диспетчера ссылок")

 6 . В проекте UWP изменить **App.xaml.cs** для включения `Init` вызова метода внутри `OnLaunched` метод около строки 52:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . В проекте UWP изменить **MainPage.xaml** , удалив `Grid` внутри `Page` элемента.

 8 . В **MainPage.xaml**, добавьте новый `xmlns` запись для `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . В **MainPage.xaml**, изменения корневого `<Page` элемент `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . В проекте UWP, изменить **MainPage.xaml.cs** удаление `: Page` описатель наследования для имени класса (так как теперь будет наследоваться от `WindowsPage` из-за изменения, сделанные на предыдущем шаге):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . В **MainPage.xaml.cs**, добавьте `LoadApplication` вызов в `MainPage` конструктор для запуска приложения Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . Добавьте любые локальные ресурсы (например) файлы изображений) с существующие проекты платформы, которые необходимы.

<a name="troubleshooting"/>

## <a name="troubleshooting"></a>Устранение неполадок

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>«Target вызова исключений» при использовании «Компилировать с использованием цепочки собственного средства .NET»

Если приложения UWP ссылается на несколько сборок (например третьей стороной библиотеки элементов управления или само приложение разделено на несколько библиотек) Xamarin.Forms не удается загрузить объекты в эти сборки (например, пользовательские модули подготовки отчетов).

Это может произойти при использовании **компилировать с цепочка инструментов .NET Native** которого является параметром для приложений UWP в **свойства > сборки > Общие** окна для проекта.

Это можно исправить с помощью перегрузку UWP из `Forms.Init` вызов в **App.xaml.cs** как показано в приведенном ниже примере кода (следует заменить `ClassInOtherAssembly` с фактического класса ссылки в коде):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Добавьте ссылку на каждой сборки, на которую ссылается приложение.

#### <a name="dependency-services-and-net-native-compilation"></a>Службы зависимостей и .NET собственной компиляции

Выпускные построения с помощью .NET Native, возможен сбой компиляции для разрешения служб зависимостей, определенные вне основной исполняемый файл приложения (например, в отдельный проект или библиотека).

Используйте `DependencyService.Register<T>()` метод, чтобы зарегистрировать классы службы зависимостей вручную. На основании в приведенном выше примере, добавьте метод регистрации следующим образом:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
