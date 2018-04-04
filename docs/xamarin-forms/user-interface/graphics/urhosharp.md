---
title: С помощью UrhoSharp в Xamarin.Forms
description: UrhoSharp может использоваться для добавления трехмерной графики в приложение дополнительные визуализации
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 8421355e0630a637589cb4f08c2fec4ea9cdab24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>С помощью UrhoSharp в Xamarin.Forms

## <a name="what-is-urhosharp"></a>Что такое UrhoSharp?

[UrhoSharp](~/graphics-games/urhosharp/index.md) является 3D-мощный механизм для разработчиков Xamarin и .NET. [Введение](~/graphics-games/urhosharp/introduction.md) приведены дополнительные сведения о библиотеке UrhoSharp и [эти заметки](~/graphics-games/urhosharp/using.md) описывается программирование сцен и действия.

UrhoSharp можно использовать для отображения графики в приложениях Xamarin.Forms.
Это [пример](https://github.com/xamarin/urho-samples/tree/master/FormsSample) показано, как UrhoSharp может использоваться для создания интерактивных трехмерных диаграмм:

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D интерактивной диаграммы на iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D интерактивной диаграммы на Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>Добавление пакетов UrhoSharp Nuget

Прежде чем использовать UrhoSharp, разработчики должны добавить пакет UrhoSharp Nuget их решения. В этом руководстве предполагается Xamarin.Forms проект с iOS, Android и PCL проекта. Весь код будет записан в проект переносимой библиотеки Классов; но UrhoSharp Nuget необходимо добавить в iOS и Android проекты слишком.

Пакет UrhoSharp.Forms Nuget содержит все объекты, необходимые для создания объектов UrhoSharp. Пакет nuget UrhoSharp.Forms включает `UrhoSurface` класс, который используется для размещения UrhoSharp в Xamarin.Forms.
Чтобы начать, щелкните правой кнопкой мыши на PCL **пакетов** папку и выберите **Добавление пакетов...** . Введите условие поиска **UrhoSharp.Forms**выберите **UrhoSharp для Xamarin.Forms**, нажмите кнопку **добавить пакет**.

[![](urhosharp-images/add-package-sml.png "Пакеты диалоговое окно добавления")](urhosharp-images/add-package.png#lightbox "пакетов диалоговое окно добавления")

Пакет UrhoSharp.Forms NuGet будут добавлены в проект:

![](urhosharp-images/packages.png "Папка «пакеты»")

Повторите описанные выше шаги для проектов платформы (например, iOS и Android).

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>Пошаговое руководство: Добавление UrhoSharp приложения Xamarin.Forms

Следующие шаги описывают код в образце Xamarin.Forms UrhoSharp.

1. [Создать страницу Forms Xamarin](#1)
2. [Добавить UrhoSurface](#2)
3. [Построение приложения Urho](#3)
4. [Добавьте класс диаграммы UrhoSurface](#4)
5. [Взаимодействие с UrhoSharp](#5)

Обратите внимание, что образец использует возможности C# 6 может не компилироваться в более старых версиях Visual Studio.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Создание страницы форм Xamarin

В следующем примере кода показана страница Xamarin.Forms `UrhoPage` до добавления любого кода, связанные с Urho:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. Добавить UrhoSurface

UrhoSharp можно поместить в `ContentPage` как и другие элементы Xamarin.Forms.
Фрагмент кода ниже показан `UrhoSurface` добавлен на страницу Xamarin.Forms:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Сборка приложения Urho

Ссылаться на `Charts` класса для реализации Urho трехмерной графики, используемый в этом примере. Структуры базовый код показан ниже — Обратите внимание, что класс реализует интерфейс `Urho.Application` которого отличается от `Xamarin.Forms.Application` класса, реализованный в **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[UrhoSharp документации](~/graphics-games/urhosharp/index.md) содержит дополнительные сведения о построении трехмерных сцен и действия.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. Добавьте класс диаграммы UrhoSurface

Используйте `UrhoSurface.Show<T>` универсальный метод, чтобы добавить приложение Urho страницу Xamarin.Forms. В следующем фрагменте кода показан дополнительный код, необходимый для создания `Charts` класса:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Примечание: `Show<T>` метод является асинхронным и должен быть вызван с `await` ключевое слово.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. Взаимодействие с UrhoSharp

Пример позволяет выделить и изменения столбцов диаграммы. `Charts` Предоставляемых классами `Bars` и `SelectedBar` для этого взаимодействия.

Каждая «строка» имеет обработчик событий выбора, добавленные после `Charts` послал класса, путем прохода по предоставляемым `Bars` коллекции:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Обработчик событий использует значение Xamarin.Forms `Slider` регулировку высота данной строки:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Наконец, подключите два `Slider` элементы управления, чтобы при их значения влияют на холст UrhoSharp. Первый ползунок Поворот изображения трехмерной диаграммы и ползунок корректирует высоту выбранной строки.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Анимации в [вверху страницы](#) Показать выполнения образца.

## <a name="summary"></a>Сводка

На этой странице отображаются как UrhoSharp можно использовать для добавления данных трехмерной визуализации в Xamarin.Forms. Чтение [UrhoSharp документации](~/graphics-games/urhosharp/index.md) Дополнительные сведения о том, как построить Urho сцен, которые могут быть включены в Xamarin.Forms приложениях, использующих в приведенном выше примере.


## <a name="related-links"></a>Связанные ссылки

- [Пример диаграммы (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
