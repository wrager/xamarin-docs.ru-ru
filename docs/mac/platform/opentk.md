---
title: "Общие сведения о OpenTK"
description: "В этой статье содержатся вводные сведения о работе с OpenTK в приложении Xamarin.Mac. Она охватывает Создание и поддержание окна игры, Подготовка к просмотру простого объекта и отображение объекта для пользователя."
ms.topic: article
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4af76a37e5fd42ff1d6344f60642425c73e9d733
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="introduction-to-opentk"></a>Общие сведения о OpenTK

OpenTK (открыть набор) — дополнительно, низкоуровневые C# библиотека, которая упрощает работу с OpenGL и OpenCL OpenAL. OpenTK может использоваться для игр, научных приложений или другие проекты, которые требуют трехмерной графики, аудио- или вычислительные функции. В этой статье дается краткое введение в использование в приложении Xamarin.Mac OpenTK.

[ ![](opentk-images/intro01.png "Запустите пример приложения")](opentk-images/intro01.png)

В этой статье мы обсудим основные OpenTK Xamarin.Mac приложения. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>О OpenTK

Как уже говорилось выше, OpenTK (открыть набор) — дополнительно, низкоуровневые C# библиотека, которая упрощает работу с OpenGL и OpenCL OpenAL. Использование в приложении Xamarin.Mac OpenTK предоставляет следующие возможности:

- **Быстрой разработки** -OpenTK предоставляет надежный и типами данных встроенные документации улучшить рабочий процесс написания кода и перехватывать ошибки, проще и быстрее.
- **Простую интеграцию** -OpenTK предназначена для интеграции с приложениями .NET.
- **Разрешительной лицензии** -OpenTK распространяется в рамках лицензии MIT/X11 и совершенно бесплатно.
- **Форматированный, строго типизированным привязки** -OpenTK поддерживает последние версии OpenGL OpenGL | ES, OpenAL и OpenCL с загрузкой автоматическое расширение, документации ошибок проверки и встроенные.
- **Гибкие средства графического пользовательского интерфейса** -OpenTK предоставляет собственный, окна игры высокой производительности предназначен специально для игр и Xamarin.Mac.
- **Полное управление CLS-совместимого кода** -OpenTK поддерживает 32-разрядных и 64-разрядной версии macOS без неуправляемых библиотек.
- **3D Toolkit математические** предоставляет OpenTK `Vector`, `Matrix`, `Quaternion` и `Bezier` структуры через его 3D математические Toolkit.

OpenTK может использоваться для игр, научных приложений или другие проекты, которые требуют трехмерной графики, аудио- или вычислительные функции.

