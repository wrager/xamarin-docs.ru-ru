---
title: "API-интерфейсы игровой iOS"
description: "В этой статье рассматриваются усовершенствования игры, предоставляемые iOS 9, который может использоваться для улучшения графики игру Xamarin.iOS и звуковых функций."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6c735c68ee61032d8dfe3a74e6858bc058c6fa47
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="ios-gaming-apis"></a>API-интерфейсы игровой iOS

_В этой статье рассматриваются усовершенствования игры, предоставляемые iOS 9, который может использоваться для улучшения графики игру Xamarin.iOS и звуковых функций._

Apple улучшилась несколько технологий для игр API-интерфейсов на iOS 9, упрощающие процесс для реализации в приложении Xamarin.iOS игровой графики и аудио.
К ним относятся одновременно простота разработки до высокого уровня платформы и пользуясь возможностями устройства iOS GPU для повышения скорости и графические возможности.

[ ![](images/flocking01.png "Пример приложения под управлением или косяк")](images/flocking01.png)

Сюда входят вместе с новые расширенные функции исходного состояния системы, SceneKit и SpriteKit GameplayKit, ReplayKit, модель ввода-вывода, MetalKit и шейдеров производительности системы.

В этой статье приведены все способы повышения Xamarin.iOS игры с использованием новых усовершенствований игровой iOS 9 в:

## <a name="introducing-gameplaykit"></a>Знакомство с приложением GameplayKit

Новая платформа GameplayKit Apple предоставляет набор технологий, упрощающий создание игр для устройств iOS, уменьшая объем повторяющихся, общий код, необходимый для реализации. GameplayKit предоставляет инструменты для разработки игр механизм, можно затем легко объединяться с графический процессор (например, SceneKit или SpriteKit) для быстрого доставки завершения игры.

GameplayKit включает несколько распространенных, игра воспроизводить алгоритмы, такие как:

- Поведение основан на моделирование агента, которое позволяет определить перемещений и цели, которые будут автоматически применять AI.
- Minmax искусственный интеллект для основанных игры.
- Система правил для управляемых данными логику игры с Нечеткое обоснование для предоставления заняться поведения.

Кроме того GameplayKit принимает подход с использованием стандартных блоков для разработки игр с помощью модульную архитектуру, которая предоставляет следующие возможности:

- Конечный автомат для обработки сложных, процедурного кода на основе систем в игры.
- Средства для обеспечения случайного игры и непредсказуемые результаты без возникновения проблемы отладки.
- Архитектуру с возможностью повторного использования, разделенной на основе сущности.

Дополнительные сведения о GameplayKit см. в разделе Apple [руководство по программированию Gameplaykit](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) и [GameplayKit Framework ссылка](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199).

## <a name="gameplaykit-examples"></a>Примеры GameplayKit

Давайте кратко рассмотрим реализации некоторых механизм простой игры в приложения Xamarin.iOS, с помощью пакета игры.

### <a name="pathfinding"></a>Pathfinding

Pathfinding — это возможность AI элемент игры для поиска его направлении игрового поля.
Например, 2D enemy, поиск образом через Лабиринт или 3D символа с помощью поверхности Экшн-игра первый лица от world.

Рассмотрим следующие сопоставления:

[ ![](images/gkpathfindpath.png "Пример схемы pathfinding")](images/gkpathfindpath.png)

С помощью pathfinding этот код C# можно найти образом через схему:

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>Классический отчет о расходах

Следующий фрагмент кода C# показано, как GameplayKit можно использовать для реализации классического expert системы:

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

На основе заданного набора правил (`GKRule`) и известных набор входных данных, специалист системы (`GKRuleSystem`) создаст прогнозируемого результата (`fizzbuzz` для нашего примера выше).

### <a name="flocking"></a>Или косяк

Или косяк позволяет группу AI управляемых игры объектов вел себя как flock, где группе реагирует на перемещения и действия сущности как flock птички в полете или школы fish swimming интереса.

Следующий фрагмент кода C# реализует поведение или косяк, используя GameplayKit и SpriteKit для отображения графики:

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

Далее следует Реализуйте этот сцены в контроллере представления:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

