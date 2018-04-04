---
title: Создание особенности платформы
description: Поставщики могут создавать свои собственные платформы особенности с эффектами. Эффект предоставляет определенных функциональных возможностей, которые затем передаются через конкретную платформу. Результатом является эффект, можно более легко использовать через XAML и код fluent API. В этой статье показано, как предоставлять эффект через конкретную платформу.
ms.prod: xamarin
ms.assetid: 0D0E6274-6EF2-4D40-BB77-3D8E53BCD24B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/23/2016
ms.openlocfilehash: 6283e22d75d9e52ad3e2f300617818c98d887481
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="creating-platform-specifics"></a>Создание особенности платформы

_Поставщики могут создавать свои собственные платформы особенности с эффектами. Эффект предоставляет определенных функциональных возможностей, которые затем передаются через конкретную платформу. Результатом является эффект, можно более легко использовать через XAML и код fluent API. В этой статье показано, как предоставлять эффект через конкретную платформу._

## <a name="overview"></a>Обзор

Процесс создания конкретную платформу выглядит следующим образом:

1. Для реализации конкретных как эффект. Дополнительные сведения см. в разделе [Создание эффекта](~/xamarin-forms/app-fundamentals/effects/creating.md).
1. Создайте класс платформой, предоставляющее эффект. Дополнительные сведения см. в разделе [создание класса платформой](#creating).
1. В классе платформой реализуйте вложенного свойства, чтобы разрешить конкретную платформу для использования через XAML. Дополнительные сведения см. в разделе [Добавление присоединенного свойства](#attached_property).
1. В классе специфический для платформы реализации методов расширения, чтобы разрешить конкретную платформу для использования через fluent API кода. Дополнительные сведения см. в разделе [добавления методов расширения](#extension_methods).
1. Измените реализацию эффект, эффект применяется только в том случае, если был вызван для этой платформы эффект конкретную платформу. Дополнительные сведения см. в разделе [Создание эффект](#creating_the_effect).

Предоставление доступа к эффект от конкретной платформы образом, результат, можно более легко использовать через XAML и код fluent API.

> [!NOTE]
> Envisaged, что поставщики этот способ будет использоваться для создания собственных платформы особенности, облегчает их потребления пользователями. Хотя пользователи могут выбирать для создания собственных платформы особенности, следует отметить, что требуется больше кода, чем создание и использование эффекта.

Образец приложения показывает `Shadow` платформой, добавляющий тени для текста, отображаемого элементом [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) управления:

![](creating-images/screenshots.png "Специфический для платформы тени")

В образце приложения реализуется `Shadow` платформой на каждой платформе, для простоты понимания. Однако помимо Каждая реализация эффект от платформы, реализация класса тени мало чем отличается для каждой платформы. Таким образом это руководство посвящено реализации класса тени и связанные влияет на одну платформу.

Дополнительные сведения об эффектах см. в разделе [Настройка элементов управления с эффектами](~/xamarin-forms/app-fundamentals/effects/index.md).

<a name="creating" />

## <a name="creating-a-platform-specific-class"></a>Создание класса, определяемых платформой

Конкретные платформы создается как `public static` класса:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
  public static Shadow
  {
    ...
  }
}
```

В следующих разделах рассматривается реализация `Shadow` эффект от платформы и связанные.

<a name="attached_property" />

### <a name="adding-an-attached-property"></a>Добавление вложенного свойства

Необходимо добавить вложенное свойство `Shadow` платформой — разрешить использование через XAML:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        const string EffectName = "MyCompany.LabelShadowEffect";

        public static readonly BindableProperty IsShadowedProperty =
            BindableProperty.CreateAttached("IsShadowed",
                                            typeof(bool),
                                            typeof(Shadow),
                                            false,
                                            propertyChanged: OnIsShadowedPropertyChanged);

        public static bool GetIsShadowed(BindableObject element)
        {
            return (bool)element.GetValue(IsShadowedProperty);
        }

        public static void SetIsShadowed(BindableObject element, bool value)
        {
            element.SetValue(IsShadowedProperty, value);
        }

        ...

        static void OnIsShadowedPropertyChanged(BindableObject element, object oldValue, object newValue)
        {
            if ((bool)newValue)
            {
                AttachEffect(element as FormsElement);
            }
            else
            {
                DetachEffect(element as FormsElement);
            }
        }

        static void AttachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || controller.EffectIsAttached(EffectName))
            {
                return;
            }
            element.Effects.Add(Effect.Resolve(EffectName));
        }

        static void DetachEffect(FormsElement element)
        {
            IElementController controller = element;
            if (controller == null || !controller.EffectIsAttached(EffectName))
            {
                return;
            }

            var toRemove = element.Effects.FirstOrDefault(e => e.ResolveId == Effect.Resolve(EffectName).ResolveId);
            if (toRemove != null)
            {
                element.Effects.Remove(toRemove);
            }
        }
    }
}
```

`IsShadowed` Вложенное свойство используется для добавления `MyCompany.LabelShadowEffect` эффект и удалите его из элемента управления, `Shadow` назначена класса. Это присоединенное свойство регистры `OnIsShadowedPropertyChanged` метод, который выполняется при изменении значения свойства. В свою очередь, вызывает этот метод `AttachEffect` или `DetachEffect` для добавления или удаления эффект на основании значения `IsShadowed` вложенное свойство. Эффект, добавлении или удалении из элемента управления путем изменения элемента управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции.

> [!NOTE]
> Обратите внимание, указав значение, которое представляет собой объединение разрешение имени группы и уникальный идентификатор, который указывается в реализации эффект решена эффект. Дополнительные сведения см. в разделе [Создание эффекта](~/xamarin-forms/app-fundamentals/effects/creating.md).

Дополнительные сведения о вложенных свойствах см. в разделе [присоединенного свойства](~/xamarin-forms/xaml/attached-properties.md).

<a name="extension_methods" />

### <a name="adding-extension-methods"></a>Добавление методов расширения

Методы расширения должны быть добавлены `Shadow` платформой — разрешить использование через fluent API кода:

```csharp
namespace MyCompany.Forms.PlatformConfiguration.iOS
{
    using System.Linq;
    using Xamarin.Forms;
    using Xamarin.Forms.PlatformConfiguration;
    using FormsElement = Xamarin.Forms.Label;

    public static class Shadow
    {
        ...
        public static bool IsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config)
        {
            return GetIsShadowed(config.Element);
        }

        public static IPlatformElementConfiguration<iOS, FormsElement> SetIsShadowed(this IPlatformElementConfiguration<iOS, FormsElement> config, bool value)
        {
            SetIsShadowed(config.Element, value);
            return config;
        }
        ...
    }
}
```

`IsShadowed` И `SetIsShadowed` методы расширения вызова get и set для `IsShadowed` вложенное свойство зависимостей, соответственно. Каждый метод расширения работает на `IPlatformElementConfiguration<iOS, FormsElement>` тип, который указывает, что конкретные платформы может быть вызван на [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров из операций ввода-вывода.

<a name="creating_the_effect" />

### <a name="creating-the-effect"></a>Создание эффекта

`Shadow` Добавляет платформой `MyCompany.LabelShadowEffect` для [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)и удаляет его. В следующем примере кода показан `LabelShadowEffect` реализации для проекта iOS:

```csharp
[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(LabelShadowEffect), "LabelShadowEffect")]
namespace ShadowPlatformSpecific.iOS
{
    public class LabelShadowEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            UpdateShadow();
        }

        protected override void OnDetached()
        {
        }

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged(args);

            if (args.PropertyName == Shadow.IsShadowedProperty.PropertyName)
            {
                UpdateShadow();
            }
        }

        void UpdateShadow()
        {
            try
            {
                if (((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.CornerRadius = 5;
                    Control.Layer.ShadowColor = UIColor.Black.CGColor;
                    Control.Layer.ShadowOffset = new CGSize(5, 5);
                    Control.Layer.ShadowOpacity = 1.0f;
                }
                else if (!((Label)Element).OnThisPlatform().IsShadowed())
                {
                    Control.Layer.ShadowOpacity = 0;
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`UpdateShadow` Метода задает `Control.Layer` свойства для создания тени, при условии, что `IsShadowed` вложенному свойству задано `true`и при условии, что `Shadow` платформой был вызван на разные платформы, Эффект реализована. Эта проверка выполняется с `OnThisPlatform` метод.

Если `Shadow.IsShadowed` присоединенного изменения значений свойств во время выполнения потребностей эффект реагировать, удалив тени. Таким образом, переопределения `OnElementPropertyChanged` метод позволяет реагировать на изменения привязываемые свойства путем вызова `UpdateShadow` метод.

Дополнительные сведения о создании эффект см. в разделе [Создание эффекта](~/xamarin-forms/app-fundamentals/effects/creating.md) и [передача параметров эффект, как вложенные свойства](~/xamarin-forms/app-fundamentals/effects/passing-parameters/attached-properties.md).

## <a name="consuming-a-platform-specific"></a>Использование определяемых платформой

`Shadow` Платформой используются в языке XAML, задав `Shadow.IsShadowed` присоединенному свойству `boolean` значение:

```xaml
<ContentPage xmlns:ios="clr-namespace:MyCompany.Forms.PlatformConfiguration.iOS" ...>
  ...
  <Label Text="Label Shadow Effect" ios:Shadow.IsShadowed="true" ... />
  ...
</ContentPage>
```

Кроме того он может использоваться из C# с помощью плавного API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using MyCompany.Forms.PlatformConfiguration.iOS;

...

shadowLabel.On<iOS>().SetIsShadowed(true);
```

Дополнительные сведения об использовании особенности платформы см. в разделе [использование платформы особенности](~/xamarin-forms/platform/platform-specifics/consuming/index.md).

## <a name="summary"></a>Сводка

В этой статье показано, как предоставлять эффект через конкретную платформу. Результатом является эффект, можно более легко использовать через XAML и код fluent API.


## <a name="related-links"></a>Связанные ссылки

- [ShadowPlatformSpecific (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/shadowplatformspecific/)
- [Настройка элементов управления с эффектами](~/xamarin-forms/app-fundamentals/effects/index.md)
- [Вложенные свойства](~/xamarin-forms/xaml/attached-properties.md)
