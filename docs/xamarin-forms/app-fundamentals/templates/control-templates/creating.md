---
title: "Создание шаблона ControlTemplate"
description: "Шаблоны элементов управления можно задать на уровне приложения или страницы. В этой статье показано, как создавать и использовать шаблоны элементов управления."
ms.topic: article
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 53309be2712f14c79b84c2eabb519b86dd73a404
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-controltemplate"></a>Создание шаблона ControlTemplate

_Шаблоны элементов управления можно задать на уровне приложения или страницы. В этой статье показано, как создавать и использовать шаблоны элементов управления._

## <a name="creating-a-controltemplate-in-xaml"></a>Создание шаблона элемента управления в XAML

Для определения [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) на уровне приложения, [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) должны быть добавлены `App` класс. По умолчанию используют все Xamarin.Forms приложения, созданные из шаблона **приложения** класса для реализации [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) подкласс. Для объявления `ControlTemplate` на уровне приложения, в приложении `ResourceDictionary` с помощью XAML, значение по умолчанию **приложения** класса должны быть заменены XAML **приложения** класса и связанный с выделенным кодом, как показано в следующем примере кода:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Каждый [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляр создается как объект для повторного использования в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Это достигается путем предоставления каждое объявление уникальный `x:Key` атрибут, который предоставляет ему описательное ключ в `ResourceDictionary`.

В следующем примере кода показано, связанный с ним `App` кода:

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

А также параметр [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) свойства, код программной части также необходимо вызвать `InitializeComponent` метод, чтобы загрузить и проанализировать связанные XAML.

В следующем примере кода показан [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) применения `TealTemplate` для [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

`TealTemplate` Назначается [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) свойства с помощью `StaticResource` расширения разметки. [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Свойству [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , определяющий содержимое, отображаемое на [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Это содержимое будет отображаться для [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) содержащихся в `TealTemplate`. Это приводит к появлению показано на следующем снимке экрана:

![](creating-images/teal-theme.png "Шаблон элемента управления бирюзовой")

### <a name="re-theming-an-application-at-runtime"></a>RE темы приложения во время выполнения

Щелкнув **изменить тему** кнопка выполняет `OnButtonClicked` метод, как показано в следующем примере кода:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Этот метод заменяет активного [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляр с вместо `ControlTemplate` экземпляра, возникающие на следующем снимке экрана:

![](creating-images/aqua-theme.png "Шаблон элемента управления синей")

> [!NOTE]
> На `ContentPage`, `Content` свойству может быть присвоено и `ControlTemplate` можно также задать свойство. Когда это происходит, если `ControlTemplate` содержит `ContentPresenter` экземпляр содержимого, назначенного `Content` презентация свойство `ContentPresenter` в `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Установка шаблона элемента управления, с помощью стиля

Объект [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) могут также применяться через [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) для дальнейшего расширения возможность темы. Это можно сделать путем создания *неявное* или *явных* стиль представление целевого объекта в [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)и параметр `ControlTemplate` свойства целевого объекта Просмотр в [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляра. В следующем примере кода показан *неявное* стиль, который добавляется к уровню приложения [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Поскольку [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) экземпляр *неявное*, оно будет применяться ко всем `ContentView` экземпляров в приложении. Таким образом, он больше не требуется задать [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) свойства, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Дополнительные сведения о стилях см. в разделе [стили](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Создание шаблона элемента управления на уровне страниц

Помимо создания [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) экземпляров на уровне приложения, они могут также создаваться на уровне страницы, как показано в следующем примере кода:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

При добавлении [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) на уровне страницы, [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) добавляется [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), а затем `ControlTemplate` экземпляры включены в `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Создание шаблона элемента управления в C&#35;

Для определения [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) на уровне приложения, `class` должны создаваться, представляющий `ControlTemplate`. Класс должен наследоваться от [макета](~/xamarin-forms/user-interface/layouts/index.md) используется в шаблоне, как показано в следующем примере кода:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

`AquaTemplate` Класс идентичен `TealTemplate` класса, за исключением того, что используются разные цвета для [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) и [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) свойства.

В следующем примере кода показан [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) применения `TealTemplate` для [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

[ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Экземпляры создаются путем указания типа классы, определяющие шаблонов элементов управления, в `ControlTemplate` конструктора.

[ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) Свойству [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , определяющий содержимое, отображаемое на [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Это содержимое будет отображаться для [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) содержащихся в `TealTemplate`. Тот же механизм описанные ранее используется для изменения темы во время выполнения для `AquaTheme`.

## <a name="summary"></a>Сводка

В этой статье показано, как создавать и использовать шаблоны элементов управления. Шаблоны элементов управления можно задать на уровне приложения или страницы.


## <a name="related-links"></a>Связанные ссылки

- [Стили](~/xamarin-forms/user-interface/styles/index.md)
- [Тема «простой» (образец)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
