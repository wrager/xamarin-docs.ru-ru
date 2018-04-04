---
title: Справочник по игровой MonoGame
description: Игровой — это стандартные, кросс платформенных класс для доступа к устройствам ввода в MonoGame.
ms.prod: xamarin
ms.assetid: 1F71F3E8-2397-4C6A-8163-6731ECFB7E03
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 37e6b0a6365b1e93192c0eaad4fd3975c3cbf010
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="monogame-gamepad-reference"></a>Справочник по игровой MonoGame

_Игровой — это стандартные, кросс платформенных класс для доступа к устройствам ввода в MonoGame._

`GamePad` можно использовать при чтении входных данных из входного устройства на нескольких платформах MonoGame. В этом руководстве показано, как работать с игровой класса. Поскольку каждое устройство ввода отличается по структуре и количество кнопок, которые он предоставляет, данное руководство содержит диаграмм, иллюстрирующих различные сопоставления устройств.


# <a name="gamepad-as-a-replacement-for-xbox360gamepad"></a>Игровой для замены Xbox360GamePad

Исходный XNA API, обеспечиваемого `Xbox360GamePad` класс для чтения входных данных от контроллера игры на Xbox 360 или ПК. MonoGame заменил с `GamePad` класса, поскольку на большинстве платформ MonoGame (например, iOS или Xbox One) не может использоваться контроллеры Xbox 360. Несмотря на использование, изменение имени `GamePad` класс аналогичен `Xbox360GamePad` класса.


# <a name="reading-input-from-gamepad"></a>Чтение входных данных из игровой

`GameController` Класс предоставляет стандартный способ чтения входных данных на любой платформе MonoGame. Он предоставляет сведения двумя способами:

 - `GetState` — Возвращает текущее состояние кнопок, аналоговый накопители и d pad контроллера.
 - `GetCapabilities` — Возвращает сведения о возможностях оборудования, такие как контроллер, имеет ли некоторые кнопки или поддерживает вибрация.


## <a name="example-moving-a-character"></a>Пример: Перемещение символ

Следующий код показывает, как кнут левой thumb может использоваться для перемещения символа, установив его `XVelocity` и `YVelocity` свойства. Этот код предполагает, что `characterInstance` экземпляр объекта, который имеет `XVelocity` и `YVelocity` свойства:


```csharp
// In Update, or some code called every frame:
var gamePadState = GamePad.GetState(PlayerIndex.One);
// Use gamePadState to move the character
characterInstance.XVelocity = gamePadState.ThumbSticks.Left.X * characterInstance.MaxSpeed;
characterInstance.YVelocity = gamePadState.ThumbSticks.Left.Y * characterInstance.MaxSpeed;
```


## <a name="example-detecting-pushes"></a>Пример: Определение Push-уведомлений

`GamePadState` Предоставляет сведения о текущем состоянии контроллер, например ли определенные кнопка нажата. Определенные действия, например проведение символ перехода, требуют проверки, если кнопке был отправлен (не вниз последнего кадра, но не выполняется этим кадром) или освобождения (была вниз последнего кадра, но не работу данного кадра). 

Чтобы выполнить этот тип логики, локальные переменные, которые хранят предыдущего кадра `GamePadState` и текущего кадра `GamePadState` должен быть создан. В следующем примере показано, как хранить и использовать предыдущий кадр `GamePadState` для реализации перехода:


```csharp
// At class scope:
// Store the last frame's and this frame's GamePadStates.
// "new" them so that code doesn't have to perform null checks:
GamePadState lastFrameGamePadState = new GamePadState();
GamePadState currentGamePadState = new GamePadState();
protected override void Update(GameTime gameTime)
{
    // store off the last state before reading the new one:
    lastFrameGamePadState = currentGamePadState;
    currentGamePadState = GamePad.GetState(PlayerIndex.One);
    bool wasAButtonPushed = 
currentGamePadState.Buttons.A == ButtonState.Pressed
        && lastFrameGamePadState.Buttons.A == ButtonState.Released;
    if(wasAButtonPushed)
    {
        MakeCharacterJump();
    }
...
}
```


## <a name="example-checking-for-buttons"></a>Пример: Поиск кнопки

`GetCapabilities` можно использовать для проверки наличия определенных оборудованием, таким как кнопки или аналогового манипулятор контроллера. Следующий код показывает проверка на наличие кнопки B и Y на контроллере в игре, которая требует наличия обеих кнопок:


```csharp
var capabilities = GamePad.GetCapabilities(PlayerIndex.One);
bool hasBButton = capabilities.HasBButton;
bool hasXButton = capabilities.HasXButton;
if(!hasBButton || !hasXButton)
{
    NotifyUserOfMissingButtons();
}
```


# <a name="ios"></a>iOS

приложения iOS поддерживают беспроводной игровое входных данных.

> [!IMPORTANT]
> Пакеты NuGet, MonoGame 3.5 включает поддержку беспроводной игровые устройства. С помощью класса игровой IOS, требуется построение MonoGame 3.5 из источника или с помощью MonoGame 3.6 NuGet двоичные файлы. 



## <a name="ios-game-controller"></a>iOS контроллера игры

`GamePad` Возвращает класс свойств, считанных из контроллеров беспроводной сети. Свойства в `GamePad` обслуживают хорошо для стандартных операций ввода-вывода контроллеров устройств, как показано на следующей схеме:

![](input-images/image1.png "Свойства игровой содержат обширная для стандартных iOS контроллеров устройств, как показано в данной схеме.")


# <a name="apple-tv"></a>Apple TV

Apple TV игры можно использовать удаленный Siri или беспроводные игровые устройства для входных данных.


## <a name="siri-remote"></a>Удаленное Siri

*Удаленное Siri* имеет собственное устройство ввода для Apple TV. Несмотря на то, что значения от удаленных Siri могут быть получены с помощью события (как показано в [контроллеров Bluetooth и удаленного Siri руководство](~/ios/tvos/platform/remote-bluetooth.md)), `GamePad` класс может возвращать значения от удаленных Siri.

Обратите внимание, что `GamePad` только чтения ввода из кнопку воспроизведения и touch область: 

![](input-images/image2.png "Обратите внимание, что игровой можно только считывать входные данные из "Воспроизведение" touch поверхности")

С момента сенсорный прочтите поверхности перемещения `DPad` свойства, значения передаются с помощью перемещения `ButtonState` класса. Другими словами, значения доступны только как `ButtonState.Pressed` или `ButtonState.Released`, в отличие от числовых значений или жесты.


## <a name="apple-tv-game-controller"></a>Контроллер Apple TV игры

Игровые устройства для Apple TV ведут себя идентично игровые устройства для приложений iOS. Дополнительные сведения см. в разделе [iOS раздел контроллера игры](#iOS_Game_Controller). 


# <a name="xbox-one"></a>Xbox One

Консоль Xbox One поддерживает чтение входных данных из игры Xbox One контроллера.


## <a name="xbox-one-game-controller"></a>Xbox одной игрового устройства

Xbox одной игрового является наиболее распространенные устройства ввода для Xbox One. `GamePad` Класс предоставляет входные значения из игровых устройств.

![](input-images/image3.png "Класс игровой предоставляет входные значения из игровых устройств")


# <a name="summary"></a>Сводка

В этом руководстве предоставлен Обзор элемента MonoGame `GamePad` класса, как реализовать логику чтение входных данных и схемы общих `GamePad` реализации.

## <a name="related-links"></a>Связанные ссылки

- [Игровой MonoGame](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_GamePad)
