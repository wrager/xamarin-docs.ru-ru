---
title: Рисование геометрических с CCDrawNode
description: CCDrawNode предоставляет методы для рисования простые объекты, такие как линий, кругов и треугольников.
ms.topic: article
ms.prod: xamarin
ms.assetid: 46A3C3CE-74CC-4A3A-AB05-B694AE182ADB
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/24/2017
ms.openlocfilehash: 5a2471981f2e88ff8af9a803ff8f5a99e5b9266f
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="drawing-geometry-with-ccdrawnode"></a>Рисование геометрических с CCDrawNode

_`CCDrawNode` Предоставляет методы для рисования простые объекты, такие как линий, кругов и треугольников._

`CCDrawNode` Класса в CocosSharp предоставляют несколько методов для рисования общих геометрические фигуры. Он наследуется от `CCNode` класса и обычно добавляются к `CCLayer` экземпляров. В этом руководстве описывается использование `CCDrawNode` экземпляров для выполнения пользовательской отрисовки. Он также предоставляет полный список функций рисования с снимки экрана и примеры кода.


## <a name="creating-a-ccdrawnode"></a>Создание CCDrawNode

`CCDrawNode` Класс можно использовать для рисования геометрических объектов, таких как круги, прямоугольники и линии. Например, в следующем образце кода показано, как создать `CCDrawNode` экземпляр, который рисует окружность в `CCLayer` реализации класса:


```csharp
public class GameLayer : CCLayer
{
    public GameLayer ()
    {
        var drawNode = new CCDrawNode ();
        this.AddChild (drawNode);
        // Origin is bottom-left of the screen. This moves
        // the drawNode 100 pixels to the right and 100 pixels up
        drawNode.PositionX = 100;
        drawNode.PositionY = 100;

        drawNode.DrawCircle (
            center: new CCPoint (0, 0),
            radius: 20,
            color: CCColor4B.White);

    }
} 
```

Этот код выводит следующий круг во время выполнения:

![](ccdrawnode-images/image1.png "Этот код выводит этот круг во время выполнения")


## <a name="draw-method-details"></a>Сведения о методе Draw

Давайте рассмотрим некоторые сведения, относящиеся к Рисование с `CCDrawNode`:


### <a name="draw-methods-positions-are-relative-to-the-ccdrawnode"></a>Позиций методы для рисования, относительно CCDrawNode

Все методы рисования необходимо по крайней мере на одну позицию значения для рисования. — Это значение позиции относительно `CCDrawNode` экземпляра. Это означает, что `CCDrawNode` самой имеет позицию, и все рисовать вызовов, сделанных на `CCDrawNode` принять одно или несколько значений позиции. Чтобы понять, как объединить эти значения, давайте рассмотрим несколько примеров.

Сначала мы рассмотрим `DrawCircle` приведенном выше примере:


```csharp
...
drawNode.PositionX = 100;
drawNode.PositionY = 100;

drawNode.DrawCircle (center: new CCPoint (0, 0),
...
```

В этом случае `CCDrawNode` располагается в (100,100) и формируемого круг (0,0) относительно `CCDrawNode`, что может привести круга, выровненный по центру 100 пикселей до, так и справа от нижний левый угол экрана игры.

`CCDrawNode` Могут быть размещены в источнике (левой нижней части экрана), полагаясь на окружность для смещения:


```csharp
...
drawNode.PositionX = 0;
drawNode.PositionY = 0;

drawNode.DrawCircle (center: new CCPoint (50, 60),
...
```

Приведенный выше результаты в центре круга 50 единиц (`drawNode.PositionX` + `CCPoint.X`) в правой части в левой части экрана и 60 (`drawNode.PositionY` + `CCPoint.Y`) единиц измерения выше нижней части экрана.

После вызова метода draw графического объекта можно изменять только `CCDrawNode.Clear` вызывается метод, поэтому любые изменения положения необходимо сделать на `CCDrawNode` сам.

