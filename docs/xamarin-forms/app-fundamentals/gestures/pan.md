---
title: Добавление распознавателя сдвиг
description: В этой статье объясняется, как использовать жест панорамирование по горизонтали и по вертикали, перетащите изображение, чтобы все содержимое изображения можно просмотреть, когда оно отображается в окне просмотра, меньше, чем размеры изображения.
ms.prod: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: d3e4dfc57678ff75fb8f9761360748d94aeefcc2
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35239988"
---
# <a name="adding-a-pan-gesture-recognizer"></a>Добавление распознавателя сдвиг

_Жест панорамирование используется для выявления перетаскивания и реализуется с помощью класса PanGestureRecognizer. Очень распространенный сценарий для панорамирования жестов является по горизонтали и вертикали, перетащите изображение, чтобы все содержимое изображения можно просмотреть при отображении в окне просмотра, меньше, чем размеры изображения. Это достигается путем перемещения изображения в области просмотра и описанные в этой статье._

## <a name="overview"></a>Обзор

Чтобы сделать элемент пользовательского интерфейса перетаскиваемые с жестом панорамирования, создать [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) экземпляра, обрабатывать [ `PanUpdated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.PanGestureRecognizer.PanUpdated/) события, и добавьте распознаватель жестов для [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) коллекции элемента пользовательского интерфейса. В следующем примере кода показан `PanGestureRecognizer` присоединяется к [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

Это можно также сделать в XAML, как показано в следующем примере кода:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

Код для `OnPanUpdated` затем добавляется обработчик событий в файл кода:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

> [!NOTE]
> Правильный панорамирования на Android требуется [пакет NuGet Xamarin.Forms 2.1.0-pre1](https://www.nuget.org/packages/Xamarin.Forms/2.1.0.6501-pre1) по меньшей мере.

## <a name="creating-a-pan-container"></a>Создание контейнера сдвиг

Этот раздел содержит обобщенный вспомогательный класс, который выполняет панорамирования свободной формы, который обычно подходит для перемещения в пределах изображения или карты. Обработка жестов панорамирование для выполнения операции перетаскивания требует некоторых математических для преобразования пользовательского интерфейса. Это Математическая используется перетаскивать только в пределах перенесенного элемента. Следующий пример кода демонстрирует класс `PanContainer`:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

Этот класс может быть перенесено вокруг элемента пользовательского интерфейса, жест панорамирование будет перетащить перенесенного элемента. В следующем примере показан код XAML `PanContainer` упаковки [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показан способ `PanContainer` заключает в оболочку [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемента на странице C#:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

В обоих примерах [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) и [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) свойствам присваиваются значения ширины и высоты отображаемого изображения.

Когда [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) элемент получает жест панорамирование, будет перетащить отображаемому изображению. Выполняется перетаскивание `PanContainer.OnPanUpdated` метода, как показано в следующем примере кода:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

Этот метод обновляет содержимое для просмотра оболочку элемента пользовательского интерфейса, зависимости панорамирование жест пользователя. Это достигается с помощью значения [ `TotalX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalX/) и [ `TotalY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PanUpdatedEventArgs.TotalY/) свойства [ `PanUpdatedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanUpdatedEventArgs/) экземпляра для вычисления направления и расстояние сдвига. `App.ScreenWidth` И `App.ScreenHeight` свойства предоставляют высоты и ширины окна просмотра и заданы ширина экрана и значения высоты экрана устройства, соответствующие проекты под конкретные платформы. Оболочку элемента затем перетащить, установив его [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) вычисляемые значения свойств.

Когда панорамирования содержимое в элементе, который не занимает весь экран, высоты и ширины окна просмотра можно получить из элемента [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) и [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) свойства.

> [!NOTE]
> Отображение изображения с высоким разрешением могут значительно увеличить объем памяти приложения. Таким образом они должны быть созданы только при необходимости и сразу же после их больше не нужны приложению необходимо освободить. Дополнительные сведения см. в разделе [Оптимизация графических ресурсов](~/xamarin-forms/deploy-test/performance.md#optimizeimages).

## <a name="summary"></a>Сводка

Жест панорамирование используется для выявления перетаскивания и реализуется с помощью [ `PanGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/) класса.



## <a name="related-links"></a>Связанные ссылки

- [PanGesture (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/PanGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [PanGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.PanGestureRecognizer/)
