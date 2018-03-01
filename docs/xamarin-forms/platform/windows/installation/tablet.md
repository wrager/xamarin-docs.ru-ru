---
title: "Добавление приложения Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 369e8005c1d398c90b14a95ac36fc8659e462fbf
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-windows-app"></a>Добавление приложения Windows


1. **Щелкните правой кнопкой мыши решение > Добавить > Новый проект...**  и добавьте **пустое приложение (Windows)**

 ![](tablet-images/add-wu.png "Добавить диалоговое окно нового проекта")

2. **правой кнопкой мыши проект > Управление пакетами NuGet...**  и добавьте **Xamarin.Forms** пакета.

3. **правой кнопкой мыши проект > Добавить > справочник** и создать ссылку на проект в общий проект приложения Xamarin.Forms.

  ![](tablet-images/addref.png "Диалоговое окно диспетчера ссылок")

4. Изменить **App.xaml.cs** для включения `Init()` вызова метода внутри `OnLaunched` метод около строки 65

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Изменить **MainPage.xaml** -измените корневой элемент `<Page` для `<forms:WindowsPage` *и* определить `xmlns:forms` , он использует:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Изменить **MainPage.xaml.cs** удаление `: Page` описатель наследования для имени класса.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . В по-прежнему **MainPage.xaml.cs**, добавьте `LoadApplication` вызов в `MainPage` конструктор (около строки 28) для запуска приложения Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Дважды щелкните **Package.appxmanifest** для задания этих возможностей, которые часто требуются:

  Набор возможностей:

  * Интернет (клиент)
  * Расположение

9 . Наконец добавьте все локальные ресурсы (например) файлы изображений) с существующие проекты платформы, которые необходимы.

