---
title: Настройка записи
description: Управление ввода Xamarin.Forms позволяет одну строку текста, который нужно изменить. В этой статье показано, как создать пользовательское средство отрисовки для элемента управления ввода, что позволяет разработчикам переопределять собственного отрисовку по умолчанию с свои собственные настройки конкретную платформу.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c120add5a301e440911bd9794da77732e7787cc0
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2018
---
# <a name="customizing-an-entry"></a>Настройка записи

_Управление ввода Xamarin.Forms позволяет одну строку текста, который нужно изменить. В этой статье показано, как создать пользовательское средство отрисовки для элемента управления ввода, что позволяет разработчикам переопределять собственного отрисовку по умолчанию с свои собственные настройки конкретную платформу._

Каждый элемент управления Xamarin.Forms имеет сопутствующий модуль подготовки отчетов для каждой платформы, который создает экземпляр собственного элемента управления. Когда [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) элемент управления отрисовывается приложением Xamarin.Forms в iOS `EntryRenderer` создается экземпляр класса, который в свою очередь, приводит создает собственный `UITextField` элемента управления. На платформе Android `EntryRenderer` класс создает экземпляры `EditText` элемента управления. В универсальной платформы Windows (UWP), `EntryRenderer` класс создает экземпляры `TextBox` элемента управления. Дополнительные сведения о модуль подготовки отчетов и классы собственный элемент управления, которые сопоставляются с элементами управления Xamarin.Forms см [подготовки базовых классов и собственные элементы управления](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

На следующей схеме показана связь между [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления и соответствующие собственные элементы управления, которые реализуют:

![](entry-images/entry-classes.png "Связь между управление ввода и реализации собственных элементов управления")

Процесс подготовки отчета можно считать преимущества реализации настроек платформой путем создания пользовательского средства визуализации для [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления на каждой платформе. Это выполняется следующим образом:

1. [Создание](#Creating_the_Custom_Entry_Control) Xamarin.Forms пользовательского элемента управления.
1. [Использовать](#Consuming_the_Custom_Control) пользовательского элемента управления с помощью Xamarin.Forms.
1. [Создание](#Creating_the_Custom_Renderer_on_each_Platform) пользовательское средство отрисовки для элемента управления на каждой платформе.

Каждый элемент теперь обсуждаются в свою очередь, чтобы реализовать [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) элемента управления, имеющего другой цвет фона для каждой платформы.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Создание настраиваемой записи элемента управления

Настраиваемый [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления может быть создан путем создания подкласса `Entry` контролировать, как показано в следующем примере кода:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Управления созданием в проекта переносимой библиотеки классов (PCL) и просто [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления. Настройки элемента управления будет исполняться в пользовательское средство отрисовки, поэтому требуется без дополнительных изменений в `MyEntry` элемента управления.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Использование пользовательского элемента управления

`MyEntry` Элемент управления может ссылаться на языке XAML в проект PCL объявление пространства имен для его расположения и с помощью префикса пространства имен для элемента управления. В следующем примере кода показан способ `MyEntry` управления могут быть использованы XAML-страницы:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Префикс пространства имен можно присвоить любое имя. Тем не менее `clr-namespace` и `assembly` значения должны соответствовать сведений пользовательского элемента управления. После объявления пространства имен префикс используется для ссылки на пользовательский элемент управления.

В следующем примере кода показан способ `MyEntry` управления могут быть использованы на странице C#:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Этот код создает новый [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) объекта, который будет отображать [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) и `MyEntry` управления оба вертикально и горизонтально по центру страницы.

Возможность добавления пользовательского средства отрисовки в каждый проект приложения для настройки внешнего вида элемента управления на каждой платформе.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Создание пользовательского средства визуализации для каждой платформы

Создание класса пользовательского средства отрисовки выполняется следующим образом:

1. Создать подкласс `EntryRenderer` класс, который отображает собственный элемент управления.
1. Переопределить `OnElementChanged` метод, который использует собственный элемент управления и записи логику для настройки элемента управления. Этот метод вызывается, когда создается соответствующий элемент управления Xamarin.Forms.
1. Добавить `ExportRenderer` класс пользовательское средство отрисовки, чтобы указать, что он будет использоваться для отрисовки элемента управления Xamarin.Forms атрибут. Этот атрибут используется для регистрации пользовательского средства визуализации с помощью Xamarin.Forms.

> [!NOTE]
> Это необязательно для предоставления пользовательского средства отрисовки в каждом проекте платформы. Если пользовательское средство отрисовки не зарегистрирован, будет использоваться модуль подготовки отчетов по умолчанию для базового класса элемента управления.

На следующей схеме показана обязанности каждого проекта в образце приложения, а также связи между ними:

![](entry-images/solution-structure.png "Обязанности проекта MyEntry пользовательского модуля подготовки отчетов")

`MyEntry` Управления визуализируется с платформой `MyEntryRenderer` классы, которые являются производными от `EntryRenderer` класса для каждой платформы. Это приведет к каждой `MyEntry` управления готовится к просмотру с цветом фона для конкретной платформы, как показано на следующем снимке экрана:

![](entry-images/screenshots.png "Элемент управления MyEntry на каждой платформе")

`EntryRenderer` Предоставляемых классами `OnElementChanged` метод, который вызывается при создании элемента управления Xamarin.Forms для отображения соответствующего неуправляемого элемента управления. Этот метод принимает `ElementChangedEventArgs` параметра, который содержит `OldElement` и `NewElement` свойства. Эти свойства представляют собой элемент Xamarin.Forms, модуль подготовки отчетов *было* присоединен и элемент Xamarin.Forms, модуль подготовки отчетов *—* присоединенного, соответственно. В примере приложения `OldElement` свойство будет `null` и `NewElement` свойство будет содержать ссылку на `MyEntry` элемента управления.

Переопределенная версия `OnElementChanged` метод `MyEntryRenderer` класса — это место для выполнения настройки собственного элемента управления. Типизированную ссылку на собственном элементе управления, используемого на платформе можно получить с помощью `Control` свойство. Кроме того, можно получить ссылку на элемент управления Xamarin.Forms, который готовится к просмотру с помощью `Element` свойство, несмотря на то, что он не используется в примере приложения.

Каждый класс пользовательского средства отрисовки снабжен `ExportRenderer` атрибут, который регистрирует модуль подготовки отчетов с помощью Xamarin.Forms. Атрибут принимает два параметра — имя типа элемента управления Xamarin.Forms, который готовится к просмотру и имя типа пользовательского средства отрисовки. `assembly` Префикс в атрибут указывает, что атрибут применяется ко всей сборке.

В следующих разделах рассматривается реализация каждого платформой `MyEntryRenderer` класс пользовательского средства отрисовки.

### <a name="creating-the-custom-renderer-on-ios"></a>Создание пользовательского средства визуализации для iOS

В следующем примере кода показано пользовательское средство отрисовки для платформы iOS.

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Вызов базового класса `OnElementChanged` метод создает iOS `UITextField` управления со ссылкой на элемент управления, назначаемый модуль подготовки отчетов `Control` свойство. Цвет фона присваивается значение сиреневый с `UIColor.FromRGB` метод.

### <a name="creating-the-custom-renderer-on-android"></a>Создание пользовательского средства визуализации на Android

В следующем примере кода показано пользовательское средство отрисовки для платформы Android.

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Вызов базового класса `OnElementChanged` метод создает Android `EditText` управления со ссылкой на элемент управления, назначаемый модуль подготовки отчетов `Control` свойство. Цвет фона устанавливается в светло-зеленый с `Control.SetBackgroundColor` метод.

### <a name="creating-the-custom-renderer-on-uwp"></a>Создание пользовательского средства визуализации на UWP

В следующем примере кода показано пользовательское средство отрисовки для UWP.

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Вызов базового класса `OnElementChanged` метод создает `TextBox` управления со ссылкой на элемент управления, назначаемый модуль подготовки отчетов `Control` свойство. Цвет фона устанавливается в голубой путем создания `SolidColorBrush` экземпляра.

## <a name="summary"></a>Сводка

В этой статье продемонстрировано, как создать модуль подготовки пользовательского элемента управления для Xamarin.Forms [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления, позволяя разработчикам переопределять собственного отрисовку по умолчанию с отрисовку, специфический для платформы. Пользовательские модули подготовки отчетов предоставляют мощный подход для настройки внешнего вида элементов управления Xamarin.Forms. Они могут использоваться для изменения небольшой стилей или сложных макета конкретную платформу и настройка поведения.


## <a name="related-links"></a>Связанные ссылки

- [CustomRendererEntry (пример)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
