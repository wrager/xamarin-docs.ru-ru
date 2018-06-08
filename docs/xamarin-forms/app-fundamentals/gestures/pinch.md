---
title: Добавление распознавателя жестом сжатия
description: Жест сжатие используется для выполнения интерактивных масштаба и реализуется с помощью класса PinchGestureRecognizer. Обычный сценарий для жестов жестом сжатия — для выполнения интерактивных масштаба изображения на месте жестом сжатия. Это достигается путем масштабирования окна просмотра содержимого и описанные в этой статье.
ms.prod: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: b2348a1f0dfacc4a7a0e37f5c9041a07217ff802
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846117"
---
# <a name="adding-a-pinch-gesture-recognizer"></a>Добавление распознавателя жестом сжатия

_Жест сжатие используется для выполнения интерактивных масштаба и реализуется с помощью класса PinchGestureRecognizer. Обычный сценарий для жестов жестом сжатия — для выполнения интерактивных масштаба изображения на месте жестом сжатия. Это достигается путем масштабирования окна просмотра содержимого и описанные в этой статье._

## <a name="overview"></a>Обзор

Чтобы сделать элемент пользовательского интерфейса масштабируемым с жестом жестом сжатия, создать [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) экземпляра, обрабатывать [ `PinchUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PinchGestureRecognizer.PinchUpdated/) события, и добавьте распознаватель жестов для [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) коллекции элемента пользовательского интерфейса. В следующем примере кода показан `PinchGestureRecognizer` присоединяется к [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

Это можно также сделать в XAML, как показано в следующем примере кода:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Код для `OnPinchUpdated` затем добавляется обработчик событий в файл кода:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## <a name="creating-a-pinchtozoom-container"></a>Создание контейнера PinchToZoom

Обработка жестов жестом сжатия для выполнения операции масштабирования требует некоторых математических для преобразования пользовательского интерфейса. Этот раздел содержит обобщенный вспомогательного класса для выполнения математических операций, который может использоваться для интерактивного изменения масштаба любой элемент пользовательского интерфейса. Следующий пример кода демонстрирует класс `PinchToZoomContainer`:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

Этот класс может быть перенесено вокруг элемента пользовательского интерфейса, чтобы жестов сжатие будет увеличен перенесенного элемента. В следующем примере показан код XAML `PinchToZoomContainer` упаковки [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показан способ `PinchToZoomContainer` заключает в оболочку [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента на странице C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

Когда [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемент получает жест жестом сжатия, отображаемому изображению будет быть увеличения in или out. Выполняется увеличение `PinchZoomContainer.OnPinchUpdated` метода, как показано в следующем примере кода:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

Этот метод обновляет уровень масштаба оболочку элемента основании жестом сжатия жест пользователя. Это достигается с помощью значения [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale/), [ `ScaleOrigin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin/) и [ `Status` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PinchGestureUpdatedEventArgs.Status/) свойства [ `PinchGestureUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureUpdatedEventArgs/) для расчета коэффициента масштабирования, применяемый в источнике жестов жестом сжатия. Оболочку элемента затем развернуто в источнике жестов жестом сжатия, установив его [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/), и [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) вычисляемые значения свойств.

## <a name="summary"></a>Сводка

Используется для выполнения интерактивных масштаба жестов жестом сжатия и реализуется с помощью [ `PinchGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/) класса.


## <a name="related-links"></a>Связанные ссылки

- [PinchGesture (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PinchGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PinchGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PinchGestureRecognizer/)
