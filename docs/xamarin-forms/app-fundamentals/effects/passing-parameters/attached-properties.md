---
title: Передача параметров эффект, как вложенные свойства
description: Вложенные свойства можно использовать для определения параметров эффекта, реагирующие на изменения свойств в среде выполнения. В этой статье демонстрируется использование присоединенных свойств для передачи параметров эффекта и изменения параметров во время выполнения.
ms.prod: xamarin
ms.assetid: DFCDCB9F-17DD-4117-BD53-B4FB206BB387
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/05/2016
ms.openlocfilehash: 2ad27289fb7a4d34b9a951c8132f0147577dfc55
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847921"
---
# <a name="passing-effect-parameters-as-attached-properties"></a>Передача параметров эффект, как вложенные свойства

_Вложенные свойства можно использовать для определения параметров эффекта, реагирующие на изменения свойств в среде выполнения. В этой статье демонстрируется использование присоединенных свойств для передачи параметров эффекта и изменения параметров во время выполнения._

Процесс создания параметров эффекта, реагирующие на изменения свойств времени выполнения выглядит следующим образом:

1. Создайте `static` класс, который содержит вложенное свойство для каждого параметра должны быть переданы эффект.
1. Добавьте дополнительные вложенное свойство классу, который будет использоваться для управления, добавление и удаление эффекта элементу управления, который будет вложен в класс. Убедитесь, это подключена регистры свойство `propertyChanged` делегат, который выполняется при изменении значения свойства.
1. Создание `static` методы get и set для каждого вложенного свойства.
1. Реализовать логику в `propertyChanged` делегат, добавление и удаление эффекта.
1. Реализуйте класс является вложенным в `static` класс с именем после эффекта, какие подклассов `RoutingEffect` класса. Для конструктора вызовите конструктор базового класса, передав это объединение имени группы разрешения, и уникальный идентификатор, который указан на каждый класс эффект от платформы.

Параметры могут передаваться с эффектом, добавив вложенные свойства и значения свойств для соответствующего элемента управления. Кроме того параметры могут быть изменены во время выполнения, указав новое значение вложенного свойства.

> [!NOTE]
> Вложенное свойство — это специальный тип привязываемые свойства, определенные в одном классе, но подключенных к другим объектам и распознается в XAML как атрибуты, которые содержат класс и имя свойства, разделенных точкой. Дополнительные сведения см. в разделе [присоединенного свойства](~/xamarin-forms/xaml/attached-properties.md).

Образец приложения показывает `ShadowEffect` , добавляющий тени для текста, отображаемого элементом [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления. Кроме того можно изменить цвет тени во время выполнения. На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](attached-properties-images/shadow-effect.png "Обязанности проекта эффект тени")

