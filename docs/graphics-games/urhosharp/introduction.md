---
title: Общие сведения о UrhoSharp
description: В этом документе описывается базовая структура UrhoSharp приложения и ссылки на различные руководства и образцы приложений, которые демонстрируют использование UrhoSharp.
ms.prod: xamarin
ms.assetid: 18041443-5093-4AF7-8B20-03E00478EF35
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: d39faabc0851e3e89b03ad58a7f1ead3894efc15
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783559"
---
# <a name="an-introduction-to-urhosharp"></a>Общие сведения о UrhoSharp

![Эмблема UrhoSharp](introduction-images/urhosharp-icon.png)

UrhoSharp — это мощная трехмерной игры механизм для разработчиков Xamarin и .NET.  Он аналогичен в духе SceneKit Apple и SpriteKit и включать физических, навигации, сети и намного более хотя при этом различных платформах.

Привязка .NET для [Urho3D](http://urho3d.github.io/) ядра и позволяет разработчикам создавать межплатформенные код, который можно выбрать целевую Android, iOS, Windows и Mac с тем же codebase и может отображать системы OpenGL и Direct3D.

UrhoSharp представляет собой игры модуль с большим количеством функций без дополнительной настройки:

- Мощная трехмерной отрисовки графики
- [Моделирование физических](https://developer.xamarin.com/api/namespace/Urho.Physics/) (с помощью маркера библиотеки)
- [Обработка сцены](https://developer.xamarin.com/api/type/Urho.Scene/)
- Поддержка ожидания асинхронного выполнения
- [Понятное действия API](https://developer.xamarin.com/api/namespace/Urho.Actions/)
- [2D интеграция в трехмерных сцен](https://developer.xamarin.com/api/namespace/Urho.Urho2D/)
- [Отрисовки шрифта с помощью FreeType](https://developer.xamarin.com/api/type/Urho.Gui.FontFaceFreeType/)
- [Клиентские и серверные сетевые возможности](https://developer.xamarin.com/api/namespace/Urho.Network/)
- [Импорт широкий спектр средств](https://developer.xamarin.com/api/namespace/Urho.Resources/) (с открыть библиотеку ресурсов)
- [Сетка навигации и pathfinding](https://developer.xamarin.com/api/namespace/Urho.Navigation/) (с помощью приведена обход)
- [Создание выпуклой оболочки для обнаружения конфликтов](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) (с помощью StanHull)
- [Воспроизведение звука](https://developer.xamarin.com/api/namespace/Urho.Audio/) (с **libvorbis**)

## <a name="getting-started"></a>Начало работы

UrhoSharp удобным образом распространяется в виде [пакет NuGet](https://www.nuget.org/) и его можно добавить в проект C# и F #, ориентированных на Windows, Mac, Android или iOS.  NuGet поставляется с и библиотеки, которые необходимы для запуска программы, а также основные ресурсы (coredata в методе), используемые обработчиком.

### <a name="urho-as-a-portable-class-library"></a>Urho как Переносимая библиотека классов

Urho пакета могут быть использованы из проекта под конкретную платформу или из проекта переносимой библиотеки классов, что позволяет повторно использовать весь код на всех платформах.  Это означает, что все, что нужно сделать для каждой платформы, для записи на платформе конкретной точки входа и последующей передачи управления в общем коде игр.

### <a name="samples"></a>Примеры

Представление для использования возможностей Urho можно получить путем открытия в Visual Studio или Visual Studio для Mac образец решения из:

[https://github.com/xamarin/urho-samples](https://github.com/xamarin/urho-samples)

По умолчанию решение содержит проекты для Android, iOS, Windows и Mac.  Мы структурированные это решение, чтобы у нас есть средство запуска определенной платформы очень мала, и все образцы кода и кода игры живет в переносимой библиотеке классов, показывающими как увеличить повторное использование кода для всех платформ.

Обратитесь к [Urho и вашей платформе](~/graphics-games/urhosharp/platform/index.md) более подробную информацию о том, как создавать собственные решения.

Поскольку все образцы имеют общий набор элементов пользовательского интерфейса, образцы абстрагированы базовая настройка в этом файле:

[https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/Core/Sample.cs)

Это предоставляет образец базовый класс, который обрабатывает некоторые основные нажатия клавиш и touch события настройки веб-камеры, предоставляет основные элементы интерфейса и это позволяет сосредоточиться на конкретные функции, является время демонстрации каждого образца.

Следующий пример показывает, что подсистема возможностями:

- [Samply игры](https://github.com/xamarin/urho-samples/tree/master/SamplyGame) простой клон ShootySkies.

Хотя в других образцах показано отдельные свойства каждого образца.

## <a name="basic-structure"></a>Основная структура

Игра должна подкласс [ `Application` ](https://developer.xamarin.com/api/type/Urho.Application/) класса, это происходит, когда настроит игру (на [ `Setup` ](https://developer.xamarin.com/api/member/Urho.Application.Setup/) метод) и запускать игру (в [ `Start` ](https://developer.xamarin.com/api/member/Urho.Application.Start) метода).  Затем можно создать основной пользовательский интерфейс.  Мы собираемся перемещайтесь небольшой образец, демонстрирующий API-интерфейсы для 3D-сцену, некоторые элементы пользовательского интерфейса и присоединение к нему простое поведение программы установки.

```csharp
class MySample : Application {
    protected override void Start ()
    {
        CreateScene ();
        Input.KeyDown += (args) => {
            if (args.Key == Key.Esc) Exit ();
        };
    }

    async void CreateScene()
    {
        // UI text
        var helloText = new Text()
        {
            Value = "Hello World from MySample",
            HorizontalAlignment = HorizontalAlignment.Center,
            VerticalAlignment = VerticalAlignment.Center
        };
        helloText.SetColor(new Color(0f, 1f, 1f));
        helloText.SetFont(
            font: ResourceCache.GetFont("Fonts/Font.ttf"),
            size: 30);
        UI.Root.AddChild(helloText);

        // Create a top-level scene, must add the Octree
        // to visualize any 3D content.
        var scene = new Scene();
        scene.CreateComponent<Octree>();
        // Box
        Node boxNode = scene.CreateChild();
        boxNode.Position = new Vector3(0, 0, 5);
        boxNode.Rotation = new Quaternion(60, 0, 30);
        boxNode.SetScale(0f);
        StaticModel modelObject = boxNode.CreateComponent<StaticModel>();
        modelObject.Model = ResourceCache.GetModel("Models/Box.mdl");
        // Light
        Node lightNode = scene.CreateChild(name: "light");
        lightNode.SetDirection(new Vector3(0.6f, -1.0f, 0.8f));
        lightNode.CreateComponent<Light>();
        // Camera
        Node cameraNode = scene.CreateChild(name: "camera");
        Camera camera = cameraNode.CreateComponent<Camera>();
        // Viewport
        Renderer.SetViewport(0, new Viewport(scene, camera, null));
        // Perform some actions
        await boxNode.RunActionsAsync(
            new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
        await boxNode.RunActionsAsync(
            new RepeatForever(new RotateBy(duration: 1,
                deltaAngleX: 90, deltaAngleY: 0, deltaAngleZ: 0)));
     }
}
```

При запуске этого приложения, вы быстро поймете жалуется среда выполнения средств не существует.  Вам нужно будет создать иерархию в проекте, который начинается с имени специальные папки «Данные», а внутри этого следует включить необходимые ресурсы в программу ссылку.  Затем необходимо задать в свойствах элемента для каждого средства «Копировать в выходной каталог» для «Копирования если новее», который будет убедитесь, что данных существует.

Сообщите нам объясняется, что происходит здесь.

Для запуска приложения вызовите функцию инициализации подсистемы, следуют создания нового экземпляра класса приложения, следующим образом:

```csharp
new MySample().Run();
```

Среда выполнения будет вызывать `Setup` и `Start` методы.  При переопределении `Setup` можно настроить параметры ядра (не показано в этом образце).

Необходимо переопределить `Start` как это приведет к запуску игру.  В этом методе будет загрузить Ваши активы, подключите обработчики событий, установки сцены и запустите каких-либо действий, которые необходимы.  В нашем примере мы оба создать небольшую пользовательский Интерфейс для отображения для пользователя, а также настройка объемной сцены.

Следующий фрагмент кода пользовательского интерфейса используется для создания текстового элемента и добавить его в приложение:

```csharp
// UI text
var helloText = new Text()
{
    Value = "Hello World from UrhoSharp",
    HorizontalAlignment = HorizontalAlignment.Center,
    VerticalAlignment = VerticalAlignment.Center
};
helloText.SetColor(new Color(0f, 1f, 1f));
helloText.SetFont(
    font: ResourceCache.GetFont("Fonts/Font.ttf"),
    size: 30);
UI.Root.AddChild(helloText);
```

Платформа пользовательского интерфейса существует для предоставления очень простой игры пользовательский интерфейс, и он работает путем добавления новых узлов для [ `UI.Root` ](https://developer.xamarin.com/api/property/Urho.Gui.UI.Root/) узла.

Во второй части наш пример настройки основных сцены.  Это включает в себя несколько этапов, создание объемной сцены, Создание 3D поля на экране, добавление источника света, камеры и область просмотра.  Они изучаются более подробно в разделе [сцены, узлы, компоненты и камер](~/graphics-games/urhosharp/using.md#scenenodescomponentsandcameras).

В третьей части выборка запускает несколько действий.  Действия, рецепты, описывающие тот или иной эффект и после создания они могут быть выполнены узлом по требованию, вызвав [ `RunActionAsync` ](https://developer.xamarin.com/api/member/Urho.Node.RunActionsAsync) метод `Node`.

Первое действие масштабирует поле с эффектом прыгающего и второй поворачивает поле навсегда:

```csharp
await boxNode.RunActionsAsync(
    new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1)));
```

Выше демонстрирует, как первое действие, которое мы создадим [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo/) действия, это просто инструкций, который указывает, что для масштабирования для второго сторону одно значение свойства шкалы узла.  Это действие в свою очередь окружает действие плавности [ `EaseBounceOut` ](https://developer.xamarin.com/api/type/Urho.Actions.EaseBounceInOut/) действия.  Плавности действия исказить линейной выполнения действия и применение эффекта, в этом случае она предоставляет эффект перебрасываются out.
Поэтому наши инструкций можно записать так:

```csharp
var recipe = new EaseBounceOut(new ScaleTo(duration: 1f, scale: 1));
```

После создания рецепт мы выполним рецепт:

```csharp
await boxNode.RunActionsAsync (recipe)
```

Указывает, что await будет нужно возобновить выполнение после этой строки, после выполнения действия.  После выполнения действия мы триггер второй анимации.

[С помощью UrhoSharp](~/graphics-games/urhosharp/using.md) документа более подробно рассматривается более подробно принципы Urho и как следует структурировать код для создания игр.

## <a name="copyrights"></a>Авторские права

В этой документации содержит исходное содержимое из Xamarin Inc, но рисует широко из открытой документации для проекта Urho3D и содержит снимки экрана из проекта Cocos2D.

### <a name="related-links"></a>Связанные ссылки

- [Планеты Земли книги](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Изучение координаты книги](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
