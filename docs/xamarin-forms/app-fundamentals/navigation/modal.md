---
title: Модальные страниц
description: Xamarin.Forms поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена. В этой статье показано, как для перехода по страницам модальным.
ms.prod: xamarin
ms.assetid: 486CB7FD-2B9A-4DE3-94BD-C8D904E5D3C6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 909a04398043a3c2f0c30e4da82d174a6bfaf148
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="modal-pages"></a>Модальные страниц

_Xamarin.Forms предоставляет поддержку для модального страниц. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена. В этой статье показано, как для перехода по страницам модальным._

В этой статье рассматриваются следующие вопросы:

- [Выполнение навигации](#Performing_Navigation) — передача страниц на стеке модальное извлечение страниц из модального стека, отключение кнопки "Назад" и анимации переходов.
- [Передача данных при переходе](#Passing_Data_when_Navigating) — передача данных через конструктор страницы и `BindingContext`.

## <a name="overview"></a>Обзор

Модальные страницы может быть любой из [страницы](~/xamarin-forms/user-interface/controls/pages.md) типы, поддерживаемые Xamarin.Forms. Для отображения модальных страницы приложения будет передавать его в модальное стек которых она становится активной страницы, как показано на следующей схеме:

![](modal-images/pushing.png "Отправить страницу модальное стека")

Для возврата на предыдущую страницу приложение отобразит текущей страницы из модального стека, и новый верхний страница становится активной, как показано на следующей схеме:

![](modal-images/popping.png "Выталкивания страницы из модального стека")

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Выполнение навигации

Методы модальной навигации предоставляются свойством [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) любых типов, производных от класса [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Эти методы предоставляют возможность [push модальное страниц](#Pushing_Pages_to_the_Modal_Stack) стек модальные и [pop модальное страниц](#Popping_Pages_from_the_Modal_Stack) из модального стека.

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Также предоставляет свойство [ `ModalStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) свойства, из которого можно получить модальное страниц в модальное стека. Однако не существует средств для работы с модальным стеком или перехода к корневой странице модальной навигации. Причина в том, что такие операции поддерживаются не всеми базовыми платформами.

> [!NOTE]
> Экземпляр [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) не требуется для навигации по модальным страницам.

<a name="Pushing_Pages_to_the_Modal_Stack" />

### <a name="pushing-pages-to-the-modal-stack"></a>Передача страниц на модальное стека

Для перехода к `ModalPage` необходимо вызвать [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) метод [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) свойства текущей страницы, как демонстрируется в следующем примере кода:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    ...
    await Navigation.PushModalAsync (detailPage);
  }
}
```

В результате `ModalPage` экземпляр помещается в модальное стек, где оно станет активной странице, указано, что элемент был выбран в [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) на `MainPage` экземпляра. `ModalPage` На следующих снимках экрана показан экземпляр:

![](modal-images/modalpage.png "Модальные пример страницы")

Когда [ `PushModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page)/) вызывается, происходят следующие события:

- Вызов страницы `PushModalAsync` имеет его [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) переопределение вызывается при условии, что базовая платформа не Android.
- На странице, куда выполняется переход его [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределение вызывается.
- `PushAsync` Завершения задачи.

Тем не менее точный порядок, что эти события происходят — зависит от используемой платформы. Дополнительные сведения см. в разделе [Глава 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Чарльз Петцольд Xamarin.Forms книги.

> [!NOTE]
> Вызовы [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) и [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределения не может рассматриваться как гарантированную указаний по страницам. Например, для операций ввода-вывода `OnDisappearing` переопределение вызывается на активной странице, когда приложение завершает работу.

<a name="Popping_Pages_from_the_Modal_Stack" />

### <a name="popping-pages-from-the-modal-stack"></a>Извлечение страниц из модального стека

Активная страница может извлекается из модального стека, нажав клавишу *обратно* кнопку на устройстве, независимо от ли это физическую кнопку на устройстве или на экране кнопку.

Чтобы вернуться на исходную страницу программным образом, экземпляр `ModalPage` должен вызвать метод [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/), как показано в следующем примере кода:

```csharp
async void OnDismissButtonClicked (object sender, EventArgs args)
{
  await Navigation.PopModalAsync ();
}
```

В результате `ModalPage` экземпляр удаляется из модального стека с новой страницей верхних становится активной странице. Когда [ `PopModalAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/) вызывается, происходят следующие события:

- Вызов страницы `PopModalAsync` имеет его [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) переопределение вызывается.
- На странице, возвращается его [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределение вызывается при условии, что базовая платформа не Android.
- `PopModalAsync` Возвращает задач.

Тем не менее точный порядок, что эти события происходят — зависит от используемой платформы. Дополнительные сведения см. в разделе [Глава 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Чарльз Петцольд Xamarin.Forms книги.

### <a name="disabling-the-back-button"></a>Отключение кнопки "Назад"

На Android и Windows Phone, пользователь всегда можно вернуться на предыдущую страницу, нажав клавишу стандартной *обратно* кнопку на устройстве. Если модальные страницы требуется пользователь должен выполнить задачу самодостаточным перед выходом из страницы, необходимо отключить приложение *обратно* кнопки. Это можно сделать путем переопределения [ `Page.OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed/) метода на странице модальным. Дополнительные сведения см. [Глава 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Чарльз Петцольд Xamarin.Forms книги.

### <a name="animating-page-transitions"></a>Страница проходит анимации

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Также предоставляет свойства каждой страницы переопределенный принудительной отправки и pop методы, которые включают `boolean` параметр, который управляет отображением страницы анимация во время перехода, как показано в следующем коде Пример:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushModalAsync (new DetailPage (), false);
}

async void OnDismissButtonClicked (object sender, EventArgs args)
{
  // Page appearance not animated
  await Navigation.PopModalAsync (false);
}
```

Установка `boolean` параметр `false` отключает переход страниц анимацию, установив параметр в `true` позволяет анимации переход страниц, при условии, что он поддерживается используемой платформой. Тем не менее помещать и извлекать методы, которые не хватает этот параметр включает анимации по умолчанию.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Передача данных при переходе

Иногда она необходима для страницы для передачи данных на другую страницу во время навигации. Два способа для решения этой проблемы, при передаче данных с помощью конструктора страницы и задав новую страницу [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) к данным. Каждый теперь обсуждаются в свою очередь.

### <a name="passing-data-through-a-page-constructor"></a>Передача данных с помощью конструктора страницы

Проще всего для передачи данных на другую страницу во время навигации осуществляется через страницу параметра конструктора, как показано в следующем примере кода:

```csharp
public App ()
{
  MainPage = new MainPage (DateTime.Now.ToString ("u")));
}
```

Этот код создает `MainPage` экземпляра, передавая текущую дату и время в формат ISO8601.

`MainPage` Экземпляр получает данные с помощью параметра конструктора, как показано в следующем примере кода:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Данные отображаются на странице, задав [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойство.

### <a name="passing-data-through-a-bindingcontext"></a>Передача данных через объект BindingContext

Альтернативный подход для передачи данных на другую страницу во время перехода является установка на новую страницу [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) к данным, как показано в следующем примере кода:

```csharp
async void OnItemSelected (object sender, SelectedItemChangedEventArgs e)
{
  if (listView.SelectedItem != null) {
    var detailPage = new DetailPage ();
    detailPage.BindingContext = e.SelectedItem as Contact;
    listView.SelectedItem = null;
    await Navigation.PushModalAsync (detailPage);
  }
}
```

Этот код задает [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) из `DetailPage` экземпляр `Contact` экземпляра, а затем переходит к `DetailPage`.

`DetailPage` Затем использует привязку данных для отображения `Contact` экземпляра данных, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="ModalNavigation.DetailPage">
    <ContentPage.Padding>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,40,0,0" />
      </OnPlatform>
    </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" FontSize="Medium" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
              ...
            <Button x:Name="dismissButton" Text="Dismiss" Clicked="OnDismissButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как в C#, в котором можно выполнить привязку данных.

```csharp
public class DetailPageCS : ContentPage
{
  public DetailPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var dismissButton = new Button { Text = "Dismiss" };
    dismissButton.Clicked += OnDismissButtonClicked;

    Thickness padding;
    switch (Device.RuntimePlatform)
    {
        case Device.iOS:
            padding = new Thickness(0, 40, 0, 0);
            break;
        default:
            padding = new Thickness();
            break;
    }

    Padding = padding;
    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        dismissButton
      }
    };
  }

  async void OnDismissButtonClicked (object sender, EventArgs args)
  {
    await Navigation.PopModalAsync ();
  }
}
```

Данные отображаются на странице ряд [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) элементов управления.

Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/index.md).

## <a name="summary"></a>Сводка

В этой статье показано, как для перехода по страницам модальным. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена.


## <a name="related-links"></a>Связанные ссылки

- [Навигация по страницам](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Modal (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Modal/)
- [PassingData (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