Дополнительные сведения см. в разделе [откройте Toolkit](http://www.opentk.com) веб-сайта.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>Краткое руководство OpenTK

Как краткое введение в использование в приложении Xamarin.Mac OpenTK мы собираемся создать простое приложение, которое открывается представление, игры, прорисовывает простой прямоугольник в режиме и в attachs представлении игры на главное окно приложения Mac для отображения треугольника.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>Запуск нового проекта

Запустите Visual Studio для Mac и создайте новое решение Xamarin.Mac. Выберите **Mac** > **приложения** > **Общие** > **Cocoa приложения**:

[ ![](opentk-images/sample01.png "Добавление нового приложения Cocoa")](opentk-images/sample01.png)

Введите `MacOpenTK` для **имя проекта**:

[ ![](opentk-images/sample02.png "Задание имени проекта")](opentk-images/sample02.png)

Нажмите кнопку **создать** кнопка для создания нового проекта.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>Включая OpenTK

Прежде чем открыть TK можно использовать в приложении Xamarin.Mac, необходимо включить ссылку на сборку OpenTK. В **обозревателе решений**, щелкните правой кнопкой мыши **ссылки** папку и выберите **изменить ссылки...** .

Установите флажок, `OpenTK` и нажмите кнопку **ОК** кнопки:

[ ![](opentk-images/sample03.png "Изменение ссылок проекта")](opentk-images/sample03.png)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>С помощью OpenTK

Создан новый проект, дважды щелкните `MainWindow.cs` файла в **обозревателе решений** чтобы открыть его для редактирования. Сделать `MainWindow` внешний класс следующим образом:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

Давайте посмотрим, этот код, подробно ниже.

<a name="Required_APIs" />

### <a name="required-apis"></a>Необходимые интерфейсы API

Несколько ссылок, необходимых для использования OpenTK в классе Xamarin.Mac. В начале определения включены следующие `using` инструкции:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
Это минимальный набор требуется для любого класса, с помощью OpenTK.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>Добавление представления игры

Далее нам нужно создать представление игры, чтобы содержать все наши взаимодействие с OpenTK и отображает результаты. Мы использовали следующий код:

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

Здесь мы внесли представление игры по размеру окна Mac Main и заменена новой представление содержимого окна `MonoMacGameView`. Так как мы заменили существующего содержимого окна, нашей представление было присвоено автоматически устанавливается размер при изменении размера главного окна.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>Реагирование на события

Есть несколько событий по умолчанию, каждое представление игра должна отвечать на. В этом разделе будут рассмотрены основные события.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Событие загрузки

`Load` События — это место для загрузки ресурсов из диска, например изображения, текстуры и музыки. Для наших простых тестового приложения не используются, `Load` события, но включен для ссылки:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>Событие изменения размера

`Resize` Событий должен вызываться каждый раз меняется размер окна просмотра игры. Для нашего примера приложения мы выполняем просмотра GL совпадает с размером нашей представление игры (что является автоматического изменения размера окна Main Mac) с помощью следующего кода:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>Событие UpdateFrame

`UpdateFrame` Событие используется для обработки вводимых пользователями данных, объект позиций, выполнения физических или AI вычисления. Для наших простых тестового приложения не используются, `UpdateFrame` события, но включен для ссылки: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Не включает реализацию Xamarin.Mac OpenTK `Input API`, поэтому необходимо использовать Apple при условии поддержки API-интерфейсы для добавления клавиатуры и мыши. При необходимости можно создать пользовательский экземпляр `MonoMacGameView` и Переопределите `KeyDown` и `KeyUp` методы.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>Событие RenderFrame

`RenderFrame` Событие содержит код, который используется для отрисовки (draw) рисунки. Для нашего примера приложения мы заполняют представление игры с треугольником простой: 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

Обычно код отрисовки будет выполняется с помощью вызова `GL.Clear` для удаления всех существующих элементов перед нарисовать новые элементы.

> [!IMPORTANT]
> Для версии Xamarin.Mac OpenTK **не** вызвать `SwapBuffers` метод вашей `MonoMacGameView` экземпляра после подготовки к просмотру кода. В результате представления игры strobe быстро вместо отображения отображаемого представления.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>Представление игры

С помощью всех событий, необходимо определить и представление игры, присоединенные к окне главного Mac из нашего приложения, читаемых для запуска представления игры и отображения нашей графики. Используйте следующий код:

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

Мы передаем в нужную частоту кадров, которые мы хотим представление игры, чтобы обновить в, в нашем примере мы выбрали `60` кадров в секунду (же частота как обычный ТВ).

Давайте запуска нашего приложения и просмотреть выходные данные:

[ ![](opentk-images/intro01.png "Образец выходных данных приложения")](opentk-images/intro01.png)

Если мы размер окна, игра представление также будет находиться, треугольник будет изменять размер и также обновлены в режиме реального времени.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>Где Далее?

С основами работы с OpenTk в приложении Xamarin.mac сделать ниже приведены некоторые рекомендации о том, что на пробное использование Далее.

- Попробуйте изменить цвет треугольника и цвет фона для представления игры в `Load` и `RenderFrame` события.
- Сделать изменить цвет, когда пользователь клавишу в треугольник `UpdateFrame` и `RenderFrame` события или пользовательские `MonoMacGameView` класса и переопределить `KeyUp` и `KeyDown` методы.
- Сделать перемещение по экрану, с помощью клавиш учитывать в треугольник `UpdateFrame` событий. Подсказки: использование `Matrix4.CreateTranslation` метод для создания матрицы преобразования и вызова `GL.LoadMatrix` метод, чтобы загрузить его в `RenderFrame` событий.
- Используйте `for` цикла для подготовки к просмотру несколько треугольники в `RenderFrame` событий.
- Поворот камеры, чтобы предоставить другое представление треугольника в трехмерном пространстве. Подсказки: использование `Matrix4.CreateTranslation` метод для создания матрицы преобразования и вызова `GL.LoadMatrix` метод, чтобы загрузить его. Можно также использовать `Vector2`, `Vector3`, `Vector4` и `Matrix4` классы для манипуляции с камеры.

Дополнительные примеры см. в разделе [GitHub примеров OpenTK](https://github.com/opentk/opentk/tree/master/Source/Examples) репозитория. Он содержит список официальный примеры использования OpenTK. Вам потребуется адаптировать эти примеры использования с версией Xamarin.Mac OpenTK.

Более сложный пример Xamarin.Mac OpenTK реализации, см. в разделе нашей [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) образца.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье были предприняты никакие кратко рассмотрим работа OpenTK в приложении Xamarin.Mac. Мы узнали, как создать окно игры, как присоединить окна игры в окно Mac и Подготовка к просмотру простых фигур в окне игры.

## <a name="related-links"></a>Связанные ссылки

- [MacOpenTK (пример)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (пример)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Работа с окнами](~/mac/user-interface/window.md)
- [Откройте набор средств](http://www.opentk.com)
- [Рекомендации по интерфейсу отдела OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
