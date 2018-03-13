---
title: SpriteKit
ms.topic: article
ms.prod: xamarin
ms.assetid: 93971DAE-ED6B-48A8-8E61-15C0C79786BB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: c533e38fcd8775fd6b9a49c2e14de76673641553
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="spritekit"></a>SpriteKit

Комплект Sprite, платформа игр 2D Apple, имеет некоторые интересные новые возможности в iOS 8 и OS X Yosemite. К ним относятся интеграция с Kit сцены, поддержка построителя текстуры, освещения, теней, ограничения, создания карты нормалей и улучшения физических. В частности новые возможности физических сделать очень легко включить реалистичных эффектов в игру.

## <a name="physics-bodies"></a>Физический тела

Комплект Sprite включает двухмерные, физические жесткие текст API. Каждый спрайт имеет тело связанных физических (`SKPhysicsBody`), определяющий физических свойств, например запоминающих устройств и повышение эффективности, а также геометрию текста в мире физических.

## <a name="creating-a-physics-body-from-a-texture"></a>Создание текста физических из текстуры
Комплект Sprite теперь поддерживает унаследовав тексте физических спрайт текстура. Это позволяет легко реализовать конфликтов, которые выглядят более естественным.

Например Обратите внимание, в следующих конфликтов как банана и monkey конфликтуют почти в рабочей области каждого изображения:
 
![](spritekit-images/image13.png "Банана и monkey конфликтуют почти в рабочей области каждого изображения")

Комплект Sprite значительно упрощает создание тело физических возможно с единой строки кода. Просто вызвать `SKPhysicsBody.Create` текстур и размером: sprite. PhysicsBody = SKPhysicsBody.Create (sprite. Текстуры, sprite. Размер);

## <a name="alpha-threshold"></a>Пороговое значение альфа-канала

Помимо простого задания `PhysicsBody` свойство непосредственно геометрического объекта, производного от текстуры, приложение может задать и альфа-пороговое значение для управления как извлекаются геометрии. 

Порог alpha определяет минимальное значение альфа-канала, которые должны быть пиксель, должны быть включены в тексте полученный физических. Например следующий код приводит в теле немного отличается физических:

```chsarp
sprite.PhysicsBody = SKPhysicsBody.Create (sprite.Texture, 0.7f, sprite.Size);
```

Эффект настройки порог alpha следующим образом fine-tunes предыдущего конфликта, таким образом, что monkey находится при несовместимости с банана:

![](spritekit-images/image14.png "Monkey находится при несовместимости с банана")
 
## <a name="physics-fields"></a>Физический поля

Другой Почетное место в комплект Sprite — поддерживают новое поле физических. Они позволяют добавлять такие элементы, как поля vortex радиального тяжести полей и полей spring для других.

Физический поля создаются с помощью класса SKFieldNode, который добавляется в сцене аналогично любому другому `SKNode`. Существуют различные методы фабрики для `SKFieldNode` для создания различных физических полей. Можно создать поле spring путем вызова `SKFieldNode.CreateSpringField()`, поле радиального тяжести путем вызова `SKFieldNode.CreateRadialGravityField()`, и т. д.

`SKFieldNode` также имеет свойства, определяющие атрибуты поля, такие как силы поля области поля и затухания поля сил.

## <a name="spring-field"></a>Поле пружины

Например следующий код создает поле spring и добавляет его в сцену:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateSpringField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 0.5f;
fieldNode.Region = new SKRegion(Frame.Size);
AddChild (fieldNode);
```

Затем добавьте спрайтов и задавать их `PhysicsBody` свойства, чтобы поле физических оказывали спрайты, как в следующем коде выполняется, когда пользователь касается экрана:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    var touch = touches.AnyObject as UITouch;
    var pt = touch.LocationInNode (this);
    var node = SKSpriteNode.FromImageNamed ("TinyBanana");
    node.PhysicsBody = SKPhysicsBody.Create (node.Texture, node.Size);
    node.PhysicsBody.AffectedByGravity = false;
    node.PhysicsBody.AllowsRotation = true;
    node.PhysicsBody.Mass = 0.03f;
    node.Position = pt;
    AddChild (node);
}
```

В результате "Бананы" колебания как spring вокруг узел поля:

![](spritekit-images/image15.png ""Бананы" колебания как spring вокруг узел поля")
 
## <a name="radial-gravity-field"></a>Радиальный тяжести поля

Аналогично Добавление другое поле. К примеру следующий код создает радиального тяжести поля:

```csharp
SKFieldNode fieldNode = SKFieldNode.CreateRadialGravityField ();
fieldNode.Enabled = true;
fieldNode.Position = new PointF (Size.Width / 2, Size.Height / 2);
fieldNode.Strength = 10.0f;
fieldNode.Falloff = 1.0f;
```

Это приводит к другому полю force, где "Бананы" извлекаются Радиально о поле:

![](spritekit-images/image16.png ""Бананы" извлекаются Радиально вокруг поля")