---
title: "Передача параметров эффект, как общие свойства среды выполнения языка"
description: "Общие свойства Language Runtime (CLR) можно использовать для определения влияния параметров, не отвечайте на изменения свойств в среде выполнения. В этой статье демонстрируется использование свойств CLR для передачи параметров в силу."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B50466C-5DBD-45DD-B1E6-BE9524C92F27
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: afe30ae87aa2e465013eb7fef3089cf701d98da6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="passing-effect-parameters-as-common-language-runtime-properties"></a>Передача параметров эффект, как общие свойства среды выполнения языка

_Общие свойства Language Runtime (CLR) можно использовать для определения влияния параметров, не отвечайте на изменения свойств в среде выполнения. В этой статье демонстрируется использование свойств CLR для передачи параметров в силу._

Процесс создания силу параметры, которые не отвечайте на изменения свойств времени выполнения выглядит следующим образом:

1. Создание `public` класса, который наследуется от класса [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) класса. `RoutingEffect` Класс представляет эффект независимая от платформы, который создает оболочку для внутреннего эффект, обычно от платформы.
1. Создайте конструктор, который вызывает конструктор базового класса, передавая объединения имени группы разрешения и уникальный идентификатор, который указан на каждый класс эффект от платформы.
1. Добавьте свойства класса для каждого параметра должны быть переданы эффект.

Параметры могут передаваться с эффектом путем указания значения для каждого свойства, при создании экземпляра эффект.

Образец приложения показывает `ShadowEffect` , добавляющий тени для текста, отображаемого элементом [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления. На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](clr-properties-images/shadow-effect.png "Обязанности проекта эффект тени")

