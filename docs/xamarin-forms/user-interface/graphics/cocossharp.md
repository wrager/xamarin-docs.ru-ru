---
title: "С помощью CocosSharp в Xamarin.Forms"
description: "CocosSharp может использоваться для добавления точный фигуры, изображения и отрисовки текста в приложение дополнительные визуализации"
ms.topic: article
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 04268f38e558d438a4c84069400e4dc6be213009
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>С помощью CocosSharp в Xamarin.Forms

_CocosSharp может использоваться для добавления точный фигуры, изображения и отрисовки текста в приложение дополнительные визуализации_

[ ![Cocos # в Xamarin.Forms](cocossharp-images/evolve-mike-cocossharp-sml.png "CocosSharp в Xamarin.Forms")](https://evolve.xamarin.com/session/56e210d1bad314273ca4d81e)

[Развивать 2016: Cocos # в Xamarin.Forms](https://evolve.xamarin.com/session/56e210d1bad314273ca4d81e)

## <a name="overview"></a>Обзор

CocosSharp — это мощные и сложные технология отображение графики, чтении сенсорный ввод, воспроизведение аудио-и управление ими. В этом руководстве описывается добавление CocosSharp в Xamarin.Forms приложения. Она охватывает следующее:

* [Что такое CocosSharp?](#what)
* [Добавление пакетов CocosSharp Nuget](#nuget)
* [Пошаговое руководство: Добавление CocosSharp приложения Xamarin.Forms](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>Что такое CocosSharp?

[CocosSharp](~/graphics-games/cocossharp/index.md) — это ядро игры открытым исходным кодом, который доступен на платформе Xamarin.
CocosSharp является эффективным для среды выполнения библиотекой, которая включает следующие компоненты:

* Отрисовка изображения с помощью [CCSprite-класс](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* Фигуры с помощью визуализации [CCDrawNode-класс](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* Каждый кадр логику, используемую [CCNode.Schedule-метод](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* Управление содержимым (загрузки и выгрузки ресурсы, например PNG-файлы) с помощью [CCTextureCache-класс](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* С помощью анимации [CCAction-класс](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

Основная цель CocosSharp — упростить создание кросс платформенных игр 2D; Он также может быть Почетное место в форме Xamarin приложений. Поскольку игр обычно требуется эффективная визуализация и точный контроль над визуальных элементов, CocosSharp можно использовать для добавления мощные визуализации и эффектов в приложения не игры.

Xamarin.Forms построена на основе собственного, специфический для платформы пользовательского интерфейса системы. Например [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) отображаются по-разному в iOS и Android и даже могут зависеть от версии операционной системы. Напротив CocosSharp не использует визуальных объектов любой платформой, поэтому все визуальные объекты выглядят одинаково на всех платформах. Конечно разрешение и пропорции различаются между устройствами, и это может повлиять на прорисовку CocosSharp его визуальные элементы. Эти сведения будут обсуждаться далее в этом руководстве.

Более подробные сведения можно найти в [CocosSharp раздел](~/graphics-games/cocossharp/index.md).

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>Добавление пакетов CocosSharp Nuget

Прежде чем использовать CocosSharp, разработчики должны дополнять несколько свой проект Xamarin.Forms.
В этом руководстве предполагается Xamarin.Forms проект с iOS, Android и PCL проекта.
Весь код будет записан в проект переносимой библиотеки Классов; Тем не менее библиотеки должны быть добавлены проекты iOS и Android.

Пакет CocosSharp Nuget содержит все объекты, необходимые для создания объектов CocosSharp.
Пакет nuget CocosSharp.Forms включает `CocosSharpView` класс, который используется для размещения CocosSharp в Xamarin.Forms.
Добавить **CocosSharp.Forms** NuGet и **CocosSharp** , будут автоматически добавляться также.
Для этого щелкните правой кнопкой мыши на PCL <span class="UIItem">пакетов</span> папку и выберите <span class="UIItem">Добавление пакетов... </span>. Введите условие поиска <span class="UIItem">CocosSharp.Forms</span>выберите <span class="UIItem">CocosSharp для Xamarin.Forms</span>, нажмите кнопку <span class="UIItem">добавить пакет</span>.

![](cocossharp-images/image1.png "Добавление пакетов диалоговое окно")

Оба **CocosSharp** и **CocosSharp.Forms** пакетов NuGet, которые будут добавлены в проект:

![](cocossharp-images/image2.png "Папка «пакеты»")

Повторите описанные выше шаги для проектов платформы (например, iOS и Android).

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>Пошаговое руководство: Добавление CocosSharp приложения Xamarin.Forms

Выполните следующие действия для добавления простого представления CocosSharp приложение Xamarin.Forms.

1. [Создание Xamarin Forms страницы](#1)
1. [Добавление CocosSharpView](#2)
1. [Создание GameScene](#3)
1. [Добавление окружности](#4)
1. [Взаимодействие с CocosSharp](#5)

После добавления представления CocosSharp успешно приложение Xamarin.Forms, посетите [CocosSharp документации](~/graphics-games/cocossharp/index.md) для получения дополнительных сведений о создании содержимого с CocosSharp.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. Создание Xamarin Forms страницы

CocosSharp может размещаться в любом контейнере Xamarin.Forms. В этом примере для данной страницы используется страница с именем `HomePage`. `HomePage` Разделение пополам по `Grid` демонстрирует, как Xamarin.Forms и CocosSharp может осуществляться одновременно на одной странице.

Во-первых, Настройка параметров страницы, поэтому он содержит `Grid` и два `Button` экземпляров:


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

На iOS `HomePage` отображается так, как показано на следующем рисунке:

![](cocossharp-images/image3.png "Снимок экрана домашней страницы")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. Добавление CocosSharpView

`CocosSharpView` Класс используется для внедрения в приложение Xamarin.Forms CocosSharp. Поскольку `CocosSharpView` наследует от [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) класса, он предлагает привычный интерфейс для макета, и он может использоваться внутри контейнеров макета например [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/). Добавьте новый `CocosSharpView` к проекту, выполнив `CreateTopHalf` метод:


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

Инициализация CocosSharp выполняется не мгновенно, поэтому зарегистрирует событие, когда `CocosSharpView` завершения его создания. Для этого в `HandleViewCreated` метод:


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` Метод имеет две важные сведения, которые будут рассмотрены на. Во-первых, `GameScene` класс, который будет создан в следующем разделе. Это важно отметить, что приложение не компилируются до `GameScene` создается и `gameScene` экземпляр ссылка разрешается.

Второй важные сведения о `DesignResolution` свойство, которое определяет игры в видимой области для CocosSharp объектов. `DesignResolution` Свойства будут рассмотрены после создания `GameScene`.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. Создание GameScene

`GameScene` Класс наследует от элемента CocosSharp `CCScene`. `GameScene` является первой точке, где мы имеем дело исключительно с CocosSharp. Код, содержащийся в `GameScene` будет функции во всех приложениях CocosSharp ли она размещается в рамках проекта Xamarin.Forms или нет.

`CCScene` Является корнем visual все CocosSharp отрисовки. Любой видимый CocosSharp объект должны находиться в пределах `CCScene`. В частности, необходимо добавить визуальных объектов `CCLayer` экземпляров, а также `CCLayer` экземпляры должны быть добавлены `CCScene`.

С помощью следующих диаграммы визуализировать типичный CocosSharp иерархии:

![](cocossharp-images/image4.png "Типичный CocosSharp иерархии")

Только один `CCScene` может быть активна одновременно. Большинство игр используйте несколько `CCLayer` экземпляров для сортировки контента, но наше приложение использует только один. Аналогичным образом большинство игр использовать несколько визуальных объектов, но у нас будет только один в нашем приложении. Более подробное обсуждение CocosSharp, можно найти в визуальной иерархии [Пошаговое руководство перебрасываются игры](~/graphics-games/cocossharp/first-game/index.md).

Изначально `GameScene` класса будет практически пустым, — мы создадим только его соответствие ссылка в `HomePage`. Добавьте новый класс в Переносимую `GameScene`. Он должен наследовать от `CCScene` следующим образом:


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

Теперь, когда `GameScene` будет определен, можно, направленных `HomePage` и добавьте поля:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

Теперь можно скомпилировать нашего проекта и запустите его, чтобы увидеть CocosSharp под управлением. Мы еще не добавили ничего для наших `GameScene,` поэтому черный — цвет по умолчанию это может быть CocosSharp в верхней части нашу страницу:

![](cocossharp-images/image5.png "Пустой GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. Добавление окружности

В настоящее время имеет запущенный экземпляр подсистемы CocosSharp Отображение пустой приложение `CCScene`. Далее мы добавим визуальный объект: круг. `CCDrawNode` Класс может использоваться для рисования различных геометрические фигуры, описанной в [Geometry рисования направляющей CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

Добавление окружности к нашей `GameScene` класса и создать его экземпляр в конструкторе, как показано в следующем коде:


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

Выполнение приложения теперь показаны круг в левой части области отображения CocosSharp:

![](cocossharp-images/image6.png "Кружок в GameScene")


#### <a name="understanding-designresolution"></a>Основные сведения о DesignResolution

Теперь, когда отображается визуального объекта CocosSharp, можно исследовать `DesignResolution` свойство.

`DesignResolution` Представляет ширину и высоту области CocosSharp по размещению и изменения размеров объектов. Фактическое разрешение области измеряется в *пикселей* при `DesignResolution` измеряется в мире *единицы*. На следующей диаграмме показаны различные части представления, которые отображаются в iPhone 5 с разрешением экрана 640 x 1136 пикселей разрешения:

![](cocossharp-images/image7.png "iPhone 5s разрешение разработки")

На приведенной выше схеме показаны измерения пикселей за пределами экрана черным шрифтом. Единицы измерения отображаются внутри диаграммы на белым текстом. Ниже приведены некоторые важные сведения, отображенный выше:

* Отображение CocosSharp находятся слева внизу. При перемещении вправо увеличивает значение X, и при перемещении вверх значения Y. Обратите внимание, что инвертировано значения Y по сравнению с некоторые другие механизмы двумерного макета, где (0,0) не верхней левой части холста.
* Поведение по умолчанию CocosSharp является пропорции его представления. Так как первую строку в сетке шире, чем больше высоты, CocosSharp не занимали всю ширину его ячейки с точками белый прямоугольник, как показано. Это поведение можно изменить, как описано в [руководство по обработке ряд разрешений в CocosSharp](~/graphics-games/cocossharp/resolutions.md).
* В этом примере CocosSharp сохранит 100 единиц ширину, высоту независимо от размера области отображения или пропорции его устройства. Это означает, что код можно предположить, что X = 100 представляет привязанный правом CocosSharp отобразить, сохранение макета согласованы для всех устройств.


#### <a name="ccdrawnode-details"></a>Сведения о CCDrawNode

Наш простое приложение использует `CCDrawNode` класс Рисование окружности. Этот класс может быть очень полезны для бизнес-приложений, так как она предоставляет отрисовки векторной geometry — это компонент, который отсутствует в Xamarin.Forms. Помимо круги `CCDrawNode` класс может использоваться для рисования прямоугольников, сплайны, линий и многоугольников пользовательских. `CCDrawNode` можно легко использовать, так как он не требует использования файлов изображений (.png). Более подробное рассмотрение CCDrawNode можно найти в [Geometry рисования направляющей CCDrawNode](~/graphics-games/cocossharp/ccdrawnode.md).

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. Взаимодействие с CocosSharp

CocosSharp визуальные элементы (такие как `CCDrawNode`) наследуют от `CCNode` класса. `CCNode` предоставляет два свойства, которые можно использовать для размещения объекта относительно его родительского элемента: `PositionX` и `PositionY`. Наш код в настоящее время использует следующие два свойства для размещения центра круга, как показано в этом фрагменте кода:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

Важно отметить, что CocosSharp объекты располагаются с явно указанными значениями позиции, в отличие от большинства Xamarin.Forms представлений, которые автоматически помещается согласно поведению их родительские элементы управления макета.

Мы добавим код, чтобы разрешить пользователю нажмите одну из двух кнопок для перемещения круга влево или вправо на 10 единиц (не в пикселях, так как Рисование окружности в мировом пространстве единицы, CocosSharp). Сначала мы создадим два открытых методов в `GameScene` класса:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

Далее мы добавим обработчики две кнопки в `HomePage` реакция на щелчок мыши. После окончания нашей `CreateBottomHalf` метод содержит следующий код:


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

В ответ на щелчок достигнутом CocosSharp круга. Мы также видит границы CocosSharp холста, переместив круг достаточное количество влево или вправо:

![](cocossharp-images/image8.png "GameScene с перемещением окружности")

## <a name="summary"></a>Сводка

В этом руководстве показано, как добавить существующие Xamarin.Forms CocosSharp проект, как создать взаимодействие между Xamarin.Forms и CocosSharp и рассматриваются различные вопросы при создании макетов в CocosSharp.

Ядро игры CocosSharp предоставляет множество функций и глубина, в этом руководстве далеко не CocosSharp действий. Разработчиков, заинтересованных в изучать дополнительные сведения о CocosSharp можно найти множество статей в [CocosSharp раздел](~/graphics-games/cocossharp/index.md).



## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы CocosSharp](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (пример)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
