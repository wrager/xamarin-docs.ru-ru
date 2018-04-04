---
title: Привязки на основе шаблона элемента управления
description: Привязки шаблонов позволяют привязать элементы управления в шаблоне элемента управления к данным открытым свойствам Включение значения свойств для элементов управления в шаблоне элемента управления должен быть легко изменен. В этой статье показано, с помощью привязки шаблонов, чтобы выполнить привязку данных с помощью шаблона элемента управления.
ms.prod: xamarin
ms.assetid: 794A663C-3A8D-438A-BD02-8E97C919B55F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 3b306c79aea9bd2192aa73eddcf95790a9b24353
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="binding-from-a-controltemplate"></a>Привязки на основе шаблона элемента управления

_Привязки шаблонов позволяют привязать элементы управления в шаблоне элемента управления к данным открытым свойствам Включение значения свойств для элементов управления в шаблоне элемента управления должен быть легко изменен. В этой статье показано, с помощью привязки шаблонов, чтобы выполнить привязку данных с помощью шаблона элемента управления._

Объект [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) используется для привязки свойства элемента управления в шаблоне элемента управления привязываемые свойства от родителя *целевой* представление, владеющее шаблон элемента управления. Например, вместо определения текста, отображаемого элементом [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров внутри [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/), можно использовать шаблон привязки для привязки [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства привязываемые свойства, определяющие отображаемый текст.

Объект [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) похож на существующий [ `Binding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Binding/), за исключением того, что *источника* из `TemplateBinding` всегда автоматически устанавливается на родительский объект *целевой* представление, владеющее шаблон элемента управления. Однако обратите внимание, что использование `TemplateBinding` за пределами [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) не поддерживается.

## <a name="creating-a-templatebinding-in-xaml"></a>Создание TemplateBinding в XAML

В языке XAML [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) создается с помощью [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.TemplateBindingExtension/) расширения разметки, как показано в следующем примере кода:

```xaml
<ControlTemplate x:Key="TealTemplate">
  <Grid>
    ...
    <Label Text="{TemplateBinding Parent.HeaderText}" ... />
    ...
    <Label Text="{TemplateBinding Parent.FooterText}" ... />
  </Grid>
</ControlTemplate>
```

Вместо установки [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства в статический текст свойства привязки шаблонов для привязки можно использовать для привязываемые свойства от родителя *целевой* представление, владеющее [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Однако обратите внимание, что привязки шаблонов привязка к `Parent.HeaderText` и `Parent.FooterText`, а не `HeaderText` и `FooterText`. Это, поскольку в этом примере привязываемые свойства определяются на прародителем *целевой* просмотреть, а не родительского элемента, как показано в следующем примере кода:

```xaml
<ContentPage ...>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
          ...
    </ContentView>
</ContentPage>
```

*Источника* шаблона привязки всегда автоматически устанавливается на родительский объект *целевой* представление, владеющее шаблон элемента управления, который вот [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) экземпляр. Привязка использует шаблон [ `Parent` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.Parent/) свойство для возврата родительского элемента `ContentView` экземпляра, то есть [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляра. Таким образом, использование [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) в [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) для привязки к `Parent.HeaderText` и `Parent.FooterText` находит привязываемые свойства, которые определены в `ContentPage`, как в следующем примере кода демонстрируются:

```csharp
public static readonly BindableProperty HeaderTextProperty =
  BindableProperty.Create ("HeaderText", typeof(string), typeof(HomePage), "Control Template Demo App");
public static readonly BindableProperty FooterTextProperty =
  BindableProperty.Create ("FooterText", typeof(string), typeof(HomePage), "(c) Xamarin 2016");

public string HeaderText {
  get { return (string)GetValue (HeaderTextProperty); }
}

public string FooterText {
  get { return (string)GetValue (FooterTextProperty); }
}
```

Это приводит к появлению показано на следующем снимке экрана:

![](template-binding-images/teal-theme.png "С помощью привязки шаблонов бирюзовой шаблон элемента управления")

## <a name="creating-a-templatebinding-in-c35"></a>Создание TemplateBinding в C&#35;

В C# [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) создается с помощью `TemplateBinding` конструктора, как показано в следующем примере кода:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var topLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    topLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.HeaderText"));
    ...
    var bottomLabel = new Label { TextColor = Color.White, VerticalOptions = LayoutOptions.Center };
    bottomLabel.SetBinding (Label.TextProperty, new TemplateBinding ("Parent.FooterText"));
    ...
  }
}
```

Вместо установки [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства в статический текст свойства привязки шаблонов для привязки можно использовать для привязываемые свойства от родителя *целевой* представление, владеющее [ `ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/). Привязка шаблона создается с помощью [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/) метод, указав [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) экземпляр в качестве второго параметра. Обратите внимание, что привязки шаблонов привязки к `Parent.HeaderText` и `Parent.FooterText`, так как привязываемые свойства определяются на прародителем *целевой* просмотреть, а не родительского элемента, как показано в следующем примере кода:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    Content = new ContentView {
      ControlTemplate = tealTemplate,
      Content = new StackLayout {
        ...
      },
      ...
    };
    ...
  }
}
```

Привязываемые свойства определяются на `ContentPage`, как описано выше.

### <a name="binding-a-bindableproperty-to-a-viewmodel-property"></a>Привязка к свойству ViewModel BindableProperty

Как уже упоминалось, [ `TemplateBinding` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/) привязку свойства элемента управления в шаблоне элемента управления привязываемые свойства от родителя *целевой* представление, владеющее шаблон элемента управления. В свою очередь эти привязываемые свойства можно привязать к свойствам в ViewModels.

В следующем примере кода определяется два свойства на ViewModel:

```csharp
public class HomePageViewModel
{
  public string HeaderText { get { return "Control Template Demo App"; } }
  public string FooterText { get { return "(c) Xamarin 2016"; } }
}
```

`HeaderText` И `FooterText` ViewModel свойства могут быть привязаны к, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns:local="clr-namespace:SimpleTheme;assembly=SimpleTheme"
             HeaderText="{Binding HeaderText}" FooterText="{Binding FooterText}" ...>
    <ContentPage.BindingContext>
        <local:HomePageViewModel />
    </ContentPage.BindingContext>
    <ContentView ControlTemplate="{StaticResource TealTemplate}" ...>
    ...
    </ContentView>
</ContentPage>
```

`HeaderText` И `FooterText` привязываемые свойства привязаны к `HomePageViewModel.HeaderText` и `HomePageViewModel.FooterText` свойства из-за параметра [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) к экземпляру `HomePageViewModel` класса. В целом, в результате свойства элемента управления в [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) , привязанной к [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) экземпляров [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), который в свою очередь привязка к Свойства ViewModel.

Эквивалентный код на языке C# показан в следующем примере:

```csharp
public class HomePageCS : ContentPage
{
  ...
  public HomePageCS ()
  {
    BindingContext = new HomePageViewModel ();
    this.SetBinding (HeaderTextProperty, "HeaderText");
    this.SetBinding (FooterTextProperty, "FooterText");
    ...
  }
}
```

Дополнительные сведения о привязке данных к ViewModels см. в разделе [из привязки к данным MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

## <a name="summary"></a>Сводка

В этой статье, с использованием привязки шаблонов, чтобы выполнить привязку данных с помощью шаблона элемента управления. Привязки шаблонов позволяют привязать элементы управления в шаблоне элемента управления к данным открытым свойствам Включение значения свойств для элементов управления в шаблоне элемента управления должен быть легко изменен.



## <a name="related-links"></a>Связанные ссылки

- [Основные сведения о привязке данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [От привязок данных для MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
- [Простой темы с Привязка шаблона (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebinding/)
- [Простой темы с Привязка шаблона и ViewModel (пример)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simplethemewithtemplatebindingandviewmodel/)
- [TemplateBinding](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplateBinding/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
