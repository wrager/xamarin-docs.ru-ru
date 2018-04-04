---
title: С помощью UrhoSharp
description: Обзор подсистемы UrhoSharp
ms.prod: xamarin
ms.assetid: D9BEAD83-1D9E-41C3-AD4B-3D87E13674A0
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: cdb32c0fe9aa1a267bda5768b9026667723d694c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="using-urhosharp"></a>С помощью UrhoSharp

_Обзор подсистемы UrhoSharp_

Перед тем как записывать первой игры, необходимо получить для начала ознакомьтесь с основами: Настройка сцены, загрузка ресурсов (содержит иллюстрации) и как создать простое взаимодействие для игры.

<a name="scenenodescomponentsandcameras"/>

# <a name="scenes-nodes-components-and-cameras"></a>Сцен, узлы, компоненты и камеры

Модели сцены можно описать как граф на основе компонентов сцены. Сцены состоит из иерархии узлов сцены, начиная с корневого узла, представляющего всей сцены. Каждый [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/) имеет 3D-преобразование (позиции, поворот и масштабирование), имя, идентификатор, а также произвольное число компонентов.  Компоненты воплощать узла, они могли добавить визуальное представление ([`StaticModel`](https://developer.xamarin.com/api/type/Urho.StaticModel)), они могут выдавать звука ([`SoundSource`](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource)), они предоставляют границе конфликтов и т. д.

Можно создать сцен и настройка узлов с помощью [Urho редактор](#UrhoEditor), или можно выполнить действия в коде C#.  В этом документе мы рассмотрим действия параметр посредством кода, как они показывают элементы, необходимые для действия, которые можно отобразить на экране

В дополнение к настройке сцены, необходимо установить [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera/), это то, что определяет, что можно получить отображаемые для пользователя.

## <a name="setting-up-your-scene"></a>Настройка сцены

Как правило создаются этой формы метод Start:

```csharp
var scene = new Scene ();
// Create the Octree component to the scene. This is required before
// adding any drawable components, or else nothing will show up. The
// default octree volume will be from -1000, -1000, -1000) to
//(1000, 1000, 1000) in world coordinates; it is also legal to place
// objects outside the volume but their visibility can then not be
// checked in a hierarchically optimizing manner
scene.CreateComponent<Octree> ();
// Create a child scene node (at world origin) and a StaticModel
// component into it. Set the StaticModel to show a simple plane mesh
// with a "stone" material. Note that naming the scene nodes is
// optional. Scale the scene node larger (100 x 100 world units)
var planeNode = scene.CreateChild("Plane");
planeNode.Scale = new Vector3 (100, 1, 100);
var planeObject = planeNode.CreateComponent<StaticModel> ();
planeObject.Model = ResourceCache.GetModel ("Models/Plane.mdl");
planeObject.SetMaterial(ResourceCache.GetMaterial("Materials/StoneTiled.xml"));
```

## <a name="components"></a>Компоненты

Отрисовку трехмерных объектов, воспроизведение звука, физических и скрипт логики обновления они все включены путем создания различных компонентов в узлы путем вызова [ `CreateComponent<T>()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateComponent%3CT%3E/p/Urho.CreateMode/System.UInt32/).  Например установки, узел и светлой компонент следующим образом:

```csharp
// Create a directional light to the world so that we can see something. The
// light scene node's orientation controls the light direction; we will use
// the SetDirection() function which calculates the orientation from a forward
// direction vector.
// The light will use default settings (white light, no shadows)
var lightNode = scene.CreateChild("DirectionalLight");
lightNode.SetDirection (new Vector3(0.6f, -1.0f, 0.8f));
```

Мы создали над узлом с именем «`DirectionalLight`» и задать направление письма для его, но ничего более.  Теперь вернемся выше узла в узел, создающие свет, присоединив отладчик [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light/) компонента, с `CreateComponent`:

```csharp
var light = lightNode.CreateComponent<Light>();
```

Компоненты, созданные в `Scene` себе имеет специальную роль: для реализации функций сцены. Они должны создаваться перед всеми другими компонентами и включают следующее:

* [`Octree`](https://developer.xamarin.com/api/type/Urho.Octree/): реализует пространственных секционирование и accelerated видимость запросов. Без этого трехмерные объекты не могут быть представлены.
* [`PhysicsWorld`](https://developer.xamarin.com/api/type/Urho.Physics.PhysicsWorld/): реализует физическое моделирование. Физических компонентов, таких как [ `RigidBody` ](https://developer.xamarin.com/api/type/Urho.Physics.RigidBody/) или [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape/) может работать неправильно без этого.
* [`DebugRenderer`](https://developer.xamarin.com/api/type/Urho.DebugRenderer/): реализует отладки geometry подготовки к просмотру.

Обычные компоненты, такие как [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light), [ `Camera` ](https://developer.xamarin.com/api/type/Urho.Camera) или [ `StaticModel` ](https://developer.xamarin.com/api/type/Urho.StaticModel) не следует создавать непосредственно в [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Scene), но вместо этого в дочерних узлов.

Библиотеке включает широкий набор компонентов, которые можно подключить с узлами, их воплощать: видимыми элементами (модели), звуки, жесткая тел, конфликт фигур, камеры, источники света, передатчики примитивов и многое другое.

## <a name="shapes"></a>Фигуры

Для удобства как простой узлы в пространстве имен Urho.Shapes доступны различные фигуры.  К ним относятся поля, сферы, конусы, цилиндров и плоскости.

## <a name="camera-and-viewport"></a>Камера и просмотра

Так же, как свет, камеры являются компонентами, поэтому необходимо подключение к узлу компонента, это делается следующим образом:

```csharp
var CameraNode = scene.CreateChild ("camera");
camera = CameraNode.CreateComponent<Camera>();
CameraNode.Position = new Vector3 (0, 5, 0);
```

Таким образом, вы создали веб-камеры и помещен в мире 3D камеры, следующим шагом является для информирования `Application` , это камеры, который вы хотите использовать, это делается с помощью следующего кода:

```csharp
Renderer.SetViewPort (0, new Viewport (Context, scene, camera, null))
```

А теперь должны иметь возможность видеть результаты на создание.

## <a name="identification-and-scene-hierarchy"></a>Идентификация и сцены иерархии

В отличие от узлов компоненты не имеют имен; только компонентами в том же узле идентифицируются по их типу и индекс в списке компонентов узла, который заполняется порядок создания, например, вы можете получить [ `Light` ](https://developer.xamarin.com/api/type/Urho.Light) компонента из `lightNode` объекта над следующим образом:

```csharp
var myLight = lightNode.GetComponent<Light>();
```

Можно также получить список всех компонентов, получая [ `Components` ](https://developer.xamarin.com/api/property/Urho.Node.Components/) свойство, которое возвращает `IList<Component>` , который можно использовать.

При создании узлов и компонентов получить глобальный сцены целочисленных идентификаторов. Они могут запрашиваться из сцены с помощью функций [ `GetNode(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetNode/p/System.UInt32/) и [ `GetComponent(uint id)` ](https://developer.xamarin.com/api/member/Urho.Scene.GetComponent/p/System.UInt32/). Это намного быстрее, чем например рекурсивные запросы сцены на основе имени узла.

Отсутствует понятие встроенные сущности или объекта игры; а в том на программиста выбрать иерархию узлов и узлы которого для размещения логики сценариев. Как правило свободное перемещение объектов в мире 3D будут создаваться как дочерние элементы корневого узла. Узлы могут создаваться с указанием или без имени с помощью [ `CreateChild()` ](https://developer.xamarin.com/api/member/Urho.Node.CreateChild/p/System.String/Urho.CreateMode/System.UInt32/). Не обеспечивается уникальность имен узлов.

При наличии некоторых иерархических композиции, его рекомендуется (и на самом деле необходимо, поскольку компоненты не имеют своих собственных трехмерных преобразований) для создания дочернего узла.

Например, если символ удержание объекта в его вручную, объект должен быть собственный узел, который будет принадлежать к костей Рука символа (также [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node/)).  Исключением является физических [ `CollisionShape` ](https://developer.xamarin.com/api/type/Urho.Physics.CollisionShape), который может быть offsetted и поворачивать по отдельности относительно узла.

Обратите внимание, что [ `Scene` ](https://developer.xamarin.com/api/type/Urho.Node/)его владельцем преобразования намеренно учитывается для оптимизации при вычислении преобразования производных world дочерних узлов, поэтому изменение не оказывает влияния и его следует оставить как (место происхождения, отсутствие вращения без масштабирования.)

[`Scene`](https://developer.xamarin.com/api/type/Urho.Node/) узлы могут свободно изменении родителя. В отличие от компонентов всегда принадлежит к узлу, назначенные им не могут перемещаться между узлами. Узлы и компоненты предоставляют [ `Remove()` ](https://developer.xamarin.com/api/member/Urho.Node.Remove()/) функции для выполнения этой задачи без необходимости прохождения через родителя. После удаления узла после вызова этой функции могут использоваться никакие операции на узле или рассматриваемого компонента.

Также можно создать `Node` , не принадлежит сцены. Это полезно, например с помощью камеры, перемещение в сцене, может загрузить или сохранить, поскольку затем камеры, не будут сохранены вместе с фактическое сцены, а не уничтожаются при загрузке сцены. Однако обратите внимание, что создание компонентов geometry, физических или сценарий на незакрепленный узел и переместите его сцены позже вызовет эти компоненты не будут работать правильно.

## <a name="scene-updates"></a>Обновления сцены

Это может быть, обновления которого разрешены (по умолчанию) автоматически обновляются при каждой итерации основного цикла.  Приложения [ `SceneUpdate` ](https://developer.xamarin.com/api/event/Urho.Scene.SceneUpdate/) в нем вызывается обработчик события.

Узлы и компоненты можно исключить из обновления сцены, отключив их см. в разделе [ `Enabled` ](https://developer.xamarin.com/api/member/Urho.Node.Enabled).  Поведение зависит от указанного компонента, но, например выключение drawable компонент также делает его невидимым, во время отключения звука исходный компонент выключает его. Если узел отключен, все ее компоненты рассматриваются как отключенный независимо от состояния включения/отключения.

# <a name="adding-behavior-to-your-components"></a>Добавление поведения в компонентах

Для структуры игру рекомендуется сделать собственный компонент, инкапсулировать субъекта или элемент на игру.  Это делает компонент автономные, из ресурсы, используемые для отображения его поведение.

Для использования действий, являющиеся инструкции можно поместить в очередь и соединить его с асинхронного программирования на C# — самый простой способ добавления поведения в компонент.  Это позволяет определить поведение компонент может быть очень высоким уровнем и делает его проще понять, что произошло.

Кроме того можно управлять точно что происходит в компонент путем обновления свойств компонента в каждом кадре (см. в разделе поведения, основанного на кадр).

## <a name="actions"></a>Действия

Можно добавить поведение к узлам, очень легко с помощью действия.  Действия можно изменять различные свойства узла и выполнять их за период времени или повторять их несколько раз с кривой данной анимации.

Например, рассмотрим узел «облака» или сцены, эффект затухания его следующим образом:

```csharp
await cloud.RunActionsAsync (new FadeOut (duration: 3))
```

Действия являются неизменяемыми объектов, которые позволяет повторно использовать действия для управления различными объектами.

Создайте действие, которое выполняет обратную операцию является распространенных случаях:

```csharp
var gotoExit = new MoveTo (duration: 3, position: exitLocation);
var return = gotoExit.Reverse ();
```

Следующий пример бы затухание объект автоматически в течение 3 секунд.  Можно также запустить одно действие за другим, например, можно сначала переместить облака и скрыть его:

```csharp
await cloud.RunActionsAsync (
    new MoveBy  (duration: 1.5f, position: new Vector3(0, 0, 15),
    new FadeOut (duration: 3));
```

Если требуется, чтобы оба действия должно выполняться в то же время, можно использовать параллельные действия и предоставления всех действий, которые нужно сделать в параллельном режиме:

```csharp
  await cloud.RunActionsAsync (
    new Parallel (
      new MoveBy  (duration: 3, position: new Vector3(0, 0, 15),
      new FadeOut (duration: 3)));
```

В приведенном выше примере облаке перемещения и Исчезание, в то же время.

Обратите внимание, что они используют C# ожидать, что позволяет вам понять поведение, которое требуется для обеспечения линейно.

## <a name="basic-actions"></a>Основные операции

Ниже приведены действия, поддерживаемые в UrhoSharp:

* Перемещение узлов: [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo), [ `MoveBy` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveBy), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `BezierTo` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierTo), [ `BezierBy` ](https://developer.xamarin.com/api/type/Urho.Actions.BezierBy) , [`JumpTo`](https://developer.xamarin.com/api/type/Urho.Actions.JumpTo), [`JumpBy`](https://developer.xamarin.com/api/type/Urho.Actions.JumpBy)
* Поворот узлов: [ `RotateTo` ](https://developer.xamarin.com/api/type/Urho.Actions.RotateTo), [`RotateBy`](https://developer.xamarin.com/api/type/Urho.Actions.RotateBy)
* Масштабирование узлов: [ `ScaleTo` ](https://developer.xamarin.com/api/type/Urho.Actions.ScaleTo), [`ScaleBy`](https://developer.xamarin.com/api/type/Urho.Actions.ScaleBy)
* Эффект постепенного увеличения узлов: [ `FadeIn` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeIn), [ `FadeTo` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeTo), [ `FadeOut` ](https://developer.xamarin.com/api/type/Urho.Actions.FadeOut), [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [`Blink`](https://developer.xamarin.com/api/type/Urho.Actions.Blink)
* Оттенков: [ `TintTo` ](https://developer.xamarin.com/api/type/Urho.Actions.TintTo), [`TintBy`](https://developer.xamarin.com/api/type/Urho.Actions.TintBy)
* Моменты: [ `Hide` ](https://developer.xamarin.com/api/type/Urho.Actions.Hide), [ `Show` ](https://developer.xamarin.com/api/type/Urho.Actions.Show), [ `Place` ](https://developer.xamarin.com/api/type/Urho.Actions.Place), [ `RemoveSelf` ](https://developer.xamarin.com/api/type/Urho.Actions.RemoveSelf), [`ToggleVisibility`](https://developer.xamarin.com/api/type/Urho.Actions.ToggleVisibility)
* Циклы: [ `Repeat` ](https://developer.xamarin.com/api/type/Urho.Actions.Repeat), [ `RepeatForever` ](https://developer.xamarin.com/api/type/Urho.Actions.RepeatForever), [`ReverseTime`](https://developer.xamarin.com/api/type/Urho.Actions.ReverseTime)

Другие дополнительные функции включают сочетание [ `Spawn` ](https://developer.xamarin.com/api/type/Urho.Actions.Spawn) и [ `Sequence` ](https://developer.xamarin.com/api/type/Urho.Actions.Sequence) действия.

## <a name="easing---controlling-the-speed-of-your-actions"></a>Упрощение - управления скоростью действий

Упрощение — это способ, указывающий способ будет развертывать анимацию, что может сделать более удобную анимации.  По умолчанию действия будут иметь линейной поведение, например [ `MoveTo` ](https://developer.xamarin.com/api/type/Urho.Actions.MoveTo) действие будет иметь очень автоматической перемещения.  Можно создать оболочку действия в действие замедление для изменения поведения, например, задача, которая будет медленно начать перемещение, ускоряющих и медленно окончанием ([`EasyInOut`](https://developer.xamarin.com/api/type/Urho.Actions.EasyInOut)).

Это можно сделать путем заключения существующее действие в действие реалистичной анимации, например:

```csharp
await cloud.RunActionAsync (
   new EaseInOut (
     new MoveTo (duration: 3, position: new Vector (0,0,15)), rate:1))
```

Существует много плавности режима, в следующей таблице представлены различные типы реалистичной анимации и их поведение на значение объекта, к которому они осуществляется управление за период времени, от начала до конца.

![Упрощение режимы](using-images/easing.png "на этой диаграмме показано плавности различных типов и их поведение на значение объекта, они также осуществляется управление за период времени")

## <a name="using-actions-and-async-code"></a>Использование действий и асинхронного кода

В вашей [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component/) подкласс, следует вызвать асинхронный метод, который подготавливает поведение компонента и повышает функциональные возможности для него.
Затем можно будет вызвать этот метод, с помощью C# `await` ключевое слово из другой части программы, либо к `Application.Start` метода или в ответ на момент пользователя или истории в приложении.

Пример:

```csharp
class Robot : Component {
    public bool IsAlive;
    async void Launch ()
    {
        // Dress up our robot
        var cache = Application.ResourceCache;
        var model = node.CreateComponent<StaticModel>();
        model.Model = cache.GetModel ("robot.mdl"));
        model.SetMaterial (cache.GetMaterial ("robot.xml"));
        Node.SetScale (1);

        // Bring the robot into our scene
        await Node.RunActionsAsync(
            new MoveBy(duration: 0.6f, position: new Vector3(0, -2, 0)));
        // Make him move around to avoid the user fire
        MoveRandomly(minX: 1, maxX: 2, minY: -3, maxY: 3, duration: 1.5f);
        // And simultaneously have him shoot at the user
        StartShooting();
    }

    protected async void MoveRandomly (float minX, float maxX,
                                       float minY, float maxY,
                       float duration)
    {
        while (IsAlive){
            var moveAction = new MoveBy(duration,
                new Vector3(RandomHelper.NextRandom(minX, maxX),
                            RandomHelper.NextRandom(minY, maxY), 0));
            await Node.RunActionsAsync(moveAction, moveAction.Reverse());
        }
    }
    protected async void StartShooting()
    {
        while (IsAlive && Node.Components.Count > 0){
            foreach (var weapon in Node.Components.OfType<Weapon>()){
                await weapon.FireAsync(false);
                if (!IsAlive)
                    return;
            }
            await Node.RunActionsAsync(new DelayTime(0.1f));
        }
    }
}
```

В `Launch` запускаются метод выше трех действий: робота поступления в сцене, это действие приведут к изменению расположения узла за период 0,6 секунды.  Так как это параметр async этого одновременно как следующую инструкцию, которая представляет собой вызов к `MoveRandomly`.  Этот метод будет изменить положение робота параллельно в произвольное расположение.  Это достигается путем выполнения двух действий комплексная, перемещения в новое расположение и вернуться к оригиналу размещения и повторите это действие до тех пор, пока робота остается активным.  И, чтобы упростить более интересным, робота будет хранить устранение одновременно.  Устранение будет загружаться только каждые 0,1 сек.

## <a name="frame-based-behavior-programming"></a>Программирование поведения, основанного на кадр

Если вы хотите управлять поведением компонента на основе фрейм за фреймом вместо использования действия, что следует делать является переопределение [ `OnUpdate` ](https://developer.xamarin.com/api/member/Urho.Component.OnUpdate) метод вашей [ `Component` ](https://developer.xamarin.com/api/type/Urho.Component) подкласс.  Этот метод вызывается один раз за кадр и вызывается только в том случае, если свойство ReceiveSceneUpdates задано значение true.

В следующем примере показано создание `Rotator` компонент, который затем подключается к узлу, который вызывает узел, чтобы повернуть:

```csharp
class Rotator : Component {
    public Rotator()
    {
        ReceiveSceneUpdates = true;
    }
    public Vector3 RotationSpeed { get; set; }
    protected override void OnUpdate(float timeStep)
    {
        Node.Rotate(new Quaternion(
            RotationSpeed.X * timeStep,
            RotationSpeed.Y * timeStep,
            RotationSpeed.Z * timeStep),
            TransformSpace.Local);
    }
}
```

И вот как этот компонент будет подключение к узлу:

```csharp
Node boxNode = new Node();
var rotator = new Rotator() { RotationSpeed = rotationSpeed };
boxNode.AddComponent (rotator);
```

## <a name="combining-styles"></a>Объединение стилей

Можно использовать async и действия на основе модели программирования большую часть поведения, который отлично подходит для выстрелил и забыл стиль программирования, но можно также настроить, поведение компонента также выполнять определенный код обновления в каждом кадре.

Например, во время демонстрации SamplyGame используется в `Enemy` класс кодирует действия использует базовое поведение, но она также гарантирует, что компоненты указывают к пользователю, задав направление узла с `Node.LookAt`:

```csharp
    protected override void OnUpdate(SceneUpdateEventArgs args)
    {
        Node.LookAt(
            new Vector3(0, -3, 0),
            new Vector3(0, 1, -1),
            TransformSpace.World);
        base.OnUpdate(args);
    }
```

# <a name="loading-and-saving-scenes"></a>Загрузка и сохранение сцены

Сцены можно загрузить и сохранить в формате XML. просмотреть функции [ `LoadXml` ](https://developer.xamarin.com/api/member/Urho.Scene.LoadXml) и [ `SaveXML()` ](https://developer.xamarin.com/api/member/Urho.Scene.SaveXml). При загрузке сцены сначала удаляется все существующее содержимое (дочерние узлы и компоненты). Узлы и компоненты, помеченные как временные с `Temporary` свойства не будут сохранены. Сериализатор обрабатывает все встроенные компоненты и свойства, но не является достаточно широкими возможностями для обработки пользовательских свойств и полей, определенных в подклассах вашего компонента. Однако он предоставляет два виртуальных метода для этого:

* [`OnSerialize`](https://developer.xamarin.com/api/member/Urho.Component.OnSerialize) где можно зарегистрировать вы пользовательских состояний для сериализации

* [`OnDeserialized`](https://developer.xamarin.com/api/member/Urho.Component.OnDeserialize) где можно получить ваш сохраненные пользовательские состояния.

Как правило пользовательский компонент будет выглядеть следующим образом:

```csharp
class MyComponent : Component {
    // Constructor needed for deserialization
    public MyComponent(IntPtr handle) : base(handle) { }
    public MyComponent() { }
    // user defined properties (managed state):
    public Quaternion MyRotation { get; set; }
    public string MyName { get; set; }

    public override void OnSerialize(IComponentSerializer ser)
    {
        // register our properties with their names as keys using nameof()
        ser.Serialize(nameof(MyRotation), MyRotation);
        ser.Serialize(nameof(MyName), MyName);
    }

    public override void OnDeserialize(IComponentDeserializer des)
    {
        MyRotation = des.Deserialize<Quaternion>(nameof(MyRotation));
        MyName = des.Deserialize<string>(nameof(MyName));
    }
    // called when the component is attached to some node
    public override void OnAttachedToNode()
    {
        var node = this.Node;
    }
}
```

## <a name="object-prefabs"></a>Объект Prefabs

Просто загрузки или сохранения всей сцены не является достаточной гибкостью для игр где новые объекты должны создаваться динамически. С другой стороны создавая сложные объекты и устанавливая их свойства в коде также будет утомительным. По этой причине можно также сохранить узел сцены, включающие его дочерние узлы, компоненты и атрибуты. Их можно загрузить позже удобным образом как группа.  Сохраненный объект часто называется prefab. Это можно сделать тремя способами:

- В коде путем вызова [ `Node.SaveXml` ](https://developer.xamarin.com/api/member/Urho.Node.SaveXml) на узле
- В редакторе, при выборе узла в окне "Иерархия" и выбрав команду «Сохранить узел как» в меню «Файл».
- С помощью команды «узла» в `AssetImporter`, которой будет сохранен в иерархии узлов сцены и всех моделей, содержащихся во входном активе (например) файл Collada)

Чтобы создать сохраненный узел сцены, вызовите [ `InstantiateXml()` ](https://developer.xamarin.com/api/member/Urho.Scene.InstantiateXml). Данный узел будет создан в качестве дочернего элемента сцены, но может быть свободно изменении родителя после этого. Положения и поворота для размещения узла должны быть указаны. В следующем коде показано, как создать экземпляр prefab `Ninja.xm` в сцену с требуемой положения и поворота:

```csharp
var prefabPath = Path.Combine (FileSystem.ProgramDir,"Data/Objects/Ninja.xml");
using (var file = new File(Context, prefabPath, FileMode.Read))
{
    scene.InstantiateXml(file, desiredPos, desiredRotation,
        CreateMode.Replicated);
}
```

# <a name="events"></a>События

UrhoObjects привести к возникновению ряда событий, они отображаются как события C# в различных классах, которые создают им.  В дополнение к C#-модели на основе событий, можно также использовать `SubscribeToXXX` методы, которые можно подписаться и хранить токен подписки, можно позже использовать для отмены подписки.  Различие заключается в первый позволит много вызывающие объекты для подписки, во время второй только позволяет одному, а также позволяет лучше стиле лямбда-выражения подход для использования, а еще, позволяют легко удалять подписки.  Они являются взаимоисключающими.

При оформлении подписки на событие, необходимо указать метод, который принимает в качестве аргумента с аргументами соответствующее событие.

Например Вот как подписаться на событие нажатия кнопки мыши.

```csharp
public void override Start ()
{
    UI.MouseButtonDown += HandleMouseButtonDown;
}

void HandleMouseButtonDown(MouseButtonDownEventArgs args)
{
    Console.WriteLine ("button pressed");
}
```

Стилем лямбда-выражения:

```csharp
public void override Start ()
{
    UI.MouseButtonDown += args => {
        Console.WriteLine ("button pressed");
    };
}
```

Иногда требуется прекратить получение уведомлений для события, в таких случаях следует сохранить возвращаемое значение вызова `SubscribeTo` метода и вызвать метод Unsubscribe:

```csharp
Subscription mouseSub;

public void override Start ()
{
    mouseSub = UI.SubscribeToMouseButtonDown (args => {
    Console.WriteLine ("button pressed");
      mouseSub.Unsubscribe ();
    };
}
```

Параметр, полученный обработчиком событий является класса строго типизированные события, который будет различаться для каждого события и содержит полезные данные события.

# <a name="responding-to-user-input"></a>Отвечать на действия пользователя

Можно подписаться на различные события, например нажатий клавиш вниз подписаться на событие и реагирование на доставляются входные данные:

```csharp
Start ()
{
    UI.KeyDown += HandleKeyDown;
}

void HandleKeyDown (KeyDownEventArgs arg)
{
     switch (arg.Key){
     case Key.Esc: Engine.Exit (); return;
}
```

Однако во многих сценариях необходимо обработчиков обновления сцены для проверки на текущем состоянии ключей при они обновляются и соответствующим образом обновите ваш код.  Например ниже можно использовать для обновления камеры расположение, в зависимости от клавиатуры:

```csharp
protected override void OnUpdate(float timeStep)
{
    Input input = Input;
    // Movement speed as world units per second
    const float moveSpeed = 4.0f;
    // Read WASD keys and move the camera scene node to the
    // corresponding direction if they are pressed
    if (input.GetKeyDown(Key.W))
        CameraNode.Translate(Vector3.UnitY * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.S))
        CameraNode.Translate(new Vector3(0.0f, -1.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.A))
        CameraNode.Translate(new Vector3(-1.0f, 0.0f, 0.0f) * moveSpeed * timeStep, TransformSpace.Local);
    if (input.GetKeyDown(Key.D))
        CameraNode.Translate(Vector3.UnitX * moveSpeed * timeStep, TransformSpace.Local);
}
```

# <a name="resources-assets"></a>Ресурсы (средства)

Ресурсы включают для большинства задач UrhoSharp, которые загружаются из запоминающего устройства во время инициализации или среды выполнения:

- [`Animation`](https://developer.xamarin.com/api/type/Urho.Animation/) -используется для базовой анимации
- [`Image`](https://developer.xamarin.com/api/type/Urho.Resources.Image) -Представляет изображения, хранимые в различные форматы рисунков
- [`Model`](https://developer.xamarin.com/api/type/Urho.Model/) -3D-моделей
- [`Material`](https://developer.xamarin.com/api/type/Urho.Material) -материалы, используемые для отображения моделей.
- [`ParticleEffect`](https://developer.xamarin.com/api/type/Urho.ParticleEffect)- [Описывает](http://urho3d.github.io/documentation/1.4/_particles.html) работа передатчика примитив. в разделе «[частиц](#particles)» ниже.
- [`Shader`](https://developer.xamarin.com/api/type/Urho.Shader) -пользовательских шейдеров
- [`Sound`](https://developer.xamarin.com/api/type/Urho.Audio.Sound) -звуки воспроизведения, в разделе «[звук](#sound)» ниже.
- [`Technique`](https://developer.xamarin.com/api/type/Urho.Technique/) -методы материала отрисовки
- [`Texture2D`](https://developer.xamarin.com/api/type/Urho.Urho2D.Texture2D/) -Двухмерной текстуры
- [`Texture3D`](https://developer.xamarin.com/api/type/Urho.Texture3D/) -3D-текстуры
- [`TextureCube`](https://developer.xamarin.com/api/type/Urho.TextureCube/) -Cube текстуры
- `XmlFile`

Они управляются и загруженных [ `ResourceCache` ](https://developer.xamarin.com/api/type/Urho.Resources.ResourceCache/) подсистемы (доступной в качестве [ `Application.ResourceCache` ](https://developer.xamarin.com/api/property/Urho.Application.ResourceCache/)).

Самих ресурсах идентифицируются по их пути к файлам относительно зарегистрированных ресурсов каталоги или файлы пакета. По умолчанию ядро регистрирует каталоги ресурсов `Data` и `CoreData`, или пакеты `Data.pak` и `CoreData.pak` , если они существуют.

При неудачной загрузке ресурса, в журнал, и возвращается пустая ссылка.

Следующий пример показывает типичный способ получения ресурса из кэша ресурса.  В этом случае текстуры для элемента пользовательского интерфейса, этот механизм использует `ResourceCache` свойство из `Application` класса.

```csharp
healthBar.SetTexture(ResourceCache.GetTexture2D("Textures/HealthBarBorder.png"));
```

Ресурсы можно создавать вручную и сохранена в кэш ресурсов, как если бы они были загружены из диска.

Можно задать бюджеты памяти каждого типа ресурсов: Если ресурсы потребляют больше памяти, чем разрешено, самая старая ресурсы будут удалены из кэша, если не используется больше. По умолчанию бюджеты памяти присваивается неограниченное.

## <a name="bringing-3d-models-and-images"></a>Перевод 3D-модели и изображений

Urho3D пытается использовать существующие форматы файлов, когда это возможно и определить настраиваемые форматы файлов только при крайней необходимости, например для моделей (*.mdl) и для анимации (*.ani). Для этих типов средств Urho предоставляет преобразователь - [AssetImporter](http://urho3d.github.io/documentation/1.4/_tools.html) которых требует многие популярные 3D форматы, такие как fbx, dae, 3ds и obj, и т. д.

Имеется также удобно надстройки для Blender [ https://github.com/reattiva/Urho3D-Blender ](https://github.com/reattiva/Urho3D-Blender) , можно экспортировать в формате, который подходит для Urho3D Blender активов.

## <a name="background-loading-of-resources"></a>Фоновую загрузку ресурсов

Как правило при запросе с помощью одного из ресурсов `ResourceCache` `Get` метода, они загружаются непосредственно в основном потоке, что может занять несколько миллисекунд для всех необходимых действий (загрузить файл с диска, синтаксический анализ данных, передача GPU, при необходимости ) и может привести падения частоты кадров.

Если известно заранее какие ресурсы необходимо, можно запросить их должна быть загружена в фоновом потоке, вызвав `BackgroundLoadResource()`. Можно подписаться на событие загрузить фоновый ресурс с помощью `SubscribeToResourceBackgroundLoaded` метод. Он поможет определить, если загрузка была фактически успеха или сбоя. В зависимости от ресурсов, только часть процесса загрузки могут быть перемещены в фоновом потоке, например завершения шага передачи GPU всегда должно происходить в основном потоке. Обратите внимание, что при вызове методов для ресурса, который помещается в очередь для фона загрузке загрузки ресурса основной поток будет простаивать до завершения его загрузки.

Асинхронные функции загрузки сцены `LoadAsync()` и `LoadAsyncXML()` имеется возможность загрузки фона ресурсы сначала перед продолжить так, чтобы загрузить содержимое сцены. Он также позволяет загружать только ресурсы без изменения сцены, указав `LoadMode.ResourcesOnly`. Это позволяет готовить сцены или объекта prefab файлы для быстрого создания экземпляра.

Наконец максимальное время (в миллисекундах), затраченное на каждый кадр на завершение фоновой загрузки ресурсов можно настроить, задав `FinishBackgroundResourcesMs` свойство `ResourceCache`.

<a name="sound"/>

# <a name="sound"></a>Звуковой сигнал

Звук является важной частью игры и UrhoSharp framework предоставляет способ воспроизведение звуков в игру.  Воспроизведение звуков путем присоединения [ `SoundSource` ](https://developer.xamarin.com/api/type/Urho.Audio.SoundSource/) компонент [ `Node` ](https://developer.xamarin.com/api/type/Urho.Node) и затем воспроизведение файл с именем из ресурсов.

Вот как это делается:

```csharp
var explosionNode = Scene.CreateChild();
var sound = explosionNode.CreateComponent<SoundSource>();
soundSource.Play(Application.ResourceCache.GetSound("Sounds/ding.wav"));
soundSource.Gain = 0.5f;
soundSource.AutoRemove = true;
```

<a name="particles"/>

# <a name="particles"></a>Примитивы

Примитивы предоставляют простой способ добавления некоторые эффекты, простой и недорогой в приложение.  Можно использовать примитивы, которые хранятся в формате PEX, с помощью таких средств, как [ http://onebyonedesign.com/flash/particleeditor/ ](http://onebyonedesign.com/flash/particleeditor/).

Примитивы, компоненты, которые могут быть добавлены в узел.  Необходимо вызвать узла `CreateComponent<ParticleEmitter2D>` способ создать фрагмент, а затем настройте примитив, задав свойство эффект 2D эффект загружается из кэша ресурса.

Например можно вызвать этот метод в компоненте для отображения некоторых примитивы, которые преобразуются в развертывания при попадании:

```csharp
public async void Explode (Component target)
{
    // show a small explosion when the missile reaches an aircraft.
    var cache = Application.ResourceCache;
    var explosionNode = Scene.CreateChild();
    explosionNode.Position = target.Node.WorldPosition;
    explosionNode.SetScale(1f);
    var particle = explosionNode.CreateComponent<ParticleEmitter2D>();
    particle.Effect = cache.GetParticleEffect2D("explosion.pex");
    var scaleAction = new ScaleTo(0.5f, 0f);
    await explosionNode.RunActionsAsync(
        scaleAction, new DelayTime(0.5f));
    explosionNode.Remove();
}
```

Приведенный выше код создаст развертывание узла, подключенного к текущего компонента, внутри этого узла развертывания мы создать передатчика 2D примитивов и настроить его, присвоив свойству эффект.  Запустим два действия, один масштабирует узел меньше по размеру, а, оставляет ее в такой размер 0,5 секунд.  Затем мы удаляем развертывания, которая также устраняет влияние примитив на экране.

Выше примитив обрабатывается следующим образом, при использовании текстуры сфере:

![Примитивы с текстурным заполнением сфере](using-images/image-1.png "выше примитив отрисовывает следующим образом, при использовании сфере текстуры")

И это выглядит при использовании иметь дефекты текстуры.

![Примитивы с текстурным заполнением поле](using-images/image-2.png ", и это выглядит при использовании иметь дефекты текстуры")

# <a name="multithreading-support"></a>Поддержка многопоточности

UrhoSharp представляет собой отдельную однопотоковое библиотеку.  Это означает, что не следует пытаться вызывать методы в UrhoSharp из фонового потока, или существует риск повреждения состояния приложения и скорее всего приводит к сбою приложения.

Если вы хотите выполнять определенный код в фоновом режиме, а затем обновите Urho компоненты на основной пользовательский Интерфейс, можно использовать [ `Application.InvokeOnMain(Action)` ](https://developer.xamarin.com/api/member/Urho.Application.InvokeOnMain) метод.  Кроме того, вы можете использовать await C# и .NET задач API, чтобы убедиться, что код выполняется в соответствующем потоке.


# <a name="urhoeditor"></a>UrhoEditor

Можно загрузить редактор Urho для вашей платформы из [Urho веб-сайт](http://urho3d.github.io/), перейдите к загрузкам и получить последнюю версию.

# <a name="copyrights"></a>Авторские права

В этой документации содержит исходное содержимое из Xamarin Inc, но рисует широко из открытой документации для проекта Urho3D и содержит снимки экрана из проекта Cocos2D.



## <a name="related-links"></a>Связанные ссылки

- [Планеты Земли книги](https://developer.xamarin.com/workbooks/graphics/urhosharp/planetearth/planetearth.workbook)
- [Изучение координаты книги](https://developer.xamarin.com/workbooks/graphics/urhosharp/coordinates/ExploringUrhoCoordinates.workbook)
