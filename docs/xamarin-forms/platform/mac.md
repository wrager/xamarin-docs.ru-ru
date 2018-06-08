---
title: Настройка платформ Mac
description: Xamarin.Forms теперь имеет поддержку предварительной версии для платформ Mac
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: c3a2c36463b2934254c54f3f2250ee253d57798b
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848239"
---
# <a name="mac-platform-setup"></a>Настройка платформ Mac

![Предварительный просмотр](~/media/shared/preview.png)

Прежде чем начать, создайте (или использовать существующий) Xamarin.Forms проекта.
Можно добавить только Mac приложений с помощью Visual Studio для Mac.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**Путем добавления проекта macOS Xamarin.Forms, [университета Xamarin](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Добавление приложения Mac

Следуйте этим инструкциям, чтобы добавить приложение Mac, который будет выполняться на macOS Сьерра и Mac OS X El Capitan:

1. В Visual Studio для Mac, щелкните правой кнопкой мыши существующее решение Xamarin.Forms и выберите **Добавить > Добавить новый проект...**

2. В **новый проект** окна выберите **Mac > приложения > приложения Cocoa** и нажмите клавишу **Далее**.

3. Тип **имя приложения** (и при необходимости выберите другое имя для закрепления элемента), затем нажмите клавишу **Далее**.

4. Проверьте конфигурацию и нажмите клавишу **создать**. Эти действия показаны в ниже:

  ![Анимированные инструкции, показывающий, как добавить приложение Cocoa](mac-images/add-macos-proj.gif)

5. В проекте Mac, щелкните правой кнопкой мыши **пакеты > Добавление пакетов...**  добавление [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet. Следует также обновить другие проекты в этой версии.

6. В проекте Mac, щелкните правой кнопкой мыши **ссылки** и добавьте ссылку на проект Xamarin.Forms (проект библиотеки общий проект или .NET Standard).

  ![Добавьте ссылку в проекте с общим кодом Xamarin.Forms](mac-images/references-sml.png)

7. Обновление **Main.cs** для инициализации `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. Обновление `AppDelegate` для инициализации Xamarin.Forms, создать окно и загрузить приложение Xamarin.Forms (Обратите внимание задать соответствующий `Title`). _Если у вас есть другие зависимости, которые должны быть инициализированы, сделать здесь также._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. Дважды щелкните **Main.storyboard** для редактирования в Xcode. Выберите **окна** и _снимите_ **является начальной контроллера** флажок (это так, как приведенный выше код создает окно):

  [![Снимите флажок является начальной контроллера в Xcode](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  Вы можете изменить систему меню в раскадровку, чтобы удалить ненужные элементы.

10. Наконец добавьте все локальные ресурсы (например) файлы изображений) с существующие проекты платформы, которые необходимы.

11. Проект Mac должна работать на macOS Xamarin.Forms кода!

## <a name="next-steps"></a>Следующие шаги

### <a name="styling"></a>Задание стиля

С помощью последних изменений, внесенных в `OnPlatform` теперь можно назначить любое количество платформы. Включающий macOS.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

Обратите внимание также может дважды на платформах следующим образом: `<On Platform="iOS, macOS" ...>`.

### <a name="window-size-and-position"></a>Размер и положение окна

Можно настроить исходный размер и расположение окна в `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>Известные проблемы

Это предварительная версия, поэтому следует ожидать, что не все из которых работает готовности. Ниже приведены некоторые моменты, которые могут возникнуть при добавлении macOS в проекты.

### <a name="not-all-nugets-are-ready-for-macos"></a>Не все NuGets готовых macOS

Пакеты должны быть предназначены «xamarinmac20» для работы в проекте macOS. Вы можете обнаружить, что некоторые из библиотек, которые можно использовать еще не поддерживают macOS.

В этом случае вам потребуется отправить запрос исполнителю проекта, чтобы добавить его. Искать альтернативы может потребоваться в том случае, пока они поддерживают.

### <a name="missing-xamarinforms-features"></a>Для отсутствующих компонентов Xamarin.Forms

Не все функции Xamarin.Forms выполнены в этой предварительной версии; Ниже приведен список некоторых функциональных возможностей, которые еще не реализован.

* Нижние колонтитулы
* Изображение — аспект
* UnevenRows ListView — ScrollTo, поддерживают обновление, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* Навигация — InsertPageBefore
* OpenGLRenderer
* Палитра – Bindable/Observable реализации
* BarTextColor TabbedPage — BarBackgroundColor,
* TableView – UnevenRows
* ForceUpdateSize ViewCell — IsEnabled,
* WebView — большинство WebNavigationEvents


## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Mac](~/mac/index.yml)
