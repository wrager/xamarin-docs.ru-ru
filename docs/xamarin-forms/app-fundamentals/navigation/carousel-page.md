---
title: Страница «карусель»
description: Xamarin.Forms CarouselPage — это страница, пользователи могут проведите стороны перемещаться по страницам содержимого, такие как коллекции. В этой статье демонстрируется использование CarouselPage для перемещения по коллекции страниц.
ms.prod: xamarin
ms.assetid: 2D14FC9D-DF5F-427E-9006-2AAE61ECF8DC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: d55d8c8d98828097c842cc383037db88097b963d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="carousel-page"></a>Страница «карусель»

_Xamarin.Forms CarouselPage — это страница, пользователи могут проведите стороны перемещаться по страницам содержимого, такие как коллекции. В этой статье демонстрируется использование CarouselPage для перемещения по коллекции страниц._

## <a name="overview"></a>Обзор

В следующих снимках экрана показано [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) на каждой платформе:

![](carousel-page-images/thirdpage.png "Элемент Thid CarouselPage")

Макет [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) идентичны для каждой платформы. Страницы может быть проведена через, проведя справа налево для перехода пересылает по коллекции, а также путем считывания слева направо для перехода вперед по коллекции. Следующих снимках экрана отображение первой страницы в [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) экземпляр:

![](carousel-page-images/firstpage.png "CarouselPage первый элемент")

Проведение пальцем по экрану от правого угла до левого переходит на вторую страницу, как показано на следующем снимке экрана:

![](carousel-page-images/secondpage.png "CarouselPage второго элемента")

Перемещает на третьей странице попытку считывания справа налево, во время считывания слева направо возвращает на предыдущую страницу.

<!--
> [!NOTE]
> The [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) has been deprecated, and will be removed from Xamarin.Forms in a future release. Instead, the [`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/) should be used to provide a gallery-like view, where users can swipe from side to side to move through a collection of items.
-->

## <a name="creating-a-carouselpage"></a>Создание CarouselPage

Два подхода можно использовать для создания [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/):

- [Заполнение](#Populating_a_CarouselPage_with_a_Page_Collection) `CarouselPage` с коллекцию дочерних [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляров.
- [Назначьте](#Populating_a_CarouselPage_with_a_Template) коллекции [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) свойство и назначить [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) для [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) свойство для возврата [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляров объектов в коллекции.

Используя оба способа `CarouselPage` будет затем отображения каждой страницы в свою очередь, проведите участия, перемещение на следующую страницу для отображения. Эти возможности навигации будут испытывать естественным и знакомые пользователям Windows Phone.

> [!NOTE]
> Объект [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) можно заполнять атрибутами [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляров, или `ContentPage` производных продуктов.

<a name="Populating_a_CarouselPage_with_a_Page_Collection" />

### <a name="populating-a-carouselpage-with-a-page-collection"></a>Заполнение CarouselPage с коллекцией страницы

В следующем примере показан код XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) , отображающий три [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляров:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <ContentPage>
        <ContentPage.Padding>
          <OnPlatform x:TypeArguments="Thickness">
              <On Platform="iOS, Android" Value="0,40,0,0" />
          </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
            <Label Text="Red" FontSize="Medium" HorizontalOptions="Center" />
            <BoxView Color="Red" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
        </StackLayout>
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
    <ContentPage>
        ...
    </ContentPage>
</CarouselPage>
```

В следующем примере кода показан эквивалентный пользовательского интерфейса в C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        var redContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                Children = {
                    new Label {
                        Text = "Red",
                        FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                        HorizontalOptions = LayoutOptions.Center
                    },
                    new BoxView {
                        Color = Color.Red,
                        WidthRequest = 200,
                        HeightRequest = 200,
                        HorizontalOptions = LayoutOptions.Center,
                        VerticalOptions = LayoutOptions.CenterAndExpand
                    }
                }
            }
        };
        var greenContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };
        var blueContentPage = new ContentPage {
            Padding = padding,
            Content = new StackLayout {
                ...
            }
        };

        Children.Add (redContentPage);
        Children.Add (greenContentPage);
        Children.Add (blueContentPage);
    }
}
```

Каждый [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) просто отображает [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) конкретного цвета и [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) этого цвета.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Не поддерживает виртуализацию пользовательского интерфейса. Таким образом, если может снизиться производительность `CarouselPage` содержит слишком много дочерних элементов.

Если [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) внедряется в [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) страница [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) должно быть равно `false` для предотвращения конфликтов жестов между `CarouselPage` и `MasterDetailPage`.

Дополнительные сведения о [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), в разделе [25 главе](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Чарльз Петцольд Xamarin.Forms книги.

<a name="Populating_a_CarouselPage_with_a_Template" />

### <a name="populating-a-carouselpage-with-a-template"></a>Заполнение CarouselPage с помощью шаблона

В следующем примере показан код XAML [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) создан путем назначения [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) для [ `ItemTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemTemplate/) свойство для возврата страницы объекты в коллекции:

