---
title: Общие сведения о ARKit
description: Режиме дополненной реальности для iOS 11
ms.prod: xamarin
ms.assetid: 70291430-BCC1-445F-9D41-6FBABE87078E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: f48cdd48e63131fe234fef1bb60b555724dd8a92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-arkit"></a>Общие сведения о ARKit

_Режиме дополненной реальности для iOS 11_

ARKit включает разнообразные режиме дополненной реальности приложений и игр. Данный раздел охватывает следующее:

- [Приступая к работе с ARKit](#gettingstarted)
- [Использование ARKit с UrhoSharp](urhosharp.md)

<a name="gettingstarted" />

## <a name="getting-started-with-arkit"></a>Приступая к работе с ARKit

Чтобы приступить к работе с режиме дополненной реальности, приведенные ниже инструкции пошагового простого приложения: позиционирование 3D-модели и позволяя ARKit сохранить модель на месте с функциональными возможностями отслеживания.

![3D-модели Jet с плавающей запятой в camera изображения](images/jet-sml.png)

### <a name="1-add-a-3d-model"></a>1. Добавить 3D-модель

Ресурсы должны быть добавлены в проект с **SceneKitAsset** действие построения.

![Активы SceneKit в проекте](images/scene-assets.png)


### <a name="2-configure-the-view"></a>2. Настройка представления

В контроллере представление `ViewDidLoad` метод, загрузить ресурс сцены и задать `Scene` свойство представления:

```csharp
ARSCNView SceneView = (View as ARSCNView);

// Create a new scene
var scene = SCNScene.FromFile("art.scnassets/ship");

// Set the scene to the view
SceneView.Scene = scene;
```

### <a name="3-optionally-implement-a-session-delegate"></a>3. При необходимости реализации делегата сеанса

Несмотря на то, что не требуется для простых случаях, реализация делегата сеанса могут оказаться полезными для отладки состояние сеанса ARKit (и в реальных условиях, обратная связь с пользователем). Создание простого делегата, используя приведенный ниже код:

```csharp
public class SessionDelegate : ARSessionDelegate
{
  public SessionDelegate() {}
  public override void CameraDidChangeTrackingState(ARSession session, ARCamera camera)
  {
    Console.WriteLine("{0} {1}", camera.TrackingState, camera.TrackingStateReason);
  }
}
```

Назначить делегата в в `ViewDidLoad` метод:

```csharp
// Track changes to the session
SceneView.Session.Delegate = new SessionDelegate();
```

### <a name="4-position-the-3d-model-in-the-world"></a>4. Поместите в мире трехмерной модели

В `ViewWillAppear`, следующий код устанавливает сеанс ARKit и задает позицию 3D-модели в пространстве относительно камеры устройства:

```csharp
// Create a session configuration
var configuration = new ARWorldTrackingConfiguration {
  PlaneDetection = ARPlaneDetection.Horizontal,
  LightEstimationEnabled = true
};

// Run the view's session
SceneView.Session.Run(configuration, ARSessionRunOptions.ResetTracking);

// Find the ship and position it just in front of the camera
var ship = SceneView.Scene.RootNode.FindChildNode("ship", true);

ship.Position = new SCNVector3(2f, -2f, -9f);
```

Каждый раз, запуска или возобновления приложения 3D-модель будет располагаться камеры. После располагается модель, переместите камеры и следите за ARKit сохраняют располагается модель.

### <a name="5-pause-the-augmented-reality-session"></a>5. Приостановка сеанса режиме дополненной реальности

Рекомендуется приостановить сеанс ARKit, когда представление-контроллер не отображается (в `ViewWillDisappear` метод:

```csharp
SceneView.Session.Pause();
```

## <a name="summary"></a>Сводка

Приведенный выше код приводит простого ARKit приложения. Более сложные примеры и ожидается представление-контроллер, размещение режиме дополненной реальности сеанса для реализации `IARSCNViewDelegate`, и реализовать дополнительные методы.

ARKit предоставляет множество других более сложные функции, такие как поверхности отслеживания и взаимодействия с пользователем. В разделе [UrhoSharp Демонстрация](urhosharp.md) пример объединения отслеживания с UrhoSharp ARKit.


## <a name="related-links"></a>Связанные ссылки

- [Режиме дополненной реальности (Apple)](https://developer.apple.com/arkit/)
- [Использование ARKit с UrhoSharp](urhosharp.md)
- [Пример простого ARKit (Jet)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitSample/)
- [Размещение объектов ARKit (пример)](https://developer.xamarin.com/samples/monotouch/ios11/ARKitPlacingObjects/)
- [Представляем ARKit - режиме дополненной реальности для операций ввода-вывода (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/602/)