Объект [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления `HomePage` настраивается путем `LabelShadowEffect` в каждом проекте конкретную платформу. Параметры передаются каждому `LabelShadowEffect` через вложенные свойства в `ShadowEffect` класса. Каждый `LabelShadowEffect` класс является производным от `PlatformEffect` класса для каждой платформы. Это приведет к тень, добавляемый текст, отображаемый элементом `Label` устанавливаются, как показано на следующем снимке экрана:

![](attached-properties-images/screenshots.png "Эффект тени для каждой платформы")

## <a name="creating-effect-parameters"></a>Создание параметров эффекта

Объект `static` для представления параметров в силу, необходимо создать класс, как показано в следующем примере кода:

```csharp
public static class ShadowEffect
{
  public static readonly BindableProperty HasShadowProperty =
    BindableProperty.CreateAttached ("HasShadow", typeof(bool), typeof(ShadowEffect), false, propertyChanged: OnHasShadowChanged);
  public static readonly BindableProperty ColorProperty =
    BindableProperty.CreateAttached ("Color", typeof(Color), typeof(ShadowEffect), Color.Default);
  public static readonly BindableProperty RadiusProperty =
    BindableProperty.CreateAttached ("Radius", typeof(double), typeof(ShadowEffect), 1.0);
  public static readonly BindableProperty DistanceXProperty =
    BindableProperty.CreateAttached ("DistanceX", typeof(double), typeof(ShadowEffect), 0.0);
  public static readonly BindableProperty DistanceYProperty =
    BindableProperty.CreateAttached ("DistanceY", typeof(double), typeof(ShadowEffect), 0.0);

  public static bool GetHasShadow (BindableObject view)
  {
    return (bool)view.GetValue (HasShadowProperty);
  }

  public static void SetHasShadow (BindableObject view, bool value)
  {
    view.SetValue (HasShadowProperty, value);
  }
  ...

  static void OnHasShadowChanged (BindableObject bindable, object oldValue, object newValue)
  {
    var view = bindable as View;
    if (view == null) {
      return;
    }

    bool hasShadow = (bool)newValue;
    if (hasShadow) {
      view.Effects.Add (new LabelShadowEffect ());
    } else {
      var toRemove = view.Effects.FirstOrDefault (e => e is LabelShadowEffect);
      if (toRemove != null) {
        view.Effects.Remove (toRemove);
      }
    }
  }

  class LabelShadowEffect : RoutingEffect
  {
    public LabelShadowEffect () : base ("MyCompany.LabelShadowEffect")
    {
    }
  }
}
```

`ShadowEffect` Содержит пять вложенные свойства с `static` методы get и set для каждого вложенного свойства. Четыре эти свойства представляют параметры для передачи для каждой платформы `LabelShadowEffect`. `ShadowEffect` Класс также определяет `HasShadow` присоединенное свойство, используемое для управления, добавление и удаление эффекта в элемент управления, `ShadowEffect` назначена класса. Это присоединенное свойство регистры `OnHasShadowChanged` метод, который выполняется при изменении значения свойства. Этот метод добавляет или удаляет зависимости от значения `HasShadow` вложенное свойство.

Вложенный `LabelShadowEffect` класса какие подклассов [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) класса поддерживает эффект добавления и удаления. `RoutingEffect` Класс представляет эффект независимая от платформы, который создает оболочку для внутреннего эффект, обычно от платформы. Это упрощает процесс удаления эффекта, так как нет доступа во время компиляции для сведений о типе для создания эффекта от платформы. `LabelShadowEffect` Конструктор вызывает конструктор базового класса, передавая параметр, состоящий из объединения имени группы разрешения и уникальный идентификатор, который указан на каждый класс эффект от платформы. Это позволяет эффект добавления и удаления в `OnHasShadowChanged` метод следующим образом:

- **Добавление в силу** — новый экземпляр `LabelShadowEffect` добавляется к элементу управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции. Эта процедура заменяет использование [ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) метод, чтобы добавить эффект.
- **Эффект удаления** — первый экземпляр `LabelShadowEffect` в элементе управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) извлекается и удалить коллекцию.

## <a name="consuming-the-effect"></a>Использование эффекта

Каждый платформой `LabelShadowEffect` могут использоваться, добавив вложенных свойств для [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) контролировать, как показано в следующем примере кода XAML:

```xaml
<Label Text="Label Shadow Effect" ...
       local:ShadowEffect.HasShadow="true" local:ShadowEffect.Radius="5"
       local:ShadowEffect.DistanceX="5" local:ShadowEffect.DistanceY="5">
  <local:ShadowEffect.Color>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="Black" />
        <On Platform="Android" Value="White" />
        <On Platform="UWP" Value="Red" />
    </OnPlatform>
  </local:ShadowEffect.Color>
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

ShadowEffect.SetHasShadow (label, true);
ShadowEffect.SetRadius (label, 5);
ShadowEffect.SetDistanceX (label, 5);
ShadowEffect.SetDistanceY (label, 5);
ShadowEffect.SetColor (label, color));
```

Установка `ShadowEffect.HasShadow` присоединенному свойству `true` выполняет `ShadowEffect.OnHasShadowChanged` метод, который добавляет или удаляет `LabelShadowEffect` для [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления. В обоих примерах кода `ShadowEffect.Color` вложенное свойство предоставляет конкретную платформу цветовых значений. Дополнительные сведения см. в разделе [класса устройств](~/xamarin-forms/platform/device.md).

Кроме того [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) позволяет цвет тени должен быть изменен во время выполнения. Когда `Button` нажатии цвет тени, задав следующие изменения кода `ShadowEffect.Color` вложенное свойство:

```csharp
ShadowEffect.SetColor (label, Color.Teal);
```

### <a name="consuming-the-effect-with-a-style"></a>Использование эффекта со стилем

Эффекты, которые могут быть использованы, добавив вложенных свойств для элемента управления также могут использоваться в стиле. В следующем примере показан код XAML *явных* стиль эффект тени, который может применяться к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) элементов управления:

