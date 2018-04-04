---
title: Создание эффекта
description: Эффекты упростить настройку элемента управления. В этой статье показано, как создать эффект, который изменяет цвет фона элемента управления ввода, когда элемент управления получает фокус.
ms.prod: xamarin
ms.assetid: 9E2C8DB0-36A2-4F13-8E3C-A66D7021DB13
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2016
ms.openlocfilehash: 773636cf879439477a6f71e44f13ae66b8f10ea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="creating-an-effect"></a>Создание эффекта

_Эффекты упростить настройку элемента управления. В этой статье показано, как создать эффект, который изменяет цвет фона элемента управления ввода, когда элемент управления получает фокус._

Процесс создания эффекта в каждом проекте платформой выглядит следующим образом:

1. Создать подкласс `PlatformEffect` класса.
1. Переопределить `OnAttached` логику метода и запись для настройки элемента управления.
1. Переопределить `OnDetached` логику метода и запись для очистки настройки элемента управления, если это необходимо.
1. Добавить [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) эффект классу атрибут. Этот атрибут задает пространство имен расширенных компании для эффекта, предотвращения конфликтов с другими эффектами с тем же именем. Обратите внимание, что этот атрибут может применяться только один раз на один проект.
1. Добавить [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) эффект классу атрибут. Этот атрибут регистрирует эффект уникальный идентификатор, который используется с Xamarin.Forms, вместе с именем группы, для обнаружения эффекта перед ее применением к элементу управления. Атрибут принимает два параметра — имя типа эффект и уникальную строку, которая будет использоваться для обнаружения эффекта перед ее применением к элементу управления.

Результат может быть впоследствии использованы присоединив соответствующий элемент управления.

> [!NOTE]
> Это необязательно для предоставления эффекта в каждом проекте платформы. Попытка использовать эффекта, когда один не зарегистрирован возвращает ненулевое значение, которое не выполняет никаких действий.

Образец приложения показывает `FocusEffect` , изменяющий цвет фона элемента управления, когда он получает фокус. На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](creating-images/focus-effect.png "Обязанности проекта эффект фокус")