Объект [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления `HomePage` настраивается путем `LabelShadowEffect` в каждом проекте конкретную платформу. Параметры передаются каждому `LabelShadowEffect` свойствами в `ShadowEffect` класса. Каждый `LabelShadowEffect` класс является производным от `PlatformEffect` класса для каждой платформы. Это приведет к тень, добавляемый текст, отображаемый элементом `Label` устанавливаются, как показано на следующем снимке экрана:

![](clr-properties-images/screenshots.png "Эффект тени для каждой платформы")

## <a name="creating-effect-parameters"></a>Создание параметров эффекта

Объект `public` класса, который наследуется от класса [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) для представления параметров в силу, необходимо создать класс, как показано в следующем примере кода:

```csharp
public class ShadowEffect : RoutingEffect
{
  public float Radius { get; set; }

  public Color Color { get; set; }

  public float DistanceX { get; set; }

  public float DistanceY { get; set; }

  public ShadowEffect () : base ("MyCompany.LabelShadowEffect")
  {         
  }
}
```

`ShadowEffect` Содержит четыре свойства, которые представляют параметры для передачи для каждой платформы `LabelShadowEffect`. Конструктор класса вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения и уникальный идентификатор, который указан на каждый класс эффект от платформы. Следовательно, новый экземпляр `MyCompany.LabelShadowEffect` будут добавлены к элементу управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции при `ShadowEffect` создается.

## <a name="consuming-the-effect"></a>Использование эффекта

В следующем примере показан код XAML [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления, к которому `ShadowEffect` присоединен:

```xaml
<Label Text="Label Shadow Effect" ...>
  <Label.Effects>
    <local:ShadowEffect Radius="5" DistanceX="5" DistanceY="5">
      <local:ShadowEffect.Color>
        <OnPlatform x:TypeArguments="Color">
            <On Platform="iOS" Value="Black" />
            <On Platform="Android" Value="White" />
            <On Platform="UWP" Value="Red" />
        </OnPlatform>
      </local:ShadowEffect.Color>
    </local:ShadowEffect>
  </Label.Effects>
</Label>
```

Эквивалент [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) в C# показано в следующем примере кода:

```csharp
var label = new Label {
  Text = "Label Shadow Effect",
  ...
};

Color color = Color.Default;
switch (Device.RuntimePlatform)
{
    case Device.iOS:
        color = Color.Black;
        break;
    case Device.Android:
        color = Color.White;
        break;
    case Device.UWP:
        color = Color.Red;
        break;
}

label.Effects.Add (new ShadowEffect {
  Radius = 5,
  Color = color,
  DistanceX = 5,
  DistanceY = 5
});
```

В обоих примерах кода экземпляр `ShadowEffect` создается экземпляр класса со значениями, указанными для каждого свойства перед добавлением элемента управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции. Обратите внимание, что `ShadowEffect.Color` свойство использует специфический для платформы цветовых значений. Дополнительные сведения см. в разделе [класса устройств](~/xamarin-forms/platform/device.md).

## <a name="creating-the-effect-on-each-platform"></a>Создание влияет на каждой платформе

В следующих разделах рассматривается реализация платформой `LabelShadowEffect` класса.

### <a name="ios-project"></a>Проект iOS

В следующем примере кода показан `LabelShadowEffect` реализации для проекта iOS:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    Control.Layer.CornerRadius = effect.Radius;
                    Control.Layer.ShadowColor = effect.Color.ToCGColor ();
                    Control.Layer.ShadowOffset = new CGSize (effect.DistanceX, effect.DistanceY);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Метод извлекает `ShadowEffect` экземпляра и наборы `Control.Layer` значения заданного свойства для создания тени для свойств. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

### <a name="android-project"></a>Проект Android

В следующем примере кода показан `LabelShadowEffect` реализации для проекта Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached ()
        {
            try {
                var control = Control as Android.Widget.TextView;
                var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                if (effect != null) {
                    float radius = effect.Radius;
                    float distanceX = effect.DistanceX;
                    float distanceY = effect.DistanceY;
                    Android.Graphics.Color color = effect.Color.ToAndroid ();
                    control.SetShadowLayer (radius, distanceX, distanceY, color);
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

`OnAttached` Метод извлекает `ShadowEffect` экземпляра и вызовы [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) способ создания тени, используя заданные значения свойств. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

### <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & проекты платформы универсальных приложений Windows

В следующем примере кода показан `LabelShadowEffect` реализации для проектов Windows Phone и универсальной платформы Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("Xamarin")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.WinPhone81
{
    public class LabelShadowEffect : PlatformEffect
    {
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var effect = (ShadowEffect)Element.Effects.FirstOrDefault (e => e is ShadowEffect);
                    if (effect != null) {
                        var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;
                        var shadowLabel = new Label ();
                        shadowLabel.Text = textBlock.Text;
                        shadowLabel.FontAttributes = FontAttributes.Bold;
                        shadowLabel.HorizontalOptions = LayoutOptions.Center;
                        shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;
                        shadowLabel.TextColor = effect.Color;
                        shadowLabel.TranslationX = effect.DistanceX;
                        shadowLabel.TranslationY = effect.DistanceY;

                        ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                        shadowAdded = true;
                    }
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
    }
}
```

Среды выполнения Windows и универсальной платформе Windows не предоставляют эффект тени и поэтому `LabelShadowEffect` имитирует реализации на обеих платформах путем добавления второго смещения [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) за основной `Label`. `OnAttached` Метод извлекает `ShadowEffect` , создает новый экземпляр `Label`и задает некоторые свойства макета `Label`. Затем он создает тени, задав [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/), и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) свойства, управляющие цвет и расположение `Label`. `shadowLabel` Затем вставляется смещение за основной `Label`. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

## <a name="summary"></a>Сводка

В этой статье продемонстрированы помощи свойств среды CLR для передачи параметров в силу. Свойства CLR можно использовать для определения влияния параметров, не отвечайте на изменения свойств в среде выполнения.


## <a name="related-links"></a>Связанные ссылки

- [Пользовательские модули подготовки отчетов](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Эффект тени (пример)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffect/)