Объекты, нарисованными `CCNodes` также затрагивает `CCNode` экземпляра `Rotation` и `Scale` свойства.


### <a name="draw-methods-do-not-need-to-be-called-every-frame"></a>Методы действия не требуется вызывать каждый кадр

Методы рисования должен вызываться только один раз для создания постоянного визуальных элементов. В примере выше вызов `DrawCircle` в конструкторе `GameLayer` — `DrawCircle` не должен вызываться каждый кадр, повторно рисования круга, при обновлении экрана.

Это отличается от методы рисования в MonoGame обычно отображающего примерно на экран только один кадр и который должен вызываться каждый кадр.

При вызове методов рисования каждого кадра, а затем объектов будет со временем накапливаться внутри вызывающего `CCDrawNode` экземпляра, в результате чего падения частоты кадров, являются производными других объектов.


### <a name="each-ccdrawnode-supports-multiple-draw-calls"></a>Каждый CCDrawNode поддерживает несколько вызовов draw

`CCDrawNode` экземпляры можно использовать для рисования нескольких фигур. Это позволяет сложных визуальных объектов для заключен в одном объекте. Например, следующий код может использоваться для подготовки к просмотру нескольких кругов с одним `CCDrawNode`:


```csharp
for (int i = 0; i < 8; i++)
{
    drawNode.DrawCircle (
        center: new CCPoint (i*15, 0),
        radius: 20,
        color: CCColor4B.White);
} 
```

Это приводит к на следующем рисунке:

![](ccdrawnode-images/image2.png "Результатом является это изображение")


## <a name="draw-call-examples"></a>Примеры вызова Draw

В доступны следующие вызовы draw `CCDrawNode`:

- [`DrawCatmullRom`](#drawcatmullrom)
- [`DrawCircle`](#drawcircle)
- [`DrawCubicBezier`](#drawcubicbezier)
- [`DrawEllipse`](#drawellipse)
- [`DrawLineList`](#drawlinelist)
- [`DrawPolygon`](#drawpolygon)
- [`DrawQuadBezier`](#drawquadbezier)
- [`DrawRect`](#drawrect)
- [`DrawSegment`](#drawsegment)
- [`DrawSolidArc`](#drawsolidarc)
- [`DrawSolidCircle`](#drawsolidcircle)
- [`DrawTriangleList`](#drawtrianglelist)


### <a name="drawcardinalspline"></a>DrawCardinalSpline

`DrawCardinalSpline` Создает кривой линии через переменное число точек. 

`config` Параметр определяет, какие сплайна будут проходить через точки. В приведенном ниже примере показано кривой, проходящей через четыре точки.

`tension` Управляет как острые или круглых точек на сплайн отображается. Объект `tension` значение 0 приведет к сплайн кривых и `tension` значение 1 приведет к сплайн нарисованными прямых линий и жестких краев.

Хотя сплайны кривые линии, CocosSharp рисует сплайны с прямыми линиями. `segments` Параметр определяет, сколько сегментов, используемый для отрисовки сплайн. Большее число приводит плавно изогнутые сплайн за счет производительности. 

Пример кода:


```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (50, 70));
splinePoints.Add (new CCPoint (0, 140));
splinePoints.Add (new CCPoint (100, 210));

drawNode.DrawCardinalSpline (
    config: splinePoints,
    tension: 0,
    segments: 64,
    color:CCColor4B.Red); 
```

![](ccdrawnode-images/image3.png "Параметр сегментов определяет, сколько сегментов, используемый для отрисовки сплайн")


### <a name="drawcatmullrom"></a>DrawCatmullRom

`DrawCatmullRom` Создает кривой линии через переменное число точек, аналогично `DrawCardinalLine`. Этот метод не включает параметр натяжение.

Пример кода:

```csharp
var splinePoints = new List<CCPoint> ();
splinePoints.Add (new CCPoint (0, 0));
splinePoints.Add (new CCPoint (80, 90));
splinePoints.Add (new CCPoint (100, 0));
splinePoints.Add (new CCPoint (0, 130)); 

drawNode.DrawCatmullRom (
    points: splinePoints,
    segments: 64); 
```

![](ccdrawnode-images/image4.png "DrawCatmullRom создает кривой линии через переменное число точек, аналогично DrawCardinalLine")


### <a name="drawcircle"></a>DrawCircle

`DrawCircle` Создает периметре круга из данного `radius`.

Пример кода:

```csharp
drawNode.DrawCircle (
    center:new CCPoint (0, 0),
    radius:20,
    color:CCColor4B.Yellow); 
```

![](ccdrawnode-images/image5.png "DrawCircle создает периметра окружности определенного радиуса")


### <a name="drawcubicbezier"></a>DrawCubicBezier

`DrawCubicBezier` Рисование кривой линии между двумя точками, с помощью контрольных точек, чтобы задать путь между двумя точками.

Пример кода:

```csharp
drawNode.DrawCubicBezier (
    origin: new CCPoint (0, 0),
    control1: new CCPoint (50, 150),
    control2: new CCPoint (250, 150),
    destination: new CCPoint (170, 0),
    segments: 64,
    lineWidth: 1,
    color: CCColor4B.Green); 
```

 ![](ccdrawnode-images/image6.png "DrawCubicBezier Проведение кривой линии между двумя точками")


### <a name="drawellipse"></a>DrawEllipse

`DrawEllipse` создается по контуру *эллипса*, который часто называют Овал (несмотря на то, что два не геометрически идентичны). Можно определить форму эллипса с `CCRect` экземпляра.

Пример кода:


```csharp
drawNode.DrawEllipse (
    rect: new CCRect (0, 0, 130, 90),
    lineWidth: 2,
    color: CCColor4B.Gray); 
```

![](ccdrawnode-images/image8.png "DrawEllipse создает контура эллипса, которую часто называют Овал")


### <a name="drawline"></a>DrawLine

`DrawLine` подключается к точкам с линией заданную ширину. Этот метод аналогичен `DrawSegment`, за исключением того, он создает плоский конечных точек в отличие от round конечных точек.

Пример кода:


```csharp
drawNode.DrawLine (
    from: new CCPoint (0, 0),
    to: new CCPoint (150, 30),
    lineWidth: 5,
    color:CCColor4B.Orange); 
```

![](ccdrawnode-images/image9.png "DrawLine подключается к точкам с линией заданную ширину")


### <a name="drawlinelist"></a>DrawLineList

`DrawLineList` создает несколько строк, подключившись каждой пары точек, заданных `CCV3F_C4B` массива. `CCV3F_C4B` Структура содержит значения для положение и цвет. `verts` Параметр всегда должен содержать четное количество точек, так как каждая строка определяется двумя точками.

Пример кода:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First line:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    // second line, will blend from white to red:
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(120,120), CCColor4B.Red)
};

drawNode.DrawLineList (verts); 
```

 ![](ccdrawnode-images/image10.png "Параметр verts всегда должен содержать четное количество точек, так как каждая строка определяется двумя точками")




### <a name="drawpolygon"></a>DrawPolygon

`DrawPolygon` Создает многоугольник заполняться с контуром переменной ширины и цвета.

Пример кода:


```csharp
CCPoint[] verts = new CCPoint[] {
    new CCPoint(0,0),
    new CCPoint(0, 100),
    new CCPoint(50, 150),
    new CCPoint(100, 100),
    new CCPoint(100, 0)
};

drawNode.DrawPolygon (verts,
    count: verts.Length,
    fillColor: CCColor4B.White,
    borderWidth: 5,
    borderColor: CCColor4B.Red,
    closePolygon: true); 
```

![](ccdrawnode-images/image11.png "DrawPolygon создает многоугольник заполняться с контуром переменной ширины и цвета")


### <a name="drawquadbezier"></a>DrawQuadBezier

`DrawQuadBezier` соединяет две точки с линией. Он ведет себя так же, как `DrawCubicBezier` , но поддерживает только одну контрольную точку.

Пример кода:


```csharp
drawNode.DrawQuadBezier (
    origin:new CCPoint (0, 0),
    control:new CCPoint (200, 0),
    destination:new CCPoint (0, 300),
    segments:64,
    lineWidth:1,
    color:CCColor4B.White);
```

![](ccdrawnode-images/image12.png "DrawQuadBezier соединяет две точки с линией")


### <a name="drawrect"></a>DrawRect

`DrawRect` Создает прямоугольник заполняться с контуром переменной ширины и цвета.

Пример кода: 


```csharp
var shape = new CCRect (
    0, 0, 100, 200);
drawNode.DrawRect(shape,
    fillColor:CCColor4B.Blue,
    borderWidth: 4,
    borderColor:CCColor4B.White); 
```

![](ccdrawnode-images/image13.png "DrawRect создает прямоугольник заполняться с контуром переменной ширины и цвета")


### <a name="drawsegment"></a>DrawSegment

`DrawSegment` соединяет две точки с строки переменной ширины и цвета. Это похоже на `DrawLine`, за исключением того, он создает round конечных точек, а не плоский конечных точек.

Пример кода:


```csharp
drawNode.DrawSegment (from: new CCPoint (0, 0),
    to: new CCPoint (100, 200),
    radius: 5,
    color:new CCColor4F(1,1,1,1)); 
```

![](ccdrawnode-images/image14.png "DrawSegment соединяет две точки с строки переменной ширины и цвета")


### <a name="drawsolidarc"></a>DrawSolidArc

`DrawSolidArc` Создает Клин заполняться данного цвета и radius.

Пример кода:


```csharp
drawNode.DrawSolidArc(
    pos:new CCPoint(100, 100),
    radius:200,
    startAngle:0,
    sweepAngle:CCMathHelper.Pi/2, // this is in radians, clockwise
    color:CCColor4B.White); 
```

![](ccdrawnode-images/image15.png "DrawSolidArc создает Клин заполняться данного цвета и radius")


### <a name="drawsolidcircle"></a>DrawSolidCircle

`DrawCircle` Создает заполняться круга определенного радиуса.

Пример кода:


```csharp
drawNode.DrawSolidCircle(
    pos: new CCPoint (100, 100),
    radius: 50,
    color: CCColor4B.Yellow); 
```

![](ccdrawnode-images/image16.png "DrawCircle создает заполняться круга определенного радиуса")


### <a name="drawtrianglelist"></a>DrawTriangleList

`DrawTriangleList` Создает список треугольников. Каждый треугольник определяется три `CCV3F_C4B` экземпляров в массиве. Число вершин в массиве, передаваемый `verts` параметра должно быть кратно трем. Обратите внимание, что только сведения о содержащихся в `CCV3F_C4B` — это положение verts и их цвет — `DrawTriangleList` метод не поддерживает Рисование треугольника с текстурами.

Пример кода:


```csharp
CCV3F_C4B[] verts = new CCV3F_C4B[] {
    // First triangle:
    new CCV3F_C4B( new CCPoint(0,0), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(30,60), CCColor4B.White),
    new CCV3F_C4B( new CCPoint(60,0), CCColor4B.White),
    // second triangle, each point has different colors:
    new CCV3F_C4B( new CCPoint(90,0), CCColor4B.Yellow),
    new CCV3F_C4B( new CCPoint(120,60), CCColor4B.Red),
    new CCV3F_C4B( new CCPoint(150,0), CCColor4B.Blue)
};

drawNode.DrawTriangleList (verts); 
```

![](ccdrawnode-images/image17.png "DrawTriangleList создает список треугольники")


## <a name="summary"></a>Сводка

В этом руководстве описывается создание `CCDrawNode` и выполнения на основе примитивов отрисовки. Он предоставляет пример каждого из вызовов draw.

## <a name="related-links"></a>Связанные ссылки

- [CCDrawNode API](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
- [Полный пример](https://developer.xamarin.com/samples/mobile/CCDrawNode/)
