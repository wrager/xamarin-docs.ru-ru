---
title: "Простой анимации"
description: "Класс ViewExtensions предоставляет методы расширения, которые могут использоваться для создания простой анимации. В этой статье демонстрируется создание и Отмена анимации с помощью класса ViewExtensions."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: fb7ca216978e4c890349a44b07d5a383e9ca2384
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="simple-animations"></a>Простой анимации

_Класс ViewExtensions предоставляет методы расширения, которые могут использоваться для создания простой анимации. В этой статье демонстрируется создание и Отмена анимации с помощью класса ViewExtensions._


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Класс предоставляет следующие методы расширения, которые могут использоваться для создания простой анимации:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимирует [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) свойства [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) анимирует [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) применяет анимированных добавочное увеличиваются или уменьшаются в [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимирует [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) применяет анимированных добавочное увеличиваются или уменьшаются в [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимирует [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимирует [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) анимирует [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) свойство [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/).

По умолчанию каждый анимация займет 250 миллисекунд. Тем не менее продолжительность для каждой анимации можно указать при создании анимации.

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Класс также содержит [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) метод, который можно использовать для отмены любые анимации.

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Класс предоставляет [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) метода расширения. Однако этот метод предназначен для использования с макетами для анимации переходы между состояниями макета, которые содержат размер и положение изменения. Таким образом, он должен использоваться только с [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) подклассов.

Методы расширения анимации в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класс все асинхронные и возврата `Task<bool>` объекта. Возвращает значение `false` Если которого анимация завершается, и `true` при отмене анимации. Таким образом, методы анимации должно обычно используется с `await` оператор, который позволяет легко определить, после завершения анимации. Кроме того он становится невозможно создать последовательной анимации последующие методы для выполнения после завершения метода. Дополнительные сведения см. в разделе [составной анимации](#compound).

Если требуется, чтобы позволить анимации завершения в фоновом режиме, а затем `await` оператор можно опустить. В этом случае методы расширения анимации быстро вернуться после инициации анимации с анимацией, выполняемых в фоновом режиме. Эта операция может браться преимущества при создании составного анимации. Дополнительные сведения см. в разделе [составной анимации](#composite).

Дополнительные сведения о `await` оператор, в разделе [Общие сведения о поддержке Async](~/cross-platform/platform/async.md).

## <a name="single-animations"></a>Один анимации

Каждый метод расширения в [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) реализует операцию одной анимации, который постепенно изменяет свойство из одного значения на другое значение за период времени. В этом разделе более подробно рассматривается каждая операция анимации.

### <a name="rotation"></a>Вращение

В следующем примере кода показано использование [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод для анимации [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра, повернув более 2 секунд (2000 мс) до 360 градусов. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод получает текущую [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство значение для начала анимации, а затем выполняется поворот от этого значения для своего первого аргумента (360). Анимации завершить, изображение [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство становится равным нулю. Это гарантирует, что `Rotation` окончании анимацию, что не позволит поворотов дополнительные свойства не остаются в 360.

Следующих снимках экрана показано поворот выполняется для каждой платформы.

![](simple-images/rotateto.png "Анимация поворота")

### <a name="relative-rotation"></a>Относительный поворот

В следующем примере кода показано использование [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод постепенно увеличивать или уменьшать [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра, повернув 360 градусов относительно его начальной позиции более 2 секунд (2000 мс). [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод получает текущую [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) значение для начала анимации, а затем поворачивает от этого значения к значению плюс своего первого аргумента (360). Это гарантирует, что каждый анимации будут всегда повороту 360 градусов от начальной позиции. Таким образом Если новая анимация вызывается во время анимации уже выполняется, будет запущен в текущей позиции и могут в конечном итоге в позиции, который не является шагом 360 градусов.

Следующих снимках экрана показано относительный поворот выполняется для каждой платформы.

![](simple-images/relrotateto.png "Анимация относительный вращения")

### <a name="scaling"></a>Масштабирование

В следующем примере кода показано использование [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) метод для анимации [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра, увеличив масштаб в два раза больше более 2 секунд (2000 мс). [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Метод получает текущую [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) значение свойства (значение по умолчанию 1) для начала анимации, а затем масштабирование от этого значения для своего первого аргумента (2). Это приводит к расширения в два раза больше размера изображения.

Следующих снимках экрана показано масштабирование выполняется для каждой платформы.

![](simple-images/scaleto.png "Масштабирование анимации")

### <a name="relative-scaling"></a>Относительная масштабирование

В следующем примере кода показано использование [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод для анимации [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра, увеличив масштаб в два раза больше более 2 секунд (2000 мс). [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод получает текущую [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) значение свойства для начала анимации, а затем масштабирование от этого значения до значения, а также своего первого аргумента (2). Это гарантирует, что каждой анимации всегда будет масштабирование 2 от начальной позиции.

### <a name="scaling-and-rotation-with-anchors"></a>Масштабирование и поворот якорей

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) И [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) свойства задать центр масштабирование или поворот для [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) и [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) свойства. Следовательно, их значения также влияет на [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) и [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) методы.

Получает [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) помещена в центре макета, в следующем примере кода демонстрируется Смена изображение вокруг центра макета, установив его [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) свойство:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

Значение поворота [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляр вокруг центра макета, [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) и [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) свойства должны задаваться для значений, которые относительно ширины и высоты `Image`. В этом примере в центре `Image` определяется как в центре макета и поэтому по умолчанию `AnchorX` значение 0,5 изменять не обязательно. Тем не менее `AnchorY` свойство переопределяется значением из верхней части `Image` к центральной точке макета. Это гарантирует, что `Image` делает поворот 360 градусов относительно центральной точки макета, как показано на следующем снимке экрана:

![](simple-images/rotate-anchors.png "Анимация поворота якорей")

### <a name="translation"></a>Перевод

В следующем примере кода показано использование [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод для анимации [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) свойства [ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра, переведя его по горизонтали и вертикали более 1 секунды (1000 миллисекунд). [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод одновременно переводит изображение 100 пикселов влево и вверх 100 пикселей. Это, поскольку первый и второй аргументы обоих отрицательных чисел. Предоставление положительных чисел, преобразует изображения вправо и вниз.

Следующих снимках экрана показано перевод выполняется для каждой платформы.

![](simple-images/translateto.png "Перевод анимации")

> [!NOTE]
> Если элемент изначально располагаются вне экрана и затем преобразованы к экрану, после перевода входной макет элемента остается вне экрана, и пользователь не может взаимодействовать с ним. Таким образом рекомендуется представлении должен быть размещен в последней позиции, которое затем требуемые переводы выполнена.

### <a name="fading"></a>Исчезание

В следующем примере кода показано использование [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) метод для анимации [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) свойство [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

Этот код выполняет анимацию [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра путем плавного перехода его в более чем 4 секунды (4000 миллисекунд). [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод получает текущую [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) значение свойства для начала анимации, а затем переходы в от этого значения для своего первого аргумента (1).

Следующих снимках экрана показано исчезания выполняется для каждой платформы.

![](simple-images/fadeto.png "Затухание анимации")

<a name="compound" />

## <a name="compound-animations"></a>Составной анимации

Представляет собой последовательный сочетание анимации комплексной анимации и могут быть созданы с `await` оператор, как показано в следующем примере кода:

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

В этом примере [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) преобразуется в течение 6 секунд (6000 миллисекунд). Перевод `Image` использует пять анимации с `await` оператор, указывающий, последовательно выполняет каждой анимации. Таким образом методы последующих анимации выполняться после завершения метода.

<a name="composite" />

## <a name="composite-animations"></a>Составной анимации

Составной анимации представляет собой сочетание анимации, где одновременное выполнение двух или более анимации. Можно создать составной анимации смешиванием ожидаемой и не ожидать анимации, как показано в следующем примере кода:

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

В этом примере [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) масштабируется и одновременно поворачивать более чем 4 секунды (4000 миллисекунд). Масштабирование `Image` использует два последовательных анимации, возникших примерно в то же время поворот. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Метод выполняется без `await` оператор и немедленно возвращает, что и первый [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) затем начала анимации. `await` Оператор на первом `ScaleTo` вызова метода задерживает второй `ScaleTo` вызова метода до первой `ScaleTo` завершения вызова метода. На этом этапе `RotateTo` анимации является половиной способом завершения и `Image` будет 180 градусов. Во время последних 2 секунд (2000 мс) второй `ScaleTo` анимации и `RotateTo` анимации обе завершаются.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>Параллельное выполнение нескольких асинхронных методов

`static` `Task.WhenAny` И `Task.WhenAll` методы используются для одновременного запуска нескольких асинхронных методов и таким образом, можно использовать для создания составного анимации. Оба метода возвращают `Task` объекта и принять коллекцию методов, чтобы каждый возвращаемый `Task` объекта. `Task.WhenAny` Метод завершения любого метода в его коллекции по завершении выполнения, как показано в следующем примере кода:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

В этом примере `Task.WhenAny` вызов метода содержит две задачи. Первая задача Поворот изображения более чем 4 секунды (4000 миллисекунд), а вторая задача выполняет изменение масштаба изображения более 2 секунд (2000 мс). По завершении вторая задача `Task.WhenAny` вызов метода завершается. Тем не менее несмотря на то что [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) по-прежнему выполняется метод, второй [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) метод начинает.

`Task.WhenAll` Метод завершения после завершения выполнения всех методов в его коллекции, как показано в следующем примере кода:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

В этом примере `Task.WhenAll` вызов метода содержит три задачи, каждый из которых выполняет более 10 минут. Каждый `Task` делает Разное количество вращения 360 градусов — 307 поворотов для [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 поворотов для [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)и 199 поворотов для [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). Эти значения являются простых чисел, таким образом обеспечивая, поворотов не синхронизированы и поэтому не будет привести повторяющихся образцов.

Следующих снимках экрана показано несколько поворотов выполняется для каждой платформы.

![](simple-images/multiple-rotations.png "Составной анимации")

## <a name="canceling-animations"></a>Отмена анимации

Приложение может отменить одну или несколько анимаций с помощью вызова `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) метода, как показано в следующем примере кода:

```csharp
ViewExtensions.CancelAnimations (image);
```

Это отменит сразу все анимации, запущенных на [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) экземпляра.

## <a name="summary"></a>Сводка

В этой статье показано создание и Отмена анимации с помощью [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) класса. Этот класс предоставляет методы расширения, которые могут использоваться для создания простой анимации, поворот, масштабирование, преобразования и затухание [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) экземпляров.


## <a name="related-links"></a>Связанные ссылки

- [Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)
- [Базовая анимация (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
