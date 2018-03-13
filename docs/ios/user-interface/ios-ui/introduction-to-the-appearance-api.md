---
title: "Внешний интерфейс API"
description: "iOS позволяет применить визуальное свойство параметров на уровне статического класса, а не к отдельным объектам, таким образом, чтобы это изменение применяется ко всем экземплярам этого элемента управления в приложении."
ms.topic: article
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: f35256529d6d72a3f5e563dc88b9d5883a9724d4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="appearance-api"></a>Внешний интерфейс API

_iOS позволяет применить визуальное свойство параметров на уровне статического класса, а не к отдельным объектам, таким образом, чтобы это изменение применяется ко всем экземплярам этого элемента управления в приложении._

Эта функция выполняется через в Xamarin.iOS статический `Appearance` свойство для всех элементов управления UIKit, которые ее поддерживают. Внешний вид (свойства, как как оттенок цвета и фоновое изображение) таким образом легко настраивается для предоставления согласованного внешнего вида приложения. Внешний вид API появился в iOS 5 и пока некоторые части являются устаревшими в iOS 9 он по-прежнему является удобным способом для достижения некоторые стили и темы эффекты в Xamarin.iOS приложений.

## <a name="overview"></a>Обзор

iOS позволяет настроить внешний вид многие элементы управления UIKit стандартным элементам управления, которое соответствует этой символики, которую вы хотите применить к приложению.

Существует два способа применения настраиваемого внешнего вида:

- **Непосредственно на экземпляр элемента управления** — оттенок цвета, фоновый рисунок и положения заголовка (а также некоторые другие атрибуты) можно установить на многие элементы управления, включая панели инструментов, панели навигации, кнопок и ползунков.

- **Задать значения по умолчанию для статического свойства внешнего вида** — настраиваемые атрибуты для каждого элемента управления предоставляются через `Appearance` статическое свойство. Все настройки, применяемые к этим свойствам будет использоваться по умолчанию для любого элемента управления этого типа, созданный после свойству.

Образец приложения внешний вид показаны все три метода, как показано на эти снимки экрана:

 [![](introduction-to-the-appearance-api-images/appearance01.png "Пример приложения внешний вид демонстрирует все три метода")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Начиная с версии iOS 8 внешний прокси-сервера была расширена для TraitCollections.
 `AppearanceForTraitCollection` можно задать внешний вид по умолчанию для конкретной характеристике коллекцию. Можно прочитать подробнее об этом [введение в раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) руководства.


## <a name="setting-appearance-properties"></a>Установка свойств внешнего вида

На первом экране внешний вид статического класса, используемого для стиля кнопки и элементы желтый/оранжевый следующим образом:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Стили зеленый элементов устанавливаются наподобие этого, в `ViewDidLoad` метод, который переопределяет значения по умолчанию и *внешний вид* статического класса:

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>С помощью UIAppearance в Xamarin.Forms

Внешний вид API могут быть полезны, при [Дизайн приложения iOS](~/xamarin-forms/platform/ios/theme.md#uiappearance) в Xamarin.Forms решения. Несколько строк в `AppDelegate` класс может помочь для реализации конкретных цветовую схему без создания [пользовательское средство отрисовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).


### <a name="custom-themes-and-uiappearance"></a>Пользовательские темы и UIAppearance

iOS позволяет много visual атрибуты пользователя интерфейс элементам управления возможность «темы» с помощью *UIAppearance* API-интерфейсы для принудительного все экземпляры конкретного иметь тот же внешний вид элемента управления. Оно предоставляется как свойство внешний вид на многие классы элементов управления пользовательского интерфейса, а не для отдельных экземпляров элемента управления. Задание свойства отображения для статического `Appearance` свойство влияет на все элементы управления этого типа в приложении.

Чтобы лучше понять концепцию, рассмотрим пример.

Чтобы изменить конкретный `UISegmentedControl` чтобы пурпурный оттенок, мы ссылку определенный элемент управления на наш экран так на `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Можно также задайте значение в области свойств конструктора: 

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Оттенок панель свойств")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

На рисунке ниже показано, что это задает оттенок только с именем «sg1» элемента управления.

 [![](introduction-to-the-appearance-api-images/image53.png "Установка оттенок отдельного элемента управления")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Чтобы задать множество параметров таким образом будет полностью неэффективным, поэтому вместо статического `Appearance` свойство в самом классе. Это показано в следующем коде:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

Теперь на рисунке ниже показано оба сегментированных элемента управления с внешним видом, равным Magenta:

 [![](introduction-to-the-appearance-api-images/image54.png "Настройка внешнего вида элемента управления оттенок")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` свойства должны задаваться ранних этапах жизненного цикла приложения, такие как AppDelegate `FinishedLaunching` событий, или в ViewController, прежде чем будут отображаться соответствующие элементы управления.


Ссылаться на [введение в API внешний вид](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) более подробные сведения.


## <a name="related-links"></a>Связанные ссылки

- [Внешний вид (пример)](https://developer.xamarin.com/samples/monotouch/IntroToAppearance/)
- [Справочник по UIAppearance протокола](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Внешний вид в Xamarin.Forms](~/xamarin-forms/platform/ios/theme.md#uiappearance)