```xaml
<CarouselPage xmlns="http://xamarin.com/schemas/2014/forms"
              xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
              x:Class="CarouselPageNavigation.MainPage">
    <CarouselPage.ItemTemplate>
        <DataTemplate>
            <ContentPage>
                <ContentPage.Padding>
                  <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS, Android" Value="0,40,0,0" />
                  </OnPlatform>
                </ContentPage.Padding>
                <StackLayout>
                    <Label Text="{Binding Name}" FontSize="Medium" HorizontalOptions="Center" />
                    <BoxView Color="{Binding Color}" WidthRequest="200" HeightRequest="200" HorizontalOptions="Center" VerticalOptions="CenterAndExpand" />
                </StackLayout>
            </ContentPage>
        </DataTemplate>
    </CarouselPage.ItemTemplate>
</CarouselPage>
```

[ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Заполняется данными, задав [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiPage%601.ItemsSource/) свойство в конструктор для файла кода:

```csharp
public MainPage ()
{
    ...
    ItemsSource = ColorsDataModel.All;
}
```

В следующем примере кода показан соответствующий [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) созданных в C#:

```csharp
public class MainPageCS : CarouselPage
{
    public MainPageCS ()
    {
        Thickness padding;
        switch (Device.RuntimePlatform)
        {
            case Device.iOS:
            case Device.Android:
                padding = new Thickness(0, 40, 0, 0);
                break;
            default:
                padding = new Thickness();
                break;
        }

        ItemTemplate = new DataTemplate (() => {
            var nameLabel = new Label {
                FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
                HorizontalOptions = LayoutOptions.Center
            };
            nameLabel.SetBinding (Label.TextProperty, "Name");

            var colorBoxView = new BoxView {
                WidthRequest = 200,
                HeightRequest = 200,
                HorizontalOptions = LayoutOptions.Center,
                VerticalOptions = LayoutOptions.CenterAndExpand
            };
            colorBoxView.SetBinding (BoxView.ColorProperty, "Color");

            return new ContentPage {
                Padding = padding,
                Content = new StackLayout {
                    Children = {
                        nameLabel,
                        colorBoxView
                    }
                }
            };
        });

        ItemsSource = ColorsDataModel.All;
    }
}
```

Каждый [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) просто отображает [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) конкретного цвета и [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) этого цвета.

> [!NOTE]
> [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Не поддерживает виртуализацию пользовательского интерфейса. Таким образом, если может снизиться производительность `CarouselPage` содержит слишком много дочерних элементов.

Если [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) внедряется в [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) страница [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), [ `MasterDetailPage.IsGestureEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.MasterDetailPage.IsGestureEnabledProperty/) должно быть равно `false` для предотвращения конфликтов жестов между `CarouselPage` и `MasterDetailPage`.

Дополнительные сведения о [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/), в разделе [25 главе](https://developer.xamarin.com/r/xamarin-forms/book/chapter25.pdf) Чарльз Петцольд Xamarin.Forms книги.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать [ `CarouselPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) для перемещения по коллекции страниц. `CarouselPage` — Это страница, пользователи могут проведите стороны перемещаться по страницам содержимого, такие как коллекции и предоставляет возможности навигации, в том числе естественным и знакомые пользователям Windows Phone.


## <a name="related-links"></a>Связанные ссылки

- [Создание страницы](~/xamarin-forms/user-interface/controls/pages.md)
- [CarouselPage (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPage/)
- [CarouselPageTemplate (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/CarouselPageTemplate/)
- [CarouselPage](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)