```xaml
<Style x:Key="ShadowEffectStyle" TargetType="Label">
  <Style.Setters>
    <Setter Property="local:ShadowEffect.HasShadow" Value="True" />
    <Setter Property="local:ShadowEffect.Radius" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceX" Value="5" />
    <Setter Property="local:ShadowEffect.DistanceY" Value="5" />
  </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Может применяться к [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) , установив его [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) свойства `Style` экземпляра с помощью `StaticResource`расширения разметки, как показано в следующем примере кода:

```xaml
<Label Text="Label Shadow Effect" ... Style="{StaticResource ShadowEffectStyle}" />
```

Дополнительные сведения о стилях см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md).

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
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                Control.Layer.ShadowOpacity = 1.0f;
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateRadius ()
        {
            Control.Layer.CornerRadius = (nfloat)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            Control.Layer.ShadowColor = ShadowEffect.GetColor (Element).ToCGColor ();
        }

        void UpdateOffset ()
        {
            Control.Layer.ShadowOffset = new CGSize (
                (double)ShadowEffect.GetDistanceX (Element),
                (double)ShadowEffect.GetDistanceY (Element));
        }
    }
```

`OnAttached` Метод вызывает методы, получающие значения вложенного свойства, с использованием `ShadowEffect` методы получения и который задать `Control.Layer` свойства в значения свойств, чтобы создать тень. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если какие-либо `ShadowEffect` присоединенного изменение значения свойства во время выполнения, в силу необходимо реагировать, отображая изменения. Переопределенная версия `OnElementPropertyChanged` метод в классе эффект от платформы — это место для реагирования на изменения привязываемые свойства, как показано в следующем примере кода:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Метод обновляет radius, цвет и смещение тени, при условии, что соответствующие `ShadowEffect` изменилось значение вложенного свойства. Проверяет наличие измененного свойства должны быть выполнены, как это переопределение может быть вызван несколько раз.

### <a name="android-project"></a>Проект Android

В следующем примере кода показан `LabelShadowEffect` реализации для проекта Android:

```csharp
[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.Droid
{
    public class LabelShadowEffect : PlatformEffect
    {
        Android.Widget.TextView control;
        Android.Graphics.Color color;
        float radius, distanceX, distanceY;

        protected override void OnAttached ()
        {
            try {
                control = Control as Android.Widget.TextView;
                UpdateRadius ();
                UpdateColor ();
                UpdateOffset ();
                UpdateControl ();
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateControl ()
        {
            if (control != null) {
                control.SetShadowLayer (radius, distanceX, distanceY, color);
            }
        }

        void UpdateRadius ()
        {
            radius = (float)ShadowEffect.GetRadius (Element);
        }

        void UpdateColor ()
        {
            color = ShadowEffect.GetColor (Element).ToAndroid ();
        }

        void UpdateOffset ()
        {
            distanceX = (float)ShadowEffect.GetDistanceX (Element);
            distanceY = (float)ShadowEffect.GetDistanceY (Element);
        }
    }
```

