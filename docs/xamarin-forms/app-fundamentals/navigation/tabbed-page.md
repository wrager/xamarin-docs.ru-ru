---
title: Страница с вкладками
description: Xamarin.Forms TabbedPage состоит из список вкладок и увеличить область сведений, с каждой из вкладок загрузки содержимого в область данных. В этой статье демонстрируется использование TabbedPage для перемещения по коллекции страниц.
ms.prod: xamarin
ms.assetid: C946057F-C77C-412D-82A0-DAF475A24EF5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: 11287d38ec0e01e068ca385c92e6a6efdc323aeb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="tabbed-page"></a>Страница с вкладками

_Xamarin.Forms TabbedPage состоит из список вкладок и увеличить область сведений, с каждой из вкладок загрузки содержимого в область данных. В этой статье демонстрируется использование TabbedPage для перемещения по коллекции страниц._

## <a name="overview"></a>Обзор

В следующих снимках экрана показано [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) на каждой платформе:

![](tabbed-page-images/tab1.png "Пример TabbedPage")

Следующих снимках экрана сосредоточиться на формат вкладку на каждой платформе:

![](tabbed-page-images/tabbedpage-components.png "Вкладка TabbedPage компонентов")

Макет [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)и его вкладки, зависит от платформы:

- На iOS появится список вкладок в нижней части экрана и превышает область сведений. Каждая вкладка содержит также значка, который должен быть 30 x 30 PNG с прозрачностью для обычного разрешения, 60 x 60 для высоким разрешением и 90 x 90 для iPhone 6 Plus разрешения. При наличии более пяти вкладках *дополнительные* появится вкладка, которая может использоваться для доступа к дополнительные вкладки. Дополнительные сведения о загрузке изображений в приложении Xamarin.Forms см. в разделе [работа с образами](~/xamarin-forms/user-interface/images.md). Дополнительные сведения о требованиях к значок см. в разделе [создание приложений с вкладками](~/ios/user-interface/controls/creating-tabbed-applications.md).

    > [!NOTE]
  > Обратите внимание, что `TabbedRenderer` для операций ввода-вывода имеет переопределяемый `GetIcon` метод, который может использоваться для загрузки вкладку значки из указанного источника. Это переопределение дает возможность использовать изображения SVG в виде значков на `TabbedPage`. Кроме того могут быть предоставлены выбранном и невыбранном версии значка.

- На Android появится список вкладок в верхней части экрана, а область сведений находится ниже. Имена вкладок автоматически прописной буквы, и пользователь может прокрутить коллекцию вкладок, если слишком много помещаются на один экран.

    > [!NOTE]
  > Обратите внимание, что при использовании совместимости приложений для Android, каждая вкладка будет также отображаться со значком. Кроме того `TabbedPageRenderer` для Android AppCompat имеет переопределяемый `SetTabIcon` метод, который может использоваться для загрузки вкладку значки из настраиваемого `Drawable`. Это переопределение дает возможность использовать изображения SVG в виде значков на `TabbedPage`.

- В Windows Phone появится список вкладок в верхней части экрана и область сведений находится ниже. Если слишком много помещаются на один экран вкладку, которую имена, автоматически преобразуются в нижнем регистре и пользователь может воспользоваться прокруткой коллекцию вкладок.
- В Windows планшета форм факторов, не всегда отображаются вкладки и пользователи должны проведите вниз (или щелкните правой кнопкой мыши при наличии присоединенного мышь) для просмотра на вкладках `TabbedPage` (как показано ниже).

![](tabbed-page-images/windows-tabs.png "TabbedPage вкладки в Windows")

## <a name="creating-a-tabbedpage"></a>Создание TabbedPage

Два подхода можно использовать для создания [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/):