[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Управления `HomePage` настраивается путем `FocusEffect` класса в каждом проекте конкретную платформу. Каждый `FocusEffect` класс является производным от `PlatformEffect` класса для каждой платформы. В результате `Entry` управления готовится к просмотру с цветом фона конкретную платформу, которая изменяется, если элемент управления получает фокус, как показано на следующем снимке экрана:

![](creating-images/screenshots-1.png "Влияние на каждой платформе сосредоточиться")
![](creating-images/screenshots-2.png "сосредоточиться влияет на каждой платформе")

## <a name="creating-the-effect-on-each-platform"></a>Создание влияет на каждой платформе

В следующих разделах рассматривается реализация платформой `FocusEffect` класса.

## <a name="ios-project"></a>Проект iOS

В следующем примере кода показан `FocusEffect` реализации для проекта iOS:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.iOS
{
    public class FocusEffect : PlatformEffect
    {
        UIColor backgroundColor;

        protected override void OnAttached ()
        {
            try {
                Control.BackgroundColor = backgroundColor = UIColor.FromRGB (204, 153, 255);
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);

            try {
                if (args.PropertyName == "IsFocused") {
                    if (Control.BackgroundColor == backgroundColor) {
                        Control.BackgroundColor = UIColor.White;
                    } else {
                        Control.BackgroundColor = backgroundColor;
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Метода задает `BackgroundColor` свойство элемента управления, сиреневый с `UIColor.FromRGB` метода, а также сохраняет этот цвет в поле. Эта функциональность упаковывается в `try` / `catch` блокировки в случае элементом управления, эффект присоединяется к не `BackgroundColor` свойство. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

`OnElementPropertyChanged` Переопределения реагирует на изменения привязываемые свойства элемента управления Xamarin.Forms. Когда [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) изменения свойств `BackgroundColor` свойству элемента управления изменяется на белый, если элемент управления имеет фокус, в противном случае он принимает значение сиреневый. Эта функциональность упаковывается в `try` / `catch` блокировки в случае элементом управления, эффект присоединяется к не `BackgroundColor` свойство.

## <a name="android-project"></a>Проект Android

В следующем примере кода показан `FocusEffect` реализации для проекта Android:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly:ResolutionGroupName ("MyCompany")]
[assembly:ExportEffect (typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.Droid
{
    public class FocusEffect : PlatformEffect
    {
        Android.Graphics.Color backgroundColor;

        protected override void OnAttached ()
        {
            try {
                backgroundColor = Android.Graphics.Color.LightGreen;
                Control.SetBackgroundColor (backgroundColor);

            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached ()
        {
        }

        protected override void OnElementPropertyChanged (System.ComponentModel.PropertyChangedEventArgs args)
        {
            base.OnElementPropertyChanged (args);
            try {
                if (args.PropertyName == "IsFocused") {
                    if (((Android.Graphics.Drawables.ColorDrawable)Control.Background).Color == backgroundColor) {
                        Control.SetBackgroundColor (Android.Graphics.Color.Black);
                    } else {
                        Control.SetBackgroundColor (backgroundColor);
                    }
                }
            } catch (Exception ex) {
                Console.WriteLine ("Cannot set property on attached control. Error: ", ex.Message);
            }
        }
    }
}
```

`OnAttached` Вызовы метода `SetBackgroundColor` метод, чтобы задать цвет фона элемента управления, светло-зеленый, а также сохраняет этот цвет в поле. Эта функциональность упаковывается в `try` / `catch` блокировки в случае элементом управления, эффект присоединяется к не `SetBackgroundColor` свойство. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

`OnElementPropertyChanged` Переопределения реагирует на изменения привязываемые свойства элемента управления Xamarin.Forms. Когда [ `IsFocused` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsFocused/) изменения свойств, цвет фона элемента управления изменяется на белый, если элемент управления имеет фокус, в противном случае он изменяется на светло-зеленый. Эта функциональность упаковывается в `try` / `catch` блокировки в случае элементом управления, эффект присоединяется к не `BackgroundColor` свойство.

## <a name="windows-phone--universal-windows-platform-projects"></a>Windows Phone & проекты платформы универсальных приложений Windows

В следующем примере кода показан `FocusEffect` реализации для проектов Windows Phone и универсальной платформы Windows (UWP):

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinRT;

[assembly: ResolutionGroupName("MyCompany")]
[assembly: ExportEffect(typeof(FocusEffect), "FocusEffect")]
namespace EffectsDemo.WinPhone81
{
    public class FocusEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            try
            {
                (Control as Windows.UI.Xaml.Controls.Control).Background = new SolidColorBrush(Colors.Cyan);
                (Control as FormsTextBox).BackgroundFocusBrush = new SolidColorBrush(Colors.White);
            }
            catch (Exception ex)
            {
                Debug.WriteLine("Cannot set property on attached control. Error: ", ex.Message);
            }
        }

        protected override void OnDetached()
        {
        }
    }
}
```

`OnAttached` Метода задает `Background` свойству элемента управления голубой и наборы `BackgroundFocusBrush` свойство на белый. Эта функциональность упаковывается в `try` / `catch` блокировки в случае, если элемент управления, эффект присоединяется к не предоставляет эти свойства. Отсутствует реализация обеспечивается `OnDetached` метод так, как очистка не требуется.

## <a name="consuming-the-effect"></a>Использование эффекта

Использование эффекта с Xamarin.Forms Переносимая библиотека классов (PCL) или проекта библиотеки общих выполняется следующим образом:

1. Объявите элемент управления, который будет настраиваться эффект.
1. Присоединение эффект к элементу управления, добавьте его в элемент управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции.

> [!NOTE]
> Экземпляр эффект можно подключить только к одному элементу управления. Таким образом влияние должна быть разрешена дважды, чтобы использовать его на два элемента управления.

## <a name="consuming-the-effect-in-xaml"></a>Использование эффекта в XAML

В следующем примере показан код XAML [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления, к которому `FocusEffect` присоединен:

```xaml
<Entry Text="Effect attached to an Entry" ...>
    <Entry.Effects>
        <local:FocusEffect />
    </Entry.Effects>
    ...
</Entry>
```

`FocusEffect` Класс в PCL поддерживает использование эффекта в XAML и показано в следующем примере кода:

```csharp
public class FocusEffect : RoutingEffect
{
    public FocusEffect () : base ("MyCompany.FocusEffect")
    {
    }
}
```

`FocusEffect` Класса подклассов [ `RoutingEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.RoutingEffect/) класс, который представляет эффект независимая от платформы, который создает оболочку для внутреннего эффект, обычно от платформы. `FocusEffect` Класс вызывает конструктор базового класса, передавая параметр, состоящий из объединения разрешение имени группы (с использованием [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) атрибут в классе эффект), и уникальный идентификатор было указано при помощи [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) атрибут в классе эффект. Таким образом, когда [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) инициализируется во время выполнения, новый экземпляр `MyCompany.FocusEffect` добавляется к элементу управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции.

Эффекты также могут присоединяться к элементам управления с помощью поведения или с помощью вложенных свойств. Дополнительные сведения о присоединении эффект к элементу управления с помощью поведения см. в разделе [EffectBehavior для повторного использования](~/xamarin-forms/app-fundamentals/behaviors/reusable/effect-behavior.md). Дополнительные сведения о присоединении эффект к элементу управления с помощью вложенных свойств см. в разделе [передача параметров в силу](~/xamarin-forms/app-fundamentals/effects/passing-parameters/index.md).

## <a name="consuming-the-effect-in-cnum"></a>Использование эффекта в C&num;

Эквивалент [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) в C# показано в следующем примере кода:

```csharp
var entry = new Entry {
  Text = "Effect attached to an Entry",
  ...
};
```

`FocusEffect` Присоединяется к `Entry` экземпляр, добавив эффект к элементу управления [ `Effects` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Effects/) коллекции, как показано в следующем примере кода:

```csharp
public HomePageCS ()
{
  ...
  entry.Effects.Add (Effect.Resolve ("MyCompany.FocusEffect"));
  ...
}
```

[ `Effect.Resolve` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Effect.Resolve/p/System.String/) Возвращает [ `Effect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/) для указанного имени, который представляет собой объединение имя группы разрешения (с использованием [ `ResolutionGroupName` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResolutionGroupNameAttribute/) атрибут в классе силу) и уникальный идентификатор, который был указан с помощью [ `ExportEffect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ExportEffectAttribute/) атрибут в классе эффект. Если платформа не дает эффекта, `Effect.Resolve` метод возвращает значение, отличное от`null` значение.

## <a name="summary"></a>Сводка

В этой статье показано, как для создания эффекта, которое изменяет цвет фона [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления, когда элемент управления получает фокус.


## <a name="related-links"></a>Связанные ссылки

- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
- [Effect](https://developer.xamarin.com/api/type/Xamarin.Forms.Effect/)
- [PlatformEffect](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformEffect%3CTContainer,TControl%3E/)
- [Фоновый цвет эффект (пример)](https://developer.xamarin.com/samples/xamarin-forms/effects/backgroundcoloreffect/)
- [Эффект фокусировки (пример)](https://developer.xamarin.com/samples/xamarin-forms/effects/focuseffect/)