`OnAttached` Метод вызывает методы, получающие значения вложенного свойства, с использованием `ShadowEffect` методов получения и вызывает метод, который вызывает [ `TextView.SetShadowLayer` ](https://developer.xamarin.com/api/member/Android.Widget.TextView.SetShadowLayer/p/System.Single/System.Single/System.Single/Android.Graphics.Color/) способ создания тени с использованием значений свойств. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если какие-либо `ShadowEffect` присоединенного изменение значения свойства во время выполнения, в силу необходимо реагировать, отображая изменения. Переопределенная версия `OnElementPropertyChanged` метод в классе эффект от платформы — это место для реагирования на изменения привязываемые свойства, как показано в следующем примере кода:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.RadiusProperty.PropertyName) {
      UpdateRadius ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
      UpdateControl ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
               args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
      UpdateControl ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Метод обновляет radius, цвет и смещение тени, при условии, что соответствующие `ShadowEffect` изменилось значение вложенного свойства. Проверяет наличие измененного свойства должны быть выполнены, как это переопределение может быть вызван несколько раз.

### <a name="universal-windows-platform-project"></a>Платформа проекта универсального приложения Windows

В следующем примере кода показан `LabelShadowEffect` реализации проекта универсальной платформы Windows (UWP):

```csharp
[assembly: ResolutionGroupName ("MyCompany")]
[assembly: ExportEffect (typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace EffectsDemo.UWP
{
    public class LabelShadowEffect : PlatformEffect
    {
        Label shadowLabel;
        bool shadowAdded = false;

        protected override void OnAttached ()
        {
            try {
                if (!shadowAdded) {
                    var textBlock = Control as Windows.UI.Xaml.Controls.TextBlock;

                    shadowLabel = new Label ();
                    shadowLabel.Text = textBlock.Text;
                    shadowLabel.FontAttributes = FontAttributes.Bold;
                    shadowLabel.HorizontalOptions = LayoutOptions.Center;
                    shadowLabel.VerticalOptions = LayoutOptions.CenterAndExpand;

                    UpdateColor ();
                    UpdateOffset ();

                    ((Grid)Element.Parent).Children.Insert (0, shadowLabel);
                    shadowAdded = true;
                }
            } catch (Exception ex) {
                Debug.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }
        ...

        void UpdateColor ()
        {
            shadowLabel.TextColor = ShadowEffect.GetColor (Element);
        }

        void UpdateOffset ()
        {
            shadowLabel.TranslationX = ShadowEffect.GetDistanceX (Element);
            shadowLabel.TranslationY = ShadowEffect.GetDistanceY (Element);
        }
    }
}
```

Универсальная платформа Windows не предоставляет эффект тени и поэтому `LabelShadowEffect` имитирует реализации на обеих платформах путем добавления второго смещения [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) за основной `Label`. `OnAttached` Метод создает новый `Label` и задает некоторые свойства макета `Label`. Затем он вызывает методы, получающие значения вложенного свойства, с использованием `ShadowEffect` методов получения и создает тени, задав [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/), [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)и [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) свойства, управляющие цвет и расположение `Label`. `shadowLabel` Затем вставляется смещение за основной `Label`. Эта функциональность упаковывается в `try` / `catch` блокировать, если элемент управления, присоединенный к эффект не `Control.Layer` свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

#### <a name="responding-to-property-changes"></a>Реагирование на изменения свойств

Если какие-либо `ShadowEffect` присоединенного изменение значения свойства во время выполнения, в силу необходимо реагировать, отображая изменения. Переопределенная версия `OnElementPropertyChanged` метод в классе эффект от платформы — это место для реагирования на изменения привязываемые свойства, как показано в следующем примере кода:

```csharp
public class LabelShadowEffect : PlatformEffect
{
  ...
  protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
  {
    if (args.PropertyName == ShadowEffect.ColorProperty.PropertyName) {
      UpdateColor ();
    } else if (args.PropertyName == ShadowEffect.DistanceXProperty.PropertyName ||
                      args.PropertyName == ShadowEffect.DistanceYProperty.PropertyName) {
      UpdateOffset ();
    }
  }
  ...
}
```

`OnElementPropertyChanged` Метод обновляет цвет или смещение тени, при условии, что соответствующие `ShadowEffect` изменилось значение вложенного свойства. Проверяет наличие измененного свойства должны быть выполнены, как это переопределение может быть вызван несколько раз.

## <a name="summary"></a>Сводка

В этой статье продемонстрированы с помощью присоединенного свойства для передачи параметров эффекта и изменения параметров во время выполнения. Вложенные свойства можно использовать для определения параметров эффекта, реагирующие на изменения свойств в среде выполнения.


## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Эффект](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [RoutingEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/)
- [Эффект тени (пример)](https://developer.xamarin.com/samples/xamarin-forms/effects/shadoweffectruntimechange/)
