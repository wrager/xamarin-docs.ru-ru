---
title: Поддержка UrhoSharp Windows
description: В этом документе рассматривается поддержка UrhoSharp в Windows. Он описывает, как создать проект, настроить и запустить Urho, интеграция с WPF и интегрировать UWP.
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 094eaf0ebe84ce8c1771bd6481ee897463349856
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783237"
---
# <a name="urhosharp-windows-support"></a>Поддержка UrhoSharp Windows

Пока Urho переносимой библиотеке классов, и обеспечивает такой же API, используемые в нескольких различных платформ для логику игры по-прежнему необходимо инициализировать Urho в драйвере определенной платформы, а в некоторых случаях, необходимо воспользоваться преимуществами конкретных компонентов платформы .

В следующих страницах, предполагается, что `MyGame` является подклассом `Application` класса.

**Поддерживаемые архитектуры:** только 64-разрядная версия Windows.

Вы увидите полные примеры, показывающие, как использовать это в нашем [образцы](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>Отдельный проект

### <a name="creating-a-project"></a>Создание проекта

Создайте проект консольного, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

### <a name="configuring-and-launching-urho"></a>Настройка и запуск Urho

Чтобы запустить приложение, для этого:

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>Интегрируется с WPF

### <a name="creating-a-project"></a>Создание проекта

Создание проекта WPF, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

### <a name="configuring-and-launching-urho-from-wpf"></a>Настройка и запуск Urho из WPF

Создать подкласс `Window` и настраивать ваши активы, следующим образом:

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>Интегрируется с UWP

### <a name="creating-a-project"></a>Создание проекта

Создайте проект UWP, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

### <a name="configuring-and-launching-urho-from-uwp"></a>Настройка и запуск Urho из UWP

Создать подкласс `Window` и настраивать ваши активы, следующим образом:

```csharp
{
            InitializeComponent();
            GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
                .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
                .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
                .ToArray();
            DataContext = this;
            Loaded += (s, e) => RunGame (new MyGame ());
        }

        public void RunGame(TypeInfo value)
        {
            //at this moment, UWP supports assets only in pak files (see PackageTool)
            currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
        }
    }
```

### <a name="example"></a>Пример

[Полный пример](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windowsforms"></a>Интегрируется с Windows.Forms

### <a name="creating-a-project"></a>Создание проекта

Создайте проект Windows.Forms, ссылаются на Urho NuGet и убедитесь, что может найти активы (каталогов, содержащих каталог данных).

### <a name="configuring-and-launching-urho-from-windowsforms"></a>Настройка и запуск Urho из Windows.Forms

Запустите Urho из формы см. в разделе [полный пример](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