При запуске немного анимированных _«Boids»_ будет flock вокруг нашей пальцем касания:

[ ![](images/flocking01.png "Немного анимированных Boids будет flock вокруг касания пальцем")](images/flocking01.png)

### <a name="other-apple-examples"></a>Другие примеры Apple

Дополнение к примерам, представленные выше Apple предоставляет следующие примеры приложений, которые могут быть перекодируется в C# и Xamarin.iOS:

- [FourInARow: С помощью GameplayKit Minmax специалист по разработке стратегий для сопернику AI](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: В системе агентов в GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: Создание игр межплатформенные с SpriteKit и GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

В iOS 9 Apple внес некоторые изменения и дополнения для исходного состояния системы для обеспечения доступа облегченной в GPU. С помощью исходного состояния системы можно повысить, графики и вычислительных возможностей приложения iOS.

Платформа исходного включает следующие новые функции:

- Новый закрытый и глубина трафарета текстур для OS X.
- Улучшенная тени качества с помощью глубина отсечение и отдельные передней и задней трафарета значения.
- Улучшения исходного языка заливки и стандартной библиотеки исходного состояния системы.
- Вычислительные шейдеры поддерживается более широкий диапазон форматов пикселей.

### <a name="the-metalkit-framework"></a>Платформа MetalKit

MetalKit framework предоставляет набор служебных классов и функций, уменьшить объем работ, необходимый для использования в приложении iOS исходного состояния системы. MetalKit обеспечивает поддержку в трех ключевых областях:

1. Загрузка из различных источников, включая распространенные форматы, такие как PNG, JPEG, KTX и PVR асинхронной текстуры.
2. Удобный доступ к модели ввода-вывода на основе активов для обработки исходного конкретной модели. Эти функции высокой оптимизированы для обеспечения эффективной передачи данных между сеток модели ввода-вывода и исходного буфера.
3. Представление управления значительно уменьшить объем кода, необходимого для отображения графических готовых для просмотра в приложение iOS и предопределенные исходного представления.

Дополнительные сведения о MetalKit см. в разделе Apple [ссылке платформы MetalKit](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [руководство по программированию исходного](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [ссылке платформы исходного](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) и [исходного состояния системы Руководство по языку заливки](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

### <a name="metal-performance-shaders-framework"></a>Шейдеры Framework исходного производительности

Шейдера производительности системы framework предоставляет набор оптимизирован графики и вычислений на основе шейдеры для использования в вашей системы на основе приложений iOS. Каждый шейдера в шейдер производительности системы framework специально настроен для обеспечения высокой производительности системы поддерживаются iOS GPU.

С помощью классов шейдера производительность системы может достижения максимальной производительности невозможно для каждого конкретного iOS GPU без необходимости выбирать и поддерживать отдельные базы. Шейдеры исходного производительности можно использовать и для любого исходного ресурса, например текстур и буферов.

Платформа шейдера производительности системы предоставляет набор общих построители текстуры, такие как:

- **Гауссовский размытия** (`MPSImageGaussianBlur`)
- **Определение краев Sobel** (`MPSImageSobel`)
- **Изображение гистограммы** (`MPSImageHistogram`)

Дополнительные сведения см. в разделе Apple [руководство по языку заливки исходного](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364).

## <a name="introducing-model-io"></a>Общие сведения о модели ввода-вывода

Framework модели ввода-вывода Apple предоставляет глубокого понимания 3D ресурсов (например, модели и их связанные ресурсы). Модель ввода-вывода предоставляет игры на основе физического материалов, моделей и освещения, который может использоваться с GameplayKit, системы и SceneKit операций ввода-вывода.

С помощью модели ввода-вывода может поддерживать следующие типы задач:

- Импорт освещения, материалов, сетка данных, параметров камеры и другие сведения с учетом сцены из различных форматов игры ядра и популярными программными средствами.
- Процесс или создать на основе сцены сведения, такие как с помощью процедур создания текстурой sky domes или распродажи освещение в сетку.
- Работает с MetalKit, SceneKit и GLKit для эффективной загрузки ресурсы игры в буферы GPU для подготовки к просмотру.
- Экспортируйте сведения с учетом сцены в различные популярные программного обеспечения и форматы игры ядра.

Дополнительные сведения о модели ввода-вывода см. в разделе Apple [Справочник Framework модели ввода-вывода](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>Знакомство с приложением ReplayKit

Новая платформа ReplayKit Apple можно легко добавить запись игры в игру операций ввода-вывода и позволяют быстро и легко изменить и совместно использовать видеоролик в приложении.

Дополнительные сведения см. в разделе Apple [переходом социальных сетей, с видео ReplayKit и Game Center](https://developer.apple.com/videos/wwdc/2015/?id=605) и их [DemoBots: создание кросс-платформенной игры с SpriteKit и GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) пример приложения.

## <a name="scenekit"></a>SceneKit

Набор сцены — 3D-сцену graph API, который упрощает работу с трехмерной графикой. Впервые появившийся в OS X 10.8 и теперь пришло iOS 8. Комплект сцены Создание случайного трехмерных игр и впечатляющие трехмерной визуализации не требуется опыт в OpenGL. Основываясь на основные понятия граф сцены, комплект сцены устраняет сложности OpenGL и OpenGL ES, поскольку его легко добавить трехмерные содержимого в приложение. Тем не менее если вы не являетесь специалистом OpenGL, набор сцены имеет отличную поддержку для привязки непосредственно с также OpenGL. Также включает множество функций, которые дополняют трехмерной графики, такие как физический и хорошо интегрируется с несколькими другими средами Apple, как основной анимации, образ Core и Sprite Kit.

Дополнительные сведения см. в разделе нашей [SceneKit](~/ios/platform/gaming/scenekit.md) документации.

### <a name="scenekit-changes"></a>SceneKit изменения

Apple добавлены следующие новые функции SceneKit для iOS 9:

- Xcode теперь предоставляет сцены редактор, который позволяет быстро создавать игры и интерактивных трехмерных приложениях путем редактирования сцен непосредственно из в Xcode.
- `SCNView` И `SCNSceneRenderer` классы могут использоваться для реализации исходного отрисовки (на устройствах iOS поддерживается).
- `SCNAudioPlayer` И `SCNNode` классы могут использоваться для добавления пространственных аудио эффекты, которые автоматически отслеживать позицию проигрыватель приложения iOS.

Дополнительные сведения см. в разделе нашей [документации SceneKit](~/ios/platform/introduction-to-ios8.md#scenekit) и Apple [ссылке платформы SceneKit](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) и [Fox: создание игр с помощью Xcode сцены редактораSceneKit](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)образца проекта.

## <a name="spritekit"></a>SpriteKit

Комплект Sprite, платформа игр 2D Apple, имеет некоторые интересные новые возможности в iOS 8 и OS X Yosemite. К ним относятся интеграция с Kit сцены, поддержка построителя текстуры, освещения, теней, ограничения, создания карты нормалей и улучшения физических. В частности новые возможности физических сделать очень легко включить реалистичных эффектов в игру.

Дополнительные сведения см. в разделе нашей [SpriteKit](~/ios/platform/gaming/spritekit.md) документации.

### <a name="spritekit-changes"></a>SpriteKit изменения

Apple добавлены следующие новые функции SpriteKit для iOS 9:

- Пространственные аудио эффект, который автоматически отслеживать позицию проигрыватель с `SKAudioNode` класса.
- Xcode поворачиваются сцены редактор и редактор действий для упрощенного создания двухмерной игры и приложения.
- Просто прокрутки игры поддержка с помощью новых узлов камеры (`SKCameraNode`) объектов.
- На устройствах iOS, поддерживающих исходного состояния системы SpriteKit будет автоматически использовать для подготовки к просмотру, даже если вы уже использовали пользовательские шейдеров OpenGL ES.

Дополнительные сведения см. в разделе нашей [документации SpriteKit](~/ios/platform/introduction-to-ios8.md#spritekit) Apple [ссылке платформы SpriteKit](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) и их [DemoBots: создание кросс-платформенной игры с SpriteKit и GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) пример приложения.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован новой функции игры, iOS 9, предоставляемые для Xamarin.iOS приложений.
Он появился GameplayKit и модель ввода-вывода; основные усовершенствования системы; и новые функции SceneKit и SpriteKit.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