- [Заполнение](#Populating_a_TabbedPage_with_a_Page_Collection) [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) с коллекцию дочерних [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) объекты, такие как коллекцию [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляров.
- [Назначить](#Populating_a_TabbedPage_with_a_Template) коллекции [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) свойство и назначить [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) для [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) свойство для возврата страницы объекты в коллекции.

Используя оба способа [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) отобразит каждой страницы, как пользователь выбирает каждой из вкладок.

> [!NOTE]
> Рекомендуется [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) должен быть заполнен [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) и [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)только экземпляр. Это поможет обеспечить целостное взаимодействие с пользователем на всех платформах.

<a name="Populating_a_TabbedPage_with_a_Page_Collection" />

### <a name="populating-a-tabbedpage-with-a-page-collection"></a>Заполнение TabbedPage с коллекцией страницы

В следующем примере показан код XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) создаваемых заполнение коллекцию дочерних [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) объектов:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
            xmlns:local="clr-namespace:TabbedPageWithNavigationPage;assembly=TabbedPageWithNavigationPage"
            x:Class="TabbedPageWithNavigationPage.MainPage">
    <local:TodayPage />
    <NavigationPage Title="Schedule" Icon="schedule.png">
        <x:Arguments>
            <local:SchedulePage />
        </x:Arguments>
    </NavigationPage>
</TabbedPage>
```

В следующем примере кода показан соответствующий [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) созданных в C#:

```csharp
public class MainPageCS : TabbedPage
{
  public MainPageCS ()
  {
    var navigationPage = new NavigationPage (new SchedulePageCS ());
    navigationPage.Icon = "schedule.png";
    navigationPage.Title = "Schedule";

    Children.Add (new TodayPageCS ());
    Children.Add (navigationPage);
  }
}
```

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Заполняется двух дочерних [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) объектов. Первый дочерний элемент — [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляра, а второй вкладки — [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) содержащий `ContentPage` экземпляра.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Не поддерживает виртуализацию пользовательского интерфейса. Таким образом, если может снизиться производительность `TabbedPage` содержит слишком много дочерних элементов.

В следующих снимках экрана показано `TodayPage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляр, который отображается на *сегодня* вкладки:

![](tabbed-page-images/today-page.png "ContentPage в TabbedPage")

При выборе *расписания* вкладка отображает `SchedulePage` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляр, который помещается в [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) экземпляром и отображается в Следующий снимок экрана:

![](tabbed-page-images/schedule-page.png "NavigationPage в TabbedPage")

Дополнительные сведения о структуре [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), в разделе [выполнении навигации](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

> [!NOTE]
> Хотя это приемлемо для размещения [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) в [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), не рекомендуется размещать `TabbedPage` в `NavigationPage`. Это происходит потому, IOS, `UITabBarController` всегда служит оболочкой для `UINavigationController`. Дополнительные сведения см. в разделе [сочетании интерфейсы контроллера представления](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Chapters/CombiningViewControllers.html) в библиотеке разработчика iOS.

#### <a name="navigation-inside-a-tab"></a>Навигации на вкладке

Навигации могут выполняться со второй закладки с помощью вызова [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) метод [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) свойство [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляра как показано в следующем примере кода:

```csharp
async void OnUpcomingAppointmentsButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new UpcomingAppointmentsPage ());
}
```

В результате в стек навигации помещается экземпляр `UpcomingAppointmentsPage`, где он становится активной страницей. Это показано на следующих снимках экрана:

![](tabbed-page-images/navigationpage.png "Навигации на вкладке")

Дополнительные сведения о выполнении навигации с помощью [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) см. в описании [иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

<a name="Populating_a_TabbedPage_with_a_Template" />

### <a name="populating-a-tabbedpage-with-a-template"></a>Заполнение TabbedPage с помощью шаблона

В следующем примере показан код XAML [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) создан путем назначения [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) для [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) свойство для возврата страницы объекты в коллекции:

```xaml
<TabbedPage xmlns="http://xamarin.com/schemas/2014/forms"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="clr-namespace:TabbedPageDemo;assembly=TabbedPageDemo"
            x:Class="TabbedPageDemo.TabbedPageDemoPage">
  <TabbedPage.Resources>
    <ResourceDictionary>
      <local:NonNullToBooleanConverter x:Key="booleanConverter" />
    </ResourceDictionary>
  </TabbedPage.Resources>
  <TabbedPage.ItemTemplate>
    <DataTemplate>
      <ContentPage Title="{Binding Name}" Icon="monkeyicon.png">
        <StackLayout Padding="5, 25">
          <Label Text="{Binding Name}" Font="Bold,Large" HorizontalOptions="Center" />
          <Image Source="{Binding PhotoUrl}" WidthRequest="200" HeightRequest="200" />
          <StackLayout Padding="50, 10">
            <StackLayout Orientation="Horizontal">
              <Label Text="Family:" HorizontalOptions="FillAndExpand" />
              <Label Text="{Binding Family}" Font="Bold,Medium" />
            </StackLayout>
            ...
          </StackLayout>
        </StackLayout>
      </ContentPage>
    </DataTemplate>
  </TabbedPage.ItemTemplate>
</TabbedPage>
```

[ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Заполняется данными, задав [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) свойство в конструктор для файла кода:

```csharp
public TabbedPageDemoPage ()
{
  ...
  ItemsSource = MonkeyDataModel.All;
}
```

В следующем примере кода показан соответствующий [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) созданных в C#:

```csharp
public class TabbedPageDemoPageCS : TabbedPage
{
  public TabbedPageDemoPageCS ()
  {
    var booleanConverter = new NonNullToBooleanConverter ();

    ItemTemplate = new DataTemplate (() => {
      var nameLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label)),
        FontAttributes = FontAttributes.Bold,
        HorizontalOptions = LayoutOptions.Center
      };
      nameLabel.SetBinding (Label.TextProperty, "Name");

      var image = new Image { WidthRequest = 200, HeightRequest = 200 };
      image.SetBinding (Image.SourceProperty, "PhotoUrl");

      var familyLabel = new Label {
        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
        FontAttributes = FontAttributes.Bold
      };
      familyLabel.SetBinding (Label.TextProperty, "Family");
      ...

      var contentPage = new ContentPage {
        Icon = "monkeyicon.png",
        Content = new StackLayout {
          Padding = new Thickness (5, 25),
          Children = {
            nameLabel,
            image,
            new StackLayout {
              Padding = new Thickness (50, 10),
              Children = {
                new StackLayout {
                  Orientation = StackOrientation.Horizontal,
                  Children = {
                    new Label { Text = "Family:", HorizontalOptions = LayoutOptions.FillAndExpand },
                    familyLabel
                  }
                },
                ...
              }
            }
          }
        }
      };
      contentPage.SetBinding (TitleProperty, "Name");
      return contentPage;
    });
    ItemsSource = MonkeyDataModel.All;
  }
}
```

Каждая вкладка отображает [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) , использующий ряд [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) и [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) экземпляров, чтобы отобразить данные для вкладки. Содержимое для отображения следующих снимках экрана *Tamarin* вкладки:

![](tabbed-page-images/tab3.png "Заполнение TabbedPage с помощью шаблона")

Затем при выборе другой вкладке отображается содержимое для этой вкладки.

> [!NOTE]
> [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Не поддерживает виртуализацию пользовательского интерфейса. Таким образом, если может снизиться производительность `TabbedPage` содержит слишком много дочерних элементов.

Дополнительные сведения о [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), в разделе [25 главе](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Чарльз Петцольд Xamarin.Forms книги.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать TabbedPage для перемещения по коллекции страниц. Xamarin.Forms [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) состоит из списка вкладок и увеличить область сведений, с каждой из вкладок загрузки содержимого в область данных.


## <a name="related-links"></a>Связанные ссылки

- [Создание страницы](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf)
- [TabbedPageWithNavigationPage (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPageWithNavigationPage)
- [TabbedPage (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/TabbedPage/)
- [TabbedPage](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)
